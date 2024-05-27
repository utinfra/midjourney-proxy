## Docker 배포 튜토리얼
1. /xxx/xxx/config 디렉토리에 생성 application.yml(mj구성항목)、banned-words.txt(선택항목，민간단어덮어쓰기)；참고 src/main/resources 아래파일
2. 컨테이너 시작，맵 구성 디렉토리
```shell
docker run -d --name midjourney-proxy \
 -p 8080:8080 \
 -v /xxx/xxx/config:/home/spring/config \
 novicezk/midjourney-proxy:2.6.2
```
3.  `http://ip:port/mj` 로 들어가서 API문서보기

첨부된 구성 디렉터리로 매핑하지 마세요.，시작 명령에서 직접 매개변수 설정
```shell
docker run -d --name midjourney-proxy \
 -p 8080:8080 \
 -e mj.discord.guild-id=xxx \
 -e mj.discord.channel-id=xxx \
 -e mj.discord.user-token=xxx \
 novicezk/midjourney-proxy:2.6.2
```
