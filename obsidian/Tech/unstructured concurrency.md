## unstructured concurrency
新しい関係のない`CoroutineScope`を紹介するか、`GlobalScope`を使うことでunstructured concurrencyを作成することができる。
#### 使用用途
コルーチンを呼ぶスコープよりも長く存続させたいコルーチンがある時
```ad-warning
自分で以下を実装しなければならない
- 作業を追跡する
- エラーを扱う
- 良いキャンセルのさせ方
```