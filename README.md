3d-Album-Gallery <img src="https://img.shields.io/badge/React-61DAFB?style=flat&logo=React&logoColor=white" /> <img src="https://img.shields.io/badge/typescript-3178C6?style=flat&logo=typescript&logoColor=white" /> ( ~ING )
=============
https://ontothenextlevel.github.io/3d-Album-Gallery/

입체적이고 생동감있는 구현을 통한 음악플랫폼 (계획)

* * *

### 진행사항
spotify api를 이용한 메인페이지 구현. 최신발매앨범을 화면 사이즈를 계산해 맞게 개수를 불러와서  구현

### 진행계획
1. 검색기능 추가 
2. Three.js 로 다음 단계 인터렉션 구현
3. 음악 재생기능 추가

* * *

## Memo

이번 작업은 컴포넌트를 tsx로 작성했다. TypeScript 사용해보니 좋다. 명시적으로 타입을 설정해 두니까 undefined 에러가 줄었음. 처음엔 syntax 빨간 밑줄을 하도 그어주니까 귀찮다고 생각했는데 그만큼 작성할때 처리해주니 코드를 다짜고 테스트할때 에러가 줄음.

Perspective + transform 이 생각보다 인터렉션 구현이 까다로웠음 event를 통해 위치를 가져오는 작업도 문제가 있고 위치를 설정하기도 transform때문에 fixed도 안먹어서 절대 위치도 못잡고 애니메이션이냐 3d스러움이냐 고민중에 몇가지 애니메이션을 포기하고 Perspective + transform으로 구현

spotify api에서 처음으로 http post를 사용해 봤음 다른 api들은 access token을 웹에서 string으로 제공하는데 여긴 post요청해서 받은 token으로 endpoint에서 get하는 구조 재밌었음.

youtube api에서 경험한 nextpage toke? 즉 불러온 데이터의 다음 데이터를 불러올수있는 parameter가 여기도 있다 근데 지난 작업과는 다르게 함수를 두개(firstcall, secondcall) 두지않고 fetchData 하나의 함수에서 api를 불러온후 조건에따라 다시 fetchData(limit,offset) 를 호출하는 구조로, 같은 작업을 굳이 여러개의 함수에서 하지않았다 발전했다. 

으악 github 호스팅하는데 게속 404가 뜬다 js css 404 이런식으로 떠서 찾아보니 경로의 문제같은데... 하는 과정에 문득 예전의 고생한  / ./ ../ 절대경로,상대경로 차이에서 로컬과 호스팅에서의 경로차이... 작업한 컴포넌트들에선 경로가 제대로 되어있는데 문득 upload하다 발견한 asset-manifest 파일안에서 경로들이 다 / 로 시작... 해결법 찾아보니 package.json에 "homepage": "." 입력 ! 해결...
컴퓨터와의 대화.. 조금도 대충넘어가서는 안된다..

호스팅하면서 찾아보다가 이게 문젠가 해서 다시본 app.js에서 렌더링이 2번 발생하는듯한 문제 발견 .. useEffect에 의존성배열 [] 이렇게 넣어놨는데도 2번 호출되서 app.js가 2번 렌더링 되는 문제라고 확신 허나 어디에 import한곳도없고 가장상위의 컴포넌튼데 왜 이러나 하며 검색하던 찰나에 React.StrictMode 발견.. index.js에 있는건데 npx create-react-app 하면 자동으로 만드는건데 이게 build에는 영향을 주진 않지만 개발 과정에서 잠재적인 문제를 찾기위해 일부러 2번 실행시킨단다... 괜히 함수나 다시짰네 ㅠㅠ
