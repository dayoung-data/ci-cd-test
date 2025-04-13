
# ✅ GitHub Actions 기반 Python 프로젝트 CI 구축 전체 실습 기록

## 🎯 목표
- Git + GitHub 사용법 기초 익히기
- GitHub Actions로 자동 테스트(CI) 구성하기
- 실제로 코드 push 시 자동으로 `pytest` 실행되는 경험 쌓기

## 🪜 전체 진행 단계 요약

### 📁 1단계. 로컬에서 프로젝트 디렉토리 생성
```bash
mkdir ci-cd-test
cd ci-cd-test
```

### 🧾 2단계. Python 파일 및 테스트 파일 만들기

✅ calculator.py
```python
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b
```

✅ test_calculator.py
```python
from calculator import add, subtract

def test_add():
    assert add(2, 3) == 5

def test_subtract():
    assert subtract(5, 2) == 3
```

✅ requirements.txt
```
pytest
```

### 🐙 3단계. Git 초기화 및 커밋
```bash
git init
git add .
git commit -m "Initial commit: calculator + test + requirements"
```

### 🌐 4단계. GitHub 저장소 생성 후 연결
```bash
git remote add origin https://github.com/your-username/ci-cd-test.git
git branch -M main
git push -u origin main
```

### 🔧 5단계. GitHub Actions 워크플로우 구성
폴더 및 파일 생성:
```bash
mkdir -p .github/workflows
nano .github/workflows/python-app.yml
```

python-app.yml:
```yaml
name: Python application
on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v3
    - name: Set up Python
      uses: actions/setup-python@v4
      with:
        python-version: "3.10"
    - name: Install dependencies
      run: |
        python -m pip install --upgrade pip
        pip install -r requirements.txt
    - name: Run tests with pytest
      run: |
        pytest
```

### ✅ 6단계. 워크플로우 파일 커밋 & 푸시
```bash
git add .github/workflows/python-app.yml
git commit -m "Add CI workflow for automatic pytest"
git push
```

### ✅ 7단계. GitHub Actions 자동 실행 확인
- GitHub 저장소 → Actions 탭 → 성공 여부 확인

### 🧪 8단계. 테스트 실패 실습
```python
def test_add():
    assert add(2, 3) == 6  # 일부러 틀리게!
```
```bash
git add test_calculator.py
git commit -m "❌ intentionally break test to verify CI failure"
git push
```

## 🎉 실습 결과
- GitHub Actions로 Python 프로젝트에 대한 CI 파이프라인 구축
- 코드 push 시 `pytest` 테스트 자동 실행 확인
- 테스트 실패 → GitHub Actions에서 감지됨

## 📌 이 경험은 이렇게 정리해서 말할 수 있어
"간단한 Python 프로젝트에 대해 GitHub Actions를 활용한 CI 파이프라인을 구축했습니다.
코드 변경 시 pytest 테스트가 자동 실행되며, 실패 시 GitHub Actions를 통해 바로 확인할 수 있는 구조를 직접 구성해보았습니다."
