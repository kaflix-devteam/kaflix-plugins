# kaflix-channel

Kaflix A2A 채널 플러그인 — 사내 동료 에이전트와 실시간 메시지 교환.

## 설치

```bash
# 마켓플레이스 등록 (1회)
/plugin marketplace add kaflix-devteam/kaflix-plugins

# 플러그인 설치
/plugin install kaflix-channel@kaflix-tools
```

설치 시 다음 값들을 입력하는 프롬프트가 나옵니다:

- **Channel Token** *(필수)*: `~/.config/kaflix-sidecar/env`의 `KAFLIX_CHANNEL_TOKEN` 값
- **Sidecar URL** *(선택)*: 기본 `http://localhost:9876` (변경 불필요)
- **Instance Label** *(선택, 멀티-테넌트)*: 같은 PC에서 Claude를 여러 개 띄워 협업할 때
  이 Claude 창의 식별 라벨 (예: `coder`, `reviewer`, `planner`). 비워두면 default 인스턴스로
  동작 (사이드카당 1개, 레거시 호환).

## 사용

```bash
# 채널 모드로 Claude Code 시작
claude --channels plugin:kaflix-channel@kaflix-tools
```

동료 에이전트가 A2A 메시지를 보내면 `<channel>` 태그로 알림이 도착합니다.
`reply` 도구를 호출해서 응답할 수 있습니다.

## 멀티-인스턴스 협업

같은 PC에서 Claude를 여러 개 동시에 운영하면서 서로 A2A로 통신할 수 있습니다.
각 창마다 `instance_label` 을 다르게 설정해서 설치하면 사이드카가 별도의 A2A
에이전트로 CP에 등록합니다.

예: 한 직원이 "coder"와 "reviewer" 두 인스턴스를 운영
- Claude 창 A: `instance_label = "coder"` → `{employee}-coder` 에이전트로 등록
- Claude 창 B: `instance_label = "reviewer"` → `{employee}-reviewer` 에이전트로 등록

Claude 안에서 `a2a_list_my_instances` 도구로 형제 인스턴스의 agent_url을 확인하고
`a2a_send_message(agent_url, "...")` 로 메시지를 보내면 됩니다. 메시지는 채널을 통해
대상 인스턴스의 `<channel>` 태그로 도착하고, 그 인스턴스가 `reply` 도구로 응답하면
호출자에게 task 응답으로 회수됩니다.

## 백그라운드 실행

install.sh가 launchd를 설정해서 `~/.kaflix` 워크스페이스에서 자동 백그라운드 실행합니다.
