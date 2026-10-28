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

## 26.10.07

# 동적 세그먼트 없는 generateStaticParams 실습
generateStaticParams를 쓰면 빌드 시점에 정적 HTML을 미리 생성합니다
예저처럼 안 쓰면, 요청 할 때마다 서버에서 동적으로 처리합니다
따라서 자주 변하지 않는 페이지는 generateStaticParams 사용을 권장합니다
-> 정적 사이트처럼 빠르기 때문입니다
사용자 입력,DB 조회 등이 필요한 경우는 generateStaticParams 없이 런타임 처리를 하는것이 좋습니다.

# generateStaticParams를 사용하는 경우 실습
문서의 예저처럼 generateStaticParams를 사용하는 경우의 예제입니다
app/
ㄴblog3/
   ㄴpage.tsx // 블로그 목록
   ㄴposts.tsx // 더미 데이터
   ㄴ [slug]

빌드 시점에 Next.js가 app/blog3/[slug]/page.tsx 같은 동적 라우트를 찾으면 generateStaticParams()를 실행합니다
generateStaticParams()가 반환하는 값은 다음과 같은 형태입니다
각 params에 대해 page.tsx를 실행하여, 정적 HTML을 생성합니다.

정리하면
-generateStaticParams() 자체는 slug 배열만 반환합니다
-Next.js 빌드 프로세스가 이 배열을 순회하며 -> 각 slug에 대해 page.tsx 실행 -> 정적 HTML

# await이 없어도 async를 붙여 두는 이유
Next.js 13+의 App Router에서 page.tsx 같은 ServerComponent는 비동기 렌더링을 전제로 하고 있습니다.
즉, page.tsx 안에서 데이터를 fetch하는 경우가 많기 때문에 async를 기본으로 붙여도 전혀 문제가 없습니다.
1. 일관성 유지 : 같은 프로젝트 안에서 어떤 페이지는 async, 어떤 페이지는 일반 function이면 혼란스러울 수 있습니다.
   -> Next.js 공식 문서도 대부분 async function으로 예시를 작성합니다.
2. 확장성 : 지금은 더미 데이터(posts.find(...))를 쓰지만, 나중에 DB 나 API에서 데이터를 가져 올 때 await fetch(...) 같은 코드가 들어갈 수 있기 때문에, 미리 async를 붙여 두면 수정할 필요가 없습니다.
3. React Server Component 호환성 : Server Component는 Promise를 반환할 수 있어야 하고, Next.js는 내부적으로 async 함수 패턴에 맞춰 최적화된 렌더링 파이프라인을 갖고 있어서 async가 붙어 있어도 불필요한 오버헤드가 거의 없스니다.

# generateStaticParams가 없는 경우와 있는 경우 비교
generateStaticParams가 없는 경우 Next.js는 slug 값을 빌드 타임에는 모르는 상태입니다.
-> 따라서 slug 페이지에 접속하면 Next.js가 서버에서 요청 할 때마다 해당 페이지를 동적으로 렌더링하며, 빌드의 결과물로 HTML 파일은 생성되지 않습니다.

generateStaticParams가 있는 경우 Next.js에 빌드 타입에 생성할 slug 목록을 알려줄 수 있습니다.
-> 이 경우에는 지정한 slug에 대해서는 정적 HTML + JSON이 빌드 타임에 생성되어, 최소 접근 시 SSR이 필요 없이 미리 만들어진 페이지 제공합니다.

2-3. 느린 네트워크
네트워크가 느리거나 불안정한 경우,

2-4 프리페칭 비활성화
<Link> 컴포넌트에서 prefetch prop을 false로 설정하여 프리페치를 사용하지 않도록 선택할 수 있습니다.
이는 대량의 링크 목록(예: 무한 스크롤 테이블)을 렌더링할 때 불필요한 리소스 사용을 방지하는 데 유용합니다.
그러나 프리페칭을 비활성화하면 다음과 같은 단점이 있습니다.
ㄴ 정적 라우팅은 사용자가 링크를 클릭할 때만 가져옵니다
ㄴ 동적 라우팅은 클라이언트가 해당 경로로 이동하기 전에 서버에서 먼저 렌더링 되어야 합니다
프리페치를 완전히 비활성화하지 않고 리소스 사용량을 줄이려면, 마우스 호버 시에만 프리페치를 사용하면 됩니다.
이렇게 하면 뷰포트의 모든 링크가 아닌, 사용자가 방문할 가능성이 높은 경로로만 프리페치가 제한됩니다.

2-5 Hydration이 완료되지 않음
<Link>는 클라이언트 컴포넌트이기 때문에 라우팅 페이지를 프리페치하기 전에 하이드레이션해야 합니다.
초기 방문 시 대용량 자바스크립트 번들로 인해 하이드레이션이 지연되어 프리페칭이 바로 시작되지 않을 수 있습니다.
React는 선택적 Hydration을 통해 이를 완화하며, 다음과 같은 방법으로 이를 더 욱 개선할수 있습니다.
@next/bundle-analyzer 플러그인을 사용하면 대규모 종속성을 제거하여, 번들 크기를 식별하고 줄일 수 있습니다.
가능하다면 클라이언트에서 서버로 로직을 이동합니다. 자세한 내용은 서버 및 클라이언트 컴포넌트 문서를 참조하세요

3.Examples - 네이티브 히스토리 API
Next.js를 사용하면 기본 window.history.pushState 및 window.history.replaceState 메서드를 사용하여 페이지를 다시 로드하지 않고도 브라우저의 기록 스택을 업데이트할 수 있습니다.
pushState 및 replaceState 호출은 Next.js 라우터에 통합되어 usePathname 및 useSearchParams와 동기화할 수 있습니다.
window.history.pushState
-이 것을 사용하여 브라우저의 기록 스택에 새 항목을 추가할 수 있습니다
-사용자는 이전 상태로 돌아갈 수 있습니다
-예를 들어 제품 목록을 정렬할 수 있습니다.

window.histoy.replaceState
-브라우저의 기록 스택에서 현재 항목을 바꾸려면 이 기능을 사용합니다.
-사용자는 이전 상태로 돌아갈 수 없습니다.
-예를 들어 애플리케이션의 *로케일(Locale)을 전환하는 경우입니다
(locale) : 사용자의 언어, 지역, 날짜/시간 형식, 숫자 표기법 등 사용자 인터페이스에서 사용되는 다양한 설정을 정의하는 문자열

# 1. server 및 client component를 언제 사용해야 하나요?

client 환경과 server 환경은 서로 다른 기능을 가지고 있습니다.
server 및 client component를 사용하면 사용하는 사례에 따라 각각의 환경에서 필요한 로직을 실행할 수 있습니다
다음과 같은 항목이 필요한 경우에는 client component를 사용합니다
-state 및 event handler. 예 : onClick, onChange
-Lifecycle logic 예: useEffect
-브라우저 전용 API. 예 localStorage, window, Navigator.geolocation, 등
-사용자 정의 Hook

다음과 같은 항목이 필요할 경우에는 server component를 사용합니다
-서버의 데이터베이스 혹은 API에서 data를 가져오는 경우 사용합니다.
-API key,token 및 기타 보안 데이터를 client에 노출하지 않고 사용합니다
-브라우저로 전송 되는 javaScript의 양을 줄이고 싶을 때 사용하니다.
-콘텐츠가 포함된 첫 번째 페인트(First Contentful Paint-FCP)를 개선하고, 콘텐츠를 client에 점진적으로 스트리밍합니다.

## 26.10.14

# Introduction
기본적으로 layout과 page는 server component입니다
server에서 데이터를 가져와 UI의 일부를 렌더링할 수 있고, 선택적으로 결과를 cache한 후 client로 스트리밍할 수 있습니다
상호작용이나 브라우저 API가 필요한 경우 client component를 사용하여 기능을 계층화 할 수 있습니다.

# 1. server 및 client component를 언제 사용해야 하나요?
client 환경과 server 환경은 서로 다른 기능을 가지고 있습니다.
server 및 client component를 사용하면 사용하는 사례에 따라 각각의 환경에서 필요한 로직을 실행할 수 있습니다.

다음과 같은 항목이 필요할 경우에는 client component를 사용합니다.
-state 및 event handler. 예 onClick, onChange
-Lifecycle logic. 예 useEffect
-브라우저 전용 API. 예 localStorage, window, Navigator,geolocation 등
-사용자 정의 Hook

다음과 같은 항목이 필요할 경우에는 server component를 사용합니다.
-서버의 데이터베이스 혹은 API에서 data를 가져오는 경우 사용합니다.
-API key, token 및 기타 보안 데이터를 client에 노출하지 않고 사용합니다.
-브라우저로 전송되는 JavaScript의 양을 줄이고 싶을 때 사용합니다.
-콘텐츠가 포함된 첫 번째 페인트(First Contentful Paint-FCP)를 개선하고, 콘텐츠를 client에 점진적으로 스트리밍합니다.

# Optimistic Update
[Optimistic Update(낙관적 업데이트)]
사용자에 의해서 이벤트(예: 좋아요 버튼 클릭)가 발생하면, 서버 응답을 가디라지 않고 클라이언트(브라우저)의 UI를 즉시 변경(업데이트)합니다.
서버에 보낸 요청의 성공을 낙관(optimistic)한다고 가정해서 먼저 화면에 변화를 보여줍니다.
서버에서 응답이 없으면, UI를 원래 상태로 되돌립니다.(rollback)
네트워크 지연 동안에도 앱이 "빠르게 반응" 하도록 

# Pessimistic Update
[Pessimistic Update(비관적 업데이트)]
이벤트가 발행하면 먼저 서버에 요청을 보내고, 서버에서 성공 응답을 받은 후에 클라이언트의 UI를 업데이트합니다.

(장점)
-서버의 응답을 기반으로 하기 때문에 데이터의 일관성이 보장
-오류가 발생할 가능이 낮고, 잘못된 정보가 표시될 염려가 없음
(단점)
-사용자는 서버의 으답을 기다려야 하므로, 응답이 늦어지면 사용자 경험이 저하될 수 있음
-특히 네트워크 지연이 발생할 경우 체감 속도가 느려짐

[isLiking state의 주요 역할]
중복 클릭 방지 : isLiking이 true인 동안은 버튼을 disabled로 만들어 중복 요청 즉 중복 낙관적 업데이트를 막는 역할을 합니다.
UI 피드백 :로딩 상태 표시(스피너나 문구)를 위해 사용이 가능합니다.
상태 안정화 : 서버에 요청이 끝날 때까지 추가 상태 변경을 잠시 멈추게 해서, 일관된 동작을 보장합니다.

# 2.Next.js에서 server와 client component는 어떻게 작동합니까?
2-1 server component의 작동
next.js는 server에서 React의 API를 사용하여 렌더링을 조정합니다.
렌더링 작업은 개별 라우팅 세그먼트 별 묶음(Chunk)으로 나뉩니다. (layout 및 page)

server component는 RSC Payload(React Server Component Payload)라는 특수한 데이터 형식으로 렌더링 됩니다.
client component 와 RSC Payload는 HTML을 미리 렌더링(prerender)하는데 사용됩니다.

React Server Component Payload(RSC)란 무엇인가요?
RSC페이로드는 렌더링된 React Server Components 트리를 직렬화하여 표현한 컴팩트한 형태입니다.
client에서 React가 브라우저의 DOM을 업데이트하는데 사용됩니다.

# RSC(RSC Payload)는 JSON인가, 바이너리인가?
과거 : JSON 기반
RSC 초기에는 JSON 형식의 문자열로 데이터를 전달

현재 : 바이너리 형식으로 최적화

왜 JSON 대신 바이너리를 쓸까?
React는 컴포넌트 구조, props, 서버에서 생성된 UI 정보를 RSC 프로토콜로 정의하고, 이를 전송하는 것에 특화된 이진 형식을 사용합니다.

어떻게 이진이 브라우저에서 처리될까?
React가 server에서 만든 UI 트리를 바이너리 스트림으로 client에 전달합니다.
client 측 React는 이를 해석해 UI를 재구성합니다.

2-2 client component의 작동 (첫 번째 load)
1. HTML은 사용자에게 경로(라우팅 페이지)의 비대화형(정적) 미리보기를 즉시 보여주는데 사용됩니다.
2. RSC 페이로드는 client와 server component 트리를 조정하는데 사용됩니다.
3. JavaScript는 client component를 hydration하고, 애플리케이션을 대화형으로 만드는 데 사용됩니다.

Hydration이란 무엇인가?
Hydration은 이벤트 핸들러를 DOM에 연결하여 정적 HTML을 인터랙티브하게 만드는 React의 프로세스입니다.

2-3 후속 네비게이션

# 3.Example

3-1 client component 사용
파일의 맨 위, 즉 import문 위에 "use client" 지시문을 추가하여 client component를 생성 할수있음
"use clinet" 는 server와 client 모듈 트리 사이의 경계를 선언하는 데 사용됩니다.
파일에 "use client"로 표시되면 해당 파일의 모든 import와 자식 component는 client 번들의 일부로 간주됩니다.
즉, client 대상으로 하는 모든 component에 이 지시문을 추가할 필요가 없습니다.

## 26.10.28

3-2 JS bundle 크기 줄이기
client JavaScript 번들의 크기를 줄이려면 UI의 큰 부분을 client component로 표시하는 대신 특정 대화형 component에 "use client"를 추가합니다.

예를 들어, 다음 예제의 <Layout> component는 로고와 탐색 링크와 같은 정적 요소가 대부분이지만 대화형 검색창이 포함되어 있습니다.
<Search />는 대화형이기 떄문에 client component가 되야 하지만, 나머지 layout은 server component로 유지될 수 있습니다.
나머지 layout은 server component로 유지해야 합니다!

<search />는 사용자와의 상호작용이 바로 이루어질 가능성이 있기 떄문에 Client Component로 사용하고, <Logo />는 상대적으로 중요하지 않고 이미지 등 용량이 크기때문에 Server Component로 사용하는 것이 좋다.

3-3 sever에서 client component로 데이터 전달
props를 사용하여 server component에서 client component로 데이터를 전달할 수 있습니다.
앞에서 작성한 PostPage(/[id]/page.tsx) server component는 line28에서 client component인 LikeButton으로 likes props
