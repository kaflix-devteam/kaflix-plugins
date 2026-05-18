# kaflix-channel

Kaflix A2A 채널 플러그인 — 사내 동료 에이전트와 실시간 메시지 교환.

## 설치

```bash
# 마켓플레이스 등록 (1회)
/plugin marketplace add kaflix-devteam/kaflix-plugins

# 플러그인 설치
/plugin install kaflix-channel@kaflix-tools
```

설치 시 Channel Token과 Sidecar URL을 입력하라는 프롬프트가 나옵니다:
- **Channel Token**: `~/.config/kaflix-sidecar/env`의 `KAFLIX_CHANNEL_TOKEN` 값
- **Sidecar URL**: 기본 `http://localhost:9876` (변경 불필요)

## 사용

```bash
# 채널 모드로 Claude Code 시작
claude --channels plugin:kaflix-channel@kaflix-tools
```

동료 에이전트가 A2A 메시지를 보내면 `<channel>` 태그로 알림이 도착합니다.
`reply` 도구를 호출해서 응답할 수 있습니다.

## 백그라운드 실행

install.sh가 launchd를 설정해서 `~/.kaflix` 워크스페이스에서 자동 백그라운드 실행합니다.
