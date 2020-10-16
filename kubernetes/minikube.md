# Install minkube on macOS

```bash
$ brew install minikube
$ minikube start
```

* 아래처럼 오류 발생시

```
$ brew cask install virtualbox
  ==> Caveats
  To install and/or use virtualbox you may need to enable its kernel extension in:
    System Preferences → Security & Privacy → General
  For more information refer to vendor documentation or this Apple Technical Note:
    https://developer.apple.com/library/content/technotes/tn2459/_index.html
  
  ==> Downloading https://download.virtualbox.org/virtualbox/6.1.0/VirtualBox-6.1.0-135406-OSX.dmg
  Already downloaded: /Users/miki/Library/Caches/Homebrew/downloads/d3700155a9ac5d3c28f736aebc70a1873313b30092a2213fd5dd48212e19ce08--VirtualBox-6.1.0-135406-OSX.dmg
  ==> Verifying SHA-256 checksum for Cask 'virtualbox'.
  ==> Installing Cask virtualbox
  ==> Running installer for virtualbox; your password may be necessary.
  ==> Package installers may write to any location; options such as --appdir are ignored.
  installer: Package name is Oracle VM VirtualBox
  installer: choices changes file '/var/folders/71/gxprdc_95plg1hzxj0_9vgl80000gn/T/choices20191217-3530-1phya1d.xml' applied
  installer: Upgrading at base path /
  installer: The upgrade failed. (설치 프로그램에서 오류가 발생하여 설치에 실패했습니다. 소프트웨어 제조업체에 지원을 문의하십시오. ‘VirtualBox.pkg’ 패키지에서 스크립트를 실행하는 동안 오류가 발생했습니다.)
  ==> Purging files for version 6.1.0,135406 of Cask virtualbox
  Error: Failure while executing; `/usr/bin/sudo -E -- /usr/bin/env LOGNAME=miki USER=miki USERNAME=miki /usr/sbin/installer -pkg /usr/local/Caskroom/virtualbox/6.1.0,135406/VirtualBox.pkg -target / -applyChoiceChangesXML /var/folders/71/gxprdc_95plg1hzxj0_9vgl80000gn/T/choices20191217-3530-1phya1d.xml` exited with 1. Here's the output:
  installer: Package name is Oracle VM VirtualBox
  installer: choices changes file '/var/folders/71/gxprdc_95plg1hzxj0_9vgl80000gn/T/choices20191217-3530-1phya1d.xml' applied
  installer: Upgrading at base path /
  installer: The upgrade failed. (설치 프로그램에서 오류가 발생하여 설치에 실패했습니다. 소프트웨어 제조업체에 지원을 문의하십시오. ‘VirtualBox.pkg’ 패키지에서 스크립트를 실행하는 동안 오류가 발생했습니다.)
  Follow the instructions here:
    https://github.com/Homebrew/homebrew-cask#reporting-bugs
  /usr/local/Homebrew/Library/Homebrew/system_command.rb:176:in `assert_success!'
  /usr/local/Homebrew/Library/Homebrew/system_command.rb:53:in `run!'
  /usr/local/Homebrew/Library/Homebrew/system_command.rb:29:in `run'
  /usr/local/Homebrew/Library/Homebrew/system_command.rb:33:in `run!'
  /usr/local/Homebrew/Library/Homebrew/cask/artifact/pkg.rb:59:in `block in run_installer'
  /usr/local/Homebrew/Library/Homebrew/cask/artifact/pkg.rb:70:in `block in with_choices_file'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/tempfile.rb:295:in `open'
  /usr/local/Homebrew/Library/Homebrew/cask/artifact/pkg.rb:67:in `with_choices_file'
  /usr/local/Homebrew/Library/Homebrew/cask/artifact/pkg.rb:52:in `run_installer'
  /usr/local/Homebrew/Library/Homebrew/cask/artifact/pkg.rb:34:in `install_phase'
  /usr/local/Homebrew/Library/Homebrew/cask/installer.rb:218:in `block in install_artifacts'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/set.rb:777:in `each'
  /System/Library/Frameworks/Ruby.framework/Versions/2.6/usr/lib/ruby/2.6.0/set.rb:777:in `each'
  /usr/local/Homebrew/Library/Homebrew/cask/installer.rb:209:in `install_artifacts'
  /usr/local/Homebrew/Library/Homebrew/cask/installer.rb:101:in `install'
  /usr/local/Homebrew/Library/Homebrew/cask/cmd/install.rb:22:in `block in run'
  /usr/local/Homebrew/Library/Homebrew/cask/cmd/install.rb:16:in `each'
  /usr/local/Homebrew/Library/Homebrew/cask/cmd/install.rb:16:in `run'
  /usr/local/Homebrew/Library/Homebrew/cask/cmd/abstract_command.rb:36:in `run'
  /usr/local/Homebrew/Library/Homebrew/cask/cmd.rb:92:in `run_command'
  /usr/local/Homebrew/Library/Homebrew/cask/cmd.rb:158:in `run'
  /usr/local/Homebrew/Library/Homebrew/cask/cmd.rb:123:in `run'
  /usr/local/Homebrew/Library/Homebrew/cmd/cask.rb:9:in `cask'
  /usr/local/Homebrew/Library/Homebrew/brew.rb:102:in `<main>'
```

* virtualbox가 제대로 설치되지 않은 것이다.
    * 설정 > 보안 및 개인 정보 보호
    * 하단에 Oracle 관련 확인되지 않은 개발자라는 표시가 뜰텐데 자물쇠를 풀고 이를 허용해주어야 한다.
    * <https://medium.com/@DMeechan/fixing-the-installation-failed-virtualbox-error-on-mac-high-sierra-7c421362b5b5>
* 설정 변경을 완료하면 아래 명령어로 virtualbox를 다시 설치
* 완료되면 minikube를 다시 시작

```
brew cask install virtualbox
minikube start
minikube stop
```

# Commands

```
# 정보 출력
minikube status
minikube service list

# 서비스 연결
# minkube는 로드 밸런서 서비스를 지원하지 않으므로 아래 명령어를 통해서 kube 서비스에 연결이 가능하다.
minikube service {service-name}

# 대시보드
minikube dashboard

# kubectl (버전 호환용)
minikube kubectl -- get pods -A
```
