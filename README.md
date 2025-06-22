# jenkins
15. PollSCM 설정을 통한 지속적인 파일 업데이트
16. SSH + Docker가 설치되어 있는 VM(컨테이너) 사용하기 (Updated: 2025-01-0241)
    - 다른 서버에 설치하기
    16.1 플러그인관리 - publish over ssh 설치
    16.2 접속할려는 서버의 이름 입력
      1) war파일을 ssh를 이용해서 복사 (서버2)
      2) (서버2) Dockerfile + ."war" -> Docker Image 빌드
      3) Docker image -> 컨테이너 생성
  
