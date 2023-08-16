# 常用语法
## 常用打印（log 用）
1. HashMap:
      ```java
      map.forEach((key, value) -> System.out.println(key + " = " + value));
      ```
2. Arrays:
   ```java
   Arrays.toString(arr);
   ```
## 常用stream:
1.  Integer ArrayList 转换为int array
      ```java
      List<Integer> ans = new ArrayList<>();
      ...add some values...
      // method 1
      ans.stream().mapToInt(Integer::intValue).toArray();

      // method 2:
      ans.toArray(new int[ans.size()]);
      ```


## 数组：
1. 翻转数组
      ```java
      Collections.reverse(arraylist);
      ```
2. 排序, 使用compare
      ```java
      Arrays.sort(points, (a, b) -> Integer.compare(a[0], b[0]));
      ```
3. 排序多维数组，增加不同条件 (a[0]增序，相等a[1]降序）：
      ```java
      Arrays.sort(people, (a, b) -> {
            if (a[0] == b[0]) return a[1] - b[1];
            return b[0] - a[0];
        });
      ```
4. 打印数组：（二维）
      ```java
      for (int[] row: dp) {
            System.out.println(Arrays.toString(row));
      }
      ```