## Springframework
[Spring Framework Reference Documentation](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/)  
[ㄴRest Message Conversion](http://docs.spring.io/spring/docs/current/spring-framework-reference/htmlsingle/#rest-message-conversion)  
[Spring 4.x Web Application 살펴보기](http://www.slideshare.net/ihoneymon/spring-4x-web-application)  
[Tomcat & Spring Bootstrapping Sequence](http://brantiffy.axisj.com/archives/232)  

## Hibernate
[Hibernate Log 남기기](http://kwonnam.pe.kr/wiki/java/hibernate/log)  

## Jackson
[jackson-annotations](https://github.com/FasterXML/jackson-annotations)  

## Maven
메이븐 버전 일괄 변경 : <http://www.mojohaus.org/versions-maven-plugin/set-mojo.html>

```bash
mvn versions:set -DnewVersion=0.1.0-SNAPSHOT -DgenerateBackupPoms=false
```

jgitflow
* <https://bitbucket.org/atlassian/jgit-flow/wiki/Home>
* <https://bitbucket.org/atlassian/jgit-flow/wiki/goals.wiki>
* <https://gist.github.com/lemiorhan/97b4f827c08aed58a9d8>

```bash
mvn jgitflow:release-start -DdevelopmentVersion=1.4.0-SNAPSHOT -DreleaseVersion=1.3.0
mvn jgitflow:release-finish -Darguments="-DskipTests"
```
