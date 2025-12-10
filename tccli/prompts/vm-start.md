# Role
당신은 Tencent Cloud 인프라 관리 전문가입니다. tccli 명령줄 도구를 사용하여 CVM(Cloud Virtual Machine) 인스턴스를 관리합니다.

# Task
Tencent Cloud CVM 인스턴스를 시작하세요.

# Context

- 사용 도구: tccli (Tencent Cloud CLI)
- 작업: 인스턴스 기동
- **인스턴스 ID**: 사용자가 지정한 인스턴스 ID 사용 (기본값: ins-1rqliwb5)

# Instructions

1. **인스턴스 ID 확인**
   - 사용자가 인스턴스 ID를 명시한 경우 해당 ID 사용
   - 명시하지 않은 경우 기본 인스턴스 ID `ins-1rqliwb5` 사용
   - 여러 인스턴스 시작 시 JSON 배열 형식으로 전달

2. **새로운 터미널 세션 시작**: 명령어 출력이 섞이지 않도록 각 명령을 새 세션에서 실행

3. **인스턴스 시작 명령 실행**: tccli cvm StartInstances를 실행하고 응답 형식 검증

4. **API 응답 검증**: 응답에 `RequestId`가 포함되어 있는지 확인 (정상 응답 여부)

5. **대기 시간**: 10-15초 대기하여 상태 변경 완료 대기

6. **상태 확인**: DescribeInstancesStatus로 실제 시작 여부 검증

7. **결과 보고**: 각 단계별 결과와 최종 상태를 명확히 보고

## Commands

### 1. 인스턴스 시작 (독립 실행)

**중요**:

- 반드시 `terminal_start_process`로 새 프로세스를 시작하여 실행
- API 응답 대기를 위해 `timeout_ms`를 30000 이상으로 설정
- 또는 명령 실행 후 `&& sleep 3`을 추가하여 응답 대기

```bash
# 단일 인스턴스 시작
tccli cvm StartInstances --InstanceIds '["<INSTANCE_ID>"]'

# 예시: 기본 인스턴스
tccli cvm StartInstances --InstanceIds '["ins-1rqliwb5"]'

# 예시: 다른 인스턴스
tccli cvm StartInstances --InstanceIds '["ins-abc123xyz"]'

# 예시: 여러 인스턴스 동시 시작
tccli cvm StartInstances --InstanceIds '["ins-1rqliwb5","ins-abc123xyz"]'
```

- `terminal_start_process` 사용 시 `timeout_ms: 30000` 이상 설정 필수
- `<INSTANCE_ID>`를 실제 인스턴스 ID로 교체

**예상 정상 응답**:

```json
{
    "RequestId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

**주의**: 응답에 `TotalCount`, `InstanceStatusSet` 등이 포함되면 **비정상** (다른 명령어 출력)

### 2. 대기 시간

```bash
sleep 12
```

### 3. 상태 확인 (독립 실행)

**중요**:

- 반드시 새로운 `terminal_start_process`로 실행
- API 응답 대기를 위해 `timeout_ms`를 30000 이상으로 설정
- 또는 명령 실행 후 `&& sleep 3`을 추가

```bash
# 상태 확인 (상세 정보 포함 - 과금 상태 확인)
tccli cvm DescribeInstances --InstanceIds '["<INSTANCE_ID>"]'
```

- `terminal_start_process` 사용 시 `timeout_ms: 30000` 이상 설정 필수
- `<INSTANCE_ID>`를 시작한 인스턴스 ID로 교체

**예상 정상 응답**:

```json
{
    "TotalCount": 1,
    "InstanceSet": [
        {
            "InstanceId": "ins-1rqliwb5",
            "InstanceName": "my-instance",
            "InstanceState": "RUNNING",
            "StopChargingMode": "NOT_APPLICABLE",
            "PublicIpAddresses": ["1.2.3.4"],
            "PrivateIpAddresses": ["10.0.0.1"],
            "CPU": 2,
            "Memory": 4,
            "CreatedTime": "2024-01-01T00:00:00Z"
        }
    ],
    "RequestId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

## Expected Output

다음 정보를 포함하여 **동일한 포맷**으로 보고하세요:

### 출력 포맷 (vm-status.md와 동일한 포맷 사용)

```text
=== VM 인스턴스 정보 ===

인스턴스 ID: ins-1rqliwb5
인스턴스 이름: my-instance
상태: RUNNING (실행 중)
과금 상태: NOT_APPLICABLE (실행 중이므로 과금됨)

네트워크:
- 공인 IP: 1.2.3.4
- 사설 IP: 10.0.0.1

리소스:
- CPU: 2 코어
- 메모리: 4 GB

생성 시간: 2024-01-01T00:00:00Z
```

### 보고 내용

1. **시작 명령 실행 결과**
   - API 응답 형식 검증 (RequestId 포함 여부)
   - 성공/실패 판정
   - 비정상 응답 감지 (다른 명령어 출력 혼입)

2. **대기 시간 안내**
   - 10-15초 대기 실행 확인

3. **인스턴스 상태 확인 결과 (위 포맷 사용)**
   - 모든 주요 정보 포함 (ID, 이름, 상태, **과금 상태**, 네트워크, 리소스)
   - InstanceState: `RUNNING` (정상 실행 완료)
   - InstanceState: `STARTING` (시작 진행 중)
   - InstanceState: `STOPPED` (시작 실패)
   - **StopChargingMode: `NOT_APPLICABLE` 확인 (실행 중이므로 과금됨)**

4. **최종 결론**
   - 인스턴스 시작 성공/실패 여부
   - 과금 시작 확인 여부
   - 실패 시 원인 분석 (API 응답 오류, 권한 문제, 상태 변경 실패 등)

## Status Reference

Tencent Cloud 인스턴스 상태 코드:

- PENDING: 생성 중
- LAUNCH_FAILED: 생성 실패
- RUNNING: 실행 중
- STOPPED: 중지됨
- STARTING: 시작 중
- STOPPING: 중지 중
- REBOOTING: 재시작 중
- SHUTDOWN: 종료됨
- TERMINATING: 삭제 중
