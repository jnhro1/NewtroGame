# 스마트미러 구현하기

## 1. 라즈비안 다운로드 받기
* https://www.raspberrypi.org/downloads/raspbian/ 사이트에 들어가서 Raspbian Buster with desktop and recommended software 파일을 압축 파일인 ZIP파일로 다운로드를 받는다.  
* os를 재설치하는 것이기 때문에 https://sd-card-formatter.kr.uptodown.com/windows 에 들어가서 sd카드를 포맷하여 기존의 운영체제 및 다른 작업들을 한 것을 지운다.  
* SD Card Formatter를 실행한 후 해당 디바이스를 설치한 후 퀵 포맷을 누르고 포맷을 시작한다.  
* balena Etcher 프로그램을 실행한 후 raspbian-buster-full.zip 파일을 알맞은 디바이스에 넣은 후 Flash를 누른다.  
* os 다운로드가 완료되면 SD Card를 라즈베리파이에 꽂아서 부팅을 한다.  

## 2. 라즈비안 부팅 설정하기
### 한글설정 하기
1. 리눅스를 열어 한글 설치를 준비한다.  
> $ sudo apt-get update <Enter>  
> $ sudo apt-get upgrade <Enter>  
> $ sudo apt-get install ibus <Enter>  
> $ sudo apt-get install ibus-hangul <Enter>  
> $ sduo apt-get install fonts-unfonts-core <Enter>  

2. 기본 설정 들어가기  
* 오른쪽 상단에 라즈베리파이 모양 <클릭>  
* 밑에서 3번째 기본설정 <클릭>  
* 위에서 5번째 Raspberry Pi Configuration<클릭>  
* Localisation <클릭>  
* Locale에 Set Locale를 <클릭>  
* Language를 ko로 변경  
* Country를 KR(Republic of Korea)로 변경  
* Character Set을 UTF-8로 설정한 후 OK버튼 <클릭>  
* Timezone에 Set Timezone <클릭>  
* Area를 Asia로 변경  
* Location을 Seoul로 변경한 후 OK버튼 <클릭>  
* 설정 창에서 OK버튼을 누른 후 reboot   
  
## 3. 매직미러 설치  
1. node 8.11.1 버전 다운로드 (매우 중요)  
> $ curl –sL https://deb.nodesource.com/setup_8.11.1 | sudo -E bash - <Enter>  
> $ sudo apt-get install nodejs <Enter>

2. npm 6.9.0 버전 다운로드 (매우 중요)  
> $ sudo apt-get install npm <Enter>  
> $ sudo npm install –g npm@6.9.0 <Enter>  
> $ sudo node –v <Enter>  
> v8. 11. 1 확인  
> $ sudo npm –v <Enter>  
> 6. 9. 0 확인  
> $ bash -c"$(curl –sL https://raw.githubusercontent.com/MichMich/MagicMirror/master/installers/raspberry.sh)" <Enter>  
> (설치 중 Y/n 두 개 전부 n입력)  
  
## 4. 매직미러 모듈 설치  
> $ sudo apt-get install libmagic-dev libatlas-base-dev sox libsox-fmt-all mpg321 libasound2-dev <Enter>  
  
### 4-1. 매직미러 핫워드 설치  
> $ cd ~/MagicMirror/modules/ <Enter>  
> $ git clone https://github.com/eouia/MMM-Hotword.git <Enter>  
> $ cd MMM-Hotword <Enter>  
> $ chmod +x ./installer/install.sh <Enter>  
> $ ./installer/install.sh <Enter>  
  
### 4-2. 매직미러 어시스턴트 설치  
> $ cd ~/MagicMirror/modules/ <Enter>  
> $ git clone https://github.com/eouia/MMM-AssistantMk2 <Enter>  
> $ cd MMM-AssistantMk2 <Enter>  
> $ npm install <Enter>  
> $ cd scripts <Enter>  
> $ chmod +x *.sh <Enter>  
> $ cd .. <Enter>  
> $ npm install --save-dev electron-rebuild <Enter>  
> $ ./node_modules/.bin/electron-rebuild <Enter>(시간이 매우 오래 걸립니다.)  
  
## 5. 구글 어시스턴트 모듈  
* 새로운 홈페이지를 열어서 https://console.actions.google.com 에 들어간다.
* 구글에 로그인을 한 후 +버튼을 눌러 새로운 프로젝트를 만든다.
* New Project에 Project Name을 입력한 후 CREATE PROJECT 버튼을 누른다.
* 새로운 프로젝트를 성공적으로 만들었으면 https://console.cloud.google.com에 들어간다.
* 구글 클라우드 플랫폼에 들어가면 오른쪽 상단에 있는 생성된 프로젝트를 선택한다.
* 구글 클라우드 플랫폼 검색창에 Google Assistant API를 검색하여 Google Assistant API를 활성화 시킨다.
* 활성화 후 왼쪽 상단 4번째 Credentials를 눌러서 CONFIGURE CONSENT SCREEN을 누른 후 Application name와 Support email에서 이름과 이메일을 입력 후 저장한다.  
* Create credentials를 선택한 후 OAuth client ID를 만든다.
* Applications type를 other로 선택하고 이름을 적어 생성한다.
* OAuth client 생성하면서 생긴 팝업에서 ok를 누르고 Credentials에서 새로 생성한 OAuth 2.0 client IDs에 새로 생성한  
  OAuth client ID가 있는지 확인한다.  
* 새로 생성한 OAuth client ID의 Client ID를 복사한 후 json포멧으로 다운로드 한다.
* 라즈베리파이 터미널로 들어온다.
> $ mv ~/Downloads/<Client ID> credentials.json <Enter>
> $ node auth_and_test.js <Enter> (이것을 실행하면 구글 로그인 브라우저가 켜진다.)
* 로그인을 한 후 클라이언트 인증 과정에 동의를 하여 구글 계정키를 복사한다.
> $ <복사 한 계정키>
> $ mv token.json ./profiles/default.json

## 6. 구글 어시스턴트 모듈 설정 값 수정  
* 라즈베리파이의 파일을 열어서 home/ pi/ MagicMirror/ config/ config.js 파일을 연다.  
* config.js 파일을 연 후 밑에서 3번째, }, 이후에 아래의 소스를 붙여준 후 저장한다.  
<code>
{
	module: "MMM-Hotword",
	position: "top_right",
	config: {
		chimeOnFinish: null,
		mic: {
			recordProgram: "arecord",
			device: "plughw:1"
		},
		models: [
			{
				hotwords    : "smart_mirror",
				file        : "smart_mirror.umdl",
				sensitivity : "0.5",
			},
		],
		commands: {
			"smart_mirror": {
				notificationExec: {
					notification: "ASSISTANT_ACTIVATE",
					payload: (detected, afterRecord) => {
						return {profile:"default"}
					}
				},
				restart:false,
				afterRecordLimit:0
			}
		}
	}
},
{
	module: "MMM-AssistantMk2",
	position: "top_right",
	config: {
		deviceLocation: {
			coordinates: {
				latitude: 37.5650168, // -90.0 - +90.0
				longitude: 126.8491231, // -180.0 - +180.0
			},
		},
		record: {
			recordProgram : "arecord",  
			device        : "plughw:1",
		},
		notifications: {
			ASSISTANT_ACTIVATED: "HOTWORD_PAUSE",
			ASSISTANT_DEACTIVATED: "HOTWORD_RESUME",
		},
		useWelcomeMessage: "brief today",
		profiles: {
			"default" : {
				lang: "en-US"
			}
		},
	}
},
</code>  
* 한국어로 패치를 위해서 “en-US”를 “KR”로 변경을 한다.

## 7. 마이크 및 스피커 연결  
* 라즈베리파이에 usb형 마이크를 연결한다.  
* 라즈베리파이에 usb형 스피커를 연결하며 라즈베리파이의 HDMI 옆 오디오 단자에 동그란 선을 연결한다.  
* 연결한 후 라즈베리파이 터미널을 열고 마이크 설정을 위한 명령어를 입력한다.  
> $ arecord –l <Enter>  
* 다음에 출력되는 값을 확인한다.  
-> 조하롭게 팀은 card 1과 device 0으로 설정되어있다.  
* 마이크 설정을 위한 명령어를 확인 후 스피커 설정을 위한 명령어를 입력한다.  
> $ aplay –l <Enter>  
* 다음에 출력되는 값을 확인한다.  
-> 조하롭게 팀은 card 1과 device 0으로 설정되어있다.  
* 값을 확인한 후 터미널에서 다음의 명령어를 입력하여 새로운 nano 파일을 만들어준다.  
> $ nano .asoundrc <Enter>  
* 새로 열린 nano 파일에 다음의 소스를 입력해준다.  
<code>
 pcm.!default {

  type asym

  capture.pcm "mic"

  playback.pcm "speaker"

}

pcm.mic {

  type plug

  slave {

    pcm "hw:<card number>,<device number>"

  }

}

pcm.speaker {

  type plug

  slave {

    pcm "hw:<card number>,<device number>"

  }

}
</code>  
* 조하롭게 팀은 <card number>와 <device number>가 스피커, 마이크 모두 1,0이였기 때문에 1,0을 넣었다.  
* 스피커 설정이 확인하기 위해서는 다음의 명령어를 입력한다.  
> $ speaker-test -t wav <Enter>  
* Front와 Left가 반복 재생된다.   
* 마이크 테스트를 하기 위해서는 다음의 명령어를 입력한 후 녹음을 한다.  
> $ arecord --format=S16_LE --duration=5 --rate=16000 —file-type=raw out.raw <Enter>  
* 녹음한 소리가 듣고 싶다면 다음의 명령어를 입력한다.  
> $ aplay --format=S16_LE --rate=16000 out.raw  <Enter>  
* 소리가 작거나 클 경우 다음의 명령어를 입력하여 저장한다.  
> $ alsamixer  <Enter>  
* 키보드의 방향키로 스피커와 마이크를 조절한다.  

## 8. 매직미러 테스트하기
> $ cd ~/MagicMirror  <Enter>  
> $ npm start  <Enter>  

### 출처 및 참고 자료  
> https://m.blog.naver.com/swlee7077/220922548791 (레트로 파이 설정)
> https://cccding.tistory.com/96 (라즈비안 한글 설정)
> https://github.com/makepluscode/rpi-tutorial-advanced/tree/master/008-raspbian-magicmirror-google-assistant-latest (매직미러)
> https://github.com/makepluscode/rpi-tutorial-advanced/tree/master/008-raspbian-magicmirror-google-assistant-latest (스피커, 마이크)
