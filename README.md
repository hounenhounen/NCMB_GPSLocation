#NCMB_GPSLocation
Monacaとニフティクラウドmobile backenを


# ポイント表示のコード実装

下記のコードを実装してください
```js
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
