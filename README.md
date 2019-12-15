# このリポジトリの目的
MySQLで使えるGISデータの配布用リポジトリ<br><br>
※e-Statで配布されている境界データのシェープファイルをogr2ogrを使ってMySQLに取り込んで、Spatialインデックスを追加したもの<br>
<br>
MySQLのGIS機能を手軽に試せるようにするためにこのリポジトリを作りました。

# 配布データの出典
政府統計の総合窓口(e-Stat)（https://www.e-stat.go.jp/）<br><br>
※このリポジトリでは、後述の手順でe-Statからダウンロードしたシェープファイルを加工したものを配布

# 配布データの作成方法
以下のコマンド例は、兵庫県のシェープファイル(h27ka28.shp)に対するコマンド例

1.ogr2ogrを使って文字コードをUTF-8に変更<br>
ogr2ogr -f “ESRI Shapefile” -lco ENCODING=UTF-8 -oo ENCODING=CP932 h27ka28_utf8.shp h27ka28.shp

2.ogr2ogrを使ってシェープファイルをMySQLへインポート<br>
ogr2ogr -f "MySQL" MySQL:"geotest,host=127.0.0.1,user=root,password=root,port=3306" h27ka28_utf8.shp

3.SHAPE列にSRIDを定義し、空間インデックスを追加<br>
mysql> ALTER TABLE geotest.h27ka28_utf8 DROP INDEX SHAPE;<br>
mysql> ALTER TABLE geotest.h27ka28_utf8 MODIFY SHAPE GEOMETRY NOT NULL SRID 4612;<br>
mysql> ALTER TABLE geotest.h27ka28_utf8 ADD SPATIAL INDEX (SHAPE);<br>
mysql> ALTER TABLE geotest.h27ka28_utf8 RENAME geotest.hyogo;<br>

# ダンプファイルのインポート方法
■geotestデータベースを作成して、その中にインポートする例<br>
mysql> create database geotest;<br>
mysql> use geotest<br>
mysql> source h27ka28(Hyogo).dmp<br>
<br>
■全ダンプファイルをインポートする場合のコマンド例(コピー＆ペースト用)<br>
source h27ka01(Hokkaido).dmp<br>
source h27ka02(Aomori).dmp<br>
source h27ka03(iwate).dmp<br>
source h27ka04(Miyagi).dmp<br>
source h27ka05(Akita).dmp<br>
source h27ka06(Yamagata).dmp<br>
source h27ka07(Fukushima).dmp<br>
source h27ka08(Ibaraki).dmp<br>
source h27ka09(Tochigi).dmp<br>
source h27ka10(Gunma).dmp<br>
source h27ka11(Saitama).dmp<br>
source h27ka12(Chiba).dmp<br>
source h27ka13(Tokyo).dmp<br>
source h27ka14(Kanagawa).dmp<br>
source h27ka15(niigata).dmp<br>
source h27ka16(Toyama).dmp<br>
source h27ka17(Ishikawa).dmp<br>
source h27ka18(Fukui).dmp<br>
source h27ka19(Yamanashi).dmp<br>
source h27ka20(Nagano).dmp<br>
source h27ka21(Gifu).dmp<br>
source h27ka22(Shizuoka).dmp<br>
source h27ka23(aichi).dmp<br>
source h27ka24(Mie).dmp<br>
source h27ka25(Shiga).dmp<br>
source h27ka26(Kyoto).dmp<br>
source h27ka27(Osaka).dmp<br>
source h27ka28(Hyogo).dmp<br>
source h27ka29(Nara).dmp<br>
source h27ka30(Wakayama).dmp<br>
source h27ka31(Tottori).dmp<br>
source h27ka32(Shimane).dmp<br>
source h27ka33(Okayama).dmp<br>
source h27ka34(Hiroshima).dmp<br>
source h27ka35(Yamaguchi).dmp<br>
source h27ka36(Tokushima).dmp<br>
source h27ka37(Kagawa).dmp<br>
source h27ka38(Ehime).dmp<br>
source h27ka39(Kochi).dmp<br>
source h27ka40(fukuoka).dmp<br>
source h27ka41(Saga).dmp<br>
source h27ka42(Nagasaki).dmp<br>
source h27ka43(Kumamoto).dmp<br>
source h27ka44(oita).dmp<br>
source h27ka45(miyazaki).dmp<br>
source h27ka46(Kagoshima).dmp<br>
source h27ka47(Okinawa).dmp<br>
<br>
# 備考
自分でシェープファイルをMySQLにインポートしたい場合は、[こちら](https://speakerdeck.com/yoshiakiyamasaki/mysql-8-dot-0deqiang-hua-saretagisji-neng-toshi-yong-shi-li-falsegoshao-jie-a?slide=40)の資料で手順を解説しています

# 免責事項
本データを用いて行う一切の行為について、いかなる責任も負いません。本データを使用したことによって被った損害、損失に対して一切の責任を負いません。
