---
title: '[Medium] Largest Number'
date: 2020-09-06 18:09:83
category: algorithm
thumbnail: { thumbnailSrc }
draft: false
---

## Description

Given a list of non negative integers, arrange them such that they form the largest number.

#### Example 1:

```
Input: [10,2]
Output: "210"
```

#### Example 2:

```
Input: [3,30,34,5,9]
Output: "9534330"
```

**Note**: The result may be very large, so you need to return a string instead of an integer.

## Submission

Runtime: **76 ms**, faster than **90.51%** of JavaScript online submissions for Largest Number.

Memory Usage: **38.7 MB**, less than **34.36%** of JavaScript online submissions for Largest Number.

```javascript
Largest Number
/**
 * @param {number[]} nums
 * @return {string}
 */
const largestNumber = function(nums) {
  let result = nums.map(a => a.toString()).sort((a, b) => {
    if (a.concat(b) >= b.concat(a)) return -1
    return 1
  }).join('')
  return result[0] === "0" ? "0" : result
};
```

## 풀이

- `nums` 배열의 요소들을 string으로 변환
- `nums` 배열 sort
- `nums` 배열 sort 함수에 넘긴 `compareFunction`
  - 두 숫자 문자열을 앞뒤로 합쳐서(ab, ba) 비교
  - ab가 더 크면 -1 (a가 앞으로 감)
  - 아니면 1 (b가 앞으로 감)
- 정렬된 `nums` 문자열로 합침
- `result`의 첫번째 요소가 `"0"`이면 `"0"`리턴 아니면 `result` 리턴
