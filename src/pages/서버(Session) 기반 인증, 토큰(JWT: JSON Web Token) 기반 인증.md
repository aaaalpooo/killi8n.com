---
title: "서버(Session) 기반 인증, 토큰(JWT: JSON Web Token) 기반 인증"
date: "2018-07-27"
---

> ##### 참고: 이 글은 velopert님의 offline 강좌인 FAST CAMPUS - REACT 강의의 일부에서 참조하여 작성 되었음을 알립니다.

#### [velopert님의 Blog] https://velopert.com

#### [패캠 강좌]: https://www.fastcampus.co.kr/dev_camp_react/

##### 홍보하는 것은 아니지만,, 출처를 밝히고 싶은 마음에 위 링크를 달아봅니다.

웹 어플리케이션을 만들때에 꼭 거쳐야되는 과정,,, 이라고 할수있죠!

> #### 바로 인증(Authentication) 시스템입니다.

뭐 여러 인증이 있겠지만, 기본적으로 회원가입을 받고, 로그인을 하여 permission을 부여 한다는 개념이겠죠?

여기서는 permission에 관한 글은 아닙니다만, permission을 구분 하기 위해서는 서버에서 이사람이 인증 된 사람이다! 즉, 인증을 확인 하는 절차가 항상 필요합니다!

#### 여기서 세션 기반 인증과, 현재 많은 곳에서 사용 중인 토큰 기반 인증으로 나뉘게 됩니다.

### 세션 기반 인증 vs 토큰 기반 인증

그렇다면 세션 기반과 토큰 기반의 차이점을 알아볼까요?

세션이 무엇이냐 하면, 인증된 정보를 담고있는것이라고 할수 있습니다.
세션 기반인증은 서버 자체에서 이 세션을 담고 있어야 합니다.
따라서 수많은 유저들이 인증절차를 거쳤다고 가정할 시에, 서버에 부담이 많이 되겠죠?

허나 토큰 기반 인증은 서버에 내 인증 정보를 위임 하는것이 아닌, 토큰 자체를 발급해 주어서 유저 자체가 주가 되어 인증을 실시하는 것이라고 볼수 있습니다.

이 토큰 기반 인증의 장점은 여기에서 나타납니다.

서버 확장이 쉬워지게 되며, 소셜 기반 인증과도 연동하여 사용할수 있습니다.
페이스북이나 구글에서 소셜 인증을 하게되면, 그때마다 다른 token값을 넘겨주게 되는데요, 이것을 사용하면 JWT, 즉 토큰 기반의 인증과 같은 의미를 가집니다.

또한 모바일 어플리케이션에서 사용도 매우 용이합니다. Session기반 인증을 사용하게 되면, 쿠키 매니저를 따로 관리해 주어야 합니다.
토큰을 사용하게 되면 헤더에 넣어서 API통신으로 깔끔하게 구현이 가능하겠죠?

> 참고: JWT에 대한 자세한 정보는 https://velopert.com/2350 에서 더 확인 바랍니다.

### 토큰 기반 인증의 단점 및 취약점

토큰 기반 인증에서는 토큰을 어디다 담을지 결정해야 겠죠?

> 첫번째는 토큰을 웹스토리지(localStorage)에 담는 방법이 있습니다.

이 경우에는 구현하기는 쉽지만, 해커에 노출되게 되면 XSS(cross site scripting) 의 위협에 놓이게 될수 있습니다..

> 다른 방법은 token을 쿠키에 담는 방법입니다.

쿠키를 설정해줄때에, httpOnly를 설정하게 되면 브라우저에서 쿠키에 접근할수 있는 방법이 없어집니다. 이 경우의 단점은 쿠키가 한정된 도메인에서만 사용할수 있게 되버립니다. 또한 XSS의 공격에서 벗어나는 대신 CSRF공격의 위협에 노출됩니다.

> 정리를 하자면, 웹스토리지에 토큰을 담으면 XSS 에 취약하고, 쿠키에 담으면 CSRF 에 취약해집니다. 하지만, CSRF는 보안에 신경을 쓰면 차단 할 수 있습니다. (velopert님이 작성한 문서에서 가져왔습니다.)
