# Linux

[리눅스를 활용한 회사 인프라 구축의 모든 것](https://www.lesstif.com/display/1STB/Home)  

## LVM

<https://www.lesstif.com/pages/viewpage.action?pageId=20775667>

* 전통적인 파티션 구조

![전통적인 파티션 구조](https://www.lesstif.com/download/attachments/20775667/image2014-5-18%201%3A48%3A42.png?version=1&modificationDate=1400344887000&api=v2)

1. 추가 디스크를 장착
2. 추가된 디스크에 파티션 생성 및 포맷
3. 새로운 마운트 포인트(/home2) 를 만들고 추가한 파티션을 마운트
4. 기존 home 데이타를 home2 에 복사 또는 이동
5. 기존 home 파티션을 언마운트(unmount)
6. home2 를 home 으로 마운트 


* LVM

![LVM](https://www.lesstif.com/download/attachments/20775667/image2014-5-21%2017%3A16%3A20.png?version=1&modificationDate=1400659731000&api=v2)

1. 추가 디스크를 장착
2. 추가된 디스크에 파티션을 만들어 물리 볼륨 생성
3. 물리 볼륨을 볼륨 그룹에 추가. home 은 vg_data 볼륨 그룹이므로 여기에 추가한다.
4. home 이 사용하는 논리 볼륨인 lv_home 의 볼륨 사이즈를 증가시켜줌

