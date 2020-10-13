2020-10-09 Site Open

가상환경 만들기
python -m venv [name]

가상환경 실행
Powershell: ./Script/Activate.ps1
CMD: ./Script/activate.bat

docker is configured to use the default machine with IP 192.168.99.100
For help getting started, check out the docs at https://docs.docker.com

사용법

1. 이미지 검색
docker search [name]

2. 이미지 다운로드
docker pull [name]

3. 이미지 목록
docker images

4. 이미지 태그 변경
docker tag [name:tag] [name:tag]

5. 이미지 삭제
docker rmi [name]

6. 컨테이너 목록
docker ps -a

7. 컨테이너 만들기
docker run -i -t -p 8888:8888 -volume 'd:s\:/home/' --name [container_name] [image_name:image_tag]
ex) docker run -i -t -p 8888:8888 --volume '/d/s:/home/docker' --name kn s:kn

8. 컨테이너 삭제
docker rm [name]

9. 실행중인 컨테이너 접속
docker attach [name]

Jupyter 외부 접속 설정
1. kn: sha1:06f70921efde:a347bc1623af18c5b4bf97e646870c457935c67a

c = get_config()
c.NotebookApp.allow_origin = '*'
c.NotebookApp.notebook_dir = '/home/'
c.NotebookApp.ip = '0.0.0.0'
c.NotebookApp.port = 8888
c.NotebookApp.password = u'sha1:06f70921efde:a347bc1623af18c5b4bf97e646870c457935c67a'
c.NotebookApp.open_browser = False
c.NotebookApp.allow_root = True

Docker ToolBox 폴더 공유
1. 도커 머신 리스트
docker-machine ls

2. 도커 ssh 연결
docker-machine ssh [name]

3. 자동 마운트 설정
docker@default:/$ sudo vi /var/lib/boot2docker/bootlocal.sh

#!/bin/sh
#mount shared directory

mkdir -p /home/docker
mount -t vboxsf [source] /home/docker

#install docker-compose
curl -L https://github.com/docker/compose/releases/download/1.7.1/docker-compose
chmod +x /usr/local/bin/docker-compose

4. 마운트 확인
df -k
