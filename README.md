### Displaying the current temperature on a Four 7-segment-LED

#### Parts needed 

Arduino
TM1637 Four 7-segment-LED
LEDs - a few
DHT11 Temperature and Humidity Sensor


#### Connections :

TM1637
CLK- 
Data
Vcc and GND

#### Library to be installed on the Arduino

https://playground.arduino.cc/Main/TM1637/


Examples : 

File -> Examples -> TM1637



This is the Arduino Sketch  where


	// Transmitter
	#include <RH_ASK.h>
	#include <SPI.h> // Not actually used but needed to compile
	#include<dht.h>
	dht DHT;
	
	#define DHT11_PIN 3
	
	
	RH_ASK driver;
	String str_out;
	String str_humid;
	String str_temp;
	
	
	void setup()
	{
	    Serial.begin(9600);    // Debugging only
	     if (!driver.init())
	         Serial.println("init failed");
	}
	
	void loop()
	{
	    int chk = DHT.read11(DHT11_PIN);
	
	
	    Serial.print ("Transmitter sending...");
	    //char *msg = "Hello World!";
	    //float temp = 31;
	    //float humidity = 40;
	    
	    float temp     = DHT.temperature;
	    float humidity = DHT.humidity;
	    Serial.print ("Temperature :" );
	    Serial.print (temp );
	
	    Serial.print ("  :Humidity :" );
	    Serial.println (humidity );
	    
	    str_temp = String(temp);
	    str_humid = String (humidity);
	    
	    str_out= str_humid + "," + str_temp;
	    static char * msg = str_out.c_str();
	    Serial.println( msg);
	    driver.send((uint8_t *)msg, strlen(msg));
	      
	    driver.waitPacketSent();
	    delay(1000);
	}
