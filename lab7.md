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

5.ไวไฟที่เชื่อมต่กับไมโครคอลโทรเลอร์

6.รีเลย์

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
	- พิมพ์ **cd 07_LED-wifi** *Enter*
	- พิมพ์ **vi src/main.cpp** เพื่อเข้าสู่โปรแกรม** *Enter*

5.อ่านข้อมูลและรายละเอียดของ Source code โปรแกรมที่ 7
        - ข้อมูลเบื้องต้นของ code
		- delay(...) : ความหน่วงเวลาของการ Set up

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

void loop() {
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
 
  int value = LOW;
  if (request.indexOf("/LED=ON") != -1) {
  digitalWrite(ledin, HIGH);
  value = HIGH;
  
  if (request.indexOf("/LED=OFF") != -1) {
  digitalWrite(ledPin, LOW);
  value = LOW;
 }
  if(value == HIGH) { 
  client.print("On");
  }else { client.print("Off");
delay(1);

```
6.อัปโหลดโปรแกรม 07_LED-wifi เข้าไปยังไมโครคอนโทรเลอร์
 - ใช้คำสั่ง **pio run -t upload** เพื่ออัพโหลด
 - กดปุ่มสีดำ เพื่ออัพโหลดโปรแกรม
 - กดปุ่มแดง เพื่อ reset โปรแกรมหยุดทำงานและเริ่มรันโปรแกรมใหม่

7.นำ microcontroller ที่เขียนโปรแกรมไว้แล้วมาต่อเข้ากับตัวรีเรย์

8.นำตัวรีเรย์ต่อเข้ากับขั้วชาร์ต(ใช้รีเลย์มาช่วยในการเปิด-ปิดสวิตซ์สามารถนำไปประยุกต์ใช้กับอุปกรณ์ที่ใช้ไฟฟ้าได้ ซึ่งรีเลย์จะทำงานทุกๆครึ่งวินาที)

9. กดปุ่มสีดำ เพื่ออัพโหลดโปรแกรม

10. ตรวจสอบผลลัพธ์ของโปรแกรม    
      - พิมพ์ **pio device monitor** เพื่อดูผลลัพธ์
      - กดปุ่มแดง เพื่อ reset โปรแกรมหยุดทำงานและเริ่มรันโปรแกรมใหม่

11.ทำการกดปุ่มสีดำ เพื่อให้อัพโหลด
      - เมื่อโปรแกรมอัพโหลดสำเร็จ โปรแกรมโดยจะส่งลิงค์ URL มาให้แล้วนำที่ได้ไปใส่ที่ Web Browser

12. ใช้คำสั่ง  pio device monitor เพื่อดูผลลัพธ์
      
##### การบันทึกผลการทดลอง
จากการทดลองได้ลองทำการปรับเปลี่ยนcode programจากเดิม  ซึ่งโครงสร้างเกี่ยวกับโค้ดในการสร้างwifi จากแลป5 นอกจากนี้ยังได้ทำการใช้ตัวรีเรย์ในการควบคุมจากการทดลองที่ 3 และแทรกหลอด LED เปล่งแสงผ่านการเชื่อมต่อ port ต่างๆที่ได้นำความรู้จากการทดลองที่ 4 จากนั้นจึงทำการใช้สัญญาณอินพุตเป็นสัญญาณไวไฟแล้วนำไปใช้ในการควบคุมการเปิดปิดของหลอด LED
##### อภิปรายผลการทดลอง
จากการทำการทดลอง การพัฒนา Source Code ของโปรแกรม โดยการนำ Source Code ของ 3 4 และ 5 โปรแกรม มาดัดแปลงและแก้ไขให้เกิดโปรแกรมในรูปแบบใหม่ พบว่า โปรแกรมใหม่ที่รันใส่ลงในไมโครคอนโทรเลอร์ สามารถใช้ไวไฟในการควบคุมการเปิด-ปิดตัวหลอดไฟ LED ได้
##### คำถามหลังจากการทดลอง
สามารถนำไปใช้นำไปประยุกต์กับอุปกรณ์อืนได้หรือไม่
 - สามารถใช้ได้แต่อาจจะต้องมีการเปลี่ยนโค้ด
