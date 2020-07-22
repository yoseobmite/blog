---
title: Hugo blog create
date: 2020-07-13
categories:
- newBlog
tag: 
- WSL
- hugo
- github
- ubuntu
- window 10
- myblog
thumbnailImagePosition: left
thumbnailImage: //live.staticflickr.com/65535/50107137301_21235ba003_k.jpg
#autoThumbnailImage: yes
coverImage: //www.ghibli.jp/images/buta1.jpg
coverSize : partial
metaAlignment: center
showDate: true
showTags: true
showPagination: true
showSocial: true
showMeta: true
showActions: true
---

여름 휴가시작후 `ubuntu20.04` 를 설치 나름 안정된 데스탑환경을 만족 스럽더군요 다시 정적 블로그 `static blog`시작 해볼까 하고 __`Hugo`__ 를 둘러 봅니다. 윈도우10에서 우분투 환경으로 동작 가능하다기에 도전 해보았습니다.  

## 개요 
  - 설치 순서 
      - [WLS2설치](#h2-idwls2설치-3wls2설치h2) 
      - [몇가지 기본설정 하기](#몇가지-기본설정-하기)
      - [zsh과 oh-my-zsh 설치, 그리고 플러그인 적용](#zsh과-oh-my-zsh-설치-그리고-플러그인-적용)
      - [Hugo 설치](#h3-idhugo-설치-17hugo-설치h3)
      - [Github deploy](#github-deploy)
      - [추가기능설정](#추가기능-설정)



## WLS2설치 
---
__Windows Subsystem for Linux?__

`WSL`은 윈도우환경에서 기존의 `Virtual Box`나 `Hyper-V`와 같은 가상머신을 이용하지 않고(실질 적으로 가상머신을 이용하기는 하지만..) 리눅스를 이용할 수 있게 해줍니다. 현재 `WSL`은 1과 2가 나온 상태이며 `WSL1`는 마이크로소프트 스토어에서 Windows Terminal을 검색하여 설치하여 사용할 수 있습니다. 현재 `WSL2`는 정식 릴리즈 버전이 아닙니다. 해당 기능을 사용하기 위해서는 윈도우즈 오픈베타와 같이 미리 시스템을 체험해보는 'Insider preview' 프로그램에 등록하여야 하며 `WSL2`이외에도 추가적인 설치가 필요합니다. 설치 과정이 수고스럽지만 `WSL2`를 설치해보고 싶으신분들은 한번쯤 해보셔도 좋을것 같습니다.

> 출처 :[개발자블로그](https://rldnddl87.github.io/devops/wsl2_setup/)
> 
```
오호라 윈도우10에서 Hyper-V 설치후 우분투18.04 돌려봤으나 엄청 느려져 지웠던터라 흥미롭더군요. 
```

>[설치가이드](https://docs.microsoft.com/ko-kr/windows/wsl/install-win10#install-the-windows-subsystem-for-linux)

__Linux용 Windows 하위 시스템 설치__

Windows에서 Linux 배포를 실행하기 전에 "Linux용 Windows 하위 시스템" 옵션 기능을 사용해야 합니다. 

`PowerShell`을 관리자 권한으로 열어 실행합니다.

```ZSH
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```
`WSL 1`만 설치하려면 지금 머신을 다시 시작하여 선택한 `Linux` 배포 설치로 이동해야 합니다. 그렇지 않으면 다시 시작될 때까지 기다렸다가 `WSL 2`로 업데이트로 이동합니다. `WSL 2`와 `WSL 1`비교에 대해 자세히 알아보세요.

__WSL 2로 업데이트__

`WSL 2`로 업데이트하려면 다음 조건을 충족해야 합니다.
`Windows 10` 실행, 버전 2004로 업데이트, 빌드 19041 이상.
`Windows 로고 키` + `R`을 선택하고 `winver`를 입력한 다음, 확인을 선택하여 `Windows 버전`을 확인합니다. (또는 `Windows 명령 프롬프트`에서 `ver` 명령을 입력합니다.) 빌드가 19041보다 낮은 경우 최신 `Windows 버전`으로 업데이트합니다. `Windows 업데이트` 도우미를 가져옵니다.

__'가상 머신 플랫폼' 옵션 구성 요소 사용__

WSL 2를 설치하기 전에 "가상 머신 플랫폼" 옵션 기능을 사용하도록 설정해야 합니다.

`PowerShell`을 관리자 권한으로 열어 실행합니다.

```ZSH
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```
머신을 다시 시작하여 `WSL` 설치를 완료하고 WSL 2로 업데이트합니다.

__WSL 2를 기본 버전으로 설정__

새 `Linux` 배포를 설치할 때 `PowerShell`에서 다음 명령을 실행하여 `WSL 2`를 기본 버전으로 설정합니다.
```ZSH
wsl --set-default-version 2
```
__선택한 Linux 배포 설치__

[`Microsoft Store`](https://aka.ms/wslstore)를 열고 즐겨 찾는 `Linux` 배포를 선택합니다.
- [Ubuntu 20.04 LTS](https://www.microsoft.com/store/apps/9n6svws3rx71)

__새 배포 설정__

새로 설치된 `Linux` 배포를 처음 시작하면 콘솔 창이 열리고 파일이 압축 해제되어 PC에 저장될 때까지 1~2분 정도 기다려야 합니다. 이후의 모든 시작은 1초도 걸리지 않습니다.
새 `Linux` 배포에 대한 사용자 계정 및 암호를 만들어야 합니다.
`WSL 2`를 기본 아키텍처로 설정하려는 경우 이 명령을 사용하여 수행할 수 있습니다.
```ZSH
wsl --set-default-version 2
```
시작 창에서 `ubuntu20.04 LTS` 를 발견 할수 있습니다. 

## 몇가지 기본설정 하기 
---
기본 패키지 설치와 전체 업데이트
```ZSH
sudo apt update
sudo apt install vim git curl 
sudo apt upgrade -y
sudo apt autoremove -y
```
## zsh과 oh-my-zsh 설치, 그리고 플러그인 적용
---
데스크탑 환경에서 `bash` 쉘의 시대는 저물고 있고 바야흐로 `zsh`의 시대입니다.
```ZSH
sudo apt install zsh -y && chsh -s `which zsh`
# chsh 명령어 실행시 password 를 확인합니다.
curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```
chsh 명령어로 기본 쉘을 변경해도 로그아웃 후 재 로그인 전까지 Terminal 앱에서는 bash 쉘이 기본으로 나타납니다.

__zsh bullet-train 테마__

zsh에서는 `agnoster` 테마가 가장 유명하지만 저는 `bullet-train` 테마를 사용합니다.
```ZSH
wget http://raw.github.com/caiogondim/bullet-train-oh-my-zsh-theme/master/bullet-train.zsh-theme && mkdir $ZSH_CUSTOM/themes && mv bullet-train.zsh-theme $ZSH_CUSTOM/themes/
```
위 명령어를 실행 후 `~/.zshrc` 파일을 열어 `ZSH_THEME` 값을 `bullet-train` 으로 바꿔줍니다.
```ZSH
# ...
ZSH_THEME="bullet-train"
```

__유용한 플러그인 설치__

다양한 기능을 이제부터 익혀보렵니다. ^^!
```ZSH
git clone git://github.com/zsh-users/zsh-autosuggestions $ZSH_CUSTOM/plugins/zsh-autosuggestions --depth=1
Bash
```

```ZSH
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting --depth=1
```
플러그인 설치 후 `~/.zshrc` 에서 설치한 플러그인을 `plugins` 에 추가합니다.
```ZSH
plugins=(
  git
  zsh-autosuggestions
  zsh-syntax-highlighting
 )
```

__Powerline 폰트 or D2 폰트설치__

윈도우 폰트에 매뉴얼로 설치 하였습니다. 
```ZSH
wget https://github.com/powerline/fonts/raw/master/UbuntuMono/Ubuntu%20Mono%20derivative%20Powerline.ttf
wget https://github.com/powerline/fonts/raw/master/UbuntuMono/Ubuntu%20Mono%20derivative%20Powerline%20Italic.ttf
wget https://github.com/powerline/fonts/raw/master/UbuntuMono/Ubuntu%20Mono%20derivative%20Powerline%20Bold.ttf
wget https://github.com/powerline/fonts/raw/master/UbuntuMono/Ubuntu%20Mono%20derivative%20Powerline%20Bold%20Italic.ttf
```

__window 10 Visual Studio Code 플러그인 설치__

The Remote - WSL extension enables you to run Visual Studio Code within the Windows Subsystem for Linux (WSL).
    
[Remote development in WSL](https://code.visualstudio.com/docs/remote/wsl-tutorial)

간단히 Visual Studio Code 터미널로 연결 됩니다. 

__`유레카. !!!`__

시험삼아 파이썬을 설치 해봅니다. 
```ZSH
sudo apt update
sudo apt install python3 python3-pip
```
우아하게 버전확인 해봅니다. (좋네유)
```ZSH
python3 --version
```
## Hugo 설치
---
우분투 설치 버전을 설치합니다(행복)
```ZSH
sudo apt-get install hugo
```
__hugo 장점__ 

* 속도가 훨씬 빠르다. 
* Dynamic site에 __비해 만들기 쉽다.__
* 내가 만들고자 하는 블로그는 __방문객 입장에서 보면 READ ONLY__ 이다.

__간단한 설정 명령__

출처 : [IALY's BLOG](https://ialy1595.github.io/post/blog-construct-1/) 
```ZSH
hugo new site <프로젝트 이름>
cd <프로젝트 이름>
```
```ZSH
$ git clone <테마 github 페이지 주소> themes/<테마 이름>
```
`테마커스트마이징`
에도 도전하고 싶으나 Go, HTML, CSS, Java Script 벽에 부디치고 나중에 바로잡으려면 험한 꼴을 봅니다. Hogo Themes Gallary 멀리하고 테마는 하나만 써야 합니다. 

```ZSH
hugo server -D
```


## Github deploy
---
다양한 호스팅 가능하고, 저는 Github를 사용합니다. 다양한 장점이 있더이다.

[hosting on github](https://gohugo.io/hosting-and-deployment/hosting-on-github/)

명령어를 deploy.sh 운영가능 하고, 어찌저찌 makefile로 설정해서 가능하네요.

```makefile
# makefile in <프로젝트이름> 두고, .gitignore 설정도 필요 합니다. 
# make push 하면 알아서 올려줍니다. 
HUGO = hugo

COMMIT_MESSAGE = "rebuilding site $(shell date +%Y-%m-%d)"

push:
	echo "\033[0;32mDeploying updates to GitHub...\033[0m"

	rm -rf public 
	git clone https://github.com/yoseobmite/yoseobmite.github.io public
	$(HUGO) -D
	cd ./public && git add . && git commit -m $(COMMIT_MESSAGE) && git push
```

### 추가기능 설정 
이제 선택한 테마 에 추가기능을 넣어 봅니다.  
* [gist 설정](https://graspthegist.com/2017/01/20/using-github-gist-gem/)
* [utterances로 댓글추가 기능넣기](https://blog.outsider.ne.kr/1356?category=1)
* [Latex 용 Mathjax 변경하기](https://geoffruddock.com/math-typesetting-in-hugo/)
* [블러그 검색기 만들기](#만만치 않은 주제군요 언젠간 달겠져..)


### 글쓰기에 집중하기 냐, Go , Themes 를 더파볼지 고민 해야겠네요. 