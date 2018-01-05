# List

## 去重

### 简单数据类型去重

#### 方法一

```java
import java.util.ArrayList;
import java.util.HashSet;

/**
 * Created by Mark on 2018/1/3.
 */
public class Hello_List {
    public static void main(String args[]) {
        ArrayList<String> arrList = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            String url = "hhh";
            arrList.add(url);
        }
        removeDuplicate(arrList);
        System.out.print(arrList.size());
    }

    public static void removeDuplicate(ArrayList arlList) {
        HashSet h = new HashSet(arlList);
        arlList.clear();
        arlList.addAll(h);
    }
}
```

```java
1
```

### object去重

#### 方法一

```java
import java.util.ArrayList;
import java.util.List;

/**
 * Created by Mark on 2018/1/3.
 */
public class Hello_List {
    public static void main(String args[]) {
        List<Item> arrList = new ArrayList<>();
        for (int i = 0; i < 10; i++) {
            Item item = new Item();
            item.url = "hhh";
            arrList.add(item);
        }
        removeDuplicate(arrList);
        System.out.print(arrList.size());
    }

    public static void removeDuplicate(List list) {
        for (int i = 0; i < list.size() - 1; i++) {
            for (int j = list.size() - 1; j > i; j--) {
                Item jItem = (Item) list.get(j);
                String jValue = jItem.url;
                Item iItem = (Item) list.get(i);
                String iValue = iItem.url;
                if (jValue.equals(iValue)) {
                    list.remove(j);
                }
            }
        }
    }
}
```

```
1
```

#### 方法二

```

```

