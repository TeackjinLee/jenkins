# jenkins
15. PollSCM 설정을 통한 지속적인 파일 업데이트
16. SSH + Docker가 설치되어 있는 VM(컨테이너) 사용하기 (Updated: 2025-01-0241)
    - 다른 서버에 설치하기
    16.1 플러그인관리 - publish over ssh 설치
    16.2 접속할려는 서버의 이름 입력
      1) war파일을 ssh를 이용해서 복사 (서버2)
      2) (서버2) Dockerfile + ."war" -> Docker Image 빌드
      3) Docker image -> 컨테이너 생성
  
17. 실습4) Docker Container에 배포하기 ①
18. 실습4) Docker Container에 배포하기 ②
23. Ansible 설정과 작동 과정
24. Ansible 기본 명령어
    1. ssh-keygen
       - 키 생성) ssh-keygen
       - 키 복사) ssh-copy-id root@[접속할 서버IP]
    3. ansible
       - 멱등성 - 여러명령어를해도 같으면 한번만 실행
       1) ansible all -m ping : 실행중인 서버 확인
       2) ansible all -m shell -a "free -h" : 용량 확인
       3) ansible all -m copy -a "src=./text.txt dest=/tmp"

26.Ansible playbook 사용하기
    - ansible-playbook first-playbook.yml 실행
27. Jenkins + Ansible 연동하기
31. 실습6) Ansible Playbook으로 Docker 컨테이너 생성하기
35. Kubernetes 기본 명령어
    K8s 기본 명령어
    노드 확인  
    kubectl get nodes
    파드 확인
    kubectl get pos
    디플로이먼트 확인
    kubectl get deployments
    서비스 확인 
    kubectl get services
    Nginx 서버 실행 
    kubectl run sample-nginx --image=nginx --port=80
    컨테이너 정보 확인 
    kubectl describe pod/sample-nginx
    파드 삭제
    kubectl delete pod/sample-nginx-XXXXX-XXXXX
    Scale 변경 (2개로 변경)
    kubectl scale deployment sample-nginx --replicas=2
    Script 실행
    kubectl apply -f sample1.yml
    소스 코드
    vi sample1.yml
    https://github.com/joneconsulting/jenkins_cicd_script/blob/master/k8s_script/sample1.yml
36. Kubernetes Script 파일
    K8s 기본 명령어
    파드 확인
    kubectl get pos -o wide
    파드에 터널링으로 접속
    kubectl exec -it nginx-deployment-XXXX-XXXX -- /bin/bash
    파드 노출(공개)
    kubectl expose deployment nginx-deployment --port=80 --type=NodePort
    파드 삭제
    kubectl delete pod/nginx-deployment-XXXX-XXXX
    디플로이먼트 삭제
    kubectl delete deployment nginx-deployment
37. Kubernetes + Ansible 연동
38. Ansible에서 Kubernetes 제어하기
    Docker 컨테이너 검색
        MaxOS) docker ps -a | grep ansible
        Windows) docker ps -a | findstr "ansible"
    Ansible에서 K8s 접속 테스트
        ansible -i ./k8s/hosts kubenetes -m ping
        ansible -i ./k8s/hosts kubenetes -m ping -u [계정명]
