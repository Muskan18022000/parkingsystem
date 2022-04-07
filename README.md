# parkingsystem
#include <LiquidCrystal.h>
#include <Servo.h>
Servo myservo;
LiquidCrystal lcd(12, 11, 5, 4, 3, 2);
int inches = 0;
int pos=90;
//if Rfid reader is attached
//int count=0;//
//char input[12];//
float cm = 0;
int i=0;
int smoke=0;
int threshold=500;
long readUltrasonicDistance(int triggerPin, int echoPin)
{
  pinMode(triggerPin, OUTPUT);
  pinMode(A0,INPUT);
  digitalWrite(triggerPin, LOW);
  delayMicroseconds(2);
  digitalWrite(triggerPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(triggerPin, LOW);
  pinMode(echoPin, INPUT);
  return pulseIn(echoPin, HIGH);
  //pinMode(0,INPUT);//RFID connection
}

void setup() {
 
  Serial.begin(9600);
  lcd.begin(16, 2);
 
}

void loop() {
  lcd.clear();
  lcd.setCursor(0,0);
  lcd.print("RFID SCAN");
/*lcd.setCursor(0,1);
 while(Serial.available() && count < 12)          // Read 12 characters and store them in input array

                              {

                             input[count] = Serial.read();//storing 12 characters one by one

                                             count++;

                                              lcd.print(input[count]);//showing 12 characters on LCD one by one

                                              if (count==12)

                                              {

                                                              lcd.print("             ");

                                              count = 0;// once 12 characters are read get to start and wait for second ID

                                                               lcd.setCursor(0, 1);//move courser to start.

                                               }

                              }*/
 
   
    lcd.setCursor(0,1);
    lcd.print("vehicle no.1009");//as scanned by EM-18 RFID reader
    //(here we dont have RFID reader connected so in refrence taken arbitiary number
   
    delay(4000);
    lcd.clear();
  smoke=analogRead(A0);
    Serial.println(smoke);
   
    lcd.print("smoke:");
    lcd.setCursor(7,0);
    lcd.print(smoke,DEC);
  delay(1000);
 
    if(smoke<=threshold)
     
    {
       myservo.attach(9);
      myservo.write(pos);
      tone(10,500,1000);
      delay(2000);
      tone(10,1000,1000);
   delay(1000);
      noTone(10);
    lcd.setCursor(0,1);
  lcd.print("Dis(cm)");
   
  for(i=0;i<=100;i++)
    {
      cm = 0.01723 * readUltrasonicDistance(7, 7);
  inches = (cm / 2.54);
  Serial.print(inches);
  Serial.print("in, ");
  Serial.print(cm);
  Serial.println("cm");
  delay(100);
  lcd.setCursor(12, 1);
  lcd.print(cm);
      delay(2000);
    if(cm>170)
    {
      pos=-pos;
      myservo.write(pos);
      tone(10,500,1000);
      delay(2000);
      tone(10,1000,1000);
   delay(1000);
      noTone(10);
      delay(6000);
      i=100;
    }
    }
    }

   
   else
 
   {
    lcd.setCursor(0,1);
    lcd.print("SERVICE");
     tone(10,1000,2000);
      delay(1000);
      tone(10,2000,1000);
   delay(1000);
      noTone(10);
     delay(4000);
     lcd.clear();
     delay(6000);
   }
 

    }
