# Facade Pattern

使用地圖 SDK 時大多時候提供的功能都非常豐富，但實作時不一定會用到所有的功能這時候就可以透過 Facade Pattern 降低複雜度，也可以很容易的抽換掉實作。

這裡透過 **Google Map SDK** 與 **MapBox SDK** 做範例

先定義出 **MapFacade** 的介面

```kotlin
interface MapFacade {
    var onMapReadyCallBack: () -> Unit    fun initMap()    fun moveCamera(lat: Double, lng: Double, zoomLevel: Double)}
```

**MapFacade **提供了下面這些功能

1. **onMapReadyCallBack**  當地圖初始化完成的 callback
2. **initMap** 初始化地圖
3. **moveCamera** 移動地圖到指定坐標及 zoomLevel

接著我們來實作 GoogleMap 及

