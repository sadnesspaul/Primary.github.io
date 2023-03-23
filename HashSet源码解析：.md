# HashSet源码解析：

 注意点：1.添加一个元素时，会先得到hash值，会转成->索引值

​				2.找到存储数据表table，看这个索引位置是否已经存放元素若没有，则直接加入；若有，调用             					equals比较，如果相同，就放弃添加，如果不相同，则添加到最后

### 1. add(E e):

1.1 # put()        - 底层使用Map添加方式，(Key,Value)

~~~ java
     map.put(e, PRESENT)==null;//PRESENT:HashSet中的静态常量对象，占位作用
~~~

1.2 # put Val()    

~~~java
    putVal(hash(key), key, value, false, true);
    //计算添加索引位置
	hash(Object key)：return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
~~~

1.3 第一次添加，tab==null，进入resize()方法，进行构建Node[]（length==16）

### 

1.4 第二次添加：双指针不断移动找到null位置添加，

~~~java
 for (int binCount = 0; ; ++binCount) {
                    if ((e = p.next) == null) {
                        p.next = newNode(hash, key, value, null);
                        if (binCount >= TREEIFY_THRESHOLD - 1) //binCount>=8-1
                            treeifyBin(tab, hash);//树化：该方法里还有一个判断条件
                        break;
                    }
                    //值相同处理方法
                    if (e.hash == hash &&
                        ((k = e.key) == key || (key != null && key.equals(k))))
                        break;
                    p = e;
                }
~~~

~~~java
 treeifyBin(tab, hash)->
     if (tab == null || (n = tab.length) < MIN_TREEIFY_CAPACITY//64)
         else 未达到就按数组扩容
~~~

1.5 HashSet 扩容：总元素大于临界值就扩容

~~~java
  if (++size > threshold)
            resize();
~~~



​		

