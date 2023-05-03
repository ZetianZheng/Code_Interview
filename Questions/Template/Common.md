# 常用语法
## 常用打印（log 用）
1. HashMap:
      ```java
      map.forEach((key, value) -> System.out.println(key + " = " + value));
      ```
## 常用stream:
1.  Integer ArrayList 转换为int array
      ```java
      List<Integer> ans = new ArrayList<>();
      ...add some values...
      ans.stream().mapToInt(Integer::intValue).toArray();
      ```
## 数组：
1. 翻转数组
      ```java
      Collections.reverse(arraylist);
      ```