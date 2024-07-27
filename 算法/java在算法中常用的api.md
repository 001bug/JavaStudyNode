想要hashmap像c++那样key没有的值有刚声明的时候的默认值
```java
import java.util.HashMap;
import java.util.Map;

public class Main {
    public static void main(String[] args) {
        Map<String, String> map = new HashMap<>();
        map.put("key1", "value1");

        // key 为 "key1" 时，返回 "value1"
        System.out.println(map.getOrDefault("key1", "Default Value"));

        // key 为 "key2" 时，返回默认值 "Default Value"
        System.out.println(map.getOrDefault("key2", "Default Value"));

        // key 为 null 时，返回默认值 "Default Value"
        System.out.println(map.getOrDefault(null, "Default Value"));
    }
}

```
for循环中所有的类型必须一致,不然会不能够正常循环