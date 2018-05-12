# Facade Pattern

這邊使用跟上課一樣的例子 **JSON library** 的範例，在 **Android** 中我最常用的 **JSON library** 是 **google** 自家推出的 **gson**，但聽說效能最好的是 **com.fastxml.jackson** 這套。這時候可以透過 **Facade Pattern** 把 **json library** 的功能做一次封裝只把最常用的** serialize 及 deserialize** 透露給外部，並且把使用哪一個 **JSON  library** 的實作細節也給藏起來。



