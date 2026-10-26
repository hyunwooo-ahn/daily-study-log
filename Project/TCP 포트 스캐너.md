
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

 

        sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM )

  

    

        sock.settimeout(10)

        try:


            result = sock.connect_ex((target_ip, target_port))

  

           

            if result == 0:

                print("PORT %d: OPEN" %target_port)

            elif  result == errno.ECONNREFUSED:

                print("PORT %d: CLOSED" %target_port)

            elif result == errno.EHOSTUNREACH:

                 print("PORT %d: 대상 호스트 또는 네트워크에 도달할 수 없음" %target_port)

            else:

                 print("PORT %d: 확인 불가"%target_port)

        except OSError:

            

            print("네트워크 오류 확인 불가")

  
  

        finally:

            

            sock.close()

TCP_port_finder()
```
