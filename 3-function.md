因为开发中经常用到数组函数，很早就想找个机会好好总结，后续会坚持更新！今天看到一句话感觉很给力！特地在此勉励自己！

> 平庸与卓越的距离，可能只差2毫米！

                                    -- 2018年09月03日19:36:38



#### 

1. #### array\_merge\(\);    合并数组
2. #### array\_keys\(\);
3. #### array\_filter\(\);
4. #### in\_array\(\);
5. #### array\_shift\(\);
6. #### array\_map\(\);
7. #### array\_diff\(\);
8. #### array\_values\(\);
9. #### array\_unique\(\);
10. #### array\_flip\(\)
11. #### array\_multisort\(\)
12. #### array\_column\(\)
13. #### array\_intersect\(\)
14. #### array\_key\_exists\(\)
15. #### array\_pad\(\)
16. #### array\_pop\(\)
17. #### array\_product\(\)
18. #### array\_sum\(\)
19. #### array\_push\(\)
20. #### array\_search\(\)

    ---

### PHP怎样定义数组和赋值？ {#articleHeader2}

这个简单，给简单列一下，欢迎补充：

（1）数组定义

```
<?php
    // 数组定义
    $arr1 = array();
    $arr2 = [];
?>
```

（2）数组赋值

```
<?php
    // 利用 list 函数给数组赋值
    list($arr[], $arr[], $arr[]) = [1, 2, 3];
?>
```

### array\_multisort\(\) - 数组排序 {#articleHeader3}

函数功能：可以同时对多个数组进行排序，关联键名保持不变，数字键名会被重新索引。

```
 <?php
    // 自定义数据
    $data[] = array('volume' => 67, 'edition' => 2);
    $data[] = array('volume' => 86, 'edition' => 1);
    $data[] = array('volume' => 85, 'edition' => 6);
    $data[] = array('volume' => 98, 'edition' => 2);
    $data[] = array('volume' => 86, 'edition' => 6);
    $data[] = array('volume' => 67, 'edition' => 7);

    // 取得列的列表
    foreach ($data as $key => $row) {
        $volume[$key]  = $row['volume'];
        $edition[$key] = $row['edition'];
    }

    // 先将数据根据 volume 降序排列，出现重复时再根据 edition 升序排列
    // 把 $data 作为最后一个参数，以通用键排序
    array_multisort($volume, SORT_DESC, $edition, SORT_ASC, $data);
    print_r($data);
 ?>
```

### array\_column\(\) - 获取数组指定一列 {#articleHeader4}

函数功能：根据指定的 key，获取指定的那一列数据。

```
<?php
    // 对目标数组获取 key 的一列，并复制到结果数组
    $resultArr = array_column($targetArr, 'key');
?>
```

### array\_diff\(\) - 数组相减求差集合 {#articleHeader5}

函数功能：对两个数组进行比较，求两个数组的差集。

```
<?php
    // 把两个数组的差集保存到结果数组
    $diffArr = array_diff($arr1, $arr2);
?>
```

### array\_flip\(\) - 数组键和值互换位置 {#articleHeader6}

函数功能：将数组中的键和值进行位置调换，

```
<?php
    // 把目标数组的键和值互换位置
    array_flip($targetArr);
?>
```

### array\_intersect\(\) - 两个数组的交集 {#articleHeader7}

函数功能：比较两个数据的交集，算出两个数组的相同部分。

```
<?php
    // 两个数组的交集保存到结果数组
    $resultArr = array_intersect($arr1, $arr2)
?>
```

### array\_key\_exists\(\) - 判断数组键名是否存在 {#articleHeader8}

函数功能：判断数组中指定键名或索引是否存在，仅适用一维数组。

&lt;?php

```
// 判断数组是否有 key 这个键
if(!array_key_exists('key', $targetArr)) {
    throw new \Exception('目标数组没有key这个键！');
}
```

?&gt;

### array\_merge\(\) - 合并数组 {#articleHeader9}

函数功能：合并多个数据，不会合并相同键值的元素。

```
<?php
    // 合并数组
    $resultArr = array_merge($arr1, $arr2)
?>
```

### array\_pad\(\) - 按照设定补全数组元素 {#articleHeader10}

函数功能：设定函数长度，多除少补地保证数组长度跟设定的一致，可以设置补充元素的值。

```
<?php
    // 结果计划是：$resultArr = [1,2,3,0,0]
    $resultArr = array_pad([1,2,3], 5, 0);
?>
```

### array\_pop\(\) - 数组最后一个元素出栈（删） {#articleHeader11}

函数功能：把数组最后一个函数去掉。

```
<?php
    // 删掉最后一个元素
    $resultArr = array_pop([1,2,3]);// $resultArr = [3]; [1,2]
?>
```

### array\_product\(\) - 数组内元素相乘 {#articleHeader12}

函数功能：计算数组内的所有元素相乘的结果，空数组返回1。

```
<?php
    // 数组内元素相乘
    $result = array_product([1,2,3]) // $result = 6
?>
```

### array\_sum\(\) - 数组内元素相加 {#articleHeader13}

函数功能：计算数组内所有元素相加的结果，空数组返回0。

```
<?php
    // 数组内元素相加
    $result = array_sum([1,2,3,4]) // $result = 10
?>
```

### array\_push\(\) - 数组叠加元素 {#articleHeader14}

函数功能：给数组叠加（入栈）元素，可以是多个。

```
<?php
    //  数组加元素 
    $resultArr = array_push([1,2],3,4); // $resultArr = [1,2,3,4]
?>
```

### array\_search\(\) - 数组搜索键值 {#articleHeader15}

函数功能：搜索数组指定值，搜索成功将返回首个元素的键值。

```
<?php
    // 把数组搜索 needle 的结果保存起来
    $result = array_search('needle', $targetArr);
?><?php
    // 删掉第一个元素
    $resultArr = array_shift([1,2,3]); // [2,3]
?>
```

### array\_shift\(\) - 数组第一个元素出栈（删） {#articleHeader16}

函数功能：把数组中的第一个元素删掉，弹出第一个元素。

```
<?php
    // 删掉第一个元素
    $resultArr = array_shift([1,2,3]); // [2,3]
?>
```

### implode\(\) - 数组转字符串 {#articleHeader17}

函数功能：把数组以一定格式转为字符串。

```
<?php
    $arr = array('Hello','World!','I','love','Shanghai!');
    echo implode(" ",$arr);// 数组以空格连在一起，转成字符串
?>
```

### explode\(\) - 字符串转数组 {#articleHeader18}

函数功能：把字符串以一定格式切割转为数组。

```
<?php
    $str = "Hello world. I love Shanghai!";
    print_r (explode(" ",$str));// 字符串以空格的方式切割，转为数组
?>
```



