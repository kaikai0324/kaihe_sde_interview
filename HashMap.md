重写equals方法后重写hashCode方法：
而hashcode是根据对象地址生成一个整数数值。


Java规定：如果根据 equals(Object) 方法，两个对象是相等的，那么对这两个对象中的每个对象调用 hashCode 方法都必须生成相同的整数结果。  

hashMap的存储：一个对象存储到hashMap中的位置是由其key 的hashcode值决定的;查hashMap查找key: 找key的时候hashMap会先根据key值的hashcode经过取余算法定位其所在数组的位置，再根据key的equals方法匹配相同key值获取对应相应的对象；
key的hashcode定位数组bucket，key的equals方法匹配key




hashMap的key和value可以为null，ConcurrentHashMap与Hashtable均不能


HashMap:
源码：
keySet()和values()可用来遍历key和value
Node里有value和next，和final修饰的hash和key




ConcurrentHashMap:
源码：
1.Node里value和next是被volatile修饰，hash和key依然是final
2.put方法return putVal方法，里面有synchronized修饰的代码块
3.remove方法return replaceNode(key, null, null)，里面有synchronized修饰的代码块