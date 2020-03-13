---
layout: post
title: "Yona-Jenkins 연동방법"
date: 2020-03-14 00:00:00 +0900
author: Hong Wonjun
categories: Develop
tags: develop, yona, jenkins, ci
---

회사에서 이슈 트래커 + 코드 저장소로 [Yona](https://yona.io) 를 사용하고 있고,
[필요에 의해서 하나씩 고쳐쓰다보니 committer 로 활동](https://github.com/yona-projects/yona/releases/tag/v1.8.0)하고 있다.

이번 포스팅은 Yona 와 CI 툴인 Jenkins 와 연동하는 기본 방식을 설명한다.

Yona 버전은 포스팅 기준 가장 최신버전인 [Yona v1.13.0 Pre-release](https://github.com/yona-projects/yona/releases/tag/v1.13.0-pre) dlek.

1. 우선 Yona 에 ` Jenkins-test` 새로운 프로젝트를 만들었다.

![2020-03-14-01.png](../../assets/post-images/2020-03-14/2020-03-14-01.png)

2. Jenkins 에서도 `Freestyle project` 로 새로운 아이템을 만들어준다.

![2020-03-14-02.png](../../assets/post-images/2020-03-14/2020-03-14-02.png)

3. `Source Code Management` 에서 `Git` 을 선택한다.

- `Repository URL` 에 Yona Project 의 URL 을 입력한다.
- `Credentials` 에 해당 Yona 계정을 입력한다.

![2020-03-14-03.png](../../assets/post-images/2020-03-14/2020-03-14-03.png)

4. `Build Triggers` 에서 `Trigger builds remotely (e.g., from scripts)` 를 선택하고, `Authentication Token` 에 값을 입력한다.

![2020-03-14-04.png](../../assets/post-images/2020-03-14/2020-03-14-04.png)

5. 작동 확인을 위하여 `Post-build Actions` 에 Trigger 가 잘 작동하는지 확인하기 위한 방법을 추가한다. (여기서는 Google Chat Notification)

![2020-03-14-05.png](../../assets/post-images/2020-03-14/2020-03-14-05.png)

6. Jenkins 의 기본 설정을 완료하였다.

![2020-03-14-06.png](../../assets/post-images/2020-03-14/2020-03-14-06.png)

7. 4번의 Screenshot 을 다시 보자. 아래와 같은 문구가 있다.

```
Use the following URL to trigger build remotely: JENKINS_URL/job/Jenkins-test/build?token=TOKEN_NAME or /buildWithParameters?token=TOKEN_NAME
Optionally append &cause=Cause+Text to provide text that will be included in the recorded build cause.
```

이를 Yona 에서 프로젝트 오른쪽의 설정에 들어간 후 Webhooks 탭으로 가서 주소를 추가해준다.

![2020-03-14-07.png](../../assets/post-images/2020-03-14/2020-03-14-07.png)


8. Yona 에 추가할 경우 해당 주소만 쓰는 것이 아니라 계정 페이지에서 API Token 을 생성해서 아래와 같이 주소를 작성한다.

```
http://{jenkins ID}:{API token}@JENKINS_URL/job/Jenkins-test/build?token={token}
```

![2020-03-14-09.png](../../assets/post-images/2020-03-14/2020-03-14-07.png)

9. 기본 Push 를 진행해보자.

![2020-03-14-11.png](../../assets/post-images/2020-03-14/2020-03-14-11.png)

10. Trigger 가 진행된 것을 볼 수 있다.

![2020-03-14-12.png](../../assets/post-images/2020-03-14/2020-03-14-12.png)
