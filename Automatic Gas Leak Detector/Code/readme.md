#define ledRpin 13
#define buzzerPin 7
#define relayPin 8      // ขาสำหรับควบคุม Relay ตามรูปวงจร
const int MQ2_AOUT_PIN = A0;
const int threshold = 600;

void setup() {
  Serial.begin(9600);
  pinMode(ledRpin, OUTPUT);
  pinMode(buzzerPin, OUTPUT);
  pinMode(relayPin, OUTPUT);
  
  // สถานะเริ่มต้น: ปิดพัดลม (Relay ส่วนใหญ่เป็น Active Low)
  digitalWrite(relayPin, HIGH); 
  
  Serial.println("System Ready: Fan, Light, and Sound alert");
  delay(2000);
}

void loop() {
  int sensorValue = analogRead(MQ2_AOUT_PIN);
  float voltage = sensorValue * (5.0 / 1023.0);

  Serial.print("Sensor Value: ");
  Serial.print(sensorValue);
  Serial.print(" | ");
  Serial.print(voltage);
  Serial.println(" V");

  if (sensorValue > threshold) {
    // --- สภาวะอันตราย ---
    digitalWrite(relayPin, LOW);   // เปิดพัดลมค้างไว้เพื่อระบายอากาศ
    
    // ทำไฟกะพริบและเสียงแจ้งเตือน
    digitalWrite(ledRpin, HIGH); 
    digitalWrite(buzzerPin, HIGH); 
    delay(200);                    
    
    digitalWrite(ledRpin, LOW);  
    digitalWrite(buzzerPin, LOW);  
    delay(200);
    
    Serial.println("!!! DANGER: FAN ON !!!");
  } 
  else {
    // --- สภาวะปกติ ---
    digitalWrite(relayPin, HIGH);  // ปิดพัดลม
    digitalWrite(ledRpin, LOW);
    digitalWrite(buzzerPin, LOW);
    delay(500); // หน่วงเวลาสั้นๆ เพื่อการอ่านค่าที่ต่อเนื่อง
  }
}
