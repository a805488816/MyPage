package com.thy.util;
### 怎么将属性定价为


```java
import java.lang.reflect.Field;
import java.util.HashMap;
import java.util.Map;

public class MiniUtil {

    public Map<String, Object> tomap() throws IllegalAccessException {
        String map_name = this.getClass().getName();// 类名
        Map<String, Object> map = new HashMap<>();
        Field[] fields = this.getClass().getDeclaredFields();// 属性
        int length = fields.length;
//        for (Constructor field : fields) {
//            System.out.println(field);
//        }
        for (int i = 0; i < length - 1; i++) {
            boolean accessFlag = fields[i].isAccessible();//获取原权限
            String varName = fields[i].getName();//属性名
            fields[i].setAccessible(true);//修改权限
            Object o = fields[i].get(this);
            System.out.println(varName + ":" + o.toString());
            map.put(varName, o);
            fields[i].setAccessible(accessFlag);//还原权限
        }
        return map;
    }
}
```