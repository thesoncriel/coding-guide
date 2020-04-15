# Architecture

프로젝트에 응용되는 구조나 각종 아키텍처를 설명한다.

## Project Structure
아래는 Single Page Application (이하 SPA) 기준 프로젝트 구조이다.

* environments - 환경변수 파일들. 개발/빌드 진행 시 환경에 알맞은 파일이 /.env 파일로 복사 되어 쓰인다.
  - .env.dev - 개발용 환경변수
  - .env.production - 운영서버용 환경변수
  - .env.test - 개발서버용 환경변수
* gulp
  - css-sprite - CSS Background Image Sprite 를 생성하는 스크립트
    - images - 스프라이트를 생성 할 아이콘 파일(.png)을 두는 곳. 하위 폴더를 두면 생성 뒤 테스트 페이지(sprite-test.html) 에서 그룹별로 나눠진 모습을 볼 수 있다. (실제 아이콘 클래스명에는 영향을 끼치지 않음) 그리고 하위 폴더나 파일명 앞에 @를 붙이면 스프라이트 생성 시 무시(ignore) 한다.
  - s3-upload - AWS S3 에 빌드된 내용을 업로드 하는 스크립트. 수행 시 awsaccess.json 내용을 참고 한다.
* public - 정적 파일 모음
  - sprite - 이미지 스프라이트가 포함된 폴더. 배포전에 sprite.png 파일을 https://tinypng.com/ 에서 반드시 최적화 시키도록 한다.
  - index.html - 프로젝트가 시작되는 html 파일
* src
  - common - 모든 페이지에 공통으로 쓰이는 것들.
    - app.config.ts - 앱 내 환경설정. 상기 environments 의 내용을 따른다.
  - modules - 앱 내 각종 모듈을 정의 해 놓은 곳.
    - _shared - 공통으로 쓰이는 각종 컴포넌트 및 서비스가 모여있는 모듈
    - root - 루트 (/) 경로에서 쓰이는 내용
    - {feature} - 특정 페이지에서만 쓰이는 모듈
      - components - 각종 React Component 가 위치한다.
        - atoms - 단일 컴포넌트
        - molecules - 조합 컴포넌트
        - organisms - 복합 컴포넌트
        - adaptives - 적응형 (Adaptive) 컴포넌트 모음. 사용자 Device 나 해상도에 따라 기능이 달라지는 컴포넌트가 필요할 때 쓰인다.
      - containers - 컨테이너 컴포넌트 모음
      - pages - 페이지 컴포넌트 모음
      - contexts - 특정 컴포넌트들에 자료를 공유 할 때 쓰이는 Context 를 정의 해 둔 곳. 가급적 Flux Architecture 를 활용하되 특수한 경우 (예: resizing 여부 전파)만 사용한다.
      - hooks - Custom Hook 함수 모음
      - hoc - High Order Component 모음
      - actions - redux 액션들
      - effects - redux 의 side effect 를 처리하기 위한 이펙트 함수 모음
      - reducers - redux 의 state 를 변경하기 위한 역할인 리듀서 모음
      - selectors - redux 의 state 값을 받아오기위한 셀렉터 모음
      - models - 현재 모듈에서만 쓰이는 모델들을 정의 해 둔다.
      - constants - 현재 모듈에서만 쓰이는 상수를 정의 해 둔다. 메시지 내용은 아래 messages 에 정의 한다.
      - messages - 사용자 UI에 출력되는 각종 메시지 상수를 정의 해 둔다.
      - services - 데이터 처리를 위한 각종 함수 및 객체 모음. API Service 도 여기에 포함된다.
      - styles - 현재 모듈에서만 사용되는 CSS Style 을 정의한다. 일반적으론 styled-components 를 사용하기에 쓸일은 없지만, 유사한 스타일이 반복될 경우 여기에 별도 스타일을 두어 사용한다.
  - factories - 개체를 생성하는 Factory 를 모아둔 곳.
  - routes - 라우팅 세팅을 하는 곳.
  - utils - 프로젝트 전반적으로 고루 쓰이는 각종 유틸리티 함수가 모인 곳.