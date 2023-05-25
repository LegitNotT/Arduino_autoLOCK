# Arduino_autoLOCK
Arduino UNO
Connect the Bluetooth Modem (HC05)
connect the RX pin to 10 and TX pin to 11
Connect the servo or the motor to the pin number 2 and to the battery

#include <SoftwareSerial.h>
#include <Servo.h>

SoftwareSerial bluetooth(10, 11);  // RX, TX pins for Bluetooth module

const char* authorizedPhone1 = "PHONE1_MAC_ADDRESS";
const char* authorizedPhone2 = "PHONE2_MAC_ADDRESS";

Servo doorServo;

void setup() {
  doorServo.attach(2);
  doorServo.write(0);

  Serial.begin(9600);
  bluetooth.begin(9600);
}

void loop() {
  if (bluetooth.available()) {
    String receivedData = bluetooth.readStringUntil('\n');
    receivedData.trim();

    if (receivedData == authorizedPhone1 || receivedData == authorizedPhone2) {
      openDoor();
      delay(5000);  // Door will be open for 5 seconds
      closeDoor();
    }
  }
}

void openDoor() {
  Serial.println("Opening the door");
  doorServo.write(90);
}

void closeDoor() {
  Serial.println("Closing the door");
  doorServo.write(0);
}
