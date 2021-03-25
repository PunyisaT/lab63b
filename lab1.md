# การทดลองที่ 1 เรื่อง การเขียนโปรแกรมเพื่อรันบนไมโครคอนโทรเลอร์

## วัตถุประสงค์  
1.เพื่อศึกษาเกี่ยวกับการทำงานและการเชื่อมต่อเบื้องต้นของ microcontroller 

2.เพื่อศึกษาเกี่ยวกับการ run microcontroller

### อุปกรณ์ที่ใช้
1.คอมพิวเตอร์

2.microcontroller ESP01

3.อุปกรณ์ต่อ USB-Serial

#### ศึกษาข้อมูลเบื้องต้น
run example 1 https://youtu.be/NLIUsWLEpmg

code 01_Serial-Monitor https://github.com/choompol-boonmee/lab63b/blob/master/examples/01_Serial-Monitor/src/main.cpp

##### วิธีทำการทดลอง
1.เชื่อมต่อ microcontroller เข้ากับคอมพิวเตอร์ด้วยการต่อ  USBไปยังSerial

![image](https://user-images.githubusercontent.com/80880126/112262904-f9e32100-8ca0-11eb-9f47-268601cf5927.png)

2.เปิด cmd ซึ่งจากตัวอย่างอยู้ในโฟล์เดอร์  ใช้คำสั่ง cd pattani

![image](https://user-images.githubusercontent.com/80880126/112263053-3b73cc00-8ca1-11eb-9208-d6c6f034ab40.png)

 
3.ใช้คำสั่ง cd_Serial-Monitor ตามด้วย vi srv/main.cpp  เพื่อการดูและศึกษาโค้ดตัวอย่างโปรแกรมที่ใช้ในการทดสอบ microcontroller
 
 ```
#include <Arduino.h>

int cnt = 0;

void setup()
{
	Serial.begin(115200);
}

void loop()
{
	cnt++;
	Serial.printf("PATTANI :%d\n",cnt);
	delay(1000);
}

```
 
4.ใช้คำสั่ง vi platformio.ini เพื่อให้ทราบข้อมูลเกี่ยวกับ  microcontroller
 
5.ใช้คำสั่ง pio run -t upload เพื่อทำการรัน microcontroller

6.กดที่ปุ่มของ microcontroller เพื่อให้โปรแกรมรับคำสั่งใหม่เข้าไป
 
 ![image](https://user-images.githubusercontent.com/80880126/112263157-6a8a3d80-8ca1-11eb-95f8-a52ef839065b.png)

7.หากขึ้น success ให้ใช้สั่ง pio device monitor

8.รอดูผลการทดลอง

![image](https://user-images.githubusercontent.com/80880126/112263245-9279a100-8ca1-11eb-88c0-53347d55686d.png)


###### บันทึกผลการทดลอง
จากการใช้คำสั่ง vi platformio.ini ทำให้เราทราบข้อมูลดังนี้

1.เป็น platform ของ espressif8266

2.bord ชื่อ esp01_1m

3.ใช้วิธีการเขียนโปรแกรม arduino

##### อภิปรายผลการทดลอง
จากการใช้คำสั่ง vi src/main.cpp ทำให้เราทราบว่าโปรแกรม 01_Serial-Monitor มี 2 ส่วนดังนี้

1.ส่วน set up

2.ส่วน loop ที่เพิ่มจะเพิ่มตัวแปร count และแสดงผลของตัวแปรโดยมีการหน่วงเวลา 1000 ms โดยการทดลองการเขียนโปรแกรมเพื่อรันบนไมโครคอนโทรเลอร์จะเห็นว่าเมื่อเราอัพโหลดโปรแกรม 01_Serial-Monitor ลงในตัวไมโครคอนโทรเลอร์ที่เรียบร้อยแล้ว และใช้คำสั่ง pio device monitor เพื่อดูการทำงานของไมโทรคอนโทรเลอร์ หน้าจอจะขึ้นว่ามีการเปลี่ยนแปรทุกๆ 1 วินาทีตามโปรแกรม
#### คำถามหลังการทดลอง

ถ้าเราต้องการดูรายละเอียดของ platformio ต้องรันคำสั่งใด?

คำตอบ รันคำสั่ง vi platformio.ini
