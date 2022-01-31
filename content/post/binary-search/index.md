---
title: "Binary Search"
date: 2022-01-30T18:15:41+08:00
draft: false
image: LanYang-Museum.jpg
---

# Binary Search

## 介紹

- Binary Search (`二元搜索法`) 是大家剛接觸程式設計就學習到的演算法，但是一點也不簡單，是標準的一看就會一寫就廢類型 (**[引自代碼隨想錄](https://programmercarl.com/0704.%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE.html#_704-%E4%BA%8C%E5%88%86%E6%9F%A5%E6%89%BE)**)，寫不好的原因有兩個
    1.  區間的定義用的是哪一種? 
        - `[left, right]`, `[left, right)`, `(left, right]`, `(left, right)`
    2. `left`, `right` 兩個索引在算法結束後分別會指向哪裡?
- 這篇文章將會描述Binary Search的算法模板，以及介紹五種常見的Binary Search

## 模板

- 模板是讀了**[jiah](https://leetcode.com/jiah/)**在Leetcode討論區下方的**[留言](https://leetcode.com/discuss/general-discussion/786126/Python-Powerful-Ultimate-Binary-Search-Template.-Solved-many-problems/656934)**之後理解整理出來
- 我們先從討論Binary Search的基本模板開始，後續才會說明和五種Binary Search之間的關係

### 基本模板

> 給定一個陣列`arr`，找出`最小的索引k`，使得`condition(arr[k])`結果為**true**。
> 
- 舉個例子: Condition函數判斷給定值是否大於等於6
    
    → 找出陣列中最小的索引K，而且arr[k] ≥ 6，在這個例子中我們想要的到的k值就是2 (因為arr[2] ≥ 6)
    
    ```cpp
    arr = [1, 5, 7, 20]
    bool condition(int val) {
        return (val >= 6);
    }
    ```
    
    | k | 0 | 1 | 2 | 3 |
    | --- | --- | --- | --- | --- |
    | arr[k] | 1 | 5 | 7 | 20 |
    | condition(arr[k]) | false | false | true | true |

### 循環不變量定義

## 五種二元搜索法

## Leetcode 相關題目

### 依照索引搜尋 (Search by Index)

- **[Leetcode 1146. Snapshot Array](https://leetcode.com/problems/snapshot-array/)**

### 依照範圍可能搜尋 (Search by space)

- **[Leetcode 378. Kth Smallest Element in a Sorted Matrix](https://leetcode.com/problems/kth-smallest-element-in-a-sorted-matrix/)**
- **[Leetcode 410. Split Array Largest Sum](https://leetcode.com/problems/split-array-largest-sum/)**
- **[Leetcode 878. Nth Magical Number](https://leetcode.com/problems/nth-magical-number/)**
- **[Leetcode 1231. Divide Chocolate](https://leetcode.com/problems/divide-chocolate/)**