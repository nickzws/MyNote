# 开发总结

```php
return array_map(function ($v) use ($tagObj) {
       return [$v => $tagObj->getTagInfo($v)->title,];
}, array_unique($all));
```



