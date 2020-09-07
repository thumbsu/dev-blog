---
title: '[Easy] Detect pattern of length m repeated k or more times'
date: 2020-09-07 11:09:72
category: algorithm
thumbnail: { thumbnailSrc }
draft: false
---

## Description

Given an array of positive integers `arr`, find a pattern of length `m` that is repeated `k` or more times.

A **pattern** is a subarray (consecutive sub-sequence) that consists of one or more values, repeated multiple times **consecutively** without overlapping. A pattern is defined by its length and the number of repetitions.

Return `true` if there exists a pattern of length `m` that is repeated `k` or more times, otherwise return `false`.

#### Example 1:

```
Input: arr = [1,2,4,4,4,4], m = 1, k = 3
Output: true
Explanation: The pattern (4) of length 1 is repeated 4 consecutive times. Notice that pattern can be repeated k or more times but not less.
```

#### Example 2:

```
Input: arr = [1,2,1,2,1,1,1,3], m = 2, k = 2
Output: true
Explanation: The pattern (1,2) of length 2 is repeated 2 consecutive times. Another valid pattern (2,1) is also repeated 2 times.
```

#### Example 3:

```
Input: arr = [1,2,1,2,1,3], m = 2, k = 3
Output: false
Explanation: The pattern (1,2) is of length 2 but is repeated only 2 times. There is no pattern of length 2 that is repeated 3 or more times.
```

#### Example 4:

```
Input: arr = [1,2,3,1,2], m = 2, k = 2
Output: false
Explanation: Notice that the pattern (1,2) exists twice but not consecutively, so it doesn't count.
```

#### Example 5:

```
Input: arr = [2,2,2,2], m = 2, k = 3
Output: false
Explanation: The only pattern of length 2 is (2,2) however it's repeated only twice. Notice that we do not count overlapping repetitions.
```

#### Constraints:

- `2 <= arr.length <= 100`
- `1 <= arr[i] <= 100`
- `1 <= m <= 100`
- `2 <= k <= 100`

## Submission

Runtime: **88 ms**, faster than **52.41%** of JavaScript online submissions for Detect Pattern of Length M Repeated K or More Times.

Memory Usage: **43.2 MB**, less than **6.95%** of JavaScript online submissions for Detect Pattern of Length M Repeated K or More Times.

```javascript
/**
 * @param {number[]} arr
 * @param {number} m
 * @param {number} k
 * @return {boolean}
 */
const containsPattern = function(arr, m, k) {
  if (arr.length < m * k) return false

  let pt
  for (let i = 0; i < arr.length; i++) {
    for (let j = i; j < arr.length; j += m) {
      pt = arr.slice(j, j + m).join('')
      if (pt.repeat(k) === arr.slice(j, j + m * k).join('')) return true
    }
  }

  return false
}
```

## 풀이

- 배열 `arr`의 길이가 패턴길이(m)과 패턴반복횟수(k)를 곱한 것보다 작으면 `false` 리턴
- 패턴을 담을 변수 선언
- 반복문 변수 `i`가 0부터 시작해서 배열 `arr` 길이보다 작을 때까지만 도는 반복문
  - i : 0, 1, 2, 3, 4, ... < arr.length
- 위의 반복문 안에서 반복문 변수 `j`가 `i`와 같은 숫자부터 배열 `arr` 길이보다 작을 때까지 패턴길이만큼 커지며 도는 반복문
  - m: 2일때,
  - k: 0, 2, 4, 6, ... < arr.length
- 배열 `arr`을 현재 위치부터 패턴 길이만큼 잘라서 `pt` 변수에 저장
- `pt`를 패턴반복횟수만큼 반복시킨 것과 배열 `arr`을 현재위치부터 `패턴길이 * 패턴반복횟수`까지 자른 것과 같으면 `true` 리턴
- 반복문이 종료했는데 리턴되지 않았다면 `false` 리턴
