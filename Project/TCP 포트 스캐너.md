
```python
import socket

import ipaddress

import errno

  
  

def TCP_port_finder():

    while True:

        try:

            target_ip = input("검사할 IP 주소: ")

  

            ip_obj = ipaddress.IPv4Address(target_ip)

            break

        except ValueError:

             print("잘못된 IP주소")

  
  
  

    # 검사 대상 TCP 포트 번호를 지정한다.

    while True:

        try:

            target_port_start = int(input("검사할 포트 시작 번호:"))

            target_port_end = int(input("검사 포트 끝 번호:"))

            if  target_port_start >= 1 and target_port_end <= 65535 and target_port_start <= target_port_end:

               break

            else:

                print("잘못된 포트 범위입니다\n시작 포트는 끝 포트보다 작거나 같아야 합니다.")

                continue

        except ValueError:

                    print("잘못된 port번호")

  

  

    for target_port in range(target_port_start, target_port_end+1):

    # IPv4 기반 TCP 소켓을 생성한다.

        sock = socket.socket(

            socket.AF_INET,   #IPv4 주소 체계 사용

            socket.SOCK_STREAM #TCP 방식으로 통신할 소켓을 생성

        )

  

        # 연결 제한 시간을 1초로 설정한다.

        sock.settimeout(10)

        try:

            # 지정한 IP 주소와 포트에 TCP 연결을 시도한다.

            #connect_ex()는 지정한 IP 주소와 포트 번호로 TCP 연결을 시도하고, 그 결과를 숫자로 반환하는 함수

            result = sock.connect_ex((target_ip, target_port))

  

            # 반환값이 0이면 TCP 연결에 성공한 것이다.

            if result == 0:

                print("PORT %d: OPEN" %target_port)

            elif  result == errno.ECONNREFUSED:

                print("PORT %d: CLOSED" %target_port)

            elif result == errno.EHOSTUNREACH:

                 print("PORT %d: 대상 호스트 또는 네트워크에 도달할 수 없음" %target_port)

            else:

                 print("PORT %d: 확인 불가"%target_port)

        except OSError:

            # 네트워크 연결 과정에서 발생한 예외를 처리한다.

            print("네트워크 오류 확인 불가")

  
  

        finally:

            # 검사에 사용한 소켓 자원을 정리한다.

            sock.close()

TCP_port_finder()
```
