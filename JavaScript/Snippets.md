# 字符串

### Snippet_001:

> 循环调用 `indexOf` 找到所有匹配的子字符串：

```javascript
(function () {
  var str = 'Lorem ipsum dolor sit amet, consectetur adipisicing elit.';
  var positionArr = [];
  var pos = str.indexOf('e');

  while (pos > -1) {
    positionArr.push(pos);
    pos = str.indexOf('e', pos + 1);
  }

  console.log(positionArr); // [3, 24, 32, 35, 52]
})();
```
