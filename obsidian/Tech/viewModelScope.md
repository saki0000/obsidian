#### viewModelScope
```
class MyViewModel(): ViewModel() {  
	fun userNeedsDocs() {  
		// Start a new coroutine in a ViewModel  
		viewModelScope.launch {  
			fetchDocs()  
		}  
	}  
}
```
`viewModelScope`はviewModelで`onCleared()`が呼び出された時に、自動でコルーチンをキャンセルする。
- 開始させたコルーチンが続けて他のコルーチンを開始させた時、同じスコープ内になる
- 必要なくなったらキャンセルされるため、ループ処理を書いても、キャンセルされずに永遠実行されることはなくなる
```ad-warning
Coroutinesはコルーチンが一時停止した時に`CancellationException`を投げて協調的にキャンセルされる。
`Throwable`のようなトップレベルの例外をキャッチするException handlersは`CancellationException`をキャッチできる。
例外ハンドラーで例外を消費するか、一時停止しない場合、コルーチンは半キャンセルされた状態のままになります。
```