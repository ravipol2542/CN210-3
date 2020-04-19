# รายงานวิชาCN210
## สรุปเนื้อหาการบ้านครั้งที่ 1
**MIPS Instruction format**
MIPS คอมพิวเตอร์ชนิด RISC ผลิตโดย MIPS Technologies ซึ่งในทุกๆคำสั่งของ MIPS จะมีขนาด 32 bit ประกอบด้วย 3 ชนิดด้วยกัน คือ
**R-Format** -เป็นคำสั่งที่ใช้ดำเนินการทางคณิตศาสตร์และตรรกศาสตร์ มีโครงสร้างต่อไปนี้

|**R-Format**|     |     |     |     |     |
|------------|-----|-----|-----|-----|-----|
|     op     | $rs | $rt | $rd |shamt|func |
|     6bit   | 5bit| 5bit| 5bit|5bit |6bit |

|**คำสั่ง**    |     |     |     |     |     |
|------------|-----|-----|-----|-----|-----|
|ALU         | alu | $rd | $rs | $rt |     |
|JR          | jr  | $rs |     |     |     |

สำหรับ R-Format นั้นส่วนใหญ่จะเป็นคำสั่งทางคณิตศาสตร์ เช่น คำสั่ง add เพื่อบวกกัน sub เพื่อลบ
โดยขั้นตอนในการทำคำสั่ง ADD นั้นจะมีดังนี้
1. ทำการอ่าน op-code ซึ่งสำหรับ R-Format จะเป็น 000000
2. เช็คว่า func มีค่าเป็นอะไรเช่น หาก func มีค่าเป็น 100000 จะเป็นคำสั่ง ADD
3. จากตัวอย่างที่ยกมา ให้ค่า func เป็นคำสั่ง ADD จะทำการบวกกันโดยนำค่าที่เก็บอยู่ใน $rs และ $rt มาทำการบวกกัน หลังจากนั้นนำผลลัพธ์ที่ได้ไปเก็บใน $rd

**I-Format** -เป็นคำสั่งที่ใช้ย้ายข้อมูล

|**I-Format**|     |     |     |
|------------|-----|-----|-----|
|     op     | $rs | $rt | offset |
|     6bit   | 5bit| 5bit| 16bit  |

|**คำสั่ง**    |     |     |        |         | 
|------------|-----|-----|---------|--------|
|ALUi        |alui | $rt | $rs     | value   |
|Data Transfer | lw | $rt | offset($rs)  |   |
|             |  sw | $rt | offset($rs)  |   |
|Branch      |  beq | $rs | $rt | offset |   | 


**J-Format** -เป็นคำสั่งสำหรับ jump จาก Address ปัจจุบัน ไปยังอีกตำแหน่งนึง

|**J-Format**|     |
|------------|-----|
|     op     | Address |
|     6bit   | 26bit|

|**คำสั่ง**    |           |
|------------|------------|
| jump       |  j address|
| jump&link  |jal address|

### งานครั้งที่ 1
  [คลิปงานครั้งที่ 1](https://www.youtube.com/watch?v=uxKd0FtUXx8&t=9s)

## สรุปเนื้อหาการบ้านครั้งที่ 2
**การทำงานของ CPU**
ภาษาที่มนุษย์ใช้สื่อสารกับคอมพิวเตอร์เช่น JAVA,Python,C++ นั้นจริงๆแล้วคอมพิวเตอร์จะทำการแปลง คำสั่งต่างๆที่เราเขียนผ่านภาษาเหล่านี้ เป็นภาษาคอมพิวเตอร์ก่อน 

###### คำสั่งที่มนุษย์ใช้สื่อสาร/สั่งการคอมพิวเตอร์ 
    public class Test{
        public static void (String[]args){
              int a = 10;
              int b = 20;
              int c = a + b; 
        }
    } 

ภาษาข้างบนนั้นเป็นภาษา Java หากภาษาที่คอมพิวเตอร์สามารถเข้าใจหรือ Machine Language จะเป็น 
                >00000000:           08400000  

ซึ่งแปลความได้ว่า  j  01000000  คอมพิวเตอร์ MIPS จะมีขั้นตอนการดำเนินคำสั่งตามตัวอย่างต่อไปนี้
1. เมื่อผู้ใช้ทำการเปิดคอมพิวเตอร์ คอมพิวเตอร์จะมาที่ address ที่ 00000000 เพื่ออ่านคำสั่งซึ่งก็คือ คำสั่ง 08400000 เป็นเลขฐาน 16
2. MIPS จะทำการเปลี่ยนคำสั่งเลขฐาน 16 เป็นเลขฐาน 2 จะได้ 00001000010000000000000000000000
3. Decoder ทำการอ่านคำสั่งที่แปลงเป็นเลขฐาน 2 โดยจะเริ่มอ่าน 6 บิทแรกก่อน จากตัวอย่างคือ 000010 เป็น J-Format
4. 26 บิทที่เหลือคือตำแหน่ง Address ที่จะทำการ JUMP โดยนำเลข 26 บิทนั้นมาเติม 0 ข้างหน้า 4 ตัวและข้างหลัง 2 ตัว จะได้ Address ที่มีขนาด 32 บิท
นั่นก็คือ Address 01000000

                00000004:           1A000000
                
5. Address ถัดมาจะลงท้ายด้วย 4 เนื่องจาก 1 คำสั่งมีขนาด 4 byte 
6. คอมพิวเตอร์ได้ทำการ JUMP มาที่ Address 01000000 ซึ่งมีคำสั่ง 8C090004 ดังบรรทัดด้านล่าง

                01000000:           8C090004


7. คอมพิวเตอร์จะทำการแปลงคำสั่งเลขฐาน 16 ให้เป็นเลขฐาน 2 ได้ 0001100000010010000000000000100
8. Decoder ทำการอ่าน 6 bit แรกคือ 000110 พบว่าเป็น I-Format คำสั่ง lw (load word) 5 bits ถัดมาคือ 00000 คือ $rs=0 5 bits ถัดมาคือ 01001 $rt=9 และ    16 bits ที่เหลือคือ 0000000000000100 หมายความว่า offset = 4 ซึ่งหลักการของ lw นั้นคือการ นำค่า offset ไปบวกกับ $rs จะได้ Address ที่ต้องไปดึงข้อมูลออก    มาอ่านและเก็บ ใน $rt 

### งานครั้งที่ 2
  [คลิปงานครั้งที่ 2](https://www.youtube.com/watch?v=afBzikgJ6VA)



## สรุปเนื้อหาการบ้านครั้งที่ 3
 **ความแตกต่างระหว่าง Single Cycle และ Multi Cycle**
 ![singlecycle](https://slideplayer.com/slide/4943101/16/images/5/Single+Cycle+Implementation.jpg)
 ##### ลักษณะของ Single Cycle มีดังนี้
    - มี ALU 3 ตัว
    - มี Memory 2 ตัว
    - 1 คำสั่ง = 1 Cycle
    - ในทุกๆคำสั่งใช้เวลาเท่ากันทำให้ใช้เวลามากเนื่องจากจะใช้เวลาของคำสั่งที่ทำงานใช้เวลามากสุด
    
![multicycle](https://people.cs.pitt.edu/~don/coe1502/current/Unit4a/fig548.jpg)
##### ลักษณะของ Multi Cycle มีดังนี้
    - มี ALU เพียงตัวเดียว
    - มี Memory เพียงตัวเดียว
    - มีการพักข้อมูลที่ตำแหน่ง a และ b ในรูป
    - ใช้เวลาแต่ละคำสั่งไม่เท่ากัน
    - มี ALUout ที่เก็บค่าหลังจากคำนวณ
 
 ### งานครั้งที่ 3
  [คลิปงานครั้งที่ 3](https://www.youtube.com/watch?v=D7P8hxrkiEY)










* งานครั้งที่ 4
  [คลิปงานครั้งที่ 4](https://www.youtube.com/watch?v=dOmY8EDUxi0&t=19s)
  อธิบายการทำงานของคำสั่ง lw ใน cycle
* งานครั้งที่ 5
  [คลิปงานครั้งที่ 5](https://www.youtube.com/watch?v=IAmkzRGe4yQ&t=14s)
  อธิบายการทำงานของคำสั่ง beq ใน cycle
* งานครั้งที่ 6
  [คลิปงานครั้งที่ 6](https://www.youtube.com/watch?v=kINS_f38R6I&t=9s)
* งานครั้งที่ 7
  [คลิปงานครั้งที่ 7](https://www.youtube.com/watch?v=NQ4u19d90rE&t=1s)
