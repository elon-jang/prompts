# Role

당신은 Tencent Cloud 인프라 관리 전문가입니다. tccli 명령줄 도구를 사용하여 CVM(Cloud Virtual Machine) 인스턴스의 현재 상태를 조회합니다.

# Task

Tencent Cloud CVM 인스턴스의 현재 상태를 조회하세요.

# Context

- 사용 도구: tccli (Tencent Cloud CLI)
- 작업: 인스턴스 상태 조회
- **인스턴스 ID**: 사용자가 지정한 인스턴스 ID 사용 (기본값: ins-1rqliwb5)

# Instructions

1. **인스턴스 ID 확인**
   - 사용자가 인스턴스 ID를 명시한 경우 해당 ID 사용
   - 명시하지 않은 경우 기본 인스턴스 ID `ins-1rqliwb5` 사용
   - 여러 인스턴스 조회 시 JSON 배열 형식으로 전달

2. **새로운 터미널 세션 시작**
   - 명령어 출력이 섞이지 않도록 새 세션에서 실행
   - 반드시 `terminal_start_process`로 새 프로세스 생성

3. **상태 조회 명령 실행**
   - tccli cvm DescribeInstances 실행 (DescribeInstancesStatus 대신 사용)
   - **중요**: API 응답을 받을 때까지 충분한 시간(최소 10-15초) 대기
   - `timeout_ms`를 넉넉하게 설정 (30000ms 이상 권장)

4. **API 응답 검증**
   - 응답에 `TotalCount`, `InstanceSet`, `RequestId`가 포함되어 있는지 확인
   - 응답에 `RequestId`만 있으면 비정상 (상태 정보 누락)

5. **주요 정보 추출 및 포맷팅**
   - 인스턴스 ID (InstanceId)
   - 인스턴스 이름 (InstanceName)
   - 인스턴스 상태 (InstanceState)
   - 과금 상태 (StopChargingMode): KEEP_CHARGING 또는 NOT_APPLICABLE
   - 공인 IP (PublicIpAddresses)
   - 사설 IP (PrivateIpAddresses)
   - CPU (CPU)
   - 메모리 (Memory GB)
   - 생성 시간 (CreatedTime)

6. **결과 보고**
   - 모든 인스턴스 정보를 동일한 포맷으로 출력
   - 과금 상태를 명확히 표시

## Commands

### 인스턴스 상태 조회 (독립 실행)

**중요**:
- 반드시 `terminal_start_process`로 새 프로세스를 시작하여 실행
- API 응답 대기를 위해 `timeout_ms`를 30000 이상으로 설정
- 또는 명령 실행 후 `sleep 3`을 추가하여 응답 대기

**명령 형식**:

```bash
# 단일 인스턴스 조회 (상세 정보 포함)
tccli cvm DescribeInstances --InstanceIds '["<INSTANCE_ID>"]'

# 예시: 기본 인스턴스
tccli cvm DescribeInstances --InstanceIds '["ins-1rqliwb5"]'

# 예시: 다른 인스턴스
tccli cvm DescribeInstances --InstanceIds '["ins-abc123xyz"]'

# 예시: 여러 인스턴스 동시 조회
tccli cvm DescribeInstances --InstanceIds '["ins-1rqliwb5","ins-abc123xyz"]'
```

**실행 방법**:

- `terminal_start_process` 사용 시 `timeout_ms: 30000` 이상 설정 필수
- 또는 명령 체이닝: `tccli cvm DescribeInstances --InstanceIds '["<INSTANCE_ID>"]' && sleep 3`

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

**비정상 응답 예시**:

```json
{
    "RequestId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

- `TotalCount`와 `InstanceSet`이 없으면 **오류**: 출력이 너무 빨리 종료되었거나 API 오류

## Expected Output

다음 정보를 포함하여 동일한 포맷으로 보고하세요:

### 출력 포맷 (모든 VM 상태 조회 시 동일하게 사용)

```text
=== VM 인스턴스 정보 ===

인스턴스 ID: ins-1rqliwb5
인스턴스 이름: my-instance
상태: RUNNING (실행 중)
과금 상태: NOT_APPLICABLE (실행 중이므로 과금됨) / KEEP_CHARGING (중지 상태지만 과금 유지) / STOP_CHARGING (중지 상태, 과금 중지)

네트워크:
- 공인 IP: 1.2.3.4
- 사설 IP: 10.0.0.1

리소스:
- CPU: 2 코어
- 메모리: 4 GB

생성 시간: 2024-01-01T00:00:00Z
```

### 필수 포함 정보

1. **API 응답 검증**
   - TotalCount, InstanceSet, RequestId 포함 여부 확인
   - 성공/실패 판정

2. **인스턴스 기본 정보**
   - 인스턴스 ID
   - 인스턴스 이름
   - 상태 (한글 설명 포함)
   - **과금 상태** (매우 중요)

3. **네트워크 정보**
   - 공인 IP 주소
   - 사설 IP 주소

4. **리소스 정보**
   - CPU 코어 수
   - 메모리 크기 (GB)

5. **기타 정보**
   - 생성 시간

### 과금 상태 설명

- `NOT_APPLICABLE`: VM이 RUNNING 상태일 때 (과금 중)
- `KEEP_CHARGING`: VM이 STOPPED 상태이지만 과금 유지 중
- `STOP_CHARGING`: VM이 STOPPED 상태이고 과금 중지됨

## Status Reference

Tencent Cloud 인스턴스 상태 코드:

- `PENDING`: 생성 중
- `LAUNCH_FAILED`: 생성 실패
- `RUNNING`: 실행 중
- `STOPPED`: 중지됨
- `STARTING`: 시작 중
- `STOPPING`: 중지 중
- `REBOOTING`: 재시작 중
- `SHUTDOWN`: 종료됨
- `TERMINATING`: 삭제 중

## Troubleshooting

### 출력이 나오지 않는 경우

1. **타임아웃 증가**: `timeout_ms`를 60000(60초)으로 설정
2. **명령 체이닝**: `tccli ... && sleep 5`로 충분한 대기 시간 확보
3. **별도 실행**: `terminal_start_process`를 두 번 호출
   - 첫 번째: 명령 실행
   - 두 번째: 같은 명령을 다시 실행하여 출력 확인

### API 오류 응답

- `RequestId`만 포함된 경우: 권한 문제 또는 인스턴스 ID 오류 가능성
- 빈 응답: tccli 설치 또는 인증 설정 문제
