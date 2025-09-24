# ใบงานที่ 2 : การใช้ Analog to Digital Converter (ADC)

## วัตถุประสงค์
1. เพื่อศึกษาการทำงานของ ADC  
2. เพื่อเขียนโปรแกรมอ่านค่า Analog จากอุปกรณ์รับสัญญาณ เช่น Potentiometer และ LDR  
3. เพื่อทำความเข้าใจการปรับปรุงความแม่นยำของ ADC  

---

## อุปกรณ์ที่ใช้
- ESP32 Development Board  
- Potentiometer 10kΩ  
- LDR + Resistor 10kΩ  
- สาย Jumper, Breadboard  

---

## ภาคทฤษฎี

### ความรู้พื้นฐาน ADC
- **ADC (Analog to Digital Converter)** คือวงจรที่ใช้แปลงสัญญาณแอนะล็อก → ดิจิทัล  
- ESP32 มี ADC ความละเอียด 12 บิต (0–4095)  
- ย่านแรงดันที่วัดได้: 0–3.3V  

### คุณสมบัติ ADC ของ ESP32
- ความละเอียด: 12 บิต (0–4095)  
- จำนวนช่องสัญญาณ: 18 ช่อง  
- ช่วงแรงดันไฟฟ้า: 0–3.3V  

### สูตรคำนวณ
```
V = (ADC Value / 4095) × 3.3
```

---

## ภาคปฏิบัติ

### 🔹 การทดลองที่ 1 – Potentiometer
**วงจร:**  
- ขา VCC → 3.3V  
- ขากลาง → ADC1_CH0 (GPIO36)  
- ขา GND → GND  

**โค้ดตัวอย่าง:**
```cpp
int adcValue = analogRead(36);
float voltage = adcValue * (3.3 / 4095.0);
Serial.println(voltage);
```

**คำถาม:**  
1. ค่า ADC เปลี่ยนแปลงอย่างไรเมื่อหมุน Potentiometer?  
2. ค่าแรงดันไฟฟ้าที่คำนวณได้ตรงกับทฤษฎีหรือไม่?  

**ตารางบันทึกผล:**

| ตำแหน่ง Potentiometer | ค่า ADC | ค่าแรงดัน (V) |
|------------------------|---------|----------------|

---

### 🔹 การทดลองที่ 2 – LDR
**วงจร:**  
- LDR + Resistor 10kΩ ต่อแบบ Voltage Divider  
- อ่านค่าแรงดันที่ขา ADC  

**หลักการทำงาน:**  
- ความต้านทาน LDR ลดลงเมื่อมีแสง → ค่า ADC สูงขึ้น  
- ความต้านทาน LDR สูงขึ้นเมื่อมืด → ค่า ADC ลดลง  

**โค้ดตัวอย่าง:**
```cpp
int adcValue = analogRead(36);
float voltage = adcValue * (3.3 / 4095.0);
Serial.println(voltage);
```

---

### 🔹 การทดลองที่ 3 – ปรับปรุงความแม่นยำ ADC
**เทคนิคที่ใช้:**
1. Oversampling – อ่านค่า ADC หลายครั้งแล้วหาค่าเฉลี่ย  
2. Calibration – ปรับเทียบกับแรงดันอ้างอิงที่ทราบค่า  
3. Filtering – ใช้ Moving Average Filter  

**โค้ดตัวอย่าง (Oversampling):**
```cpp
long sum = 0;
for (int i = 0; i < 100; i++) {
  sum += analogRead(36);
}
int avgValue = sum / 100;
float voltage = avgValue * (3.3 / 4095.0);
Serial.println(voltage);
```

---

## บทสรุป
- ESP32 สามารถอ่านค่า Analog ได้ด้วย ADC ที่มีความละเอียด 12 บิต  
- สามารถนำไปประยุกต์ใช้กับเซนเซอร์ต่าง ๆ เช่น Potentiometer, LDR  
- การใช้ Oversampling และ Filtering ช่วยเพิ่มความแม่นยำของผลการอ่าน  
