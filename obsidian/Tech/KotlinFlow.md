source: https://developer.android.com/kotlin/flow?hl=ja
## Flowとは
Flowは、[[コルーチン]]の一種で、[[suspend関数]]とは異なり、複数の値を順次出力できる。
### 特徴
- Flowは非同期に処理を行える
	- 出力する値は全て同じ型 
		- 例：`Flow<Int>`
- [[Iterator]]とは違って、メインスレッドをブロックせずに、安全にネットワークリクエストを行い値を取得できる
## 流れ
### 構成
- __プロデューサ__：ストリームに出力するデータを生成。
	- 例：リポジトリ
- __（必要な場合）インターミディアリ__：ストリームに出力された値やストリームそのものをバリデーションする 
- __コンシューマ__：ストリームから得られた値を使用する
	- 例：UI
## 作成方法
フローを作成するには、[[Tech/フロービルダー]]を使用する。
[[Tech/emit関数]]を使用してデータストリームに新しい値を手動で出力できる。

```
class NewsRemoteDataSource(    private val newsApi: NewsApi,    private val refreshIntervalMs: Long = 5000) {    val latestNews: Flow<List<ArticleHeadline>> = flow {        while(true) {            val latestNews = newsApi.fetchLatestNews()            emit(latestNews) // Emits the result of the request to the flow            delay(refreshIntervalMs) // Suspends the coroutine for some time        }    }}// Interface that provides a way to make network requests with suspend functionsinterface NewsApi {    suspend fun fetchLatestNews(): List<ArticleHeadline>}
```
