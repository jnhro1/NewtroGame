# 스마트미러 구현하기

## 라즈비안 다운로드 받기
1. https://www.raspberrypi.org/downloads/raspbian/ 사이트에 들어가서 Raspbian Buster with desktop and recommended software 파일을 압축 파일인 ZIP파일로 다운로드를 받는다.  
2. os를 재설치하는 것이기 때문에 https://sd-card-formatter.kr.uptodown.com/windows 에 들어가서 sd카드를 포맷하여 기존의 운영체제 및 다른 작업들을 한 것을 지운다.  
3. SD Card Formatter를 실행한 후 해당 디바이스를 설치한 후 퀵 포맷을 누르고 포맷을 시작한다.  
4. balena Etcher 프로그램을 실행한 후 raspbian-buster-full.zip 파일을 알맞은 디바이스에 넣은 후 Flash를 누른다.  
5. os 다운로드가 완료되면 SD Card를 라즈베리파이에 꽂아서 부팅을 한다.  

## 라즈비안 부팅 설정하기
### 한글설정 하기
1. 리눅스를 열어 한글 설치를 준비한다.  
<code>
$ sudo apt-get update <Enter>  
$ sudo apt-get upgrade <Enter>  
$ sudo apt-get install ibus <Enter>  
$ sudo apt-get install ibus-hangul <Enter>  
$ sduo apt-get install fonts-unfonts-core <Enter>  
  </code>  
2. 기본 설정 들어가기  
오른쪽 상단에 라즈베리파이 모양 <클릭>  
    밑에서 3번째 기본설정 <클릭>  
    위에서 5번째 Raspberry Pi Configuration<클릭>  
    Localisation <클릭>  
    Locale에 Set Locale를 <클릭>  
    Language를 ko로 변경  
    Country를 KR(Republic of Korea)로 변경  
    Character Set을 UTF-8로 설정한 후 OK버튼 <클릭>  
    Timezone에 Set Timezone <클릭>  
    Area를 Asia로 변경  
    Location을 Seoul로 변경한 후 OK버튼 <클릭>  
    설정 창에서 OK버튼을 누른 후 reboot   
  
## 매직미러 설치  
1. node 8.11.1 버전 다운로드 (매우 중요)  
<code>
$ curl –sL https://deb.nodesource.com/setup_8.11.1 | sudo -E bash - <Enter>  
$ sudo apt-get install nodejs <Enter>
</code>  
2. npm 6.9.0 버전 다운로드 (매우 중요)  
<code>
$ sudo apt-get install npm <Enter>  
$ sudo npm install –g npm@6.9.0 <Enter>  
$ sudo node –v <Enter>  
v8. 11. 1 확인  
$ sudo npm –v <Enter>  
6. 9. 0 확인  
$ bash -c"$(curl –sL https://raw.githubusercontent.com/MichMich/MagicMirror/  
master/installers/raspberry.sh)“ <Enter> (설치 중 Y/n 두 개 전부 n입력)  
</code>  
  
## 매직미러 모듈 설치  
<code>
 $ sudo apt-get install libmagic-dev libatlas-base-dev sox libsox-fmt-all mpg321 libasound2-dev <Enter>  
  </code>
