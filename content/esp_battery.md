Title: ESP-WROOM-02の駆動、電圧まわりについて調べてみた
Date: 2017-08-20
Slug: esp_battery
Category: 電子工作
Tags: ESP-WROOM-02


# バッテリーの種類
* 乾電池
    - NiMH電池 → 5.4~4.0V
    -  アルカリ乾電池 → 4.95~2.7V
        * [ディープスリープモードで3ヶ月ほど持ち続けている](https://blogs.yahoo.co.jp/bokunimowakaru/55401333.html)
        * [特定時しか動作しないようにして約6.8ヶ月の動作を確認](https://blogs.yahoo.co.jp/bokunimowakaru/55477942.html)
        * [乾電池2個と昇圧電源回路で駆動](http://cl.hatenablog.com/entry/esp-wroom-02-tps63000)

* リチウムイオン電池

* 空気亜鉛電池
    - 小型で大容量
    - [ESP-WROOM-02には向いてないっぽい](https://blogs.yahoo.co.jp/bokunimowakaru/folder/1681421.html)

# 回路の組み方

## 電圧について

ESP-WROOM-02は、3.3Vをかけなければ動かない.

また、瞬間的に350mAぐらい(コンデンサーに使っては変わる？他のサイトでは200mA)使うので、それを流しても落ちない電圧が必要.

そして、Wi-Fi接続時は、75mA前後の電流消費で、時々120~180mA程度の電流が流れることがあるらしい.

## 三端子レギュレーター

電圧には変動があるので、回路の中で電源として用いる場合は、出力電圧が一定な3.3V出力の三端子レギュレーターを使うことになる.

しかし、三端子レギュレーターの仕組みとして、入出力電圧の差に相当する損失（ドロップアウト）が生じる.

なので、3.3Vの安定した出力電圧を作るためには、入力電圧に損失分も加えた電圧をレギュレータに投入する必要がある.

秋月電子に、何種類か三端子レギュレーターが売っていて、今回は、Wi-Fiモジュールの特性を考えて、スタンバイ電流が少ないNJU7223F33を用いることにする.

## コンデンサー
レギュレーターの入力側と出力側にコンデンサーを入れる

コンデンサーを付加しないと、3端子レギュレーターが発振することがある. （発振 = 回路に振動が発生してしまうこと)

入力側のコンデンサの容量は0.1~0.3μF程度のセラミックコンデンサ(周波数特性のよいもの)をつける

出力側には、出力電流とレギュレーターでドロップする電圧からレギュレーターでの熱消費を計算して、必要な場合は放熱器をつける.

出力側のコンデンサーの容量が47μFのとき、電源の立ち上がり時に200mAの電流が流れるらしい.

# 参考記事
* [Arduinoをリチウムイオン電池で動かすには | スイッチサイエンス マガジン](http://mag.switch-science.com/2016/02/12/arduino-lithium/)
* [ESP-WROOM-02 で、モバイル物理スイッチを作る - Qiita](http://qiita.com/ie4/items/ae850cdb2c617f3fd6af)
    - 設計が参考になる
* [esp8266(esp-wroom-02)　電池ボックスから電源供給](http://knaka0209.blogspot.jp/2015/08/esp-wroom-02-05.html)
    - レギュレーターやコンデンサーの説明が載っている
* [ESP-WROOM-02とBME280 電池駆動で温度／湿度／気圧測定 (その1) | 東京お気楽カメラ](http://okiraku-camera.tokyo/blog/?p=5032)
    - コンデンサの製品の特性を比べたり、電池の特性を比べている
    - 回路設計も参考になる
* [ESP8266の消費電流の徹底調査 – Ambient](https://ambidata.io/examples/esp8266-current/)
* [ESP8266+ESP32 - 詳細表示 - ボクにもわかる電子工作 - Yahoo!ブログ](https://blogs.yahoo.co.jp/bokunimowakaru/folder/1681421.html)
    - 空気亜鉛電池の説明
* [ESP-WROOM-02 と心拍センサーをバッテリーで動かしてみた。：息子と一緒に Makers：So-netブログ](http://makers-with-myson.blog.so-net.ne.jp/2016-06-02)
    - リチウムイオン電池の回路説明あり
* [ESP-WROOM-02を昇圧電源回路で乾電池2本（2.4V）駆動 - M.C.P.C. (Mamesibori Creation Plus Communication)](http://cl.hatenablog.com/entry/esp-wroom-02-tps63000)
* [Hue Tapスイッチが高いのでESP-WROOM-02で物理スイッチを自作する　後編 | MUDAなことをしよう。](http://make-muda.weblike.jp/2016/07/4209/)
* [ESP8266の消費電流の徹底調査 – Ambient](https://ambidata.io/examples/esp8266-current/)
    - ESP8266の消費電流について詳しい記述がある
    - 乾電池3つで駆動させた回路図参考になる
* [三端子レギュレータの使い方](http://www.riric.jp/electronics/design/power/regu3pin_usage.html)
* [LDOレギュレーターのはなし: SUDOTECK](http://sudoteck.way-nifty.com/blog/2010/06/ldo-b66b.html)
    - Low DropOutレギュレーターの話
* [34391_onboard.pdf](http://www.cqpub.co.jp/hanbai/books/34/34391/34391_onboard.pdf)
    - 三端子レギュレーターの基本動作と正しい使い方
    - 内部の仕組みの説明
