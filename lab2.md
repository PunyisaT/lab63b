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

2.เปิด cmd ใช้คำสั่งซึ่งจากตัวอย่างอยู่ในโฟล์เดอร์ pattani ใช้คำสั่ง cd

3.ใช้คำสั่ง 1s ตามด้วยค่ำสั่ง vi src/main.cpp เพื่อเตรียมอัพโหลดโปรแกรมใน microcontroller
![image](https://user-images.githubusercontent.com/80880126/112181568-22cace00-8c2f-11eb-8acd-444cdbe95bd4.png)


4.ใช้คำสั่ง pio run -t upload เพื่ออัพโหลดโปรแกรมเข้า microcontroller

5.ใช้คำสั่ง pio device monitor เพื่อเตรียมสแกนหา wifi

###### การบันทึกผลการทดลอง
จากการใช้คำสั่ง pio device monitor จะเริ่มสแกนหา wifi จะขึ้นเป็นจำนวนและชื่อของwifiที่ตรวจพบ
###### อภิปรายผลการทดลอง
จากการทดลองการเขียนโปรแกรมค้นหาไวไฟ จะเห็นว่าเมื่อเราอัพโหลดโปรแกรมลงในไมโครคอนโทรเลอร์ และใช้คำสั่ง pio device monitor เพื่อดูการทำงานของไมโครคอนโทรเลอร์และหน้าจอจะแสดงผลจำนวนและชื่อของwifiที่ตรวจพบ


###### คำถามหลังการทดลอง
