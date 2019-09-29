# weather-station-cloud
//nodemcu-esp32s bme280 + blynk app weather station
    #include <Wire.h>
    #include <BlynkSimpleEsp32.h>
    #include <Adafruit_Sensor.h>
    #include <Adafruit_BME280.h>
    #define SEALEVELPRESSURE_HPA (1013.25)
    #define BMP280_ADDRESS (0x77)
    //Setup connection of the sensor
    Adafruit_BME280 bme; // I2C

    char auth[] = "**********************";
    char ssid[] = "**********";
    char pass[] = "**********";
    
    BlynkTimer timer;

    //Variables
   
    float temperature;  //To store the temperature (oC
    float humidity;     //To store the humidity (%) (you can also use it as a float variable)
    float pressure;     //To store the barometric pressure (Pa)
    int altimeter;      //To store the humidity (%) (you can also use it as a float variable)

    void setup() {
      bme.begin(0x76);    //Begin the sensor
      Serial.begin(9600); //Begin serial communication at 9600bps
      Serial.println("Adafruit BME280 test:");
      Blynk.begin(auth, ssid, pass);
      timer.setInterval(2000L, ReadSensors);   // read sensor every 5s 
    }

    void ReadSensors(){
      //Read values from the sensor:
      pressure = bme.readPressure();
      temperature = bme.readTemperature();
      humidity = bme.readHumidity ();
     
      Blynk.virtualWrite(V1, temperature);  // write temperature to V1 value display widget
      Blynk.virtualWrite(V2, humidity);    // write Relative Humidity to V2 value display widget
      Blynk.virtualWrite(V3, pressure/100); // write pressure to V3 value display widget
      
      //Print values to serial monitor:
      Serial.print(F("Pressure: "));
      Serial.print(pressure);
      Serial.print(" Mb");
      Serial.print("\t");
      Serial.print(("Temp: "));
      Serial.print(temperature);
      Serial.print(" °C");
      Serial.print("\t");
      Serial.print("Humidity: ");
      Serial.print(humidity); // this should be adjusted to your local forcase
      Serial.println(" %");    
      //delay(2000); //Update every 5 sec  
    }

    void loop() {
      Blynk.run();
        timer.run();      
        }
