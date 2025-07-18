# 🤖 مشروع روبوت يمشي باستخدام 6 محركات سيرفو - Humanoid Walking Robot (Arduino UNO)

## 📌 وصف المشروع | Project Description

هذا المشروع يهدف إلى التحكم في روبوت بشري بسيط (Humanoid) يستخدم **6 محركات سيرفو** متصلة بـ **لوحة أردوينو UNO** لتنفيذ حركة مشي متكررة.

This robot simulates a basic walking cycle using controlled joint angles. The movement respects physical constraints to avoid instability or damage.

---

## 🧠 الخوارزمية (Walking Algorithm)

### 1. بدء النظام – initialize()

- تحميل مكتبة servo
- توصيل المحركات بالمنافذ المناسبة

---

### 2. تعيين نطاقات الحركة (Envelopes)

#### ✅ Working Envelope (النطاق الميكانيكي الأقصى):
- مفصل الورك (Hip): من **-45° إلى +45°**
- مفصل الركبة (Knee): من **0° إلى 90°**
- مفصل الكاحل (Ankle): من **-30° إلى +30°**

#### ✅ Operating Envelope (النطاق المستخدم فعليًا):
- الورك: من **-20° خلف** إلى **+25° أمام**
- الركبة: من **0° إلى 60°**
- الكاحل: من **-15° إلى +15°**

#### ❌ Dead Envelope (المناطق العمياء أو الخطرة):
- الورك أكثر من ±45°
- الركبة أكثر من 90°
- الكاحل أكثر من ±30°
- أو أي وضعية تسبب **فقدان التوازن** (مثل رفع الرجلين معًا)

---

### 3. الوقوف في وضع التوازن – Neutral Position

- جميع المفاصل على زاوية **90°**

---

### 4. حلقة المشي – Walking Loop

#### كل خطوة تتضمن:

1. Check next move is inside **Operating Envelope**
2. **رفع الرجل اليمنى**: hip + knee + ankle
3. تثبيت التوازن باستخدام الرجل اليسرى
4. إنزال الرجل اليمنى للأرض
5. تحريك الجذع قليلاً للأمام (Shift torso)
6. **التبديل** إلى الرجل اليسرى وتكرار الخطوات

---

### 5. السلامة – Safety

- If any joint exceeds **Dead Envelope** → **Stop system** or correct posture.

---

### 6. إيقاف النظام – Stopping

- عند الضغط على زر توقف أو تلقي إشارة.

---

## 🔌 التوصيل – Wiring Diagram (6 محركات سيرفو)

(Servo Motor Connections)
          
Arduino UNO         
 [3]  --> Signal --> servo1 
 [5]  --> Signal --> servo2 
 [6]  --> Signal --> servo3 
 [9]  --> Signal --> servo4 
 [10]  --> Signal --> servo5 
 [11]  --> Signal --> servo6 
                             
5V   --> VCC لجميع السيرفو 
 GND  --> GND لجميع السيرفو 
كل سيرفو يحتوي على:

VCC (سلك ازرق و تركوازي) → 5V من الأردوينو أو مصدر طاقة خارجي

GND (سلك أسود و رمادي) → GND مشترك

Signal (سلك أصفر و احمر) → إلى منفذ رقمي (3،5،6،9،10،11)


> ⚠️ **ملاحظة مهمة:** إذا كنت تستخدم أكثر من 2 سيرفو، ينصح باستخدام مصدر طاقة خارجي 5V–6V مع توصيل GND مع الأردوينو.

---

## 💻 كود الأردوينو | Arduino Code Example


#include <Servo.h>

Servo servo1;
Servo servo2;
Servo servo3;
Servo servo4;
Servo servo5;
Servo servo6;

unsigned long startTime;

void setup() {
  servo1.attach(3);
  servo2.attach(5);
  servo3.attach(6);
  servo4.attach(9);
  servo5.attach(10);
  servo6.attach(11);

  startTime = millis();
}

void loop() {
  if (millis() - startTime < 2000) { // تشغيل sweep لمدة 2 ثانية
    for (int pos = 0; pos <= 180; pos += 5) {
      servo1.write(pos);
      servo2.write(pos);
      servo3.write(pos);
      servo4.write(pos);
      servo5.write(pos);
      servo6.write(pos);
      delay(15);
    }

  for (int pos = 180; pos >= 0; pos -= 5) {
      servo1.write(pos);
      servo2.write(pos);
      servo3.write(pos);
      servo4.write(pos);
      servo5.write(pos);
      servo6.write(pos);
      delay(15);
    }
  } else {
    // تثبيت جميع السيرفو على 90 درجة بعد 2 ثانية
    servo1.write(90);
    servo2.write(90);
    servo3.write(90);
    servo4.write(90);
    servo5.write(90);
    servo6.write(90);
  }
}


📦 المتطلبات | Components
✅ 1x Arduino UNO

✅ 6x Servo Motors (مثل SG90 أو MG996R)

✅ أسلاك توصيل (Jumper wires)

⛔ بطارية خارجية (اختياري لكن مفضل للثبات)

✅ Breadboard (اختياري)


صورة لشكل التوصيل : 

<img width="680" height="400" alt="شكل التوصيل " src="https://github.com/user-attachments/assets/cfc41475-1034-41ff-acee-7117eb990809" />

          



👨‍💻 Created By | تم بواسطة

روان الحربي / تاريخ المشروع:  July 2025
