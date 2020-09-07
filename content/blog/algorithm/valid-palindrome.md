---
title: '[Easy] Valid Palindrome'
date: 2020-09-07 16:09:85
category: algorithm
thumbnail: { thumbnailSrc }
draft: false
---

## Description

Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.

**Note**: For the purpose of this problem, we define empty string as valid palindrome.

#### Example 1:

```
Input: "A man, a plan, a canal: Panama"
Output: true
```

#### Example 2:

```
Input: "race a car"
Output: false
```

#### Constraints:

- `s` consists only of printable ASCII characters.

## Submission

Runtime: **84 ms**, faster than **92.10%** of JavaScript online submissions for Valid Palindrome.

Memory Usage: **42.7 MB**, less than **28.71%** of JavaScript online submissions for Valid Palindrome.

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
const isPalindrome = function(s) {
  s = s
    .replace(/[^0-9a-zA-Z]/gi, '')
    .toLowerCase()
    .split('')
  if (s.length === 0 || s.join('') === s.reverse().join('')) return true
  return false
}
```

## 풀이

- 문자열 s에서 특수문자를 제거
- 모두 소문자로 만든 후 배열로 바꿈
- 그렇게 변환한 문자열의 길이가 0이면 true 반환
- 특수문자를 제거한 문자열과 순서를 뒤집은 문자열이 같으면 true 반환
- 아니면 false
