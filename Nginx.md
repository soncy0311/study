# Flask - uWSGI - Nginx 연동 on CentOS 8, python3.7

## uWSGI 다운
- epel-release 패키지 설치 => 최신 버전의 패키지를 제공하는 저장소
```bash
yum -y install epel-release
```

- pip update
```
python3 -m pip3 install -U pip
```

- 가상환경 생성 및 활성화
```bash
python3 -m venv <venv_name>
source <venv_name>/bin/activate
```

- uWSGI 설치
```
yum install -y install gcc
pip3 install uwsgi
```

## .ini 파일 만들기
- 실행 폴더로 이동 해서 [name].ini 생성
```
[uwsgi]
module = main:app               >> 실행시킬 파일 이름 : 실행시킬 flask이름

master = true
processes = 5

socket = ./[name].sock          >> socket 경로 및 name 설정
chmod-socket = 660              >> socket 접근권한 root/group/personal

vacuum = true

daemonize = /tmp/uwsgi.log      >> error log file 경로지정

die-on-term = true

py-autoreload = 1               >> python code 수정시 바로 반영
```
- ini 파일 실행
```
uwsgi --ini [name].ini
```

## downloads nginx
- yum 외부 저장소 추가
```
cd etc/yum.repos.d
vim nginx.repo
# 내용
[nginx]
name=nginx repo
daseurl=http://nginx.org/packages/centos/7/$basearch/
gpgcheck=0
enabled=1
```
- yum install nginx
```
sudo yum update
yum insatll -y nginx
```
- nginx 설정
```
cd /etc/nginx/sites-available
sudo vim [name]
# 내용
server {
    listen 80 ;                 >> 80 port로 열기
    server_name [domain주소];   >> domain_address 등록

    location / {
        include uwsgi_params;
        uwsgi_pass unix:[sock파일경로]; >> 켜질 sock파일 경로 등록

    }
}
sudo ln -s /etc/nginx/sites-available/nginx/[name] /etc/nginx/sites-enabled >> symbolic link를 설정하여 사이트 활성화
sudo rm /etc/nginx/sites-enabled/default    >> 기본구성 삭제
sudo service nginx restart                  >> nginx restart
```