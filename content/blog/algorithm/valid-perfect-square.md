---
title: '[Easy] Valid Perfect Square'
date: 2020-09-09 12:09:94
category: algorithm
thumbnail: { thumbnailSrc }
draft: false
---

## Description

Given a **positive** integer num, write a function which returns True if num is a perfect square else False.

**Follow up: Do not** use any built-in library function such as sqrt.

#### Example 1:

```
Input: num = 16
Output: true
```

#### Example 2:

```
Input: num = 14
Output: false
```

#### Constraints:

- `1 <= num <= 2^31 - 1`

## Submission

Runtime: **76 ms**, faster than **69.05%** of JavaScript online submissions for Valid Perfect Square.

Memory Usage: **37 MB**, less than **7.82%** of JavaScript online submissions for Valid Perfect Square.

```javascript
/**
 * @param {number} num
 * @return {boolean}
 */
const isPerfectSquare = function(num) {
  if (num === 1) return true
  for (let i = 1; i <= Math.ceil(num / 2); i++) {
    if (i * i === num) return true
    if (i * i > num) return false
  }
}
```

## 풀이

- `num`이 1이라면 바로 true 반환
- 반복문 변수 `i`는 1부터 `num`을 2로 나눠서 나머지는 올림한 수만큼 반복
  - `i * i`가 `num`과 같아지면 true 반환
  - `i * i`가 `num`보다 커지면 더 반복할 필요없이 false 반환
