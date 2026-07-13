# 📋 전체 프로젝트 검증 워크플로우 - 단계별 가이드

## 상황: 전체 프로젝트를 검증하고 싶어요

이미 agents-main이 설치되어 있고, 모든 플러그인이 제대로 작동하는지 확인하고 싶은 경우입니다.

---

## 🎯 검증 워크플로우 (4단계)

### **STEP 1: 현재 상태 확인**
```bash
python orchestrate.py status
```

**출력 예상**:
```
============================================================
  PROJECT STATUS
============================================================

✓ Codex artifacts: 156 items
✗ Cursor artifacts: NOT GENERATED
✗ OpenCode artifacts: NOT GENERATED
✓ Gemini skills (root): 162 items
✓ Gemini agents (root): 199 items
✓ Gemini commands (root): 106 items
✓ Plugins: 92 found
```

**해석**:
- 생성된 아티팩트가 있는지 없는지 확인
- 플러그인 개수 (92개)가 맞는지 확인

---

### **STEP 2: 의존성 설치 (처음 1회만)**
```bash
make install
```

또는:
```bash
python orchestrate.py setup
```

**출력 예상**:
```
uv sync --project plugins/plugin-eval
Resolved 28 packages in 450ms
Installed 28 packages in 1.2s
✓ Setup complete!
```

**이것은 한 번만**: Python 의존성 설치 (uv, pyyaml, 테스트 도구)

---

### **STEP 3: 아티팩트 생성 (모든 하네스)**

#### 옵션 A: 전체 생성 (권장)
```bash
make generate-all
```

또는:
```bash
python orchestrate.py generate
```

**소요 시간**: 약 2-3분

**출력 예상**:
```
--- codex ---
[✓] Generated .codex/skills, .codex/agents, .agents/plugins/marketplace.json

--- cursor ---
[✓] Generated .cursor-plugin/plugin.json, .cursor-plugin/marketplace.json

--- opencode ---
[✓] Generated .opencode/skills/, .opencode/agents/, .opencode/commands/

--- gemini ---
[✓] Generated skills/, agents/, commands/ at root

--- copilot ---
[✓] Generated .agents/, .copilot/
```

#### 옵션 B: 특정 하네스만 생성
```bash
# Cursor만 생성
make generate HARNESS=cursor --all
python orchestrate.py generate --harness cursor

# Gemini만 생성
make generate HARNESS=gemini --all
python orchestrate.py generate --harness gemini
```

---

### **STEP 4: 구조 검증**

#### 4-1. 엄격한 구조 검증 (필수)
```bash
make validate STRICT=1
```

또는:
```bash
python orchestrate.py validate --strict
```

**출력 예상**:
```
Validating all harnesses...
✓ Codex: 88 plugins validated
✓ Cursor: 88 plugins validated
✓ OpenCode: 88 plugins validated
✓ Gemini: 88 plugins validated
✓ Copilot: 88 plugins validated

✓ All validations passed!
```

**검사 내용**:
- 스키마 정확성
- 모든 참조 링크 검증
- 파일 크기 제한 (Codex: 8KB)
- 포터블 성 확인

#### 4-2. 드리프트 감지 (중요!)
```bash
make garden
```

또는:
```bash
python orchestrate.py garden
```

**출력 예상**:
```
Checking for drift (dead links, oversize skills, stale artifacts)...
✓ No dead links found
✓ No oversize skills detected
✓ All artifacts are current

✓ No drift detected!
```

**검사 내용**:
- 데드 링크 (문서에서 존재하지 않는 파일 참조)
- 오버사이즈 스킬 (>8KB)
- 생성된 아티팩트 vs 소스 불일치

---

### **STEP 5: 테스트**

#### 5-1. 단위 테스트
```bash
make test
```

또는:
```bash
python orchestrate.py test
```

**출력 예상**:
```
Collecting tests...
plugins/plugin-eval/tests/ ... 45 tests
tools/tests/ ... 28 tests
Total: 73 tests

Running pytest...
✓ 73 passed in 2.4s
```

#### 5-2. 실제 CLI 스모크 테스트 (선택)
```bash
make smoke-test
```

**이것은**: Codex, Cursor, OpenCode, Gemini 실제 CLI 호출 테스트

---

## 📊 완전한 검증 워크플로우 (한 번에)

```bash
# Option 1: Makefile
make generate-all && \
make validate STRICT=1 && \
make garden && \
make test

# Option 2: Python CLI (권장)
python orchestrate.py build
```

**시간**: ~5-10분

**출력**:
```
============================================================
  FULL BUILD WORKFLOW
============================================================

Step 1/5: Setup
  [1/1] Installing uv dependencies...
  ✓ Setup complete!

Step 2/5: Generate All
  [1/5] Generating codex artifacts
  [2/5] Generating cursor artifacts
  [3/5] Generating opencode artifacts
  [4/5] Generating gemini artifacts
  [5/5] Generating copilot artifacts
  ✓ Generated 5 harness(es)

Step 3/5: Validate (Strict)
  [1/1] Running structural validation
  ✓ Validation passed!

Step 4/5: Garden (Drift)
  [1/1] Checking for drift...
  ✓ No drift detected!

Step 5/5: Test
  [1/2] Running unit tests (pytest)
  ✓ 73 passed
  [2/2] Running smoke tests (real CLIs)
  ✓ Smoke tests complete!

============================================================
  BUILD COMPLETE
============================================================
✓ All steps passed!

Next steps:
  • Commit and push changes
  • Wait for CI validation (.github/workflows/validate.yml)
  • Optionally deploy: python orchestrate.py deploy
```

---

## 🔍 각 단계에서 뭐가 검증되나?

| 단계 | 검사 내용 | 시간 | 명령어 |
|------|---------|------|--------|
| **Setup** | Python 의존성 설치 | 30s | `make install` |
| **Generate** | 5개 하네스 아티팩트 생성 | 3min | `make generate-all` |
| **Validate** | 스키마, 크기, 참조 검증 | 1min | `make validate STRICT=1` |
| **Garden** | 데드 링크, 드리프트 감지 | 1min | `make garden` |
| **Test** | 73개 단위 테스트 + CLI 테스트 | 2min | `make test` |

---

## 🎓 검증 실패 시 해결법

### 1. "Validation failed"
```bash
# 어디가 실패했는지 확인
make validate STRICT=1

# 특정 하네스만 검증
make validate HARNESS=cursor STRICT=1

# 생성된 파일 문제면 재생성
make clean-generated HARNESS=cursor
make generate HARNESS=cursor --all
```

### 2. "Drift detected"
```bash
# 구체적으로 뭐가 드리프트되었는지 확인
make garden --strict

# 보통 해결법: 소스 편집 후 재생성
make generate-all
```

### 3. "Tests failed"
```bash
# 어떤 테스트가 실패했는지 보기
make test 2>&1 | tail -50

# 특정 테스트 파일만 실행
uv run -p plugins/plugin-eval pytest plugins/plugin-eval/tests/test_parser.py -v
```

---

## ✅ 검증 완료 체크리스트

- [ ] `python orchestrate.py status` → 92개 플러그인 확인
- [ ] `make install` → 완료 (Python 의존성)
- [ ] `make generate-all` → 5개 하네스 생성 완료
- [ ] `make validate STRICT=1` → ✓ All validations passed
- [ ] `make garden` → ✓ No drift detected
- [ ] `make test` → ✓ 73 tests passed
- [ ] 커밋 준비 완료

---

## 💡 팁

**빠른 검증** (생성 skip):
```bash
make validate STRICT=1 && make garden && make test
```

**특정 플러그인만** 검증:
```bash
python tools/validate_generated.py --strict  # 모든 생성 파일
make generate HARNESS=cursor PLUGIN=python-development  # 특정 플러그인만
```

**매번 실행할 필요 있는 것**:
- Validate (변경 감지)
- Garden (드리프트 감지)
- Test (회귀 테스트)

**처음 1회만**:
- Setup (의존성)

---

**이제 준비 되셨나요? 실제로 한 번 해보시겠어요?**
