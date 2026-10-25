# cron-scheduler-stg

서비스의 정기 처리 주소를 일정한 간격으로 호출하는 GitHub Actions 워크플로 저장소입니다.

## 파일
- `.github/workflows/stg-cron.yml` — 호출 워크플로. 정해진 주기(`schedule`)와 수동 실행(`workflow_dispatch`)을 지원합니다.

## 설정(Secrets)
호출 주소와 인증 값은 저장소 Secrets 에만 둡니다. 값은 코드·문서·로그에 기록하지 않습니다.

| 이름 | 내용 |
|---|---|
| `STG_BASE_URL` | 서비스 주소(끝에 `/` 없이) |
| `CRON_SECRET` | 호출 인증 값 |

## 호출 경로와 주기
| 경로 | 주기 |
|---|---|
| `send-notifications` | 5분 |
| `reconcile-payments`, `release-suspensions` | 10분 |
| `account-suspend-retry` | 15분 |
| `process-account-deletions` | 하루 1회(UTC 04:10) |

- 각 호출은 `Authorization: Bearer <인증 값>` 헤더로 인증하고, 실행 로그에는 경로 이름과 HTTP 상태 코드만 남깁니다. 응답이 200 이 아니면 실행이 실패로 표시됩니다.
- GitHub 스케줄은 최소 5분 간격이며 부하에 따라 지연되거나 건너뛰는 경우가 있습니다.

## 수동 실행
Actions → stg-cron → Run workflow 에서 경로를 골라 한 번 실행할 수 있습니다.

## 실패 알림
스케줄 실행이 실패하면 GitHub 이 저장소 소유자에게 이메일을 보냅니다. GitHub → Settings → Notifications → Actions 에서 실패 알림이 켜져 있는지 확인하세요.
