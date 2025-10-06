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
52. 실습12) Jenkins Pipeline 프로젝트 - Tomcat 서버에 배포

54. SonarQube 사용하기
    - 지속적인 통합과 분석을 하기 위한 도구
    - 취약성이 있는 코드 및 불필요한 코드를 찾음
    - 요약하면 코드의 품질을 올려줌
    - Windows $ docker run --rm -p 9000:9000 --name sonarqube sonarqube
    - Apple m1 $ docker run -rm -p 9000:9000 --name sonarqube edowon0623/sonarqube:arm
    - Windows, MacOS intel chip) docker run --rm -p 9000:9000 --name sonarqube sonarqube
    - MacOS silicon chip, m1) docker run --rm -p 9000:9000 --name sonarqube  edowon0623/sonarqube:arm
        - 오류로 docker run -d --name sonarqube -p 9000:9000 mwizner/sonarqube:8.7.1-community로 실행
55. 실습14) SonarQube + Maven 프로젝트 사용하기
    https://docs.sonarqube.org/latest/analysis/scan/sonarqube-for-maven
    https://mvnrepository.com/artifact/org.sonarsource.scanner.maven/sonar-maven-plugin
     
    
    Maven에서 SonarQube 빌드
    mvn sonar:sonar -Dsonar.host.url=http://IP_address:9000 -Dsonar.login=[the-sonar-token]
    Windows Powershell 등에서 명령어 오류가 발생하면 다음과 같이 인용부호(작은따옴표)를 붙이고 실행
    mvn sonar:sonar '-Dsonar.host.url=http://IP_address:9000''-Dsonar.login=[the-sonar-token]'
    
56. 실습15) Bad code 조사하기
    mvn clean compile package -DskipTests=true 에서 문제가 없어도
    mvn sonar:sonar '-Dsonar.host.url=http://IP_address:9000''-Dsonar.login=[the-sonar-token]'에서는 System.out.println();같은거는 리소스를 잡아먹어 문제 발생. 코드의 품질 상승



57. Jenkins + SonarQube 연동
    - System Sonarqube down -> Security -> Credentials에서 Sonarqube token입력
    - docker network inspect bridge

58. 실습16) SonarQube 사용을 위한 Pipeline 사용하기
SonarQube 연동 pipline 코드
stage('SonarQube analysis') {

    steps {

        withSonarQubeEnv('SonarQube-server') {

            sh 'mvn sonar:sonar'

        }

  }

}


60. 실습17) Jenkins Node 추가하기
새로운 Jenkins 서버1 실행 
Windows, MacOS intel chip) docker run --privileged --name jenkins-node1 -itd -p 30022:22 -e container=docker -v /sys/fs/cgroup:/sys/fs/cgroup --cgroupns=host edowon0623/docker:latest /usr/sbin/init
MacOS silicon chip, m1) docker run --privileged --name jenkins-node1 -itd -p 30022:22 -e container=docker -v /sys/fs/cgroup:/sys/fs/cgroup:rw --cgroupns=host  edowon0623/docker-server:m1 /usr/sbin/init
Jenkins 서버에 JDK 설치
yum install -y ncurses git
yum list java*jdk-devel
yum install -y java-11-openjdk-devel.aarch64


61. 실습18) Jenkins Slave Node에서 빌드하기

새로운 Jenkins 서버2 실행 
Windows, MacOS intel chip) docker run --privileged --name jenkins-node2 -itd -p 40022:22 -e container=docker -v /sys/fs/cgroup:/sys/fs/cgroup --cgroupns=host edowon0623/docker:latest /usr/sbin/init
MacOS silicon chip, m1) docker run --privileged --name jenkins-node2 -itd -p 40022:22 -e container=docker -v /sys/fs/cgroup:/sys/fs/cgroup --cgroupns=host  edowon0623/docker-server:m1 /usr/sbin/init

79. AWS EC2 인스턴스 생성
80. AWS EC2 인스턴스 접속
    aws coreetto 다운로드
    sudo curl -L https://corretto.aws/downloads/latest/amazon-corretto-11-x64-linux-jdk.rpm -o aws_corretto_jdk11.rpm
    jdk11 설치
    sudo yum localinstall aws_corretto_jdk11.rpm.rpm
    java 버전 확인
    java -version
    javac -version
    다운받은 패키지 삭제
    rm -rf aws_corretto_jdk11.rpm
64. 이미지를 이용하여 AWS EC2 생성하기

65. 실습19) AWS EC2에 Jenkins 서버 설치하기
    ## Maven 버전과 Jenkins 버전은 변경 될 수 있습니다.
    
    https://mirror.navercorp.com/apache/maven/maven-3/
     
    
    1. AWS EC2 - Amazon Linux release 2023.4.20240513 (Amazon Linux)
    
    JDK 설치 
    sudo dnf update
    sudo dnf instlal java-17-amazon-corretto-devel
    java -version
    Maven 설치 
    cd /opt
    ls -ltr
    sudo wget https://mirror.navercorp.com/apache/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
    sudo tar -xvf apache-maven-3.9.6-bin.tar.gz
    (기존에 maven 파일 존재할 경우) sudo rm -rf maven 으로 
    sudo mv apache-maven-3.9.6-bin.tar.gz maven
    cd maven (확인)
    2. AWS EC2 - Amazon Linux release 2 
    
    https://maven.apache.org/download.cgi
    Maven 설치 (EC2에서 실행, Maven 버전은 변경될 수 있으니, 위 사이트에서 버전 확인 필요)
    sudo amazon-linux-extras install epel -y
    cd /opt
    ls -ltr
    sudo wget https://mirror.navercorp.com/apache/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
    sudo tar -xvf apache-maven-3.9.6-bin.tar.gz
    sudo mv apache-maven-3.9.6-bin.tar.gz maven
    cd maven (확인)
     
    
    공통) .bash_profile 수정
    
    vi ~/.bash_profile
    source ~/.bash_profile
    MAVEN_HOME=/opt/maven
    
    MAVEN=/opt/maven/bin
    
    PATH=
    PATH:
    MAVEN:$MAVEN_HOME
    3. Jenkins 설치  https://pkg.jenkins.io/redhat-stable/
    
    Jenkins 설치 (EC2에서 실행)
    sudo amazon-linux-extra install epel -y
    sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
    sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
    sudo yum install jenkins
    sudo yum upgrade
    sudo systemctl enable jenkins
    sudo systemctl start jenkins
    sudo systemctl status jenkins
    Jenkins 초기 암호 확인
    cat /var/lib/jenkins/secrets/initialAdminPassword

66. 실습20) AWS EC2에 Docker 서버 설치하기
    AWS의 EC2에 Docker Server 설치
    sudo amazon-linux-extras install epel -y
    sudo yum install –y docker
    Docker Test
    docker –version
    Start Docker 
    sudo usermod –aG docker ec2-user (인스턴스 재 접속)
    sudo service docker start
    docker run hello-world
    
68. 실습21) AWS EC2에 Tomcat 서버 설치하기
    https://tomcat.apache.org/download-90.cgi  (다운로드 가능한 최신 버전 확인 후 진행)
        apache-tomcat-9.0.68.tar.gz
    AWS의 EC2에 Tomcat Server 설치
        sudo amazon-linux-extras install epel -y
        cd /opt
        wget https://mirror.navercorp.com/apache/tomcat/tomcat-9/v9.0.68/bin/apache-tomcat-9.0.68.tar.gz
        tar –xvzf apache-tomcat-9.0.68.tar.gz
        chmod +x /opt/apache-tomcat-9.0.68.tar.gz
        ln –s /opt/apache-tomcat-9.0.68/bin/startup.sh /usr/local/bin/tomcat_startup
        ln –s /opt/apache-tomcat-9.0.68/bin/shutdown.sh /usr/local/bin/tomcat_shutdown

69. 실습23) AWS EC2에 SonarQube 설치하기
    EC2 인스턴스 타입 변경 
        t2.micro -> t2.small
    SonarQube 설치
        sudo amazon-linux-extras install epel -y
        sudo mkdir /opt/sonarqube
        cd /opt/sonarqube
        sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-7.6.zip
        sudo unzip sonarqube-7.6.zip
        sudo chown -R ec2-user:ec2-user /opt/sonarqube/
    SonarQube 실행
        ./bin/[사용하는 OS]/sonar.sh start
    SonarQube 테스트
        http://[public ip address]:9000/
    
71. 실습24) Jenkins를 이용하여 Tomcat 서버에 배포하기
