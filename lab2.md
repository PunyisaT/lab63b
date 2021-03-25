# การทดลองที่ 2 เรื่อง การเขียนโปรแกรมค้นหาไวไฟ

## วัตถุประสงค์
1.เพื่อศึกษา microcontroller ในการใช้ค้นหา wifi ที่อยู่รอบๆ

2.เพื่อศึกษาเกี่ยวกับการ run microcontroller

### อุปกรณ์ที่ใช้
1.คอมพิวเตอร์

2.microcontroller ESP01

3.อุปกรณ์ต่อ USB-Serial

4.เสาไวไฟ
#### ศึกษาข้อมูลเบื้องต้น
run example 2 https://youtu.be/yBjab0UNuB8

code 02_scan-wifi https://github.com/choompol-boonmee/lab63b/blob/master/examples/02_Scan-Wifi/platformio.ini

##### วิธีทำการทดลอง
1.เชื่อมต่อ microcontroller เข้ากับคอมพิวเตอร์ด้วยการต่อ  USB ไปยัง Serial

2.เปิด cmd ซึ่งจากตัวอย่างอยู้ในโฟล์เดอร์ ใช้คำสั่ง cd pattani 

3.ใช้คำสั่ง cd 02_scan-wifi

3.ใช้คำสั่ง 1s ตามด้วยค่ำสั่ง vi src/main.cpp เพื่อเตรียมอัพโหลดโปรแกรมใน microcontroller

```
#include <Arduino.h>
#include <ESP8266WiFi.h>

int cnt = 0;

void setup()
{
	Serial.begin(115200);
	WiFi.mode(WIFI_STA);
	WiFi.disconnect();
	delay(100);
	Serial.println("\n\n\n");
}

void loop()
{
	Serial.println("========== เริ่มต้นแสกนหา Wifi ===========");
	int n = WiFi.scanNetworks();
	if(n == 0) {
		Serial.println("NO NETWORK FOUND");
	} else {
		for(int i=0; i<n; i++) {
			Serial.print(i + 1);
			Serial.print(": ");
			Serial.print(WiFi.SSID(i));
			Serial.print(" (");
			Serial.print(WiFi.RSSI(i));
			Serial.println(")");
			delay(10);
		}
	}
	Serial.println("\n\n");
}
```


4.ใช้คำสั่ง pio run -t upload เพื่ออัพโหลดโปรแกรมเข้า microcontroller

5.ใช้คำสั่ง pio device monitor เพื่อเตรียมสแกนหา wifi

![image](https://user-images.githubusercontent.com/80880126/112263855-8c37f480-8ca2-11eb-863b-71d68cf96fa7.png)


##### การบันทึกผลการทดลอง

จากการใช้คำสั่ง pio device monitor จะเริ่มสแกนหา wifi จะขึ้นเป็นจำนวนและชื่อของwifiที่ตรวจพบ

##### อภิปรายผลการทดลอง

จากการทดลองการเขียนโปรแกรมค้นหาไวไฟ จะเห็นว่าเมื่อเราอัพโหลดโปรแกรมลงในไมโครคอนโทรเลอร์ และใช้คำสั่ง pio device monitor เพื่อดูการทำงานของไมโครคอนโทรเลอร์และหน้าจอจะแสดงผลจำนวนและชื่อของwifiที่ตรวจพบ

##### คำถามหลังการทดลอง
