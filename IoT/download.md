# ESP 8266 설치, ESP 32 설치

[Arduino IDE]

File → Preferences → Additional boards manager URLs → OK

ESP 8266

<aside>
💡 http://arduino.esp8266.com/stable/package_esp8266com_index.json

</aside>

ESP 32

<aside>
💡 https://dl.espressif.com/dl/package_esp32_index.json

</aside>

Tools → Board → Board Manager → ESP8266/ESP32 검색 후 설치

# Arduino IDE 설치

https://www.arduino.cc/en/software/

[참고](https://m.blog.naver.com/PostView.naver?blogId=roboholic84&logNo=221227871374&proxyReferer=https:%2F%2Fm.blog.naver.com%2FPostView.naver%3FblogId%3Droboholic84%26logNo%3D221238141049%26proxyReferer%3Dhttps:%252F%252Fm.search.naver.com%252Fsearch.naver%253Fquery%253Dmqtt%252B%2525EB%25259D%2525BC%2525EC%2525A6%252588%2525EB%2525B2%2525A0%2525EB%2525A6%2525AC%2525ED%25258C%25258C%2525EC%25259D%2525B4%252B%2525EC%252584%2525A4%2525EC%2525B9%252598%2526where%253Dm%2526sm%253Dmob_hty.idx%2526qdt%253D1) [참고](https://makeutil.tistory.com/210)

1. mosquitto 인증키 다운로드

```jsx
wget http://repo.mosquitto.org/debian/mosquitto-repo.gpg.key
sudo apt-key add mosquitto-repo.gpg.key
```

 → warring 떠도 설치에 상관 없으니 무시해도 됨.

1. mosquitto 저장소 패키지 등록

```jsx
cd /etc/apt/source.list.d/
sudo wget http://repo.mosquitto.org/debian/mosquitto-stretch.list
```

1. MQTT 브로커 설치

```jsx
sudo apt-get update
sudo apt-cache search mosquitto
sudo apt-get install mosquitto mosquitto-clients
```

— 설치 완료 —

1. mosquitto 실행

```jsx
sudo /etc/init.d/mosquitto -v
```

1.