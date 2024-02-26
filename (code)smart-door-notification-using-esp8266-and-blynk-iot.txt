#define BLYNK_TEMPLATE_ID "TMPL34LioZ5gm"
#define BLYNK_TEMPLATE_NAME "Smart Door Notification"
#define BLYNK_AUTH_TOKEN "WsFYVUyAiqaD9wIwWzc0M1d_f32G47Cd"

#define BLYNK_PRINT Serial
#include <WiFi.h>
#include <BlynkSimpleEsp32.h>

#define DOOR_SENSOR  4
#define buzzer 2
BlynkTimer timer;
char auth[] = BLYNK_AUTH_TOKEN; //Enter the authentication code sent by Blynk to your Email
char ssid[] = "time"; //Enter your WIFI SSID
char pass[] = "98765432"; //Enter your WIFI Password
int flag=0;

void notifyOnButtonPress()
{
  int isButtonPressed = digitalRead(DOOR_SENSOR);
  if (isButtonPressed==1 && flag==0) {
    Serial.println("Someone Opened the door");
    Blynk.logEvent("security_alert","Someone opened the Door");
    digitalWrite(buzzer, HIGH);
    flag=1;
  }
  else if (isButtonPressed==0)
  {
    flag=0;
    Serial.println("Door Closed");
    digitalWrite(buzzer, LOW);
  }
}
void setup()
{
Serial.begin(9600);
Blynk.begin(auth, ssid, pass);
pinMode(DOOR_SENSOR,INPUT_PULLUP);
pinMode(buzzer, OUTPUT);
timer.setInterval(5000L,notifyOnButtonPress); 
}
void loop()
{
  Blynk.run();
  timer.run();
}
