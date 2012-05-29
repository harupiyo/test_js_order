html 中に書いたJavaScript の実行タイミングの実験
================================================

結論
----

次の順番で実行される

 1. HTML に書かれたJavascript の実行
    script タグが解析され次第、順に実行される.
    外部JS の場合は読み込みと実行が行われてから次のscript タグの実行に移る.
	なお外部JS は複数が並列に読み込まれることはあるが(firebug の「ネット」タブで判る)
	実行順は必ずHTML に記述した順である
 2. DOM 構築完了(DOMContentLoaded のイベント発生)
 3. 外部JS でdefer(HTML5) が指定されているものの実行(html に記述した順で)
 4. 画像も含めて全ての読み込み完了(onload のイベント発生)

番外:
外部JS で非同期読込が指定されているものについては、これらと並列でロード・実行される.
非同期を指定するには、script タグにasync 属性を付けるか(HTML5)、動的に生成したscript タグをappendChild する方法の２通りがある。

実行結果
========
	head 1			< HTML に記述した順で実行されている
	run jquery1		<   〃
	run normal.js	<   〃
	head 2			<   〃
	body 1			<   〃
	run jquery2		< 非同期指定したものがここで実行されている
	run defer.js	< defer 指定したものがここで実行されている. DOMContentLoaded に関係するイベントである.
	jquery.ready	< DOMContentLoaded に関係するイベントである.
	run jquery3		< 非同期指定したものがここで実行されている
	onload

MEMO
----
実行順に不安がある時はこのようにconsole.log() を使った検証を行えば簡単に分かる。

参考文献
========
 * パーフェクトJavaScript 井上 誠一郎、土江 拓郎、浜辺 将太著 P.226
 * http://www.html5.jp/tag/elements/script.html

