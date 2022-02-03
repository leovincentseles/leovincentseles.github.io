---
title: "Binary Search"
date: 2022-01-30T18:15:41+08:00
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

- 模板是讀了[**jiah**](https://leetcode.com/jiah/)在Leetcode討論區下方的[**留言**](https://leetcode.com/discuss/general-discussion/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems/656934)之後理解整理出來
- 我們先討論`二元搜索法`的基本模板，後續才會說明如何利用模板轉換成三種常見的`二元搜索法`問題

### 基本模板說明

> **目標:** 給定一個陣列`arr`，找出`最小的索引k`，使得`condition(arr[k])`結果為**true**。

- 舉個例子: 找出陣列中第一個大於等於6的元素，
  - 用基本模板來看就是找出陣列中最小的索引k，而且arr[k] ≥ 6 (`condition函數`)
  - 在這個例子中我們想要的k就是2 (因為arr[2] ≥ 6)  

    ```cpp
    // The given array
    vector<int> arr{1, 5, 7, 20};

    // The condition function
    bool condition(int val) {
        return (val >= 6);
    }
    ```

- 在更深入討論基本模板之前，我們需要先了解模板的使用前提，並設計適當的condition函數
  - 調用condition函數後**所有true發生的位置必須落在所有false的右邊**
  - 合法的情形
    - (前半部為false, 後半部為true)，false, false, false, **true**, **true**, **true**
    - (全為false)，false, false, false, false, false, false
    - (全為true)，**true**, **true**, **true**, **true**, **true**, **true**
  - 不合法的情形
    - false, **true**, false, **true**, **true**

### 循環不變量

- 實作二元搜索法之前，我們先定義清楚一些相關的循環不變量(**Loop Invariants**)。
- 在二元搜索法執行前、執行中與執行後我們都需要遵守這些循環不變量的規則，而這些麻煩的步驟也幫我們解決了兩個難點
  - 區間的定義
  - `二元搜索法`中被left和right指向的元素意義
- index \\(\in [left,\ right)\\) &rarr; **待檢測集合**
  - 所有`arr[index]`為待檢測的元素 \\((left \leq\\) index \\(< right)\\)
- index \\(\in (-\infty,\ left)\\) &rarr; **false集合**
  - 所有`arr[index]`皆無法滿足condition函數(**false**) \\((-\infty <\\) index \\(< left)\\)
- index \\(\in [right,\ +\infty)\\) &rarr; **true集合**
  - 所有`arr[index]`皆滿足condition函數(**true**) \\((right \leq\\) index \\(< \infty)\\)

## 五種二元搜索法

## Leetcode 相關題目

### 依照索引搜尋 (Search by Index)

- **[Leetcode 1146. Snapshot Array](https://leetcode.com/problems/snapshot-array/)**

### 依照範圍可能搜尋 (Search by space)

- **[Leetcode 378. Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)**
- **[Leetcode 410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)**
- **[Leetcode 878. Nth Magical Number](https://leetcode.com/problems/nth-magical-number/)**
- **[Leetcode 1231. Divide Chocolate](https://leetcode.com/problems/divide-chocolate/)**
