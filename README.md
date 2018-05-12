---
description: >-
  在經歷了學習 Design Pattern、 看到什麼都是 Design Pattern、誤用 Design Pattern、忘了 Design
  Pattern 後透過這次在 SkillTree 上決戰設計模式再次 recap 自己對於 Design Pattern 的理解
---

# SkillTree Design Pattern 上課心得

附上決戰設計模式課程網址給有興趣的人：[https://skilltree.my/events/8ebcg](https://skilltree.my/events/8ebcg)

> 課程的內容都是透過 C\# 當作範例
>
> 但這邊的範例都會使用 Android \(kotlin\) 來撰寫，畢竟我是個 Android Developer。

範例程式碼 GitHub : [https://github.com/bng86/design-pattern-in-android](https://github.com/bng86/design-pattern-in-android)

記得在你的 **string.xml** 中補上你的 **sdk api key**

{% code-tabs %}
{% code-tabs-item title="string.xml" %}
```markup
<resources>
    <string name="mapbox_key">YOUR MAPBOX API KEY HERE</string>
    <string name="google_map_key">YOUR GOOGLE MAP API KEY HERE</string>
</resources>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

這邊會把上課講到的 **Pattern** 透過我自己內化後再次撰寫成 **Android Kotlin Sample**

* 生成模式
  * Singleton
  * Factory
* 結構模式
  * Facade
  * [Adapter](https://andyang.gitbook.io/design-pattern/adapter_pattern)
* 行為模式
  * Template

