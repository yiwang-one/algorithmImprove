### 总结

### 题目

#### [126. 单词接龙 II](https://leetcode-cn.com/problems/word-ladder-ii/)

  * 给定两个单词（beginWord 和 endWord）和一个字典 wordList，找出所有从 beginWord 到 endWord 的最短转换序列。转换需遵循如下规则：

    每次转换只能改变一个字母。
    转换后得到的单词必须是字典中的单词。
    说明:

    如果不存在这样的转换序列，返回一个空列表。
    所有单词具有相同的长度。
    所有单词只由小写字母组成。
    字典中不存在重复的单词。
    你可以假设 beginWord 和 endWord 是非空的，且二者不相同。

    

  ```
  class Solution {
      public int majorityElement(int[] nums) {
          int count = 0;
          Integer candidate = null;
          for (int num : nums) {
              if (count == 0) {
                  candidate = num;
              }
              count += (num == candidate) ? 1 : -1;
          }
  
          return candidate;
      }
  }
  ```

#### [17. 电话号码的字母组合](https://leetcode-cn.com/problems/letter-combinations-of-a-phone-number/)

给定一个仅包含数字 `2-9` 的字符串，返回所有它能表示的字母组合。

给出数字到字母的映射如下（与电话按键相同）。注意 1 不对应任何字母。

* 回溯法，在每个数字喂尝试所有对应的字符可能，

  与括号组合题目类似 [(22括号生成) ](https://leetcode-cn.com/problems/generate-parentheses)

```
class Solution {
    public List<String> letterCombinations(String digits) {
        List<String> ans = new ArrayList<>();
        if (digits.isEmpty()) {
            return ans;
        }
        Map<Character, String> map = new HashMap<>();
        map.put('2', "abc");
        map.put('3', "def");
        map.put('4', "ghi");
        map.put('5', "jkl");
        map.put('6', "mno");
        map.put('7', "pqrs");
        map.put('8', "tuv");
        map.put('9', "wxyz");
        
        combination(digits, map, 0, "", ans);
        return ans;
    }
    void combination(String digits, Map<Character, String> map, 
                     int index, String str, List<String> ans) {
        // 1. terminator
        if (index == digits.length()) {
            ans.add(str);
            return;
        }
        // 2. process current logica：在当前数字位，尝试放置所有的对应的字符
        
        String curContent = map.get(digits.charAt(index));
        for (Character chr : curContent.toCharArray()) {
            // 3. drill down
            combination(digits, map, index + 1, str + chr, ans);
        }
        // 4. reverse states
    }
}
```

#### 51. [N皇后](https://leetcode-cn.com/problems/n-queens/)

*n* 皇后问题研究的是如何将 *n* 个皇后放置在 *n*×*n* 的棋盘上，并且使皇后彼此之间不能相互攻击。

* 分析：上诉所讲回溯算法，都是在一维位置上进行尝试所有可能的值，而N皇后或者迷宫问题，都是在二维位置上进行尝试所有可能值，并且每一行之间存在某些约束，这就又涉及到剪枝。

* 套用回溯的框架（有优化空间）

  ```
     class Solution {
          LinkedList<Integer> cols = new LinkedList<>();
          LinkedList<Integer> pie = new LinkedList<>();
          LinkedList<Integer> na = new LinkedList<>();
          char[][] itemRet;
          public List<List<String>> solveNQueens(int n) {
              List<List<String>> ans = new ArrayList<>();
              itemRet = new char[n][n];
              for (int i  = 0; i < n; ++i) {
                  for (int j = 0; j < n; ++j) {
                      itemRet[i][j] = '.';
                  }
              }
              solve(ans, n, 0);
              return ans;
          }
          private void solve(List<List<String>> ans, int n, int rowIndex) {
              if (rowIndex >= n) {
                  ans.add(convertToBoard(n));
                  return;
              }
              for (int colIndex = 0; colIndex < n; ++colIndex) {
                  if (isValid1(colIndex, rowIndex)) {
                      cols.add(colIndex);
                      pie.add(rowIndex + colIndex);
                      na.add(rowIndex - colIndex);
                      itemRet[rowIndex][colIndex] = 'Q';
                      solve(ans, n, rowIndex + 1);
                      cols.removeLast();
                      pie.removeLast();
                      na.removeLast();
                      itemRet[rowIndex][colIndex] = '.';
                  }
              }
          }
  
          private boolean isValid1(int colIndex, int rowIndex) {
              return !(cols.contains(colIndex) || pie.contains(rowIndex + colIndex) || na.contains(rowIndex - colIndex));
          }
  
          private List<String> convertToBoard(int n) {
              List<String> list = new ArrayList<>();
              for (int i  = 0; i < n; ++i) {
                  StringBuilder sb = new StringBuilder();
                  for (int j = 0; j < n; ++j) {
                      sb.append(itemRet[i][j]);
                      if (j == n - 1) {
                          list.add(sb.toString());
                      }
                  }
              }
              return list;
          }
      }
  ```

  