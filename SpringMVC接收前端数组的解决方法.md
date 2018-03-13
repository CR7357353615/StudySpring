# SpringMVC接收前端数组的解决方法
---
前端发送：
```javascript
var test = [];
```

这样类型的数组，SpringMVC接收时必须要多一个参数接收，并且这个参数的名字不能和接收对象中的属性重名。

示例：

前端：
```javascript
vm.params.arrays = ['001', '002'];
```

params中还包括一些别的属性，是后台有定义好的对象去接受的。

后端：
```java
public ResultVO queryInfo(Query query, @RequestParam(value = "arrays[]", required = false)String[] arrays, Paging paging)
```

并且在Query类中，不能有与arrays同名的属性。
