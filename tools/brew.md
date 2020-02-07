# Brew

## 명령어

```
brew help

# 정보 출력
## 설치된 패키지
brew list
brew cask list

# 설치 및 제거
brew install <package>
brew uninstall <package>

# 버전 관리
brew update
brew upgrade
brew cask upgrade

# brew link 버전 변경
brew unlink <old-package>
brew link <new-package>
```

## 문제점

```bash
$ brew link mongodb-community@4.0 
Warning: mongodb-community@4.0 is keg-only and must be linked with --force

If you need to have this software first in your PATH instead consider running:
  echo 'export PATH="/usr/local/opt/mongodb-community@4.0/bin:$PATH"' >> ~/.zshrc
```
