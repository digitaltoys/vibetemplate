# 데모 시나리오: 공학용 계산기 웹앱 개발 과정

## 1. 요구사항 정의 및 기술 스택 선정

> **Cascade 프롬프트**: ... 웹앱을 개발하려고 합니다. React와 TypeScript를 사용하고, Clean Architecture와 SOLID 원칙을 적용하려고 합니다. 어떤 기술 스택과 아키텍처를 추천하시겠습니까?"

1. 요구사항 분석
   - 

2. 기술 스택 선정
   - 프론트엔드: React 19.1 + TypeScript
   - UI 라이브러리: tailwind UI
   - 상태 관리: 
   - 빌드 시스템: Vite
   - 패키지 관리: npm
   - 테스트: Vitest + React Testing Library

3. 아키텍처 설계
   - Clean Architecture 원칙 적용
   - SOLID 원칙 준수
   - 계층 구조: entities, usecases, adapters, frameworks

## 2. 프로젝트 초기 설정

> **Cascade 프롬프트**: "선택한 기술 스택을 바탕으로 프로젝트 초기 설정을 도와주세요. Clean Architecture에 맞는 디렉토리 구조를 만들고 필요한 패키지를 설치해주세요."

1. 프로젝트 생성
   ```sh
   npm create vite@latest <projectname> -- --template react-ts
   cd <projectname>
   ```

2. 디렉토리 구조 설정
   ```sh
   mkdir -p src/{entities,usecases,adapters,frameworks/{ui,state}}
   mkdir -p docs
   ```

3. 의존성 설치
   ```sh
   npm install <packagename>
   npm install -D vitest
   ```

## 3. 핵심 기능 구현

> **Cascade 프롬프트**: "TDD 방식으로 Expression 엔티티와 기본 계산기 UI 컴포넌트를 구현해주세요."

1. 엔티티 계층 구현
   - Expression 인터페이스 및 구현체 작성
   - TDD 방식으로 테스트 먼저 작성 후 구현

2. UI 컴포넌트 구현
   - 

## 4. 다크/라이트 모드 구현

> **Cascade 프롬프트**: "웹앱에 다크/라이트 모드 기능을 추가하고 싶습니다. CSS 변수를 사용하여 테마를 구현하고, 사용자가 테마를 전환할 수 있는 토글 버튼을 추가해주세요. 또한 사용자의 선호도를 localStorage에 저장하고, 시스템 테마도 감지하도록 해주세요."

1. CSS 변수 기반 테마 시스템 구축
   ```css
   /* index.css */
   :root {
     --bg-color: #ffffff;
     --text-color: #333333;
     --primary-color: #4a90e2;
     /* 기타 라이트 모드 변수 */
   }

   [data-theme="dark"] {
     --bg-color: #1a1a1a;
     --text-color: #f0f0f0;
     --primary-color: #61dafb;
     /* 기타 다크 모드 변수 */
   }
   ```

2. 테마 토글 기능 구현
   ```tsx
   // Calculator.tsx
   const [theme, setTheme] = useState<'light' | 'dark'>('light');
   
   const toggleTheme = () => {
     const newTheme = theme === 'light' ? 'dark' : 'light';
     setTheme(newTheme);
     localStorage.setItem('theme', newTheme);
     document.documentElement.setAttribute('data-theme', newTheme);
   };
   ```

3. 시스템 테마 감지 및 적용
   ```html
   <!-- index.html -->
   <script>
     // 시스템 테마 감지 및 localStorage에 저장된 사용자 선호도 확인
     const savedTheme = localStorage.getItem('theme');
     const systemTheme = window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';
     const theme = savedTheme || systemTheme;
     document.documentElement.setAttribute('data-theme', theme);
   </script>
   ```

## 5. GitHub Pages 배포 설정

> **Cascade 프롬프트**: "GitHub Pages를 사용하여 웹앱을 배포하고 싶습니다. Vite 설정과 GitHub Actions 워크플로우를 설정해주세요."

1. Vite 설정 수정
   ```ts
   // vite.config.ts
   export default defineConfig({
     plugins: [react()],
     base: '/vibecoding-demo/',
   });
   ```

2. GitHub Actions 워크플로우 생성
   ```yaml
   # .github/workflows/deploy.yml
   name: Deploy to GitHub Pages
   
   on:
     push:
       branches: [ "main" ]
   
   permissions:
     contents: read
     pages: write
     id-token: write
   
   concurrency:
     group: "pages"
     cancel-in-progress: true
   
   jobs:
     build:
       runs-on: ubuntu-latest
       steps:
         - name: Checkout
           uses: actions/checkout@v4
         
         - name: Setup Node
           uses: actions/setup-node@v4
           with:
             node-version: '22'
         
         - name: Install dependencies
           run: npm install
         
         - name: Build
           run: npm run build
         
         - name: Setup Pages
           uses: actions/configure-pages@v4
         
         - name: Upload artifact
           uses: actions/upload-pages-artifact@v3
           with:
             path: './dist'
     
     deploy:
       permissions:
         contents: read
         pages: write
         id-token: write
       runs-on: ubuntu-latest
       needs: build
       steps:
         - name: Deploy to GitHub Pages
           id: deployment
           uses: actions/deploy-pages@v4
   ```

3. GitHub 저장소 설정
   - GitHub에 저장소 생성: `roboco-io/vibecoding-demo`
   - 로컬 저장소를 원격 저장소에 연결 및 푸시
   ```sh
   git remote add origin https://github.com/roboco-io/vibecoding-demo.git
   git push -u origin main
   ```

## 6. 배포 및 테스트

> **Cascade 프롬프트**: "GitHub Actions 워크플로우에서 yarn이 아닌 npm을 사용하도록 수정해주세요. 그리고 deploy 작업의 환경 설정 오류를 해결해주세요."

1. GitHub Actions 워크플로우 실행 확인
   - GitHub 저장소 > Actions 탭에서 워크플로우 실행 상태 확인

2. 배포된 사이트 확인
   - 

3. 다크/라이트 모드 기능 테스트
   - 테마 토글 버튼 작동 확인
   - 시스템 테마 감지 확인
   - 사용자 선호도 저장 확인

## 7. 문서화 및 체크리스트 업데이트

> **Cascade 프롬프트**: "프롬프트 내용을 중심으로 데모 시나리오를 docs에 만들어주세요. 그리고 README에 문서들에 대한 링크섹션과 GitHub workflow 배지를 추가해주세요."

1. README.md 업데이트
   - 프로젝트 개요 및 기능 설명
   - 기술 스택 및 아키텍처 설명
   - 설치 및 실행 방법
   - GitHub Actions 배지 추가

2. 체크리스트 업데이트
   - 완료된 작업 체크
   - 다음 단계 계획

3. 데모 시나리오 문서 작성
   - 개발 과정 및 주요 기능 시연 방법 정리
