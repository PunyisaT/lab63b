# การทดลองที่ 7 เรื่อง การใช้ไวไฟในการควบคุมหลอด LED
## วัตถุประสงค์

1.เพื่อคิดค้นการทำงานในการใช้ไมโครคอลโทรเลอร์ในการนำมาประยุกต์ใช้

2.เพื่อศึกษาเกี่ยวกับการเขียนโครงสร้างของ code program ในการนำมาประยุกต์ใช้

3.เพื่อศึกษาข้อมูลของแลปที่ผ่านมา

### อุปกรณ์ที่ใช้

1.ไมโทรคอนโทรเลอร์

2.อุปกรณ์ต่อ USB (USB to serial)

3.CPU

4.ตัวโปรแกรมเชื่อมต่อไวไฟ

5.อุปกรณ์ไฟฟ้าต่างๆ 

6.ไวไฟที่เชื่อมต่กับไมโครคอลโทรเลอร์

7.รีเลย์

#### ศึกษาข้อมูลเบื้องต้น
ศึกษาจากคลิปการทดลองที่ผ่านมา 

01 run example 1 https://youtu.be/NLIUsWLEpmg

02 run example 2 https://youtu.be/yBjab0UNuB8

03 run example 3 https://youtu.be/CCnN1WJsXQY

03 run relay https://youtu.be/6JnhaUILGuw

05 run wifi https://youtu.be/VX-QNQcO-b4

จาก source code จากอาจารย์

https://github.com/choompol-boonmee/lab63b/tree/master/examples


##### วิธีการทำทดลอง

1.เขียนโปรแกรมบนตัวไมโครคอลโทรเลอร์ ด้วยการนับไปเสียงเข้ากับ USB ไปยัง serial

2.ใช้คำสั่ง cd แล้วตามด้วยชื่อโฟล์เดอร์นั้นขั้นตอนนี้จะทำการเข้าโฟลเดอร์นั้น(ในตัอย่างที่นี่้ใช้คำสั่ง cd pattani เนื่องจากโฟลเดอร์ชื่อ pattani)
   ![image](https://user-images.githubusercontent.com/80880126/113139135-e7dd2180-9250-11eb-8a08-b60a4fa8d268.png)

3. เข้าไปในโปรแกรมทั้งสาม เพื่ออ่านและทำความเข้าใจรายละเอียด
	- พิมพ์ **cd 02_Scan-Wifi** *Enter*
	- พิมพ์ **cd 03_Output-Port** *Enter*
	
4. นำทุกตัวอย่างโปรแกรมมาพัฒนา  เป็นโปรแกรมตัวอย่างที่ 7 
	- พิมพ์ **cd 07_electrical-wifi** *Enter*
	- พิมพ์ **vi src/main.cpp** เพื่อเข้าสู่โปรแกรม** *Enter*

5.อ่านข้อมูลและรายละเอียดของ Source code โปรแกรมที่ 7
- ข้อมูลเบื้องต้นของ code
		- #include <name of header file> : การบอกให้นำเฮดเดอร์ไฟล์ทั้งสาม มาร่วมในการแปลโปรแกรม
		- const char* ssid : ชื่อไวไฟ
		- const char* password : รหัสผ่านของไวไฟ
		- IPAddress local_ip(...,...,...,...) :คือ IP address หรือ Network address
		- IPAddress gateway(...,...,...,...) : คือ Default gateway
		- IPAddress subnet(...,...,...,...) :คือ Subnet Mask
		- ESP8266WebServer server(...) : กำหนดให้งาน server ที่ port
	- ส่วนของ Set up
		- Serial.beginset up : กำหนดความเร็วของการ Set up 
		- set up : กำหนด Port 0 ของ Output หรือ Port ... ของ Input
		- pinMode :  แสดงถึง output
		- delay(...) : ความหน่วงเวลาของการ Set up

	- ส่วนที่  loop
		- digitalWrite(0,1) : อ่านค่าของ Port 0 ผลที่ได้จะมีค่าแค่ Low or High
		- delay(...) : ความหน่วงเวลาของการสแกนหาไวไฟและความหน่วงเวลาของการแชร์ไวไฟ
	- พิมพ์ **:q** เพื่อออกจากโปรแกรมที่ 7
	- *เนื้อหารายละเอียดของโปรแกรมที่แสดงใน platformio*
```
#include <ESP8266WiFi.h> 
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "Toey-Fern";
const char* password = "13121313";

//ปรับเปลี่ยน IPAddress ให้ตรงกับ Wifiของบ้านตัวเอง

IPAddress local_ip(192,168,1,45)  
IPAddress gateway(192,168,1,1);
IPAddress subnet(255, 255, 255, 0);

ESP8266WebServer server(80);

int cnt = 0;

void setup(void) {
       Serial.begin(115200);
       delay(10);
       
       pinMode(ledPin, OUTPUT);
             digitalWrite(ledPin, LOW);
      // Connect to WiFi network
      Serial.println();
      Serial.println();
      Serial.print("Connecting to ");
      Serial.println(ssid);
             WiFi.begin(ssid, password);
             while (WiFi.status() != WL_CONNECTED) {
             delay(500);
      Serial.print(".");
    }
      Serial.println("");
      Serial.println("WiFi connected")
  // Start the server
      server.begin();
      Serial.println("Server started");
 // Print the IP address
      Serial.print("Use this URL to connect: ");
      Serial.print("http://");
      Serial.print(WiFi.localIP());
      Serial.println("/");
}
void loop() {
     server.handleClient();
     // Wait until the client sends some data
    void loop()
{
	cnt++;
	if(cnt % 2) {
		Serial.println("========== ON ===========");
		digitalWrite(0, HIGH);
	} else {
		Serial.println("========== OFF ===========");
		digitalWrite(0, LOW);
	}
	delay(500);
}

}
##### การบันทึกผลการทดลอง

  
##### อภิปรายผลการทดลอง


##### คำถามหลังจากการทดลอง
