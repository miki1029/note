# GC

* <https://johngrib.github.io/wiki/java-g1gc/>
* <https://findanyanswer.com/does-g1gc-stop-the-world>

# Monitoring

```
jps -v

jhsdb jmap --pid <pid>
jmap -histo:live <pid> | more
jinfo <pid> | grep jvm_args

# jstat
jstat --option $PID $MILLISECOND_INTERVAL $COUNT
jstat -gc <pid> 1000 3
jstat -gcutil -t -h10 <pid> 5s

# jstat used
jstat -gc 1 1000 3 | tail -n 3 | awk '{split($0,a," "); sum=a[3]+a[4]+a[6]+a[8]; print "used: " sum}'

# jstat used, total
jstat -gc 1 1000 3 | tail -n 3 | awk '{split($0,a," "); total=a[1]+a[2]+a[5]+a[7];sum=a[3]+a[4]+a[6]+a[8]; print "used: " sum ", total: " total}'

# ps
ps -eo user,pid,ppid,rss,size,vsize,pmem,pcpu,time,cmd --sort -rss
```

## MAT

* <https://www.eclipse.org/mat/downloads.php>
* <https://stackoverflow.com/questions/52652846/cant-install-eclipse-failed-to-create-the-java-virtual-machine-on-mac>

## JVM 진단 도구

* 실시간 진단 도구
  * jvm-tools
  * jcmd
  * jhsdb

* 사후 진단 도구
  * 쓰레드 덤프 : TDA, Thread Logic
  * 힙 덤프 : IBM Heap Analyzer, MAT

## 성능 진단 도구

* 운영 도구
  * APM : scouter, pinpoint
* 측정 도구
  * Profiling : JProfiler, YourKit
  * Objecft Layer : jol
  * cpu 진단 : jvmtop

## 부하 도구
* 벤치마크 도구
  * method 성능 비교 : jmh
* 부하 도구
  * 부하 테스트 도구 : gatling, jmeter
  * concurrent 부하 점검 : jcstress

# Reference

* <http://egloos.zum.com/sseam/v/7465658>
* <https://jupiny.com/2019/07/15/java-heap-dump-analysis/>
* <https://ktdsoss.tistory.com/438>
* <https://m.blog.naver.com/PostView.nhn?blogId=wideeyed&logNo=221071907919&categoryNo=24&proxyReferer=&proxyReferer=https:%2F%2Fwww.google.com%2F>
* <https://gem1n1.tistory.com/89> 굿
* <https://spoqa.github.io/2012/02/06/eclipse-mat.html>
