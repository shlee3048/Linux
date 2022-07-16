# systemd 실습
1. 실습용 스크립트 파일 작성 (10초마다 시스템 상황을 uptime.log파일에 저장)
* sudo vi /usr/local/bin/uptime-logger
```
#!/bin/sh
while :
do
    echo $(/bin/date) - $(/usr/bin/uptime)  >> /tmp/uptime.log
    sleep 10
done
```
<br/>

2. systemd 서비스 생성 -> service 타입의 Unit파일 생성
* sudo vim /etc/systemd/system/uptime-logger.service
```
[Unit]
Description=systemd practice.

[Service]
ExecStart=/bin/sh /usr/local/bin/uptime-logger

[Install]
WantedBy=multi-user.target
```
<br/>

3. systemd 데몬 리로드 (systemd 목록 수정시 항상 실행)
```
systemctl daemon-reload
```
<br/>

4. uptime-logger.service 시작
```
sudo systemctl start uptime-logger.service
```
<br/>

5. 시작 서비스에 my_uptime.service 등록
```
sudo systemctl enable uptime-logger.service
```
<br/>

6. 시스템 재시작
```
sudo reboot
```
<br/>

7. 서비스 상태 확인
```
sudo systemctl status uptime-logger.service
```
<br/>

8. 서비스 제거
- 설치 역순으로 진행
```
sudo systemctl disable uptime-logger.service
sudo systemctl stop uptime-logger.service
sudo rm /etc/systemd/system/uptime-logger.service
sudo rm /tmp/uptime.log
sudo systemctl daemon-reload
```
