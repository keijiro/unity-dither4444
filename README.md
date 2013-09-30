Unity で高品質な減色を実現する
==============================

概要
----

Unity ではテクスチャ画像のフォーマットに 16 bit color を指定すると単純なビットシフトによる減色が行われますが、これは画質の大きな劣化を招きます。この文書では、ディザリングアルゴリズムを使って、これを改善する方法を紹介します。

動機
----

Android では機種に依存せず使用可能なテクスチャ圧縮アルゴリズムとして ETC1 が採用されていますが、この ETC1 ではアルファチャンネルを使うことができません。そのため、Android においてアルファを使いたい場合には、画質の悪化を我慢して 16 bit color を用いるか、メモリ不足のリスクを承知で 32 bit color を用いるか、どちらか２択を強いられることになります。

（もちろん、GPU コア毎に特化されたテクスチャ圧縮形式を用いることも可能ですが、機種毎に個別のパッケージを用意する手間を考えると、なかなか採用しづらいというのが実際のところです）

Unity において 16 bit color の画質が悪いのは、その減色方式の単純さにあります。単なるビットシフトにより下 16 bit を切り捨てるという処理が行われるため、繊細なグラデーションはすべて潰れて顕著なマッハバンドを残すことになります。

たとえ単純なアルゴリズムでも、何らかのディザリングを適用すれば、そこまで悪い画質にはならないはずです。そこで、テクスチャインポート時に自動的にディザリングを適用するスクリプトを組んでみることにしました。

デフォルトの 16 bit 減色例
--------------------------

左がオリジナルの画像、右が 16 bit 減色後の画像です。グラデーションが潰れてマッハバンドが現れているのが見えます。

![Image A (original)](http://keijiro.github.io/unity-dither4444/a-original.png)![Image A (default)](http://keijiro.github.io/unity-dither4444/a-default.png)

もう一例挙げます。

![Image B (original)](http://keijiro.github.io/unity-dither4444/b-original.png)![Image B (default)](http://keijiro.github.io/unity-dither4444/b-default.png)


ディザリング適用時の 16 bit 減色例
----------------------------------

この文書で提示する方法によりディザリングを適用した場合の 16 bit 減色の例です。左がオリジナルの画像、右が 16 bit 減色後の画像です。若干、色相に乱れが生じていますが、全体としてマッハバンドは消えていることが分かります。

![Image A (original)](http://keijiro.github.io/unity-dither4444/a-original.png)![Image A (dithered)](http://keijiro.github.io/unity-dither4444/a-dither.png)

もう一例挙げます。

![Image B (original)](http://keijiro.github.io/unity-dither4444/b-original.png)![Image B (dithered)](http://keijiro.github.io/unity-dither4444/b-dither.png)
