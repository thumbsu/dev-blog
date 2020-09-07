---
title: '[Easy] Valid Palindrome II'
date: 2020-09-07 17:09:11
category: algorithm
thumbnail: { thumbnailSrc }
draft: false
---

## Description

Given a non-empty string `s`, you may delete **at most** one character. Judge whether you can make it a palindrome.

#### Example 1:

```
Input: "aba"
Output: True
```

#### Example 2:

```
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```

**Note**:

1. The string will only contain lowercase characters `a-z`. The maximum length of the string is 50000.

## Submission

Runtime: **88 ms**, faster than **94.77%** of JavaScript online submissions for Valid Palindrome II.

Memory Usage: **44.3 MB**, less than **46.11%** of JavaScript online submissions for Valid Palindrome II.

```javascript
/**
 * @param {string} s
 * @return {boolean}
 */
const validPalindrome = function(s) {
  function isPalindrome(s, deleteOneChar = false) {
    for (let i = 0, r = s.length - 1; i < s.length; i++, r--) {
      if (s[i] !== s[r]) {
        if (deleteOneChar) return false
        return (
          isPalindrome(s.slice(i, r), true) ||
          isPalindrome(s.slice(i + 1, r + 1), true)
        )
      }
    }
    return true
  }

  return isPalindrome(s)
}
```

## 풀이

- 재귀함수 `isPalindrome`
- 반복문 변수 i, r
  - `i += 1`
  - `r -= 1`
- 반복문을 돌면서 양 끝의 문자가 같은지 확인
- 다르다면 `deleteOneChar` 인자를 확인
  - `deleteOneChar`가 true면 이미 문자를 하나 지운 상태이므로, 더이상 확인할 필요없이 false 반환
  - `deleteOneChar`이 false면
    - 현재까지 확인되지 않은 문자열 중 맨 앞 문자를 제거하고 다시 `isPalindrome` 함수를 호출(이때 `deleteOneChar`는 true)
    - 맨 뒤 문자를 제거하고 동일하게 함
    - 둘 중 한번이라도 통과한다면 반목문이 계속 진행되고 true 반환
