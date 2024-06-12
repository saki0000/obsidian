## structured concurrency
structured concurrencyとは、新しいコルーチンを特定の[[CoroutineScope]]でのみ始動させること。

### structured concurrencyで出来ること
1. 必要がなくなった作業をキャンセルする
2. 実行中の作業を追跡する
3. コルーチンが失敗した時にエラーを合図する
つまり
1. When a **scope** **cancels**, all of its **coroutines** **cancel**.
2. When a **suspend fun** **returns**, all of its **work is done**.
3. When a **coroutine** **errors**, its **caller or scope is notified**.