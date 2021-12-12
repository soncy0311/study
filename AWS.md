## add user and Authentication by Password

#### 사용자 추가 및 각자의 secret_key 부여
- 계정 생성 및 password 설정
```bash
sudo adduser user_name
sudo passwd user_name
# 비밀번호 없이 생성 : sudo adduser <user_name> -disabled-password
```
- 새로운 user 홈 디렉토리에 .ssh 폴더 생성 후 권한 변경
```bash
sudo mkdir /home/user_name/.ssh
chmod 700 .ssh
```
- authorized_key 생성 후 권한 변경
```bash
touch .ssh/authorized_keys
chmod 600 .ssh/authorized_keys
```
- ssh-keygen 명령으로 새로운 key-pair 생성
```bash
ssh-keygen
# 후 enter,enter
```
- 생성된 id_rsa.pub(public_key) 값을 authorized_keys에 복사
```bash
cat .ssh/id_rsa_pub # 복사
vi authorized_keys # 붙여넣기
```
- 권한 변경 후 서비스 재시작
```bash
sudo chown -R user_name:user_name /home/user_name/.ssh
sudo service sshd restart
```
- 생성된 id_rsa(secret_key) 값을 local에 "user_name.pem" 으로 저장 후 권한 변경
```
chmod 400 user_name.pem
```
- secret_key를 통하여 해당 user_name ec2 접근 가능
```bash
ssh -i user_name.pem user_name@ip
```

#### password 인증방법
- sshd_config 수정
```bash
sudo vi /etc/ssh/sshd_config
```
```
# To enable empty passwords, change to yes (NOT RECOMMENDED)
PermitEmptyPasswords no
 
# Change to yes to enable challenge-response passwords (beware issues with
# some PAM modules and threads)
ChallengeResponseAuthentication no
 
# Change to no to disable tunnelled clear text passwords
PasswordAuthentication yes  <--이부분을 yes로 수정
```
- 수정한 sshd_config를 읽어오기 위한 서비스 재시작
```bash
sudo service ssh restart
```
- 새로운 사용자에게 권한 부여
```bash
sudo usermod -aG ec2-user user_name
sudo usermod -aG root user_name
```

***
## update yum
```bash
sudo yum update -y
```

***
## install pip

#### CentOS에서 pip 설치를 위해서는 우선 EPEL 리포지토리를 활성화
```bash
sudo yum install epel-release
```

#### insatll pip
```bash
sudo yum insatll python-pip
```

#### ubuntu
```bash
sudo apt-get update
```
```bash
sudo apt-get install python3-pip
```

***
## install git
```bash
sudo yum insatll git -y
```
```bash
sudo apt-get install git
```

***
## install mysql

#### RPM 설치
```bash
sudo yum insatll https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
```

#### yum 설치
```bash
sudo yum install mysql-community-server
```

#### apt 설치
```bash
sudo apt-get install mysql-server
```

#### 구동되는지 확인후 재시작
```bash
sudo systemctl start mysqld
sudo systemctl status mysqld

sudo systemctl restart mysqld
```

#### root 비밀번호 찾기 & 변경
```bash
sudo grep 'temporary password' /var/log/mysqld.log
# 임시 비밀번호 발행
```

#### 비밀번호 초기화
```bash
sudo mysql_secure_installiation -p
```

#### 비밀번호 정책 낮춰서 저장
- mysql 접속
- 비밀번호 정책을 LOW로 수정 { LOW:비밀번호 길이만 맞추면 가능 최소 8자 이상
                             MEDIUM : 숫자, 대문자, 소문자, 특수문자가 모두 포함
                             STRONG : dictionary file에 포함된 단어 사용 불가}
```sql
set global validate_password.policy=LOW;
```
- 비밀번호 수정
```sql
alter user 'root'@'localhost' identified with mysql_native_password by 'password';
flush privileges;
```

## python version change
- install python new version
```bash
sudo apt install software-properties-common
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt install python3.x
```
- change default python version
```bash
sudo update-alternatives --install /usr/bin/python3 python /usr/bin/python3.7 1
sudo update-alternatives --config python3
```