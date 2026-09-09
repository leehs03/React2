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
