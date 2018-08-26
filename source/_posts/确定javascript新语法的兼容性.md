title: '确定javascript新语法的兼容性'
date: 2016-11-24 08:51:59
tags: Web开发
---

前段时间使用javascript比较新的语法，比如Destructuring assignment
发现浏览器兼容问题，这个主要是因为不同浏览的版本在语法支持上有不同的要求
类似 chrome 48 不支持，chrome 49 就支持了，所以在使用上大家还是尽量注意要支持的浏览器版本的要求。

参考资料

1. https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment
2. https://www.chromestatus.com/feature/4588790303686656                                   