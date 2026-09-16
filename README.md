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

