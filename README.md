# Arrays-DoublePointers
在java中，提供了静态数组和动态数组两种。静态数组需要提前初始化好，并且初始化后长度不能被改变了，比如int[] temp = new int\[7];也可以使用动态数组List<Integer> temp = new ArrayList<>();<br>

## 对角线遍历
给定一个含有 M x N 个元素的矩阵（M 行，N 列），请以对角线遍历的顺序返回这个矩阵中的所有元素。<br>
```java
输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]

输出:  [1,2,4,7,5,3,6,8,9]
```
解决该题，只需要仔细观察出遍历的规律即可。一个指针有两种遍历方向，分别是斜向上和斜向下，在斜向上方向的遍历，有三种不同的更新横纵坐标的情况；在斜向下方向的遍历上，也有三种不同的更新横纵坐标的情况，然后用代码实现即可。
```java
package practice;

import java.util.Arrays;
import java.util.concurrent.ForkJoinPool;

// 对角线遍历
public class FindDiagonalOrder {
    public static void main(String[] args) {
        int[][] matrix = {{1, 2, 3}, {4, 5, 6}, {7, 8, 9}};
        FindDiagonalOrder obj = new FindDiagonalOrder();
        int[] res = obj.findDiagonalOrder(matrix);
        System.out.println(Arrays.toString(res));
    }

    public int[] findDiagonalOrder(int[][] matrix) {
        if(matrix == null || matrix.length == 0){
            return new int[0];
        }
        if(matrix.length == 1){  // 仅为一行
            return matrix[0];
        }
        int [] temp = new int[matrix.length];
        if(matrix[0].length == 1){  // 仅为一列
            for (int i = 0; i < matrix.length; i++) {
                temp[i] = matrix[i][0];
            }
            return temp;
        }
        int i = 0;
        int j = 0;
        int m = matrix.length;  // 行
        int n = matrix[0].length;  // 列
        int[] res = new int[m * n];
        for (int k = 0; k < m * n; k++) {
            res[k] = matrix[i][j];
            if ((i + j) % 2 == 0) {  // 上
                if (i == 0 && j < n - 1) {
                    j++;
                } else if (i < m -1 && j == n - 1) {
                    i++;
                } else {
                    i--;
                    j++;
                }
            } else {  // 下
                if (i < m -1 && j == 0) {
                    i++;
                } else if (i == m - 1 && j < n -1 ) {
                    j++;
                } else {
                    i++;
                    j--;
                }
            }
        }
        return res;
    }
}
```
## 螺旋矩阵
给定一个包含 m x n 个元素的矩阵（m 行, n 列），请按照顺时针螺旋顺序，返回矩阵中的所有元素。
```java
输入:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
输出: [1,2,3,6,9,8,7,4,5]
```
解答这类问题重点就是发现规律，我发现当一个指针进行迭代的时候，会出现四种情况需要解决，分别是向右、向下、向左和向上四种状态。这四种状态的停止的边界条件是超过数组的边界以及遇到已经访问过的元素，可以使用Set集合存储已经访问过的元素的位置信息，这样就可以解决这类问题了。
```java
package practice;

import java.util.*;

// 螺旋矩阵
public class SpiralOrder {
    public static void main(String[] args) {
        int[][] matrix = {{3}, {2}};
        SpiralOrder obj = new SpiralOrder();
        List<Integer> list = obj.spiralOrder(matrix);
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }
    }

    public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> list = new ArrayList<>();
        Set<String> set = new HashSet<>();
        if (matrix == null || matrix.length == 0) {
            return list;
        }
        if (matrix.length == 1) {  // 一行
            for (int i : matrix[0]) {
                list.add(i);
            }
            return list;
        }
        if (matrix[0].length == 1) {  // 一列
            for (int i = 0; i < matrix.length; i++) {
                list.add(matrix[i][0]);
            }
            return list;
        }

        int flag = 0;
        int m = matrix.length;  // 行
        int n = matrix[0].length;  // 列
        int i = 0;
        int j = 0;
//        list.add(matrix[i][j]);
//        set.add(String.valueOf(i) + String.valueOf(j));
        while (list.size() != m * n) {
            if (flag == 0) {
                while (j < n && !set.contains(String.valueOf(i) + String.valueOf(j))) {
                    list.add(matrix[i][j]);
                    set.add(String.valueOf(i) + String.valueOf(j));
                    j++;
                }
                flag = 1;
                j--;
                i++;
            } else if (flag == 1) {
                while (i < m && !set.contains(String.valueOf(i) + String.valueOf(j))) {
                    list.add(matrix[i][j]);
                    set.add(String.valueOf(i) + String.valueOf(j));
                    i++;
                }
                flag = 2;
                i--;
                j--;
            } else if (flag == 2) {
                while (j >= 0 && !set.contains(String.valueOf(i) + String.valueOf(j))) {
                    list.add(matrix[i][j]);
                    set.add(String.valueOf(i) + String.valueOf(j));
                    j--;
                }
                flag = 3;
                j++;
                i--;
            } else if (flag == 3) {
                while (i >= 0 && !set.contains(String.valueOf(i) + String.valueOf(j))) {
                    list.add(matrix[i][j]);
                    set.add(String.valueOf(i) + String.valueOf(j));
                    i--;
                }
                flag = 0;
                i++;
                j++;
            }
        }

        return list;
    }
}
```
## 双指针技巧-反转字符串
1) 编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char\[] 的形式给出。<br>
2) 不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。<br>
3) 你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。<br>
```java
输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```
使用双指针可以轻松解决这道题，一个指针从前往后遍历，另一个指针从后往前遍历，每次迭代一步都进行元素的交换，停止的边界条件是两指针相遇。
```java
package practice;

import java.util.Arrays;

// 反转字符串
public class ReverseString {
    public static void main(String[] args) {
        char[] s = {'h', 'e'};
        ReverseString obj = new ReverseString();
        obj.reverseString(s);
    }

    public void reverseString(char[] s) {
        int start = 0;
        int end = s.length - 1;
        while (start < end) {
            char temp;
            temp = s[start];
            s[start] = s[end];
            s[end] = temp;
            start++;
            end--;
        }
        System.out.println(Arrays.toString(s));
    }

}
```
## 双指针技巧-移除元素
1) 给定一个数组 nums 和一个值 val，你需要原地移除所有数值等于 val 的元素，返回移除后数组的新长度。<br>
2) 不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。<br>
3) 元素的顺序可以改变。你不需要考虑数组中超出新长度后面的元素。<br>
```java
给定 nums = [0,1,2,2,3,0,4,2], val = 2,

函数应该返回新的长度 5, 并且 nums 中的前五个元素为 0, 1, 3, 0, 4。

注意这五个元素可为任意顺序。

你不需要考虑数组中超出新长度后面的元素。
```
解决该问题，需要用两个指针，一个指针用于每步迭代判断该指针所指向的值和val是否一样，另一个指针记录其前面值与val不一样的个数，这样当进行到第k步的时候，如果当前值和val一致，第一个指针需要继续加1，第二个指针的用处在于不加1；第k+1步如果值和val不一致了，此时需要把该值存储在第二个指针所指向的位置上。
```java
package practice;

import java.util.Arrays;

// 移除元素
public class RemoveElement {
    public static void main(String[] args) {
        int[] nums = {0, 1, 2, 2, 3, 0, 4, 2};
        int val = 2;
        RemoveElement obj = new RemoveElement();
        System.out.println(obj.removeElement(nums, val));
    }

    public int removeElement(int[] nums, int val) {
        int res = 0;
        int flag = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == val) {
                res++;
            } else {
                nums[flag] = nums[i];
                flag++;
            }
        }
        System.out.println(Arrays.toString(nums));
        return nums.length - res;
    }
}
```
