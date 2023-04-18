# github ssh 사용법

## ssh(Secure Shell)

- 네트워크 상의 다른 컴퓨터에 로그인하거나, 원격 시스템에서 명령을 실행하고 다른 시스템으로 복사하게 해주는 응용 프로그맨 또는 보안 프로토콜(규약)
- ssh client와 ssh server간에 소통을 할 때 ssh protocal에 맞춰서 암호화 된 정보를 전송함으로써 중간에 정보가 탈취되어도 보안상 안전할 수 있음
- 리눅스, mac에는 ssh client가 기본으로 설치되어 있으나 windows는 기본 제공이 안되기에 xshell, PuTTy 등 프로그램이 필요 (원격지 컴퓨터에는 ssh server가 설치되어 있어야 함)
- 공개키 인증방식 사용: server에 공개되어도 되는 공개키가 존재하고, client는 비밀번호와 같은 존재인 개인키를 가지고 있음. client가 명령을 보낼 때 개인키를 같이 보내고 server는 명령을 받으면 함께 전달받은 개인키와 매치되는 공개키가 있는지 우선 확인. 확인이 완료되어 인증이 된 경우에 명령을 실행해 줌
  - 공개키: 외부에 공개되어도 괜찮은 key
  - 개인키: 개인 비밀번호와 같은 존재, 나만 접근할 수 있도록 안전하게 보관해야 함

## 사용방법(작성 중)

1. git bash에서 공개키와 개인키 발급받기
`$ ssh-keygen -t ed25519 -C "your_email@example.com"`

- 참고문서: [새 SSH 키 생성 및 ssh-agent에 추가](https://docs.github.com/ko/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent#about-ssh-key-passphrases)

1. github에 공개키 등록하기

- 참고문서: [GitHub 계정에 새 SSH 키 추가](https://docs.github.com/ko/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
