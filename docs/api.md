# API인터페이스 설명서

`http://ip:port/mj` 이미 API문서가 있으므로, 여기서 보충자료만 제공함

## 1. 데이터구조

### 작업
| Field | Type | 예시 | Description |
|:-----:|:----:|:----|:----|
| id | string | 1689231405853400 | 작업ID |
| action | string | IMAGINE | 과제유형: IMAGINE（绘图）、UPSCALE（选中放大）、VARIATION（选中变换）、REROLL（重新执行）、DESCRIBE（图生文）、BLEAND（图片混合） |
| status | string | SUCCESS | 과제상태: NOT_START（未启动）、SUBMITTED（已提交处理）、IN_PROGRESS（执行中）、FAILURE（失败）、SUCCESS（成功） |
| prompt | string | 고양이 | 힌트,단서 |
| promptEn | string | Cat | 영어힌트,영어단서 |
| description | string | /imagine 고양이 | 과제설명 |
| submitTime | number | 1689231405854 | 제출시간 |
| startTime | number | 1689231442755 | 시작시간 |
| finishTime | number | 1689231544312 | 종료시간 |
| progress | string | 100% | 진행률 |
| imageUrl | string | https://cdn.discordapp.com/attachments/xxx/xxx/xxxx.png | 生成图片的url, 成功或执行中时有值，可能为png或webp |
| failReason | string | [Invalid parameter] Invalid value | 失败原因, 失败时有值 |
| properties | object | {"finalPrompt": "Cat"} | 任务的扩展属性，系统内部使用 |


## 2. 작업 결과
- code=1: 提交成功，result为任务ID
    ```json
    {
      "code": 1,
      "description": "成功",
      "result": "8498455807619990",
      "properties": {
          "discordInstanceId": "1118138338562560102"
      }
    }
    ```
- code=21: 任务已存在，U时可能发生
    ```json
    {
        "code": 21,
        "description": "任务已存在",
        "result": "0741798445574458",
        "properties": {
            "status": "SUCCESS",
            "imageUrl": "https://xxxx"
         }
    }
    ```
- code=22: 提交成功，进入队列等待
    ```json
    {
        "code": 22,
        "description": "排队中，前面还有1个任务",
        "result": "0741798445574458",
        "properties": {
            "numberOfQueues": 1,
            "discordInstanceId": "1118138338562560102"
         }
    }
    ```
- code=23: 队列已满，请稍后尝试
    ```json
    {
        "code": 23,
        "description": "队列已满，请稍后尝试",
        "result": "14001929738841620",
        "properties": {
            "discordInstanceId": "1118138338562560102"
         }
    }
    ```
- code=24: prompt包含敏感词
    ```json
    {
        "code": 24,
        "description": "可能包含敏感词",
        "properties": {
            "promptEn": "nude body",
            "bannedWord": "nude"
         }
    }
    ```
- other: 提交错误，description为错误描述

## 3. `/mj/submit/simple-change` 绘图变化-simple
接口作用同 `/mj/submit/change`(绘图变化)，传参方式不同，该接口接收content，格式为`ID 操作`，例如：1320098173412546 U2

- 放大 U1～U4
- 变换 V1～V4
- 重新执行 R

## 4. `/mj/submit/describe` 图生文
```json
{
    // 图片的base64字符串
    "base64": "data:image/png;base64,xxx"
}
```

后续任务完成后，properties中finalPrompt即为图片生成的prompt
```json
{
  "id":"14001929738841620",
  "action":"DESCRIBE",
  "status": "SUCCESS",
  "description":"/describe 14001929738841620.png",
  "imageUrl":"https://cdn.discordapp.com/attachments/xxx/xxx/14001929738841620.png",
  "properties": {
    "finalPrompt": "1️⃣ Cat --ar 5:4\n\n2️⃣ Cat2 --ar 5:4\n\n3️⃣ Cat3 --ar 5:4\n\n4️⃣ Cat4 --ar 5:4"
  }
  // ...
}
```

## 5. 작업 변경 콜백
작업의 상태나 진행률이 변경될 때 비즈니스 시스템의 API를 호출
-  "mj.notify-hook"으로 구성된 설정을 통해 인터페이스 주소를 지정하고, 작업 제출 시 "notifyHook"을 전달하여 해당 작업의 콜백 주소를 변경됨.
- 두 가지 경우 모두 콜백이 비어있을 때 콜백을 트리거하지 않음.

POST  application/json
```json
{
  "id": "14001929738841620",
  "action": "IMAGINE",
  "status": "SUCCESS",
  "prompt": "猫猫",
  "promptEn": "Cat",
  "description": "/imagine 猫猫",
  "submitTime": 1689231405854,
  "startTime": 1689231442755,
  "finishTime": 1689231544312,
  "progress": "100%",
  "imageUrl": "https://cdn.discordapp.com/attachments/xxx/xxx/xxxx.png",
  "failReason": null,
  "properties": {
    "finalPrompt": "Cat"
  }
}
```
