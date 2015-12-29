#NCMB_GPSLocation
Monacaとニフティクラウドmobile backendを組み合わせて現在地付近のポイントをmapに表示する、
現在地の位置情報をポイント登録するアプリのハンズオン用リポジトリです

mapとしてOpenStreetMapを利用しています。
※OpenStreetMapに関しては以下をご参照ください
https://openstreetmap.jp/



# ポイント表示のコード実装

```js
var onFindSuccess = function(location){
		current.geopoint = location.coords; 
        var geoPoint = new ncmb.GeoPoint(location.coords.latitude, location.coords.longitude);
        console.log("findpoints:"+location.coords.latitude + ":" + location.coords.longitude);
}
```
www内app.jsの上記のconsole.logの下に
下記のコードを実装してください

```js
//insert1.js
var PlacePointsClass = ncmb.DataStore("PlacePoints");
//ニフティクラウド mobile backendにアクセスして検索開始位置を指定
PlacePointsClass.withinKilometers("geo", geoPoint, 5)
                .fetchAll()
                .then(function(results){
                 	var data = [];
                 	for (var i = 0; i < results.length; i++) {
                          var result = results[i];
                          var markers = new OpenLayers.Layer.Markers("Markers");
                          map.addLayer(markers);
                          var regist_location = result.get("geo");
                          var marker = new OpenLayers.Marker(
                              new OpenLayers.LonLat(regist_location.longitude,regist_location.latitude)
                                          .transform(
                                          new OpenLayers.Projection("EPSG:4326"), 
                                          new OpenLayers.Projection("EPSG:900913")
                                      )
                          );
                          markers.addMarker(marker);
                      }
                  });
```

#現在地のポイント登録

```js
//ポイントの登録時に位置情報取得に成功した場合のコールバック
var onSaveSuccess = function(location){
        navigator.notification.prompt(
            ' ',  // メッセージ
            onPrompt,                  // 呼び出すコールバック
            'ポンイントの登録',            // タイトル
            ['登録','やめる'],             // ボタンのラベル名
            'ポイント名'                 // デフォルトのテキスト
        );
        
        function onPrompt(results) {
            current.geopoint = location.coords; 
            var geoPoint = new ncmb.GeoPoint(location.coords.latitude, location.coords.longitude);
            console.log(location.coords.latitude + ":" + location.coords.longitude);
            
        }
        
    };
```

www内app.jsの上記のfunction onPrompt(results)内のconsole.logの下に
下記のコードを実装してください

```js
//insert2.js
var Places = ncmb.DataStore("PlacePoints");
            var point = new Places();
            point.set("name",results.input1);
            point.set("geo", geoPoint);
        
            point.save()
            .then(function(){})
            .catch(function(err){// エラー処理
            });
```
