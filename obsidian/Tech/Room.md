## Roomとは
Roomとは、DBでのデータ保存・取得を出来るライブラリ。
## 構成
- __Roomエンティティ__ : データベースのテーブルを表す。
- __Room DAO(DataAccess Objects)__ : データベース内のデータを取得、更新、挿入、削除するためのメソッド
- __Room Database__ : データベースに関連づけられているDAOのインスタンスをアプリに提供するデータベースクラス
### Roomエンティティ
Entityクラスはテーブルを定義する。
#### 構成
- __Entity fields__ : 各データの名前（ id, Name etc ) 
- Entity Instances : fieldsに紐づいたデータ自身
- __Entity class name__ : テーブルの名前
#### 実装方法
`@Entity`アノテーションをクラスにつけることで、Entityクラスとして認識させる。
- `@PrimaryKey`：インスタンスには一意の主キーが必要。
	- `autoGenerate`パラメータで主キー列を自動生成するかどうかを指定する
```
import androidx.room.PrimaryKey
@Entity(tableName = "items")
data class Item(    
	@PrimaryKey( autoGenerate = true)    
	val id: Int = 0,
	val price: Double,
	val quantity: Int
	)
```
### Room DAO
データベース内のデータを取得、更新、挿入、削除するインターフェースを定義する。
・Repositoryレイヤかな？
・抽象インターフェースに分離することができる
#### 実装方法
- `@Insert` : 挿入
	- `onConflict`パラメータで競合した時の処理を実装できる
- `@Update` : アップデート
- `@Delete` : 削除
- `@Query()` : 自分でクエリを書く時に使う
	- 戻り値を[[KotlinFlow]]にするのがおすすめ
		- データベース内のデータを変更されるたびに通知が届く
		- のでデータが変更されない限りはデータを取得しなくて良くなる
		- Roomは[[バックグランドスレッド]]でクエリを実行するため、[[suspend]]関数にする必要がない
```
@Dao  
interface ItemDao {  
  
    @Query("SELECT * from items ORDER BY name ASC")  
    fun getAllItems(): Flow<List<Item>>  
  
    @Query("SELECT * from items WHERE id = :id")  
    fun getItem(id: Int): Flow<Item>  
  
    // Specify the conflict strategy as IGNORE, when the user tries to add an  
    // existing Item into the database Room ignores the conflict.    
    @Insert(onConflict = OnConflictStrategy.IGNORE)  
    suspend fun insert(item: Item)  
  
    @Update  
    suspend fun update(item: Item)  
  
    @Delete  
    suspend fun delete(item: Item)  
}
```
### Room Database
データベース クラスは、エンティティのリストと DAO を定義する。
#### 実装方法
- `@Database` アノテーションをつけた、`RoomDatabase`クラスを作成する
	- `@Database`でエンティティを渡す
	- DAOを継承した`abstract`関数を定義
- `Instance`に`@Volatile`アノテーションをつける
	- `@Volatile`の値はキャッシュに保存されない
	-  `Instance`の値が常に最新になる
- `Instance` の下、`companion` オブジェクト内で、データベース ビルダーに必要な `Context` パラメータを持つ `getDatabase()` メソッドを定義
- `sychronized`
	- コードブロックの処理は１つの実行スレッドしか入ることができなくなる
- `Room` の `databaseBuilder`を使用して、存在しない場合にのみ（`item_database`）データベースを作成
```
package com.example.inventory.data  
  
import android.content.Context  
import androidx.room.Database  
import androidx.room.Room  
import androidx.room.RoomDatabase  
  
/**  
 * Database class with a singleton Instance object. */@Database(entities = [Item::class], version = 1, exportSchema = false)  
abstract class InventoryDatabase : RoomDatabase() {  
  
    abstract fun itemDao(): ItemDao  
  
    companion object {  
        @Volatile  
        private var Instance: InventoryDatabase? = null  
  
        fun getDatabase(context: Context): InventoryDatabase {  
            // if the Instance is not null, return it, otherwise create a new database instance.  
            return Instance ?: synchronized(this) {  
                Room.databaseBuilder(context, InventoryDatabase::class.java, "item_database")  
                    /**  
                     * Setting this option in your app's database builder means that Room                               * permanently deletes all data from the tables in your database when it                            * attempts to perform a migration with no defined migration path.                                  */
                    .fallbackToDestructiveMigration()  
                    .build()  
                    .also { Instance = it }  
            }       
        }  
    }  
}
```