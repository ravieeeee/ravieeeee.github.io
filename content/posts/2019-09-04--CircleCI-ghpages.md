---
title: "CircleCI Github page 브랜치 빌드 skip하기"
date: "2019-09-04"
template: "post"
draft: false
slug: "/posts/circleci-ghpages/"
category: "Develop"
tags:
  - "CI/CD"
  - "CircleCI"
description: "gh-pages의 기본 브랜치에서 CircleCI 빌드를 skip하자."
socialImage: "/media/190904-circleci-1.png"
---

[gatsby-starter-lumen](https://www.gatsbyjs.org/starters/alxshelepenok/gatsby-starter-lumen/) 템플릿에서 시작해서 CircleCI까지 붙이는 것은 [gatsby 정적블로그 생성하기 - 자동deploy, CircleCI연동](https://kujyp.github.io/posts/2019-02-24---gatsby-정적블로그-생성하기---자동deploy,-CircleCI연동/) 등 자료를 참고해서 만들 수 있다.

다만, [gh-pages](https://github.com/tschaub/gh-pages)의 기본 브랜치인 `gh-pages` 혹은 위의 튜토리얼을 따라했다면 `master` 브랜치에 프로덕션으로 나갈 파일들이 푸시되기 때문에 CircleCI에 의해 빌드가 일어나게 된다.

하지만 `gatsby build` 명령이 포함된 `yarn build` 명령이 수행되는 곳은 개발 브랜치기 때문에 CircleCI는 다른 브랜치에서 돌 필요가 없다. 게다가 `gh-pages`의 옵션으로 지정된 브랜치에는 `.circleci/config.yml` 파일이 존재하지 않기 때문에 결과는 항상 아래처럼 fail이 된다.

![CircleCI Fail](/media/190904-circleci-1.png)

실제 github page 배포에는 영향이 없지만 Fail은 기분이 나쁘니 없애보자.

[CircleCI 문서](https://circleci.com/docs/2.0/skip-build/)를 보면, 커밋 메세지에 `[ci skip]`을 추가하면 해당 커밋 뿐만 아니라 푸시할 때 포함된 다른 커밋까지도 CircleCI가 skip한다고 한다.

편리하게도 `gh-pages` 에서 커밋 메세지를 커스텀할 수 있는 옵션을 준다. 따라서 `package.json` 파일 내 deploy script를 아래와 같이 변경하자.

```diff
- yarn run clean && gatsby build --prefix-paths && gh-pages -d public -b master -r <repo 주소>
+ yarn run clean && gatsby build --prefix-paths && gh-pages -d public -b master -r <repo 주소> -m '[ci skip]'
```

해결되었다!

![resolution](/media/190904-circleci-2.png)
