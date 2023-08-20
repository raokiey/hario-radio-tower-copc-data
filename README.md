# hario-radio-tower-copc-data

長崎県が公開している3次元点群データから針尾無線塔付近のデータをダウンロードし、  
COPC（Cloud Optimized Point Cloud）形式に変換してみました。
こちらより、COPC Viewerで表示することができます。  

## データの出典
[長崎県「オープンナガサキ」(https://opennagasaki.nerc.or.jp/)](https://opennagasaki.nerc.or.jp/)

## COPCの作成方法

### PDALを用いた作成
以下の条件で作成しています。  
- Ubuntu 20.04 LTS（WSL）
- conda 23.5.2
- Python 3.11.4
- PDAL 2.5.6

1. PDALを扱えるconda環境を作成する  
```shell
conda create -n ${環境名} python pdal=2.5
conda activate ${環境名}
```

2. 針尾無線塔付近の点群データを結合する
```shell
pdal merge 01JE7524.las 01JE7542.las hario_radio_tower.las
```

3. 座標参照系の付与
```shell
 pdal translate -i hario_radio_tower.las -o hario_radio_tower_translated.las --writers.las.a_srs="EPSG:6669"
```

4. メタデータの確認
```shell
pdal info --metadeta hario_radio_tower_translated_clip.las
```

5. COPCへの変換
```shell
pdal translate -i hario_radio_tower_translated_clip.las -o Hario_Radio_Tower.copc.laz --writers.copc.forward=all
```