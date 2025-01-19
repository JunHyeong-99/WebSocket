# 소개

이 프로젝트는 소켓 프로그래밍을 기반으로 다양한 네트워크 통신 기능을 구현하는 것을 목표로 합니다.  
Signal 핸들링, 동시 동작 에코 서버, 다중 채팅 프로그램 구현을 통해 네트워크 프로그래밍의 핵심 개념을 실습하였습니다.

---

## 주요 기능

### 1. CTRL+C 시그널 핸들러
- **설명**  
  - `SIGINT` 시그널을 처리하기 위한 핸들러 함수 구현.  
  - 프로그램 실행 중 `CTRL+C` 입력 시 핸들러가 호출되어 프로그램 종료.  

- **핸들러 동작**  
  - `"Handler is called."` 출력 후 프로그램 종료.  

- **구현 코드 개요**  

```c
void handler(int sig) {
    printf("Handler is called.\n");
    exit(EXIT_SUCCESS);
}

int main() {
    signal(SIGINT, handler);
    printf("Sleep begins!\n");
    sleep(1000);
    printf("Wake up!\n");
    return 0;
}

### 2. 동시 동작 서버 (Echo 서버)

- **설명**  
  - 여러 클라이언트와 동시 통신 가능한 Echo 서버 구현.  
  - 각 클라이언트에 대해 별도 프로세스(`fork`)를 생성하여 동작.  

- **주요 기능**  
  - 클라이언트 메시지를 그대로 반환 (Echo).  
  - 클라이언트 접속 시 현재 접속 클라이언트 수(`client_cnt`)를 출력.  

- **구현 코드 개요**  

```c
int client_cnt = 0;
printf("Number of service client: %d\n", ++client_cnt);

### 3. `select` 기반 다중 채팅 프로그램

- **설명**  
  - `select` 시스템 콜을 이용하여 다중 클라이언트와 채팅 가능한 프로그램 구현.  
  - 서버는 다수의 클라이언트로부터 메시지를 비동기로 처리.  

- **주요 기능**  
  - 클라이언트에서 메시지 입력 → 서버로 전달.  
  - 서버는 수신된 메시지를 모든 클라이언트에 브로드캐스트.  

- **구현 코드 개요**  

```c
fd_set readfds;
FD_ZERO(&readfds);
FD_SET(server_fd, &readfds);
int max_fd = server_fd;
select(max_fd + 1, &readfds, NULL, NULL, NULL);
