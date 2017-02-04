# 개발 도구

## Git
[Effective Git](http://www.slideshare.net/kexplo/ndc2016-effective-git)  
[누구나 쉽게 이해할 수 있는 Git 입문](http://backlogtool.com/git-guide/kr/)  
[카카오스토리 웹팀의 코드리뷰 경험](http://www.slideshare.net/OhgyunAhn/ss-61189141)  
[매끄러운 코드 리뷰를 돕는 10가지 방법](http://www.bloter.net/archives/238819)  
[iTerm 개발환경 세팅](http://hjh5488.tistory.com/2)  
[ㄴMenlo for Powerline](https://github.com/abertsch/Menlo-for-Powerline/archive/master.zip)  
[ㄴInconsolata for Powerline](https://github.com/powerline/fonts/raw/master/Inconsolata/Inconsolata%20for%20Powerline.otf)  

### Git Subtree
[git subtree를 사용하여 재사용할 코드 독립 시키기](http://readme.skplanet.com/?p=8542)  
[Git Subtree — Git Memo v1.1 documentation](http://git-memo.readthedocs.io/en/latest/subtree.html)  
<https://github.com/git/git/blob/master/contrib/subtree/git-subtree.txt>  

```bash
# deploy/static 저장소 추가
git remote add deploy/static [url]

# deploy/static 저장소 master 브랜치를 static 디렉토리로 추가
git subtree add --prefix=static deploy/static master

# static 디렉토리를 subtree/static 브랜치로 split
git subtree split --prefix=static -b subtree/static

# static 디렉토리를 deploy/static 저장소 master 브랜치로 push
git subtree push --prefix=static deploy/static master

# deploy/static 저장소 master 브랜치를 static 디렉토리로 pull
git subtree pull --prefix=static deploy/static master
```

### Git SourceTree Command Line Tools
<https://jira.atlassian.com/browse/SRCTREE-3934>

```bash
cp /Applications/SourceTree.app/Contents/Resources/stree /usr/local/bin/stree
```

## IDE
[IntelliJ IDEA Tips](http://tiveloper.tistory.com/category/IDE%20%26%20Apps/IntelliJ%20Idea)  
[IntelliJ에서 Spring Devtools와 매크로](http://sbcoba.tistory.com/36)  
[인텔리J 활용 꿀팁 42가지 정리](http://www.popit.kr/인텔리j-활용-꿀팁-42가지-정리/)  

## Maven
[maven-war-plugin](http://maven.apache.org/plugins/maven-war-plugin/war-mojo.html)

## etc
[맥 커맨드라인 유틸리티](http://www.mitchchn.me/2014/os-x-terminal/?x)  
