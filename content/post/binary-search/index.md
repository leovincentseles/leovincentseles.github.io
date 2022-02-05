---
author: "leovincentseles"
title: "Introduction to Binary Search"
slug: "binary-search"
date: 2022-01-30T18:15:41+08:00
description: "Explore the insight of binary search and provide a template code for implementation "
categories:
    - "Leetcode"
    - "Algorithm"  
tags:
    - "Programming"
    - "C++"
draft: false
image: LanYang-Museum.jpg
math: true
---

## 介紹

- `二元搜索法` (Binary Search) 是大家剛接觸程式設計就學習到的演算法，但是一點也不簡單，是標準的一看就會一寫就廢類型 ([**引自代碼隨想錄**](https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html#_704-%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE))，寫不好的原因有兩個
    1. 區間的定義  
        - `[left, right]`, `[left, right)`, `(left, right]`, `(left, right)`
    2. `left`, `right` 兩個索引在算法執行前、執行中與執行後後分別會指向哪種元素?
- 這篇文章將會描述`二元搜索法`的算法模板，以及介紹三種常見的`二元搜索法`問題

## 基本模板

- 模板是讀了[**jiah**](https://leetcode.com/jiah/)在Leetcode討論區下方的[**留言**](https://leetcode.com/discuss/general-discussion/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems/656934)之後整理出來
- 先討論`二元搜索法`的基本模板，後續才會說明如何利用模板轉換成三種常見的`二元搜索法`問題

### 基本模板說明

> **目標:** 給定一個陣列`arr`，找出`最小的索引k`，使得`condition(arr[k])`結果為**true**。

- 舉個例子: 找出陣列中第一個大於等於6的元素，
  - 用基本模板說明就是找出陣列中最小的索引k，而且arr[k] ≥ 6 (`condition函數`)
  - 在這個例子(下方程式碼)中我們想要的k就是2 (因為arr[2] ≥ 6)  

    ```cpp
    // The given array
    vector<int> arr{1, 5, 7, 20};

    // The condition function
    bool condition(int val) {
        return (val >= 6);
    }
    ```

- 在更深入討論基本模板之前，我們需要先了解模板的使用前提，並設計適當的condition函數
  - **前提**: 調用condition函數後**所有true發生的位置必須落在所有false的右邊**
  - 合法的情形
    - (前半部為false, 後半部為true)，false, false, false, **true**, **true**, **true**
    - (全為false)，false, false, false, false, false, false
    - (全為true)，**true**, **true**, **true**, **true**, **true**, **true**
  - 不合法的情形: 有的**true**出現在false左邊
    - false, **true**, false, **true**, **true**

### 循環不變量

- 實作二元搜索法之前，我們先定義一些循環不變量(**Loop Invariants**)，執行前、執行中與執行後我們都需要遵守這些循環不變量的規則  
- 為了避免複雜的邊界條件，我們將陣列索引範圍從原本的 \\([0, arr.size())\\)想像往左和右擴展至\\((-\infty, +\infty)\\)，同時也要符合前提: 調用condition函數後**所有true發生的位置必須落在所有false的右邊**，也就是
  - 擴展的陣列索引\\((-\infty,\ 0)\\) 皆無法滿足condition函數(**false**)
  - 擴展的陣列索引\\([arr.size(),\ +\infty)\\) 皆滿足condition函數(**true**)
- 接著定義三個循環不變量(待檢測集合、false集合與true集合)
- index \\(\in [left,\ right)\\) &rarr; **待檢測集合**
  - 在這段索引範圍的`arr[index]`為待檢測的元素 \\((left \leq\\) index \\(< right)\\)
- index \\(\in (-\infty,\ left)\\) &rarr; **false集合**
  - 在這段索引範圍的`arr[index]`皆無法滿足condition函數(**false**) \\((-\infty <\\) index \\(< left)\\)
- index \\(\in [right,\ +\infty)\\) &rarr; **true集合**
  - 在這段索引範圍的`arr[index]`皆滿足condition函數(**true**) \\((right \leq\\) index \\(< \infty)\\)
- 定義完相關的循環不變量，接著就是照著規則實作

### 實作

```cpp
// The condition function
bool condition(int val) {
    return (val >= 6);
}

int binarySearch(vector<int> &arr) {
    int left = 0, right = arr.size();

    while (left < right) {
        int mid = left + (right - left) / 2;

        if (condition(arr[mid]) == true)
            right = mid;
        else
            left = mid + 1;
    }

    if (left == arr.size())
        printf("Cannot find a valid element\n");                

    return left;
}
```

- 二元搜索執行之前
  {{< highlight cpp "linenostart=7" >}}
int left = 0, right = arr.size();{{< / highlight >}}
  - 待檢測集合為\\([0,\ arr.size())\\)，因此`left`設定為0，`right`設定為arr.size()。
  - 索引\\((-\infty,\ left) \rightarrow (-\infty,\ 0)\\) 符合**Loop invariant**，在這個範圍condition(arr[index])皆為**false**。
  - 索引\\([right,\ \infty) \rightarrow [arr.size(),\ \infty)\\) 符合**Loop invariant**，在這個範圍condition(arr[index])皆為**true**。

- 二元搜索
  - while迴圈的條件
  {{< highlight cpp "linenostart=9" >}}
while (left < right) {{{< / highlight >}}
    - 還有尚未檢測的元素就要繼續執行，也就是**待檢測集合**: \\([left, right)\\) 至少有一個元素 \\((left < right)\\)
  - 如果arr[mid]符合condition函數
  {{< highlight cpp "linenostart=12" >}}
if (condition(arr[mid]) == true)
    right = mid;{{< / highlight >}}
    - 因為有前提的保證(**所有true發生的位置必須落在所有false的右邊**)，所以對於所有大於等於mid的索引調用condition函數都是true，而**true集合**範圍為\\([right,\ \infty)\\)，因此我們必須將`right`更新為`mid`。
  - arr[mid]不符合condition函數
  {{< highlight cpp "linenostart=14" >}}
else
    left = mid + 1;{{< / highlight >}}
    - 因為有前提的保證(**所有true發生的位置必須落在所有false的右邊**)，所以對於所有小於等於mid的索引調用condition函數都是false，而**false集合**範圍為\\((-\infty, left)\\)，因此我們必須將`left`更新為`mid+1` (更新後我們可以確保left-1是原本的mid，而**condition(arr[mid])為false**)。

### 分析

- **收斂性**
  - 二元搜索法最怕的就是無窮迴圈(infinite loop)，而只要算法能夠在每次迴圈將**待搜索範圍至少減一**，就不會發生無窮迴圈。
  - 在進入迴圈的時候，\\(left < right\\)，而且 \\(mid = \frac{left + right}{2}\\) 用的是整數除法，所以這三個索引之間的關係是 \\(left \leq mid < right\\)。
  - 在迴圈中只有兩種索引更新，`right = mid`或是`left = mid + 1`，因為 `right` 不等於 `mid` ，所以`right`的更新會讓待搜索範圍至少減一，而`left`的更新也很明顯會讓待搜索範圍至少減一。
  - 在搜索範圍為空集合時，迴圈的條件 \\(left < right\\) 不會滿足，因此不可能發生無窮迴圈
- **索引left和right**
  - 在結束了while迴圈，可以確定的是 \\(left \geq right \\)，而透過以下分析我們可以知道其實 **\\(left\\) 和 \\(right\\) 會指向同個地方**
    - 每次進入迴圈，三個索引之間的關係是 \\(left \leq mid < right\\)，而且程式碼中只有兩種索引更新方式
    - `right = mid`: 執行完後的結果會是 \\(left < right\ (left \neq mid)\\) 或 \\(left = right\ (left = mid)\\)
    - `left = mid + 1`: 執行完後的結果會是 \\(left < right\ (mid < right-1)\\) 或 \\(left = right\ (mid = right-1)\\)
    - 更新完索引之後如果 \\(left = right\\)，就會結束迴圈，所以不會有 \\(left > right \\) 的可能性。
  - `left` 和 `right` 指向的地方
    - 從上面分析可以得出兩個索引最後會共同指向一個地方
    - 從每次迴圈都維護的循環不變量**false集合**與**true集合**，我們可以分析
      - \\((-\infty,\ left)\\)為**false集合**，left和right共同指向的左側(**不包含**)condition皆為**false**。
      - \\([right,\ \infty)\\)為**true集合**，left和right共同指向的右側(**包含**)condition皆為**true**。
    - `left` 和 `right` 會共同指向**滿足condition的最小索引**
- **邊界條件分析**
  1. 如果是空陣列(`arr.size() = 0`)， `left` 和 `right` 都會被初始化為 `0`，不會進到 while迴圈
  2. 如果陣列所有元素經過condition函數都是 `false`, `left` 和 `right` 最後會指到 `arr.size()`，代表找不到
  3. 如果陣列所有元素經過condition函數都是 `true`, `left` 和 `right` 都會指到 `0`

- **邊界條件處理**
  - 針對1. 2.兩種可能可以檢查`left == arr.size()`，再做對應處理

## 三種二元搜索法問題

- 接著要介紹的是三種常見的`二元搜索法`問題
- 幸運的是三種問題都可以從基本模板做稍微修改就可以解決。

### 尋找陣列中第一個6

> **目標:** 給定一個升序排列的陣列**arr**，找出並回傳**第一個6所在的位置**，**找不到則回傳-1**

- 設計condition函數使得右半邊為**true**，左半邊為**false**  

  ```cpp
  bool condition(int val) {
      return (val >= 6);
  }
  ```

- 套用基本模板，找到第一個符合condition函數的位置
  
  ```cpp
  int binarySearch(vector<int> &arr) {
      int left = 0, right = arr.size();

      while (left < right) {
          int mid = left + (right - left) / 2;

          if (condition(arr[mid]))
              right = mid;
          else
              left = mid + 1;
      }

      if (left == arr.size() || arr[left] != 6)
          return -1;

      return left;
  }
  ```

  - 找到第一個大於等於6的位置之後
    - 首先針對**邊界條件**做處理， `left == arr.size()` 代表找不到直接回傳-1。
    - 如果有找到第一個大於等於6的元素，檢查是否是6， `arr[left] == 6` ，不是就回傳-1，如果真的是6才回傳位置 `left`。

### 尋找陣列中最後一個6

> **目標:** 給定一個升序排列的陣列**arr**，找出並回傳**最後一個6所在的位置**，**找不到則回傳-1**

- 設計condition函數使得右半邊為**true**，左半邊為**false**
- 但是這次要找得是最後一個6，所以我們稍微修改了一下condition函數，目標是找到第一個大於6的位置，再做後續處理
  
  ```cpp
  bool condition(int val) {
      return (val > 6);
  }
  ```

- 套用基本模板，找到第一個大於6(符合condition函數)的位置
  
  ```cpp
  int binarySearch(vector<int> &arr) {
      int left = 0, right = arr.size();

      while (left < right) {
          int mid = left + (right - left) / 2;

          if (condition(arr[mid]))
              right = mid;
          else
              left = mid + 1;
      }

      if (left - 1 == -1 || arr[left - 1] != 6)
          return -1;
      
      return left - 1;
  }

  ```

  - 找到第一個大於等於6的位置之後，我們想找得其實是最後一個6，所以要檢查得元素位置在 `left - 1`
    - 首先針對**邊界條件**做處理，如果 `left - 1 == -1` 代表沒有最後一個小於等於6的元素，直接回傳-1。
    - 如果有找到最後一個小於等於6的元素(位置 `left - 1` ，第一個大於6的左邊)，檢查是否是6， `arr[left - 1] == 6` ，不是就回傳-1，如果真的是6才回傳位置 `left - 1`。

### 尋找陣列中任一個6

> **目標:** 給定一個升序排列的陣列**arr**，找出並回傳**任一個6所在的位置**，**找不到則回傳-1**

- 設計condition函數使得右半邊為**true**，左半邊為**false**
- 但是這次要找得是任一個6，原本condition函數定義為**大於等於6**，現在如果發現**等於6**就可以直接回傳位置了

  ```cpp
  int binarySearch(vector<int> &arr) {
      int left = 0, right = arr.size();

      while (left < right) {
          int mid = left + (right - left) / 2;

          if (arr[mid] == 6)
              return mid;
          else if (arr[mid] > 6)
              right = mid;
          else
              left = mid + 1;
      }

      return -1;
  }
  ```

## Leetcode 相關題目

### 依照索引搜尋 (Search by Index)

- **[Leetcode 35. Search Insert Position](https://leetcode.com/problems/search-insert-position/)**
- **[Leetcode 1146. Snapshot Array](https://leetcode.com/problems/snapshot-array/)**

### 依照範圍可能搜尋 (Search by space)

- **[Leetcode 378. Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)**
- **[Leetcode 410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)**
- **[Leetcode 878. Nth Magical Number](https://leetcode.com/problems/nth-magical-number/)**
- **[Leetcode 1231. Divide Chocolate](https://leetcode.com/problems/divide-chocolate/)**
