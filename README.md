[https://greatsong.github.io/schedule-poll/]
# [💕 우리 언제 만나? — 시간 약속 투표 앱] (https://greatsong.github.io/schedule-poll/)

> 선생님들 간 시간 약속을 잡기 위한 **실시간 투표 웹앱**입니다.
> 누구나 링크 하나로 접속해서 가능한 시간을 탭하면, 모든 사람의 결과가 실시간으로 모입니다.

---

## ✨ 주요 기능

| 기능 | 설명 |
|------|------|
| 📅 시간 투표 | 월~금, 1~7교시 + 점심시간 중 가능한 시간을 탭 |
| 🏆 베스트 시간 | 가장 많은 사람이 가능한 시간을 자동 하이라이트 |
| 🔔 미투표 알림 | 아직 투표 안 한 사람이 누구인지 표시 |
| 🔄 실시간 동기화 | 한 명이 투표하면 다른 사람 화면에 즉시 반영 |
| 🎨 6가지 테마 | 💕핑크 🌸블라썸 🍑코랄 💜라벤더 🌊블루 🌿그린 |
| ♻️ 재사용 | 새 투표를 계속 만들 수 있어서 다른 그룹과도 사용 가능 |

---

## 🚀 처음부터 끝까지 세팅 가이드

### 준비물

- **Google 계정** 1개 (Gmail이면 됩니다)
- **GitHub 계정** 1개 ([github.com](https://github.com) 에서 무료 가입)
- 이 저장소의 `index.html` 파일

> 💡 **비용은 전혀 들지 않습니다.** Firebase 무료 플랜 + GitHub Pages 무료 호스팅으로 충분합니다.

---

### 📌 1단계: Firebase 프로젝트 만들기

1. [Firebase Console](https://console.firebase.google.com/)에 접속합니다
2. Google 계정으로 로그인합니다
3. **"프로젝트 추가"** (또는 "Add project") 버튼을 클릭합니다
4. 프로젝트 이름을 입력합니다
   - 예: `schedule-poll` 또는 `우리반투표` (영문 권장)
5. Google 애널리틱스 설정 화면이 나오면 → **끄고** 넘어가도 됩니다
6. **"프로젝트 만들기"** 클릭 → 잠시 기다리면 완료!

> ✅ **체크포인트:** "프로젝트가 준비되었습니다" 메시지가 보이면 성공입니다.

---

### 📌 2단계: Realtime Database 만들기

1. Firebase Console 왼쪽 메뉴에서 **"빌드"** 클릭
2. 하위 메뉴에서 **"Realtime Database"** 클릭
3. **"데이터베이스 만들기"** 버튼 클릭
4. **위치 선택:** `싱가포르 (asia-southeast1)` 선택
   - 한국에서 가장 빠른 서버입니다
5. **보안 규칙:** **"테스트 모드에서 시작"** 선택
6. **"사용 설정"** 클릭

> ✅ **체크포인트:** 빈 데이터베이스 화면이 보이고, 상단에 `https://프로젝트명-default-rtdb.asia-southeast1.firebasedatabase.app/` 같은 URL이 보이면 성공입니다.

---

### 📌 3단계: 보안 규칙 설정하기

> ⚠️ 이 단계를 건너뛰면 30일 후 데이터 읽기/쓰기가 차단됩니다!

1. Realtime Database 페이지에서 **"규칙"** 탭을 클릭합니다
2. 기존 내용을 **전부 지우고** 아래 내용을 **통째로 복사해서 붙여넣습니다:**

```json
{
  "rules": {
    "polls": {
      ".read": true,
      ".write": true
    },
    "votes": {
      ".read": true,
      ".write": true
    }
  }
}
```

3. **"게시"** (Publish) 버튼을 클릭합니다

> ✅ **체크포인트:** "규칙이 게시되었습니다" 메시지가 나오면 성공입니다.
>
> 💡 **이 규칙의 의미:** `polls`(투표 정보)와 `votes`(투표 결과)만 읽기/쓰기를 허용합니다. URL을 아는 사람만 접근할 수 있으므로, 소규모 내부용으로 충분히 안전합니다.

---

### 📌 4단계: 웹 앱 등록하고 설정값 복사하기

1. Firebase Console 왼쪽 상단의 ⚙️ **톱니바퀴** 아이콘 → **"프로젝트 설정"** 클릭
2. 페이지 아래로 스크롤 → **"내 앱"** 섹션에서 **웹 아이콘 (`</>`)** 클릭
3. 앱 닉네임 입력 (예: `schedule-poll`) → **"앱 등록"** 클릭
4. 화면에 나오는 코드 중 `firebaseConfig` 부분을 **복사**합니다:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "my-project.firebaseapp.com",
  databaseURL: "https://my-project-default-rtdb.asia-southeast1.firebasedatabase.app",
  projectId: "my-project",
  storageBucket: "my-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abcdef"
};
```

> ⚠️ **중요!** `databaseURL` 줄이 반드시 포함되어야 합니다!
> 간혹 Firebase가 이 줄을 빠뜨리는 경우가 있습니다.
> 빠져 있다면 2단계에서 확인한 데이터베이스 URL을 직접 추가해주세요.

> ✅ **체크포인트:** 위와 같은 형태의 설정값을 복사했으면 성공입니다.

---

### 📌 5단계: index.html에 설정값 붙여넣기

1. 이 저장소의 `index.html` 파일을 텍스트 편집기로 엽니다
   - Windows: 메모장, VS Code
   - Mac: 텍스트 편집, VS Code
2. 파일 안에서 아래 부분을 찾습니다:

```javascript
// 🔥 Firebase 설정  ← 여기에 본인 Firebase 설정을 붙여넣으세요!
const firebaseConfig = {
  apiKey: "",
  authDomain: "",
  databaseURL: "",
  ...
};
```

3. 빈 따옴표(`""`) 부분을 **4단계에서 복사한 값**으로 교체합니다
4. 저장합니다 (`Ctrl+S` 또는 `Cmd+S`)

> ✅ **체크포인트:** `index.html` 파일을 브라우저에서 열었을 때 "우리 언제 만나?" 화면이 나오면 성공입니다! (파일을 더블클릭하면 브라우저에서 열립니다)

---

### 📌 6단계: GitHub에 올리기

#### 방법 A: GitHub 웹사이트에서 직접 (가장 쉬움!)

1. [GitHub](https://github.com)에 로그인합니다
2. 오른쪽 상단 **`+`** → **"New repository"** 클릭
3. Repository name: `schedule-poll` (원하는 이름)
4. **Public** 선택 (GitHub Pages는 Public이어야 무료)
5. **"Create repository"** 클릭
6. **"uploading an existing file"** 링크 클릭
7. `index.html` 파일을 **드래그 앤 드롭**으로 올립니다
8. **"Commit changes"** 클릭

#### 방법 B: 명령어 사용 (Git 설치 필요)

```bash
git init
git add index.html README.md
git commit -m "시간 약속 투표 앱"
git branch -M main
git remote add origin https://github.com/본인아이디/schedule-poll.git
git push -u origin main
```

> ✅ **체크포인트:** GitHub 저장소 페이지에서 `index.html` 파일이 보이면 성공입니다.

---

### 📌 7단계: GitHub Pages 배포하기 (마지막!)

1. GitHub 저장소 페이지에서 **"Settings"** 탭 클릭
2. 왼쪽 메뉴에서 **"Pages"** 클릭
3. **Source:** `Deploy from a branch` 선택
4. **Branch:** `main` 선택, 폴더는 `/ (root)` 유지
5. **"Save"** 클릭
6. 1~2분 기다립니다

> ✅ **체크포인트:** Settings → Pages 상단에 아래와 같은 URL이 나타나면 완성입니다!
>
> ```
> https://본인아이디.github.io/schedule-poll/
> ```
>
> 🎉 **이 URL을 선생님들께 공유하세요!**

---

## 📱 사용 방법

### 투표 만들기
1. 홈 화면에서 투표 제목과 참여자 이름을 입력합니다
2. 쉼표(`,`)로 이름을 구분합니다
3. **"투표 시작하기"** 버튼을 탭합니다
   - 또는 **"⚡ 수리정보교육부 바로 시작"** 버튼으로 기본 멤버로 빠르게 시작

### 투표하기
1. 선생님 성함을 선택합니다
2. 가능한 요일/교시를 탭합니다 (다시 탭하면 취소)
3. 투표는 자동 저장됩니다

### 결과 확인하기
- 🏆 가장 많은 선생님이 가능한 시간이 하이라이트됩니다
- 🔔 아직 투표하지 않은 선생님이 표시됩니다
- 각 칸에 누가 가능한지 이름이 나옵니다
- 실시간 자동 업데이트 — 새로고침 필요 없음!

### 테마 바꾸기
- 상단의 이모지 버튼을 탭하면 6가지 테마로 변경됩니다
- 💕 러블리 핑크 / 🌸 체리블라썸 / 🍑 피치 코랄 / 💜 라벤더 드림 / 🌊 오션 블루 / 🌿 포레스트 그린
- 선택한 테마는 브라우저에 저장되어 다음에 열어도 유지됩니다

---

## ❓ 자주 묻는 질문

### 비용이 드나요?
**아닙니다.** Firebase 무료 플랜(Spark)과 GitHub Pages 무료 호스팅을 사용합니다. Firebase 무료 한도는 동시 접속 100명, 저장 1GB, 월 10GB 전송으로, 투표 앱에는 넉넉합니다.

### 다른 그룹과도 쓸 수 있나요?
**네!** 홈 화면에서 "새 투표 만들기"로 별도 투표를 생성하면 됩니다. 투표 목록은 모든 사용자가 공유합니다.

### 투표를 초기화하고 싶어요
Firebase Console → Realtime Database → 데이터 탭에서 해당 투표를 찾아 삭제하면 됩니다.

### 보안은 괜찮나요?
별도 로그인 없이 이름만 선택하는 방식입니다. URL을 아는 사람만 접근할 수 있으므로, **URL을 외부에 공개하지 않는 것**을 권장합니다.

### 모바일에서도 되나요?
**네!** 반응형으로 만들어져서 스마트폰, 태블릿, PC 모두에서 잘 작동합니다.

---

## 🔧 문제 해결

| 증상 | 해결 방법 |
|------|-----------|
| "Firebase 설정 필요" 화면이 나옴 | `index.html`의 `firebaseConfig`에 설정값이 제대로 들어갔는지 확인 |
| 데이터가 저장 안 됨 | Firebase Console → Realtime Database → 규칙 탭에서 위의 보안 규칙이 적용되었는지 확인 |
| `databaseURL` 에러 | `firebaseConfig`에 `databaseURL` 줄이 있는지 확인. 없으면 Firebase Console → Realtime Database 상단의 URL을 직접 추가 |
| 30일 후 작동 안 됨 | 테스트 모드 보안 규칙이 만료된 것. 3단계의 규칙으로 다시 설정 |

---

## 📂 파일 구조

```
schedule-poll/
├── index.html   ← 앱 전체 (HTML + CSS + JS, 이 파일 하나가 전부!)
└── README.md    ← 이 안내 문서
```

---

Made with 💕 for teachers
