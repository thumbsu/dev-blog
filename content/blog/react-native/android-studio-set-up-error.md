---
title: 'Android Studio가 node 경로를 못 찾을 때'
date: 2020-08-06 14:09:75
category: react-native
thumbnail: { thumbnailSrc }
draft: false
---

## 발생 에러

- Android Studio(Version 4.0.1) 설치
- Homebrew로 `openjdk8` 설치
- Android Studio에서 `SDK Platform29`, `Google APIs Intel x86 Atom System Image`, `Android SDK Build-Tools 29.0.2` 설치 (React Native Docs recommended)
- `.bash_profile`에 `ANDROID_HOME` 세팅
- 이후 React Native 새 프로젝트 열어서 Gradle Sync
- 에러 발생

```shell
$ Cannot run program "node": error=2, No such file or directory
```

- 에러를 보니 node 경로를 못 찾아서 생기는 문제
- 열심히 stackoverflow 찾아보다가 발견한 해결책

```shell
$ sudo ln -s "$(which node)" /usr/local/bin/node
```

- 터미널에 위와 같이 입력
- Android Studio에서 다시 Sync
- 더이상 에러가 발생하지 않음
- node를 nvm으로 설치를 했는데, 이렇게 설치를 한 경우 위와 같은 에러가 발생하는 것 같음
