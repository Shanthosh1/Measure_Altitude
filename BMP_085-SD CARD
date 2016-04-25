/*
  * SD card attached to SPI bus as follows:
 ** MOSI - pin 11
 ** MISO - pin 12
 ** CLK - pin 13
 ** CS - pin 4

 * BMP085 sensor connected as follows:
 ** Vcc to 3.3V
 ** Ground to Ground
 ** SCL to A4
 ** SDA to A5
 */

#include <SD.h>
#include <Adafruit_BMP085.h>
#include <Wire.h>

Adafruit_BMP085 bmp;              // library for the sensor

const int chipSelect = 4; 
char fileName[] = "datalog.csv";    // filename to save to

void setup() 
{
  // start serial:
  Serial.begin(9600);
   if (!bmp.begin()) 
  {
  Serial.println("Sensor initialisation failed");
  while (1) {}
  }
    Wire.begin();
   
  // start the sensor and initialize the SD card:
  bmp.begin();
  startSDCard();
}

void loop()
{
 
   float temperature = bmp.readTemperature();
    float pressure = bmp.readPressure();
    float altitude=bmp.readAltitude(101100);

    // open the file:
    File dataFile = SD.open(fileName, FILE_WRITE);

    // if the file is available, write to it:
    if (dataFile) {
     dataFile.print(pressure);
      dataFile.print("\t");
      dataFile.println(temperature);
      dataFile.print("\t");
      dataFile.print(altitude);
      
      
      // print to the serial port too:
      Serial.print(pressure);
      Serial.print("\t");
      Serial.print(temperature);
      Serial.print("\t");
      Serial.println(altitude);
      // close the datafile:
      dataFile.close();
    }  
    // if the file isn't open, pop up an error:
    else {
      Serial.println("error opening datalog.csv");
    } 
  }



boolean startSDCard() {
  boolean result = false;
  Serial.print("Initializing SD card...");
  // make sure that the default chip select pin is set to
  // output, even if you don't use it:
  pinMode(10, OUTPUT);

  // see if the card is present and can be initialized:
  if (!SD.begin(chipSelect)) {
    Serial.println("Card failed, or not present");
    // don't do anything more:
    result = false;
  } 
  else {
    Serial.println("card initialized.");
    File dataFile = SD.open(fileName, FILE_WRITE);
    // open the file:
    if (dataFile) {
      dataFile.println("Pressure\tTemperature\tAltitude");
      dataFile.close();
    }
  }  
  return result;
}


