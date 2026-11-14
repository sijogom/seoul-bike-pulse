# SEOUL BIKE PULSE

서울 따릉이 대여량 예측과 AWS 기반 학습·평가·모델 등록·서빙을 위한 프로젝트입니다. Python FastAPI 백엔드, React·Vite 프론트엔드, ECS Fargate·Step Functions 작업 정의를 포함합니다.

## 주요 기능

- 데이터 변환·품질 검사·EDA·특성 생성·모델 학습·평가·패키징·등록 작업
- DynamoDB 모델 레지스트리와 S3 패키지 무결성을 검증한 뒤 예측 모델 로딩
- `/api/v1/health`, `/api/v1/model`, `/api/v1/model/schema`, `/api/v1/predict`
- React 웹 화면에서 API 연결 상태와 모델 로딩 상태 확인

예측 API는 모델 스키마에 정의된 **19개 입력 특성**을 사용합니다. 요청을 구성할 때 `/api/v1/model/schema`에서 필수 필드와 데이터 형식을 확인합니다.

## 구성

- `backend/`: API, 모델 무결성 검사, 테스트
- [frontend](frontend/README.md): React·TypeScript·Vite
- `mlops/`: 데이터 처리부터 모델 등록까지의 작업
- `infrastructure/`: AWS 정책·태스크·워크플로 설정과 생성 스크립트
- `config/`: 데이터·특성 계약
- `sql/athena/`: 데이터 검증·분석 쿼리

## 실행 준비

자신의 AWS 환경에 모델 패키지와 레지스트리를 준비하고 `MODEL_ID`, `MODEL_VERSION`, `REGISTRY_TABLE`, `BIKEPULSE_BUCKET`, `AWS_REGION`을 설정해야 합니다. AWS 자격 증명은 표준 자격 증명 공급 체인이나 실행 역할로 제공합니다.

학습 패키지의 Python·라이브러리 버전과 서빙 버전이 일치해야 합니다. `backend/requirements-serving.txt`와 모델 메타데이터를 확인한 전용 환경에서 저장소 루트 기준으로 실행합니다.

```powershell
cd backend
python -m uvicorn app.main:app --host 127.0.0.1 --port 8000
```

백엔드 테스트는 `backend/requirements-dev.txt`와 해당 런타임 의존성을 설치한 뒤 `backend/`에서 `python -m pytest -q`로 실행합니다. 

## AWS 설정 템플릿

인프라 JSON의 `AWS_ACCOUNT_ID`, `BIKEPULSE_BUCKET`, `SUBNET_ID`, `SECURITY_GROUP_ID`는 배포 전 자신의 값으로 바꿔야 하는 자리표시자입니다. 그대로 AWS에 제출하지 않습니다. 기존 생성 스크립트는 환경변수로 설정을 받지만 생성 대상이 서로 다르므로 출력 파일을 검토해야 합니다.

UCI Seoul Bike Sharing Demand 데이터와 학습 모델은 별도로 확보합니다. AWS 자격 증명은 소스에 기록하지 않고 실행 역할 또는 로컬 자격 증명 설정으로 관리합니다.
