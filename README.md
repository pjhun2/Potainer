# Potainer. 
 
install. 

Portainer Server Deployment  

docker volume create portainer_data // Potainer를 사용하기위한 볼륨 생성  

docker run -d -p 8000:8000 -p 9000:9000 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce // Potainer 구동  

구동 완료 후 http://{host_ip}:9000 url로 접속  

![image](https://user-images.githubusercontent.com/74689088/108614095-1807f800-743b-11eb-981c-50df4bfef896.png)  

패스워드 설정 후 Potainer 로그인  

이후에 local, swarm, kube 3가지 선택 가능 ( 현재 구성은 local )  

![image](https://user-images.githubusercontent.com/74689088/108614113-54d3ef00-743b-11eb-80a5-894d73dc49c9.png)  

local 클릭하여 접속  

![image](https://user-images.githubusercontent.com/74689088/108614133-85b42400-743b-11eb-9571-a8cf6c0e6b31.png)  

현재 사용 중인 container, image 등 정보 확인  

![image](https://user-images.githubusercontent.com/74689088/108614150-a2505c00-743b-11eb-91b1-ec3fbdc71abe.png)  

템플릿을 사용하여 간편한 container 생성 가능  



#1 httpd 템플릿을 사용하여 웹서버 만들기

![image](https://user-images.githubusercontent.com/74689088/108614170-cd3ab000-743b-11eb-877d-2871e4f78857.png)  

Name : Container 이름  
Network: 사용할 네트워크 지정 bridge, macvlan, host, none 다양한 네트워크 선택 가능  
사진처럼 bridge로 선택 시 host 아이피에 포트포워딩되어 접속 가능  

Access Control : Potainer에서 LDAP, OAuth 등 다양한 인증방법과 연동이 가능한데, 다른 사용자들이 해당 컨테이너에 접근이 불가능하도록 권한 설정  

Advanced option : 실제 접속할 포트를 지정 80 -> 80 으로 설정 시 host아이피가 192.168.0.10 일 경우  
192.168.0.10:80 --> 172.16.0.5:80 이런 과정으로 설정되어 컨테이너 서버에 접근 가능  

volume : 자동으로 httpd 볼륨이 생성됨 , template를 사용하지 않을 경우 수동으로 생성하여 지정 가능  

이 정도의 기본 설정을 마친 후 [ Deploy the container ]로 컨테이너 자동 생성  

컨테이너 항목으로 이동 시 생성한 컨테이너 리스트 확인 가능 

현재 접속하여 사용 중인 portainer와 방금 생성한 web_srv 확인 가능    

![image](https://user-images.githubusercontent.com/74689088/108614313-b5176080-743c-11eb-86b1-314c20c8ce83.png)  

컨테이너 이름 클릭 시 내부 상세 설정 및 접속 정보 확인 가능  

![image](https://user-images.githubusercontent.com/74689088/108614436-d167cd00-743d-11eb-9e3b-699721266764.png)  

설정된 IP와 Log, Stats, Console 등 현재 상태 확인 가능  

구동이 완료된 후 HOST_IP:80으로 접속 시 It Works! 페이지가 뜨면서 동작중인 것을 확인할 수 있음  

![image](https://user-images.githubusercontent.com/74689088/108614476-14c23b80-743e-11eb-891d-009221cde03f.png)  




#2 template 없이 수동 컨테이너 생성

Debian 서버 생성하기  

docker hub에서 생성하려고 하는 컨테이너 정보 확인 https://hub.docker.com/_/debian  

서버에서 사용할 volume 생성  

![image](https://user-images.githubusercontent.com/74689088/108616822-755c7300-7454-11eb-9abc-dbeadfc9d04f.png)  

container 리스트에서 Add Container로 새 컨테이너 생성  

![image](https://user-images.githubusercontent.com/74689088/108616794-2dd5e700-7454-11eb-98c5-a78dd5bdec78.png)  


Container에서 사용할 이름, image 이름 서비스될 포트나 접근 포트 입력

![image](https://user-images.githubusercontent.com/74689088/108616808-50680000-7454-11eb-826d-452e8c3ade31.png)  

내부 서버에 접근하기 위해서 interactive & TTY 활성화

![image](https://user-images.githubusercontent.com/74689088/108616844-b05ea680-7454-11eb-8296-0ba5e9bf2f92.png)  





#출처  
https://documentation.portainer.io/v2.0/deploy/ceinstalldocker/  
https://hub.docker.com/


