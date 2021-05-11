# Redis

## Connection pool

* `org.apache.commons:commons-pool2`
  * max-idle : pool에 반납할 때 최대로 유지될 수 있는 커넥션 개수
  * min-idle : 최소한으로 유지할 커넥션 개수
  * max-active : 동시에 사용할 수 있는 최대 커넥션 개수
  * max-wait : 커넥션 할당시 최대 대기 시간
  * time-between-eviction-runs : idle 커넥션 정리 주기
* <https://developpaper.com/redis-cluster-environment-configuration-of-springboot-series/>
* lettuce vs jedis : <https://jojoldu.tistory.com/418>
