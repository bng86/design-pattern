# Adapter Pattern

地圖 SDK 提供的功能都非常豐富，但實作時因為 SDK 的不同實作程式碼很有很大的差異，透過 **Adapter Pattern** 可以統一介面可以很容易的抽換掉實作。這時候不會因為突然的需求要置換地圖 SDK 要修改過多的 Client 端程式碼而~~崩潰~~。

這裡透過 **Google Map SDK** 與 **MapBox SDK** 做範例

Adapter Pattern source code : [https://github.com/bng86/design-pattern-in-android/tree/master/app/src/main/java/tw/andyang/designpattern/adapter](https://github.com/bng86/design-pattern-in-android/tree/master/app/src/main/java/tw/andyang/designpattern/adapter)

先定義出 **MapAdapter** 的介面

{% code-tabs %}
{% code-tabs-item title="MapAdapter.kt" %}
```kotlin
interface MapAdapter {
    var onMapReadyCallBack: () -> Unit
    fun initMap()
    fun moveCamera(lat: Double, lng: Double, zoomLevel: Double)
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**MapAdapter **提供了下面這些功能

1. **onMapReadyCallBack**  當地圖初始化完成的 callback
2. **initMap** 初始化地圖
3. **moveCamera** 移動地圖到指定坐標及 zoomLevel

接著我們來實作 **GoogleMap** 及 **MapBox** 的 **Adapter **及 **layout.xml**

**Google Map **實作 **GoogleMapAdapter** 及 **activity\_**_**google\_map.xml**_

{% code-tabs %}
{% code-tabs-item title="GoogleMapAdapter.kt" %}
```kotlin
class GoogleMapAdapter(private val mapFragment: SupportMapFragment) : MapAdapter {

    private lateinit var googleMap: GoogleMap

    override var onMapReadyCallBack: () -> Unit = {}

    override fun initMap() {
        mapFragment.getMapAsync {
            googleMap = it
            onMapReadyCallBack.invoke()
        }
    }

    override fun moveCamera(lat: Double, lng: Double, zoomLevel: Double) {
        googleMap.moveCamera(CameraUpdateFactory.newCameraPosition(CameraPosition.fromLatLngZoom(LatLng(lat, lng), zoomLevel.toFloat())))
    }
}
```
{% endcode-tabs-item %}

{% code-tabs-item title="activity\_google\_map.xml" %}
```markup
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <fragment
        android:id="@+id/mapFragment"
        android:name="com.google.android.gms.maps.SupportMapFragment"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_width="0dp"
        android:layout_height="0dp"/>

</android.support.constraint.ConstraintLayout>

```
{% endcode-tabs-item %}
{% endcode-tabs %}

**MapBox **實作 **MapBoxAdapter** 及 **activity\_**_**mapbox.xml**_

{% code-tabs %}
{% code-tabs-item title="MapBoxAdapter.kt" %}
```kotlin
class MapBoxAdapter(private val mapFragment: SupportMapFragment) : MapAdapter {

    private lateinit var mapboxMap: MapboxMap

    override var onMapReadyCallBack: () -> Unit = {}

    override fun initMap() {
        mapFragment.getMapAsync {
            mapboxMap = it
            onMapReadyCallBack.invoke()
        }
    }

    override fun moveCamera(lat: Double, lng: Double, zoomLevel: Double) {
        mapboxMap.moveCamera(CameraUpdateFactory.newLatLngZoom(LatLng(lat, lng), zoomLevel))
    }
}
```
{% endcode-tabs-item %}

{% code-tabs-item title="activity\_mapbox.xml" %}
```markup
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <fragment
        android:id="@+id/mapFragment"
        android:name="com.mapbox.mapboxsdk.maps.SupportMapFragment"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        android:layout_width="0dp"
        android:layout_height="0dp"/>

</android.support.constraint.ConstraintLayout>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

**Client** 端的程式碼實作如下，只要在宣告 **MapAdapter** 時決定 **MapAdapter** 的實作與要使用的 **UI SDK** 元件為何，三行就可以搞定快速抽換實作。

{% code-tabs %}
{% code-tabs-item title="MapActivity.kt" %}
```kotlin
class MapActivity : AppCompatActivity() {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
//        using google map sdk
        val mapAdapter = createGoogleMapAdapter()
//        using mapbox sdk
//        val mapAdapter = createMapBoxAdapter()

        mapAdapter.initMap()
        mapAdapter.onMapReadyCallBack = {
            Toast.makeText(this, "map init ready", Toast.LENGTH_LONG).show()
            mapAdapter.moveCamera(25.031179, 121.528408, 19.0)
        }
    }

    private fun createGoogleMapAdapter(): MapAdapter {
        setContentView(R.layout.activity_google_map)
        val mapFragment = supportFragmentManager.findFragmentById(R.id.mapFragment) as com.google.android.gms.maps.SupportMapFragment
        return GoogleMapAdapter(mapFragment)
    }

    private fun createMapBoxAdapter(): MapAdapter {
        setContentView(R.layout.activity_mapbox)
        val mapFragment = supportFragmentManager.findFragmentById(R.id.mapFragment) as com.mapbox.mapboxsdk.maps.SupportMapFragment
        return MapBoxAdapter(mapFragment)
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

