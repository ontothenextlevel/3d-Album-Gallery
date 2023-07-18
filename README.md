3d-Album-Gallery <img src="https://img.shields.io/badge/React-61DAFB?style=flat&logo=React&logoColor=white" /> <img src="https://img.shields.io/badge/typescript-3178C6?style=flat&logo=typescript&logoColor=white" /> ( ~ING )
=============
https://ontothenextlevel.github.io/3d-Album-Gallery/

입체적이고 생동감있는 구현을 통한 음악플랫폼 (계획)

* * *

### 진행사항
spotify api를 이용한 메인페이지 구현. 최신발매앨범을 화면 사이즈를 계산해 맞게 개수를 불러와서 구현


### 진행계획
1. 검색기능 추가(완료) > 검색기록, 즐겨찾기 기능 추가 예정
2. Three.js 로 다음 단계 인터렉션 구현 < 일단 구현 
3. 음악 재생기능 추가 < 이 기능은 적절한 api의 부재로 고민중 

* * *

## Memo

이번 작업은 컴포넌트를 tsx로 작성했다. TypeScript 사용해보니 좋다. 명시적으로 타입을 설정해 두니까 undefined 에러가 줄었음. 처음엔 syntax 빨간 밑줄을 하도 그어주니까 귀찮다고 생각했는데 그만큼 작성할때 처리해주니 코드를 다짜고 테스트할때 에러가 줄음.

Perspective + transform 이 생각보다 인터렉션 구현이 까다로웠음 event를 통해 위치를 가져오는 작업도 문제가 있고 위치를 설정하기도 transform때문에 fixed도 안먹어서 절대 위치도 못잡고 애니메이션이냐 3d스러움이냐 고민중에 몇가지 애니메이션을 포기하고 Perspective + transform으로 구현

spotify api에서 처음으로 http post를 사용해 봤음 다른 api들은 access token을 웹에서 string으로 제공하는데 여긴 post요청해서 받은 token으로 endpoint에서 get하는 구조 재밌었음.

youtube api에서 경험한 nextpage toke? 즉 불러온 데이터의 다음 데이터를 불러올수있는 parameter가 여기도 있다 근데 지난 작업과는 다르게 함수를 두개(firstcall, secondcall) 두지않고 fetchData 하나의 함수에서 api를 불러온후 조건에따라 다시 fetchData(limit,offset) 를 호출하는 구조로, 같은 작업을 굳이 여러개의 함수에서 하지않았다 발전했다. 

으악 github 호스팅하는데 게속 404가 뜬다 js css 404 이런식으로 떠서 찾아보니 경로의 문제같은데... 하는 과정에 문득 예전의 고생한  / ./ ../ 절대경로,상대경로 차이에서 로컬과 호스팅에서의 경로차이... 작업한 컴포넌트들에선 경로가 제대로 되어있는데 문득 upload하다 발견한 asset-manifest 파일안에서 경로들이 다 / 로 시작... 해결법 찾아보니 package.json에 "homepage": "." 입력 ! 해결...
컴퓨터와의 대화.. 조금도 대충넘어가서는 안된다..

호스팅하면서 찾아보다가 이게 문젠가 해서 다시본 app.js에서 렌더링이 2번 발생하는듯한 문제 발견 .. useEffect에 의존성배열 [] 이렇게 넣어놨는데도 2번 호출되서 app.js가 2번 렌더링 되는 문제라고 확신 허나 어디에 import한곳도없고 가장상위의 컴포넌튼데 왜 이러나 하며 검색하던 찰나에 React.StrictMode 발견.. index.js에 있는건데 npx create-react-app 하면 자동으로 만드는건데 이게 build에는 영향을 주진 않지만 개발 과정에서 잠재적인 문제를 찾기위해 일부러 2번 실행시킨단다... 괜히 함수나 다시짰네 ㅠㅠ


* * *


예전에 이 프로젝트를 염두하며 html,js에서 Three.js 처음 해볼때 짜봤던 코드들을 가져오는데 불편함을 느껴 react-three-fiber이나 react-three/drei같은 라이브러리를 사용했다
기존에 group,object3d,mesh들을 선언하고 .add()해서 쭉 넣고, 넣고 하는 코드가 가독성이 떨어진다는 판단에서 Jsx로 작성해서 태그들의 종속관계가 한눈에 들어와서 더 보기쉬웠다.
동시에 라이브러리에서 canvas를 이용하니 일일히 렌더링을 관여해야하던 기존작업과 반대로 너무 편해졌다

이전엔 aos라이브러리를 사용하니 애니메이션에서 원하는 좌표 혹은 값을 입력하면 그걸 애니메이션화 시켜줬는데 이번엔 사용하지 않으니 초기에 좀 헤맸는데 useFrame 사용하고 동작원리를 깨달으니 애니메이션 작업이 쉬웠다
요약하면 어떤 결과 값을 넣어서 되는게 아니라 해당 결과값까지 프레임이 동작해서 도달해야 애니메이션이 되드라

대략적으로 mesh들로 3d오브젝트를 만들고나서 정보들을 넣어야할 Html Dom 요소들이 필요했다. 이미지 같은경우는 material에 넣을수있었지만 html을 materail화 할순없었다 그래서
방법을 찾아보니 drei의 Html이라는 기능을 쓰면 html태그들을 사용할수있고 또 attr을 입력해서 오브젝트에서 작동하게 할수있더라

그리고 여기서 포인트가 3d object의 사이즈를 화면 크기에따라 resize되게 했는데 이때 Html의 Dom요소 넓이와 높이가 퍼센트로 먹질않드라 그래서 오브젝트가 변해도 Html만 변화하지 않나? 했더니
px로 잡아놓으면 오브젝트사이즈가 변할떄 알아서 이것도 resize가 되더라 이거에서 또 생각보다 시간을 많이씀...ㅠ

three.js에서 typescript 사용하니 생소한게 더 많았다 기존에는 any, string, number정도 타입을 설정해 두다가 THREE.Texture이런 타입을 설정해야하고 함수에도 파라미터를 event:ThreeEvent<MouseEvent> 이런식으로 입력해야 했다

  
* * *


검색 기능추가. chart.js라이브러리도 사용해 보고 중요하게는 useReducer라는 상태관리 기능을 사용하면서 생각보다 시간이 좀 걸렸다.

api에서 제공하는 데이터의 다양성이 좀 부족해 어떻게 구현할까 고민하다 인기도를 도넛그래프로 표현하기 위해 chart.js라이브러리 사용.

Search.tsx라는 부모컴포넌트에 SearchArtist, SeachAlbum과 같은 자식컴포넌트들을 import해서 사용하는데 자식 컴포넌트간의 props전달시 코드가 너무 지져분해지는 상황이 발생해서 상태관리기능에 대해 공부해봄.
Redux라는 상태관리 라이브러리가 보편적으로 넓게 사용되는거 같긴하나 현재 상황에는 과분하다는 느낌을 받아 상태관리 맛보기 차원에서 useReducer를 사용해봄.
state와 action이 초기엔 좀 어색했는데 사용하면서 손에 익었음 ex) 1 state에 1 action 씩을 만들다가 1 state에 여러가지 action으로 작업하면 됨을 깨달음.

기존에 작업에서 문제를 맡닿뜨렸는데 바로 첫화면에 넓이에 따라 Api로 불러오는 오브젝트 갯수를 구하는 작업인데 maxResult가 50이다보니 구해진 값을 50단위로 끊어서 api를 여러번 불러와야했는데 그 작업을
함수안에 또 함수를 넣어서 작업했는데 처음엔 되는하더니 어느순간 루프에 빠져서 게속 함수가 돌아가는 문제발생.
해결법은 while문을 사용해서  maxResult를 선언해두고 limit값은 전달한뒤 offset값을 maxResult만큼 증가시키고 limit은 maxResult만큼 감소시키는 구조로 함수를 실행하니 원하는데로 구해졌다.
  
