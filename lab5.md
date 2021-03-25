# การทดลองที่ 5 เรื่อง การเขียนโปรแกรมเชื่อมต่อไวไฟและเว็บเซอร์เวอร์

## วัตถุประสงค์
1.เพื่อศึกษาการขียนโปรแกรมเชื่อมต่อไวไฟและเว็บเซอร์เวอร์

2.เพื่อศึกษาเกี่ยวกับไมโครคอนโทรเลอร์ที่เขียนโปรแกรมแล้วนำไปประยุกต์ใช้
### อุปกรณ์ที่ใช้
1.คอมพิวเตอร์

2.microcontroller ESP01

3.อุปกรณ์ต่อ USB-Serial

4.ตัวโปรแกรมเชื่อมต่อไวไฟและเว็บเซอร์เวอร์

#### ศึกษาข้อมูลเบื้องต้น

1.05 run wifi https://youtu.be/VX-QNQcO-b4

2.code 05_run_wifi https://github.com/choompol-boonmee/lab63b/blob/master/examples/05_Wifi-Web-Server/src/main.cpp

##### วิธีทำการทดลอง

1.เปิด cmd ซึ่งจากตัวอย่างอยู้ในโฟล์เดอร์ ใช้คำสั่ง cd pattani

2.ใช้คำสั่ง 05_run_wifi ตามด้วยคำสั่ง 1s

3.ใช้คำสั่ง vi src/main.cpp จะขึ้นโค้ดดังนี้

```
#include <ESP8266WiFi.h>
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "HI_BMFWIFI_2.4G";
const char* password = "0819110933";

ESP8266WebServer server(80);

int cnt = 0;

void setup(void){
	Serial.begin(115200);

	WiFi.mode(WIFI_STA);
	WiFi.begin(ssid, password);
	while (WiFi.status() != WL_CONNECTED) {
		delay(500);
		Serial.print(".");
	}
	Serial.print("\n\nIP address: ");
	Serial.println(WiFi.localIP());

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
   1.Setup เป็นการ connect กับไวไฟที่เราใส่ชื่อไว้ตอนแรก และ Setup webserver โดยให้แสดงผลเป็น Hello cntโดย cnt จะบวก 1 ขึ้นเรื่อยๆ

   2.Loop

6.อัพโหลดโปรแกรมเข้าไมโครคอนโทรเลอร์โดยการใช้คำสั่ง pio run-t upload

7.ต่อไมโครคอนโทรเลอร์เข้ากับคอมพิวเตอร์ด้วยการต่อ USBไปยังSerial

8.กดปุ่มลัพโหลดและกดปุ่มรีเซตที่ตัวไมโครคอนโทรเลอร์เพื่อให้ตัวโปรแกรมอัพโหลดเข้าไปในตัวไมโครคอนโทรเลอร์

9.รัน pio device monitor เพื่อดูผลลัพธ์ 

![image](https://user-images.githubusercontent.com/80880126/112267472-1898e600-8ca8-11eb-8d6b-b75ba6875a0a.png)


10.ต่อมาจะขึ้น ip address ให้ copy ip address ไปบราวเซอร์ในการทดสอบแล้วบันทึกผล

##### การบันทึกผลการทดลอง
จะเห็นว่าจากค่ำสั่ง pio device monitor เพื่อดูแลลัพธ์ของการรันโปรแกรมในไมโครคอนโทรเลอร์ได้ขึ้น ip address และเมื่อ copy ip address ไปที่บราวเซอร์ก็จะป็น Hello 1 โดย cnt ก็จะเปลี่ยนตัวแปรไปเรื่อย ๆ 

##### อภิปรายผลการทดลอง

จากการทดลองเขียนโปรแกรมสามารถสรุปได้ว่าเราสามารถให้ไมโครคอนโทรเลอร์เชื่อมต่อไวไฟและเว็บเซิร์ฟเวอร์สามารถทำงานได้เหมือนเว็บเสิร์ฟเวอร์ทั่วไป

##### คำถามหลังการทดลอง
