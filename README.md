# React2

## 2026.09.09

등록한 스크립트는 애플리케이션 개발의 다양한 명령을 참조합니다.
- next dev : 개발 서버를 시작
- next build : 프로덕션을 위한 애플리케이션 빌드
- next start : 프로덕션 서버를 시작
- next lint : ESLint를 실행

이제 Turbopack이 기본 번들러입니다. Webpack을 사용하려면 next dev --webpack 또는 next build -webpack을 실행하면 됩니다.
설정에 대한 자세한 내용은 Turbopack문서를 참조하세요

# 오류처리
문서의 지시 대로만 처리하면 오류가 발생
타입스크립트 환경이 아니기 때문입니다
타입스크립트 환경에서 react와 react-dom을 사용할 수 있도록 타입 정의를 제공하는 패키지를 설치해야 합니다.
Bash - pnpm
$ pnpm add -D @types/react @types/react -dom

$ npm install -D@types/react @types/react-dom

# Installation - 프로젝트 수동 생성
[public 디렉토리 생성 (선택사항)]

이미지, 글꼴 등의 정적 리소스를 저장하기 위한 public 디렉토리를 프로젝트 루트에 생성
public 디렉토리를 생성하면 기본 URL(/)로 public 디렉토리 내부의 리소스를 참조할 수 있습니다.
예를 들어 public/profile.png는 /profile.png와 같이 참조할 수 있습니다.

[개발 서버 실행]
1. 개발 서버를 시작하려면 다음 명령을 실행합니다. pnpm dev
2. 명령을 실행한 후 http://localhost:3000 로 접속하면 App을 확인할 수 잇습니다.
   #수동으로 접속해야 합니다.(react와 다름)
3. app/page.tsx 파일을 편집하고 저장하면, 브라우저를 통해 업데이트된 결과를 확인할 수 있습니다.

[import 및 모듈의 절대 경로 별칭 설정]
Next.js에는 tsconfig.json 및 jsconfig.json 파일의 "paths" 및 "baseUrl" 옵션을 기본적으로 지원합니다.
이 옵션을 사용하면 프로젝트 디렉터리를 절대 경로로 별칭 하여 모듈을 더 쉽고 깔끔하게 가져올 수 있습니다.
별칭으로 import를 구성하려면 tsconfig.json 또는 jsconfig.json 파일의 baseUrl에 구성 옵션을 추가하세요.


# baseUrl의 근본적인 문제
baseUrl은 typescript 6.0부터 더이상 사용되지 않음 되었고, 7.0에서 완전히 삭제되었습니다.

과거에는 절대 경로와 모듈 별칭을 설정하기 위해 baseUrl이 필수적이였으나, 현재는 번들러와 타입스크립트 컴파일러가 발전하면서 불필요한 레거시가 없어짐

# 자동 생성되는 항목
강의에서는 프로젝트를 자동으로 생성해서 사용함
다음은 프로젝트를 자동 생성 할때 자동으로 생성되는 항목입니다.
package.json 파일에 script 자동 추가 / public 디렉토리
TypeScript 사용(선택) : tsconfig.json 파일 생성
Eslint 설정(선택) : eslintrc.json 대신 eslint.config.mjs 파일 생성
Tailwind CSS 사용(선택)
src 디렉토리 사용(선택)
App Router(선택),app/layout.tsx 파일 및 app/page.tsx
Turbopack 사용(선택)
import alias 사용(선택) : No로 해도 tsconfig.json에 "paths" 자동 생성
수동으로 프로젝트를 생성할 때 추가적으로 해야 하는 작업을 자동으로 처리해 줍니다.

# 실습에 사용할 프로젝트를 생성합니다.
공식 문서에서는 기본 패키지 관리자로 pnpm을 사용합니다 pnpm create next-app@latest
pnpm과 관련한 내용은 뒤에서 설명합니다.               npm create-next-app@latest
명령을 실행하고, 프로젝트 이름을 입력하면 다음과 같은 3개의 선택 항목이 나옵니다. 
Yes, use recommended defaults
No, reuse previous settings
No, customs

# .eslintrc.json vs eslint.config.mjs
JSON은 주석,변수,조건문 등을 쓸 수 없기 때문에 복잡한 설정이 어렵습니다.
(JavaScript Object Notation)

# 용어 정의
route라는 단어가 자주 등장하고, 사전적 의미로는 "경로"입니다.
route(라우트)는 "경로"를 의미하고, routing(라우팅)은 "경로를 찾아가는 과정"을 의미합니다
그런데 path도 "경로"로 번역하기 때문에의 구별을 위해 대부분 routing(라우팅)으로 번역했습니다.

directory와 folder는 특별한 구분 없이 나옵니다.
최상위 폴더의 경우 directory로 하위 폴더는 folder로 쓰는 경우가 많지만 꼭 그렇지는 않습니다.
directory와 folder는 OS에 따라 구분되는 용어이기 때문에 같은 의미로 이해하면 됩니다.
segment는 routing과 관련이 있는 directory의 별칭 정도로 이해하면 됩니다.


## 2026.09.16

# Next.js의 동적 라우팅

# 1. Folder and file conventions ( 폴더 및 파일 규칙 )

일반 동적 라우팅 (Dynamic segments)
[동적 라우팅] Dynamic routes
대괄호를 사용하여 세그먼트를 매개변수화 할 수 있습니다.
단일 매개변수의 경우 [segment]를 사용합니다.
모든 값을 포괄하는 매개변수(catch-all)의 경우 [...segment]를 사용합니다.
선택적 포괄 매개변수의 경우 [[...segment]]를 사용합니다.
params 속성을 통해 값에 접근할 수 있습니다.

Catch-all 라우팅

[라우팅 그룹 및 비공개 폴더] Route Groups and private folders
라우트 그룹(group)을 사용하여 URL을 변경하지 않고 코드를 정리할 수 있습니다.
라우팅되지 않는 파일들은 _folder라는 비공개 디렉토리에 함께 저장합니다.

Path|URL pattern|Notes
app/(marketing)/page.tsx|/| URL에서 제외된 그룹
app/(shop)/cart/page.tsx|cart|(shop)내에서 레이아웃 공유
app/blog/_components/Post.tsx| - | 라우팅 대상 아님 : UI 유틸리티를 위한 안전한 공간
app/blog/_lib/data.ts| - | 라우팅 대상 아님 : 유틸리티를 위한 안전한 공간

[병렬 및 차단 라우팅] Parallel and Intesrcepted Routes
이러한 기능은 슬롯 기반 레이아웃이나 모달 라우팅과 같은 특정 UI 패턴에 적합합니다.
부모 레이아웃에서 렌더링되는 명명된 슬롯(named slots)에는 @slot을 사용합니다.
인터셉트 패턴을 사용하면 URL을 변경하지 않고도 현재 레이아웃 내에서 다른 경로를 렌더링할 수 있습니다.
예를 들면 목록 위에 모달 형태로 상세 보기를 표시할 떄 사용할 수 있습니다.

Pattern (docs) | Meaning | 일반적인 사용 사례
@folder | 명명된 슬롯(Named slot | 사이드바 | 메인 콘텐츠
(.)folder | 동일 레벨 가로채기(intercept) | 모달에서 형제 라우트 미리보기
(..)folder | 한 레벨 위에서 가로채기 | 부모의 자식 라우트를 오버레이로 열기
(..)(..)folder | 두 레벨 위에서 가로채기 | 깊게 중첩된 오버레이
(...)folder | 루트에서 가로채기 | 현재 뷰에 임의의 라우트 표시

[메타 데이터 파일 규칙] Metadata file conventions


# Open Graph Protocol
웹사이트나 페이스북, 인스타그램, X (트위처), 카카오톡 등에 링크를 전달할 떄 미리보기를 생성하는 프로토콜

# 2.Organizing your project(프로젝트 구성하기)
Next.js는 프로젝트 파일을 어떻게 구성ㄷ하고 어디에 배치할지에 대한 제약이 없음
하지만 프로젝트를 체계적으로 구성하는데 도움이 되는 몇가지 기능을 제공

[ component의 계층 구조] Component hierarchy
특수 파일에 정의된 컴포넌트는 특정 계층 구조로 렌더링 됩니다
layout.js
template.js
error.js(React 오류 경계)
loading.js(리액트 서스펜스 경계)
not-found.js(React 오류 경계)
page.js 또는 중첩 layout.js

# layout과 template의 차이
유사한 기능을 갖고 있지만 동작 방식에 차이가 있습니다.

파일 | 특징 | 상태/DOM 유지 | 사용 사례
layout.tsx | 경로별 공유 레이아웃 | 유지됨(정적) | 네비게이션, 사이드바, 공통 레이아웃
template.tsx | 매번 새 인스턴스 생성 | 초기화됨(동적) | 페이지별로 초기화가 필요한 경우


[코로케이션] Colocation-
파일 및 폴더를 기능별로 그룹화하여 프로젝트의 구조를 명확하게 정의하는 것을 코로케이션 이라고 합니다.
app 디렉토리에서 중첩된 폴더는 라우팅 구조를 정의합니다.
각 폴더는 URL의 해당 세그먼트에 맵핑되는 라우팅 세그먼트를 나타냅니다.
그러나 폴더를 통해 라우트 구조가 정의되도, 해당 라우트 세그먼트에 page.js 또는 route.js파일이 추가되기 전까지는 외부에서 해당 라우트에 접근할 수 없습니다.

- 알아두면 좋아요
  : 프로젝트 파일을 app 폴더에 함께 저장할 수는 있지만 꼭 그럴 필요는 없습니다. 원한다면 app 디렉토리 외부에 보관할 수도 있습니다.
  이런 문제들을 모두 경험하기 떄문에 Next.js에서도 src 디렉토리 사용을 권장하고 있습니다.


[비공개 폴더] Private Polder

[라우팅 그룹] Route Groups
폴더를 괄호로 묶어 라우팅 그룹을 만둘 수 있습니다.(folderName)
이 것은 해당 폴더가 구성 목적으로 사용되는 것을 의미하여, 라우터의 URL 

라우팅 그룹은 다음과 같은 경우에 유용합니다.
사이트 섹션, 목적 또는 팀별로 라우트를 구성합니다.
-예: 마케팅 페이지, 관리 페이지 등.
동일 라우팅 세그먼트 수준에서 중첩 레이아웃 활성화 :
-공통 세그먼트 안에 여러개의 루트 레이아웃을 포함하여 여러 개의 중첩 레이아웃 만들기
-공통 세그먼트의 라우팅 하위 그룹에 레이아웃 추가

[src 디렉토리]
Next.js는 애플리케이션 코드(app 포함)를 옵션으로 선택하는 src폴더 내에 저장할 수 

## 26.09.23

## 26.09.30

# Route 방식비교
1.React vs Next.js 라우팅 방식의 차이
수동 자동
외부 라이브러리 필요 / 자체 내장된 파일 기반 라우팅 시스템
코드에서 직접 <Route>로 정의 / 파일 폴더 이름으로 라우트가 자동 매핑

React는 기본적으로 라우팅 기능이 없기 때문에, 직접 라우터 라이브러리를 설치해서 라우팅을 설정해야함

2. Next.js의 라우팅 방식 : Pages Router vs App Router
초기부터 존재 (Next.js 1~12) / Next.js 13부터 도입
pages/ / app/
pages/about.js ->/about / app/about/page.tsx -> /about
간단하고 익숙함(기존 React 방식과 유사) / 더 유연하고 강력한 기능 지원
동적 라우터,getStaticProps 등 / 레이아웃 중첩, 서버 컴포넌트, 로딩 UI, 병렬 라우트 등
유지보수 중(권장 X) / Next.js 14 부터 기본 권장 방식

export default function Page() 형식으로 구성
각 파일은 하나의 페이지 컴포넌트
SSR/SSG 함수는 getStaticProps, getSercerSideProps 등으로 처리

page.js : 각 세그먼트의 페이지
layout.js: 해당 세그먼트 이하의 모든 페이지에 공통 레이아웃 적용
추가기능 : loading.js, error.js, not-found.js, route groups, parallel routes 등


- App Router의 강력한 기능들
중첩 레이아웃 : 여러 레벨의 layout.js 파일을 통해 레이웃을 계층적으로 구성 가능
서버 컴포넌트(RSC) : 서버에서만 렌더링되는 컴포넌트로 성능 최적화 가능(React Server Component)
로딩 UI : 페이지 전환 중 보여줄 loading.js 파일 제공
에러 UI : 특정 경로에서만 발생하는 에러를 처리할 error.js 제공
병렬 라우팅 : 하나의 경로 안에서 탭 같은 독립적인 뷰를 병렬로 렌더링 가능

- 프로젝트 별 추천 방식
새 프로젝트 시작 : App Router (app 디렉토리 기반)
기존 프로젝트 유지보수 : pages/ 계속 사용 가능하지만, 점차 마이그레이션 필요
React처럼 수동 라우팅이 필요한 경우 : React + react-router-dom 사용 가능(Next.js는 자동 라우팅이 기본)

# Introduction
Next.js에서 경로(route)는 기본적으로 서버에서 렌더링됩니다.
따라서 클라이언트는 새 경로를 표시하기 전에 서버의 응답을 기다려야 하는 경우가 많습니다.

Next.js에는 prefectching, streaming, 그리고 client-side transitions(클라이언트 사이드 전환)
기능이 기본 제공되어 네비게이션 속도가 빠르고 반응성이 뛰어납니다.

1. How navigation works

1-1 Sercer Rendering(서버 렌더링)
서버 렌더링에는 발생 시점에 따라 두 가지 유형이 있습니다.
정적 렌더링(또는 사전 렌더링)은 빌드 시점이나 재검증 중에 발생하며, 결과는 캐시(cache)됩니다
(재검증을 사용하면 전체 애플리케이션을 다시 빌드하지 않고도 캐시 항목을 업데이트할수 있습니다.)
동적 렌더링은 클라이언트 요청에 대한 응답으로 요청 시점에 발생

서버 렌더링의 단점은 클라이언트가 새 경로를 표시하기 전에 서버의 응답을 기다려야 한다는 것입니다.
Next.js는 사용자가 방문할 가능성이 높은 경로를 미리 가져오고(prefectching), 클라이언트 측 전환(client-side transitions)을 수행하여 지연 문제를 해결합니다.

알아두면 좋음 : 최초 방문을 위해서 HTML이 생성됩니다.
일반적인 React 앱은 클라이언트 사이드 렌더링(CSR)만 사용하면, 처음 페이지를 방문할 때는 빈 HTML + JavaScript 파일만 내려주고, 브라우저가 JS를 실행해야 화면이 렌더링됩니다.
하지만 Next.js에서는 :
사용자가 특정 URL을 처음 방문하면 (initial visit)서버가 해당 페이

1-2 Prefetching(프리페칭 : 미리 가져오기)
프리페칭은 사용자가 해당 경로로 이동하기 전에 백그라운드에서 해당 경로를 로드하는 프로세스입니다.
사용자가 링크를 클릭하기 전에 다음 경로를 렌더링하는데 필요한 데이터가 클라이언트측에 이미 준비되어 있기 때문에 애플리케이션에서 경로 간 이동이 즉각적으로 느껴집니다.
Next.js는 <Link> 컴포넌트와 연결된 경로를 자동으로 사용자 뷰포트에 미리 가져옵니다
<a> tag를 사용하면 프리페칭을 하지 않습니다.

경로의 어느 정도를 프리페칭할지는 정적 경로인지 동적 경로인지에 따라 달라집니다.
정적 경로 : 전체 경로가 프리페치 됩니다.
동적 경로 : 프리페치를 건너뛰거나, loading.tsx가 있는 경우 경로가 부분적으로 프리페칭됩니다.

Next.js는 동적 라우팅을 건너뛰거나 부분적으로 프리페칭하는 방법으로 사용자가 방문하지 않을 수도 있는 경로에 대한 서버의 불필요한 작업을 방지합니다.
그러나 네비게이션 전에 서버 응답을 기다리면 사용자에게 앱이 응답하지 않는다는 인상을 줄 수 도 있습니다.
동적 경로에 대한 네비게이션 환경을 개선하려면 스트리밍을 사용할 수 있습니다.

1-3 Streaming(스트리밍
스트리밍을 사용하면 서버가 전체 경로가 렌더링될 때까지 기다리지 않고, 동적 경로의 일부가 준비되는 즉시 클라이언트에 전송할 수 있습니다.
즉, 페이지의 일부가 아직 로드 중이더라도 사용자는 더 빨리 콘텐츠를 볼 수 있습니다.
동적 경로의 경우, 부분적으로 미리 가져올 수 있다는 뜻입니다.
즉, 공유 레이아웃과 로딩 스켈레톤을 미리 요청할 수 있습니다.
loading skeletons : 웹 또는 앱에서 콘텐츠가 로드되는 동안 사용자에게 보여지는 빈 화면의 일


# Core Web Vitals (웹 성능 지표)
Next.js 공식 문서에서 이야기 하는 TTFB, FCP, TTI 과거에 주로 사용하던 레거시 지표입니다.
"기본적인 준비가 되었는가?"를 측정합니다.
이 지표들은 웹페이지가 기술적으로 로드되는 순서대로 시간을 측정

TTFB (Time to first Byte) : 네트웨크와 서버의 성농을 나타냄

LCP (Largest Contentful Paint) : 뷰포트 내에서 가장 큰 페이지 요소(큰 텍스트 블록, 이미지 또는 비디오)를 표시하는데 걸리는 시간
* 뷰포트 : 웹페이지 사용자가 별도의 스크롤 동작 없이 볼수 있는 영역

FID(First Input Delay) : 사용자가 웹페이지와 상호작용을 시도하는 첫 번째 순간부터 웹페이지가 응답하는 시간
CLS(Cumulative Layout Shift) : 방문자에게 콘텐츠가 얼마나 불안정한 지 측정한 값입니다.

1-4 Client-side transitions(클라이언트 측 전환)
일반적으로 서버 렌더링 페이지로 이동하면 전체 페이지가 로드됩니다.
-> 이로 인해 state가 삭제되고, 스크롤 위치가 재설정되며, 상호작용이 차단됩니다.
Next.js는 <Link> 컴포넌트를 사용하는 클라이언트 측 전환을 통해 이를 방지합니다. 페이지를 다시 로딩하는 대신 다음과 같은 방법으로 콘텐츠를 동적으로 업데이트합니다.
공유 레이아웃과 UI를 유지합니다.
현재 페이지를 미리 가져온(prefetching) 로딩 상태 또는 사용 가능한 경우 새 페이지로 바꿉니다.

클라이언트 측 전환은 서버에서 렌더링된 앱을 클라이언트에서 렌더링된 앱처럼 느껴지게 하는 요소입니다.
또한 프리페칭 및 스트리밍과 함께 사용하면 동적 경로에서도 빠른 전환이 가능합니다.

# 2. 전환을 느리게 만드는 요인은 무엇인가?
Next.js는 최적화를 통해 네비게이션 속도가 빠르고 반응성이 뛰어납니다.
하지만 특정 조건에서는 전환 속도가 여전히 느릴 수 있습니다.
다음은 몇가지 일반적인 원인과 사용자 경험을 개선하는 방법입니다.

2-1 동적 경로 없는 loading.tsx

2-2 동적 세그먼트 없는 generateStaticParams
동적 세그먼트는 사전 렌더링될수 있지만, generateStaticParams가 누락되어 사전 렌더링 되지 않는 경우, 해당 경로는 요청 시점에 동적 렌더링으로 대체됩니다.
generateStaticParams를 추가하여 빌드 시점에 경로가 정적으로 생성되도록 합니다.
