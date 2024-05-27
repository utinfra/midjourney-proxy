## 설정 항목
| 변수 이름                     | NOT NULL | 설명                                    |
|:------------------------------|:--:|:----------------------------------------------|
| mj.accounts                   | YES | 계정 풀 설정(./config.md#%E8%B4%A6%E5%8F%B7%E6%B1%A0%E9%85%8D%E7%BD%AE%E5%8F%82%E8%80%83)，설정 후 추가 설정이 필요 없습니다 mj.discord |
| mj.discord.guild-id           | YES  | discord서버ID                                  |
| mj.discord.channel-id         | YES  | discord채널ID                                   |
| mj.discord.user-token         | YES  | discord사용자Token                                |
| mj.discord.user-agent         | NO  | discord호출인터페이스、wss연결 시 user-agent，network복사(브라우저에서 권장) |
| mj.discord.core-size          | NO  | (코어)병렬 수，Default: 3                                 |
| mj.discord.queue-size         | NO  | 대기열, default: 10                                   |
| mj.discord.timeout-minutes    | NO  | 작업 제한 시간, Default: 5분                                 |
| mj.api-secret                 | NO  | 인터페이스 키가 비어있으면 인증이 비활성화됩니다. 인터페이스를 호출할 때 요청 헤더를 추가해야 합니다. mj-api-secret        |
| mj.notify-hook                | NO  | 전역 작업 상태 변경 콜백 주소                                 |
| mj.notify-notify-pool-size    | NO  | 알림 콜백 스레드 풀 크기, 기본값은 10                             |
| mj.task-store.type            | NO  | 작업 저장 방식, 기본값은 in_memory(메모리/재시작 후 손실)，선택사항: redis          |
| mj.task-store.timeout         | NO  | 작업 만료 시간, 만료 후 삭제, 기본값은 30일                            |
| mj.proxy.host                 | NO  | 프록시 호스트, 전역 프록시가 작동하지 않을 때 설정됩니다                             |
| mj.proxy.port                 | NO  | 프록시 포트, 전역 프록시가 작동하지 않을 때 설정됩니다                             |
| mj.ng-discord.server          | NO  | https://discord.com 리버스 프록시 주소             |
| mj.ng-discord.cdn             | NO  | https://cdn.discordapp.com 리버스 프록시 주소   |
| mj.ng-discord.wss             | NO  | wss://gateway.discord.gg 리버스 프록시 주소                |
| mj.ng-discord.resume-wss      | NO  | wss://gateway-us-east1-b.discord.gg 리버스 프록시 주소               |
| mj.ng-discord.upload-server   | NO  | https://discord-attachments-uploads-prd.storage.googleapis.com 리버스 프록시 주소               |
| mj.translate-way              | NO  | 중국어 프롬프트를 영어로 번역하는 방식, 선택 가능한 값은 null(기본값), baidu, gpt입니다.        |
| mj.baidu-translate.appid      | NO  | Baidu translation appid                                    |
| mj.baidu-translate.app-secret | NO  | Baidu translation app-secret                               |
| mj.openai.gpt-api-url         | NO  | 사용자 정의 GPT 인터페이스 주소, 기본값으로 구성할 필요가 없습니다      |
| mj.openai.gpt-api-key         | NO  | gpt의 API 키                                 |
| mj.openai.timeout             | NO  | OpenAI 호출의 시간 초과 기본값은 30초입니다          |
| mj.openai.model               | NO  | OpenAI 모델의 기본값은 gpt-3.5-turbo입니다          |
| mj.openai.max-tokens          | NO  | 결과의 최대 토큰 수, 기본값은 2048입니다.                             |
| mj.openai.temperature         | NO  | 유사도(0-2.0), 기본값은 0입니다.                     |
| spring.redis                  | NO  | 작업 저장 방식을 redis로 설정하려면 관련된 redis 속성을 구성해야 합니다                  |

### 계정 풀 구성 참조
```yaml
mj:
  accounts:
    - guild-id: xxx
      channel-id: xxx
      user-token: xxxx
      user-agent: xxxx
    - guild-id: xxx
      channel-id: xxx
      user-token: xxxx
      user-agent: xxxx
```

계정 필드 설명

| 변수 이름         | NOT NULL | 설명                                                                  |
|:------------------| :----: |:--------------------------------------------------------------------|
| guild-id          | YES | discord服务器ID                                                        |
| channel-id        | YES | discord频道ID                                                         |
| user-token        | YES | discord用户Token                                                      |
| user-agent        | NO | 调用discord接口、连接wss时的user-agent，建议从浏览器network复制                       |
| enable            | NO | 是否可用，默认true                                                         |
| core-size         | NO | 并发数，默认3                                                             |
| queue-size        | NO | 等待队列长度，默认10                                                         |
| timeout-minutes   | NO | 任务超时时间(分钟)，默认5                                                      |

### spring.redis 설정 참조
```yaml
spring:
  redis:
    host: 10.107.xxx.xxx
    port: 6379
    password: xxx
```
