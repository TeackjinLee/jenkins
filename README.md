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
44. 실습8) Jenkins를 이용한 CI/CD 자동화 파이프라인 구축하기 ②
    소스 코드
    vi k8s-cicd-deployment-playbook.yml
    https://github.com/joneconsulting/jenkins_cicd_script/blob/master/k8s_script/k8s-cicd-deployment-playbook.yml
    vi k8s-cicd-service-playbook.yml
    https://github.com/joneconsulting/jenkins_cicd_script/blob/master/k8s_script/k8s-cicd-service-playbook.yml
47. Delivery Pipeline 사용
48. Pipeline 생성하기
pipeline {
    agent any
    stages {
        stage('Compile') {
            steps {
                echo "Compiled successfully!";
            }
        }

        stage('JUnit') {
            steps {
                echo "JUnit passed successfully!";
            }
        }

        stage('Code Analysis') {
            steps {
                echo "Code Analysis completed successfully!";
            }
        }

        stage('Deploy') {
            steps {
                echo "Deployed successfully!";
            }
        }
    }
    
    post {
      always {
        echo "This will always run"
      }
      success {
        echo "This will run when the run finished successfully"
      }
      failure {
        echo "This will run if failed"
      }
      unstable {
        echo "This will run when the run was marked as unstable"
      }
      changed {
        echo "This will run when the state of the pipeline has changed"
      }
    }
}
50. 실습10) Jenkins Pipeline 프로젝트 - Pipeline Syntax 사용
51. 실습11) Jenkins Pipeline 프로젝트 - Maven build pipeline
