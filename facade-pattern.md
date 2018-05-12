# Facade Pattern

這邊使用跟上課一樣的例子 **JSON library** 的範例，在 **Android** 中我最常用的 **JSON library** 是 **google** 自家推出的 **gson**，但聽說效能最好的是 **com.fastxml.jackson** 這套。這時候可以透過 **Facade Pattern** 把 **json library** 的功能做一次封裝只把最常用的** serialize 及 deserialize** 透露給外部，並且把使用哪一個 **JSON  library** 的實作細節也給藏起來。

直接來看一下程式碼，只要把下面 gson 的地方註解打開就可以快速換成 gson 的 library 實作

{% code-tabs %}
{% code-tabs-item title="JsonFacade.kt" %}
```kotlin
class JsonFacade {

    companion object {
        inline fun <reified T> deserialize(json: String): T {
            val clazz: Class<T> = T::class.java
//            using gson deserialize json
//            return Gson().fromJson(json, clazz)
//            using jackson deserialize json
            return ObjectMapper().registerKotlinModule().readValue(json, clazz)
        }

        fun <T> serialize(instance: T): String {
//            using gson serialize json
//            return Gson().toJson(instance)
//            using jackson serialize json
            return ObjectMapper().registerKotlinModule().writeValueAsString(instance)
        }
    }
}
```
{% endcode-tabs-item %}

{% code-tabs-item title="TestJsonModel.kt" %}
```kotlin
data class TestJsonModel(
        val name: String,
        val age: Int
)
```
{% endcode-tabs-item %}
{% endcode-tabs %}

補上測試程式碼

> 這邊不得不說 kotlin 的 data class 是我的最愛自動實作了 toString, hasCode 及 equals 幾個方法，用 TestJsonModel 作為這次的 demo data class

{% code-tabs %}
{% code-tabs-item title="JsonFacadeTest.kt" %}
```kotlin
class JsonFacadeTest {

    @Test
    fun test_JsonFacadeTest_serialize_and_deserialize() {
        val target = TestJsonModel("andyang", 30)
        val json = JsonFacade.serialize(target)
        val excepted: TestJsonModel = JsonFacade.deserialize(json)
        Assert.assertEquals(excepted, target)
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

