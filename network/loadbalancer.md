# 로드밸런서 주요 기술

* NAT(Network Address Translation) : 사설 IP 주소를 공인 IP 주소로 변경(LB <-> real server)
* DSR(Dynamic Source Routing protocol) : 서버에서 클라이언트로 되돌아가는 경우 목적지 주소를 스위치 IP 주소가 아닌 클라이언트의 IP 주소로 전달해서 스위치를 거치지 않고 바로 클라이언트를 찾아가는 개념
* Tunneling : 인터넷 상의 통로, 데이터를 캡슐화해서 연결된 상호 간에만 캡슐화된 패킷을 구별해 캡슐화를 해제

# 로드밸런서 알고리즘

* Round Robin
* Least Connection
* Weighted Least Connections
* Fastest Response Time
* Adaptive
* Fixed, Hashing, Random, URL-based, Cookie, ...

# 성능 평가

* Connections per second
* Concurrent connections
* Throughput
* Threshold

# L7

* 쿠키 기반 연결 지속성 : 클라이언트 IP가 공인 IP(L7)로 치환되어 전송되며 클라이언트 IP는 X-Forwarded-For에 기록됨
* Dos/SYN Attack에 대한 방어
* 패킷 분석을 통한 바이러스 감염 패킷 필터링
* 자원 독점 방지 등을 통한 시스템 보안 강화

## L4 vs L7

* TCP Connection
  * L4 : 알고리즘을 통해 server를 결정하고 server가 직접 client와 3 way hanshake 하여 하나의 TCP 세션을 갖게 된다.(L4는 중계만)
  * L7 : 콘텐츠 기반 스위칭을 위해 L7과 client간 3 way handshake를 실시하여 따로 TCP 세션을 형성한다. L7과 server는 또 다른 TCP 세션을 형성하고 데이터를 중계한다.

# Reference

* <https://pakss328.medium.com/%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%84%9C%EB%9E%80-l4-l7-501fd904cf05>
* <https://deveric.tistory.com/91>
* <https://tech.kakao.com/2014/05/30/l4/>
* <https://d2.naver.com/helloworld/284659>
* <https://devhicom.tistory.com/4>
* <https://medium.com/naver-cloud-platform/l4-%ED%99%98%EA%B2%BD%EC%97%90%EC%84%9C-ha-%EA%B5%AC%EC%84%B1%ED%95%B4%EB%B3%B4%EA%B8%B0-2be30aee2b58>
* <https://tech.kakao.com/2014/05/28/l3dsr/>
* <https://stackoverflow.com/questions/60276403/kubernetes-pods-graceful-shutdown-with-tcp-connections-spring-boot>
