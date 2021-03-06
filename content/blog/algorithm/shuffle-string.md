---
title: '[Easy] Shuffle String'
date: 2020-09-06 19:09:56
category: algorithm
thumbnail: { thumbnailSrc }
draft: false
---

## Description

Given a string `s` and an integer array `indices` of the same length.

The string `s` will be shuffled such that the character at the ith position moves to `indices[i]` in the shuffled string.

Return the shuffled string.

#### Example 1:

![example1](https://assets.leetcode.com/uploads/2020/07/09/q1.jpg)

```
Input: s = "codeleet", indices = [4,5,6,7,0,2,1,3]
Output: "leetcode"
Explanation: As shown, "codeleet" becomes "leetcode" after shuffling.
```

#### Example 2:

```
Input: s = "abc", indices = [0,1,2]
Output: "abc"
Explanation: After shuffling, each character remains in its position.
```

#### Example 3:

```
Input: s = "aiohn", indices = [3,1,4,2,0]
Output: "nihao"
```

#### Example 4:

```
Input: s = "aaiougrt", indices = [4,0,2,6,7,3,1,5]
Output: "arigatou"
```

#### Example 5:

```
Input: s = "art", indices = [1,0,2]
Output: "rat"
```

#### Constraints:

- `s.length == indices.length == n`
- `1 <= n <= 100`
- `s` contains only lower-case English letters.
- `0 <= indices[i] < n`
- All values of `indices` are unique (i.e. `indices` is a permutation of the integers from `0` to `n - 1`).

## Submission

Runtime: **72 ms**, faster than **98.82%** of JavaScript online submissions for Largest Number.

Memory Usage: **38.3 MB**, less than **67.93%** of JavaScript online submissions for Largest Number.

```javascript
/**
 * @param {string} s
 * @param {number[]} indices
 * @return {string}
 */
const restoreString = function(s, indices) {
  let len = s.length
  let result = []
  for (let i = 0; i < len; i++) {
    result[indices[i]] = s[i]
  }
  return result.join('')
}
```

## 풀이

- `result` 변수 : `s`를 새롭게 정렬할 배열
- 문자열 `s` 길이만큼 반복문
- `result[indices[0 ~ s.length -1]]`에 `s[indices[0 ~ s.length -1]` 값 넣어줌
- 배열을 문자열로 합쳐서 반환
