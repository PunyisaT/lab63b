# การทดลองที่ 7 เรื่อง การใช้ไวไฟในการควบคุมอุปกรณ์ต่างๆ
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

04 run example 4 https://youtu.be/nFqoZT26U5k

05 run wifi https://youtu.be/VX-QNQcO-b4

06 run wiri AP https://youtu.be/T26DVHePlTs

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

	- ส่วนที่ 3 loop
		- WiFi.scanNetworks() : จำนวน Network หรือผลของการสแกนไวไฟรอบๆ
		- digitalWrite(0,...) : อ่านค่าของ Port 0 ผลที่ได้จะมีค่าแค่ Low or High
		- delay(...) : ความหน่วงเวลาของการสแกนหาไวไฟและความหน่วงเวลาของการแชร์ไวไฟ
	- พิมพ์ **:q** เพื่อออกจากโปรแกรมที่ 7
	- *เนื้อหารายละเอียดของโปรแกรมที่แสดงใน platformio*
```
#include <ESP8266WiFi.h> 
//#include <WiFiClient.h>
#include <ESP8266WebServer.h>

const char* ssid = "LAB-7";
const char* password = "1234567890";

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
// Check if a client has connected
WiFiClient client = server.available();
if (!client) {
return;
}
// Wait until the client sends some data
Serial.println("new client");
while(!client.available()){
delay(1);
}
// Read the first line of the request
String request = client.readStringUntil('\r'); 
Serial.println(request);
client.flush();
// Match the request
int value = LOW;
if (request.indexOf("/LED=ON") != -1) {
digitalWrite(ledPin, HIGH);
value = HIGH;
}
if (request.indexOf("/LED=OFF") != -1) {
digitalWrite(ledPin, LOW);
value = LOW;
}
// Set ledPin according to the request
//digitalWrite(ledPin, value);
// Return the response
client.println("HTTP/1.1 200 OK");
client.println("Content-Type: text/html");
client.println(""); // do not forget this one
client.println("<!DOCTYPE HTML>");
client.println("<html>");
client.print("Led pin is now: ");
if(value == HIGH) { 
client.print("On");
}else { client.print("Off");
}
client.println("<br><br>");
client.println("<a href=\"/LED=ON\"\"><button>Turn Off </button></a>");
client.println("<a href=\"/LED=OFF\"\"><button>Turn On </button></a><br />");
client.println("</html>");
delay(1);
Serial.println("Client disonnected");
Serial.println("");
##### การบันทึกผลการทดลอง

  
##### อภิปรายผลการทดลอง


##### คำถามหลังจากการทดลอง
