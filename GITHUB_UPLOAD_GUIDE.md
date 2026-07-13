# GitHub에 파일 업로드하는 가이드

> Git 설치 문제로 인해 GitHub 웹 UI를 통한 수동 업로드 방법을 제공합니다.

---

## 📋 업로드할 파일 목록

| 파일명 | 경로 | 크기 | 설명 |
|--------|------|------|------|
| **ORCHESTRATION.md** | `agents-main/ORCHESTRATION.md` | 8.8KB | 폴더 오케스트레이션 전체 가이드 |
| **orchestrate.py** | `agents-main/orchestrate.py` | 13KB | 인터랙티브 CLI 도구 (Python) |
| **QUICK_REFERENCE.md** | `agents-main/QUICK_REFERENCE.md` | 3KB | 빠른 참조 카드 |

---

## 🚀 업로드 방법 (GitHub 웹 UI)

### Step 1: GitHub 저장소로 이동
```
https://github.com/wshobson/agents  (또는 본인 fork/저장소)
```

### Step 2: agents-main 폴더로 이동
```
Click: agents-main/ → 폴더 진입
```

### Step 3: 파일 업로드
```
1. "Add file" 버튼 클릭
   └─ "Upload files" 선택

2. 세 파일을 드래그-앤-드롭 또는 선택:
   - ORCHESTRATION.md
   - orchestrate.py
   - QUICK_REFERENCE.md

3. "Commit changes" 클릭
```

### Step 4: Commit 메시지 작성
```
Add: Multi-harness orchestration tools and guides

- ORCHESTRATION.md: Folder structure, workflow phases, and core tasks
- orchestrate.py: Interactive CLI for build, validate, test, deploy
- QUICK_REFERENCE.md: Quick reference card for common commands

These tools provide end-to-end orchestration for 92 plugins across 5 harnesses
(Claude Code, Codex, Cursor, OpenCode, Gemini).

Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```

### Step 5: 확인
```
"Commit directly to main branch" 선택 (또는 본인 브랜치)
→ "Commit changes" 클릭
```

---

## 📝 파일 내용 (복사용)

### 파일 1: ORCHESTRATION.md
**경로**: `C:\Users\user\dmdkr\agents-main\agents-main\ORCHESTRATION.md`

```
[파일 내용은 아래 "파일 내용" 섹션 참조]
```

### 파일 2: orchestrate.py
**경로**: `C:\Users\user\dmdkr\agents-main\agents-main\orchestrate.py`

```
[파일 내용은 아래 "파일 내용" 섹션 참조]
```

### 파일 3: QUICK_REFERENCE.md
**경로**: `C:\Users\user\dmdkr\agents-main\agents-main\QUICK_REFERENCE.md`

```
[파일 내용은 아래 "파일 내용" 섹션 참조]
```

---

## 🔑 GitHub CLI 대안 (Git 없이)

GitHub 웹에서 직접:

1. **저장소 열기**: https://github.com/wshobson/agents
2. **`.` 키 누르기** → GitHub Codespaces 에디터 열림
3. **파일 생성**:
   ```
   Ctrl+N → ORCHESTRATION.md
   Ctrl+N → orchestrate.py
   Ctrl+N → QUICK_REFERENCE.md
   ```
4. **각 파일에 내용 붙여넣기**
5. **Source Control** (왼쪽) → Commit & Push

---

## ✅ 업로드 완료 후 확인

```bash
# GitHub 웹에서 확인
https://github.com/wshobson/agents/tree/main/agents-main

# 또는 로컬에서 (Git 설치 후)
git clone https://github.com/wshobson/agents
cd agents/agents-main
ls -la | grep -E "ORCHESTRATION|orchestrate|QUICK_REFERENCE"
```

---

## 💡 팁

- **마크다운 파일** (.md): GitHub 웹에서 직접 편집 가능
- **Python 파일** (.py): 코드 하이라이트 자동 적용
- **Commit 메시지**: 명확하고 설명적으로 작성
- **Co-authored-by**: Copilot 크레딧 포함

---

## 🆘 문제 해결

**Q: 파일 크기가 너무 크다?**
A: 모든 파일이 20KB 미만이므로 문제 없음

**Q: 어느 브랜치에 올려야 하나?**
A: main 또는 feature 브랜치 (PR 사용)

**Q: 여러 파일을 한 번에 올릴 수 있나?**
A: Yes! "Upload files" → 드래그-앤-드롭으로 여러 파일 동시 업로드

---

## 📄 파일 내용

아래는 각 파일의 전체 내용입니다.

### ORCHESTRATION.md
(8,773 bytes)

### orchestrate.py
(13,031 bytes)

### QUICK_REFERENCE.md
(3,023 bytes)
