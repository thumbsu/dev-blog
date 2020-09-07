---
title: '[Easy] Make the string great'
date: 2020-09-07 15:09:01
category: algorithm
thumbnail: { thumbnailSrc }
draft: false
---

## Description

Given a string `s` of lower and upper case English letters.

A good string is a string which doesn't have **two adjacent characters** `s[i]` and `s[i + 1]` where:

- `0 <= i <= s.length - 2`
- `s[i]` is a lower-case letter and `s[i + 1]` is the same letter but in upper-case or vice-versa.

To make the string good, you can choose **two adjacent** characters that make the string bad and remove them. You can keep doing this until the string becomes good.

Return the string after making it good. The answer is guaranteed to be unique under the given constraints.

**Notice** that an empty string is also good.

#### Example 1:

```
Input: s = "leEeetcode"
Output: "leetcode"
Explanation: In the first step, either you choose i = 1 or i = 2, both will result "leEeetcode" to be reduced to "leetcode".
```

#### Example 2:

```
Input: s = "abBAcC"
Output: ""
Explanation: We have many possible scenarios, and all lead to the same answer. For example:
"abBAcC" --> "aAcC" --> "cC" --> ""
"abBAcC" --> "abBA" --> "aA" --> ""
```

#### Example 3:

```
Input: s = "s"
Output: "s"
```

#### Constraints:

- `1 <= s.length <= 100`
- `s` contains only lower and upper case English letters.

## Submission

Runtime: **88 ms**, faster than **73.07%** of JavaScript online submissions for Make The String Great.

Memory Usage: **38.6 MB**, less than **68.38%** of JavaScript online submissions for Make The String Great.

```javascript
/**
 * @param {string} s
 * @return {string}
 */
const makeGood = function(s) {
  return String.fromCharCode(
    ...s
      .split('')
      .map(a => a.charCodeAt(0))
      .reduce((a, b) => {
        if (Math.abs(a[a.length - 1] - b) === 32) {
          return a.slice(0, a.length - 1)
        }
        return a.concat(b)
      }, [])
  )
}
```

## 풀이

- 문자열 `s`를 문자열 배열로 바꿈
- 문자열 배열을 순회하면서 유니코드 숫자(`charCodeAt` 메소드)로 변경
- `reduce` 메소드를 사용하여 순회
  - 값이 누적되는 배열 a의 마지막 요소와 비교할 b
  - 두 숫자의 차이가 **32** 차이면 누적된 배열(a)에서 맨 마지막 요소 제거
  - 아니라면 a 배열에 b 추가
  - 이렇게 되면 소문자 대문자 제거됨
- 다시 `String`의 `fromCharCode` 메소드 사용하여 문자열로 바꾼 후 반환
