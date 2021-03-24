# การทดลองที่ 6 เรื่อง การเขียนโปรแกรมสร้างไวไฟแอคเซสพอยต์ (Wifi AP)

## วัตถุประสงค์

1.เพื่อทราบการเขียนโปรแกรมสร้างไวไฟแอคแซสพอยต์

### อุปกรณ์ที่ใช้

1.ไมโทรคอนโทรเลอร์(ESP-01)

2.อุปกรณ์ต่อ USB (USB to serial)

3.CPU

4.ตัวโปรแกรมสร้างไวไฟแอคแซสพอยต์(06_Wifi-AP-Web-Server)

#### ศึกษาข้อมูลเบื้องต้น

1.06 run wiri AP https://youtu.be/T26DVHePlTs

2.src code ของโปรแกรม 06_Wifi-AP-Web-Server : https://github.com/choompol-boonmee/lab63b/blob/master/examples/06_Wifi-AP-Web-Server/src/main.cpp

##### วิธีการทำทดลอง

1.นำตัวไมโครคอนโทรเลอร์

2.เข้าcommand prompt

3.เข้าตัวอย่างโปรแกรมจากโดยใช้พิมพ์คำสั่ง cd pattani ในหน้า command prompt 

4.เลือกตัวอย่างโปรแกรมคำสั่ง cd 06_Wifi-AP-Web-Server ตามด้วยคำสั่ง pwd

5.พิมพ์คำสั่ง vi src/main.cpp เมื่อกด Enter จะขึ้นโค้ดดังนี้

```
#include <ESP8266WiFi.h>
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "MY-ESP8266";
const char* password = "choompol";

IPAddress local_ip(192, 168, 1, 1);
IPAddress gateway(192, 168, 1, 1);
IPAddress subnet(255, 255, 255, 0);

ESP8266WebServer server(80);

int cnt = 0;

void setup(void){
	Serial.begin(115200);

	WiFi.softAP(ssid, password);
	WiFi.softAPConfig(local_ip, gateway, subnet);
	delay(100);

	server.onNotFound([]() {
		server.send(404, "text/plain", "Path Not Found");
	});

	server.on("/", []() {
		cnt++;
		String msg = "Hello cnt: ";
		msg += cnt;
		server.send(200, "text/plain", msg);
	});

	server.begin();
	Serial.println("HTTP server started");
}

void loop(void){
  server.handleClient();
}

```

จากโค้ดนี้จะเห็นว่าเราต้องการให้ ESP 01 มีไวไฟในตัวเอง โดยจะกำหนดชื่อ wifi และ password ก่อน ที่จะปล่อยให้เครื่องอื่น connect และมีการสร้าง IPAddress,gateway,subnet และ เตรียมเว็บเซิร์ฟเวอร์

6.อัพโหลดโปรแกรมเข้าไมโครคอนโทรเลอร์ด้วยคำสั่ง pio run -t upload

7.กดปุ่มรีเซตที่ตัวไมโครคอนโทรเลอร์เพื่อให้ตัวโปรแกรมอัพโหลดเข้าไปในตัวไมโครคอนโทรเลอร์

8.pio device monitor เพื่อดูผลลัพธ์

![image](https://user-images.githubusercontent.com/80880126/112267769-87763f00-8ca8-11eb-95fb-a1954d9ab226.png)

9.ลองใช้โทรศัพท์มือถือหรือคอมพิวเตอร์ค้นหาไวไฟที่สร้าง

##### การบันทึกผลการทดลอง
1.เขียนโปรแกรมไมโครคอนโทรเลอร์ด้วยโปรแกรม 06_Wifi-AP-Web-Server

  จะเห็นว่าจากคำสั่ง pio device monitor เพื่อดูผลลัพธ์ของการรันโปรแกรมในโปรแกรมไมโครคอนโทรเลอร์ ได้ขึ้นคำว่า ver started และตรวจสอบผลลัพธ์จากการสร้างไวไฟด้วยการสร้างไวไฟด้วยการหาชื่อไวไฟด้วย
  โทรศัพท์มือถือ ซึ่งจากการค้นหาด้วยโทรศัพท์มือถือก็ได้พบกับไวไฟที่เราสร้างจากการเขียนโปรแกรม 
  
##### อภิปรายผลการทดลอง
จากการทดลองโปรแกรมสามารถสรุปได้ว่า เราสามารถสร้างไวไฟแอคแซสพอยต์ขึ้นมาเองเพื่อให้ผู้อื่นมาเชื่อมต่อได้

##### คำถามหลังจากการทดลอง
