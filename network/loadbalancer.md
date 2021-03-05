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

* <https://pakss328.medium.com/%EB%A1%9C%EB%93%9C%EB%B0%B8%EB%9F%B0%EC%84%9C%EB%9E%80-l4-l7-501fd904cf05>
