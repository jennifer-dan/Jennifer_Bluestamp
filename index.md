# ESP32 Weather Station 
Checking the weather on your phone is cool an all but imagine if you had a portable device that can display the humidity and light of the weather exactly where you are. 


| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Jennifer D | Granada Hills Charter Highschool | Electrical Engineering | Incoming Senior

**Replace the BlueStamp logo below with an image of yourself and your completed project. Follow the guide [here](https://tomcam.github.io/least-github-pages/adding-images-github-pages-site.html) if you need help.**

![Book logo](/IMG_8243.png)
  
# Final Milestone

For your final milestone, explain the outcome of your project. Key details to include are:
**Accomplishments**
- I was able to establish a more stable connection between the two ESP32s using an MQTT web server and Adafruit IO
**Challenges**
- Because I never dealt with MQTT before it was a challenge trying to understand how it worked. I also had toruble setting up the Adafruit IO and making sure the syntax was correct in my code. I definetly had trouble of asking questions. Reflecting back on my time at BSE I would've asked more questions and asked for assistance when I needed it in order to complete my milestone faster.
**What I learned**
- I learned about OLEDs and how to program them using Arduino IDE. In order to display something on the OLEd first you need to do some initial set up such as setting up the size, cursor position, and text color.
- Setting up Wifi on ESP32s
- Connecting two ESP32s using the ESP NOW function. How both ESP32s need to be on the same wifi channel as the mobile hotspot. 
**Future Endeavors**
- In the future I hope to learn more about different microcontrollers, not just ESP32s. I want to build my own ESP32 so I can really understand the functions of the ESP32 and how it works internally. 


# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/lubYXXxrUH4?si=uh0EF5rrIUMXEUlI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

**Don't forget to replace the text below with the embedding for your milestone video. Go to Youtube, click Share -> Embed, and copy and paste the code to replace what's below.**

**Technical Details:**
- Connected two ESP32’s in order to transmit data within one another using the esp now library
- Added a photoresistor and a thermistor to the newly added ESP32 in order to get the light intensity and the humidity level and transmit the data collected to the ESP32 receiver to display it onto the OLED

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/bxvZ5RsWZrU?si=I1TUHWwkD7sMzoyL" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

**Surprising Things:**
- During milestone 2 there has been some surprising things that I have noticed. Picking this project has allowed a lot of customizability and I can connect a lot of sensors. 
- Because the ESP32 has wifi and bluetooth there are a lot of doors that open when I am using ESP32’s

**Challenges:**
- Connecting the two ESP32s using the esp now function has been my biggest challenge. In order to connect the two esp32’s together I need to make sure that they are on the same wifi channel. So I set them both to wifi channel 6. 

- Because I am using the esp now function, I do not need wifi to connect the two esp32’s however I do need to connect the receiving esp32 to the wifi. Because the wifi channel is constantly changing on the mobile hotspot I need to make sure both esp32’s is connected to the same wifi channel as the mobile hotspot. 

**Next goals: **
- For my final milestone I plan to use MQTT client which sends my sensor data onto a web server in order for my esp32 receiver to get the data. This is a more sustainable and consistent way to transmit data between my two esp32s rather than using the esp now function which is inconsistent when connecting and takes a few tries to connect the two esp32s. 


The ESP32 weather station utilizes an ESP32 with built-in Wi-Fi to get information from an open source API, OpenWeatherStation, and displays the information on an Oled. 

**My Plan:**
1. First work on the hardware
- Connect the Oled and the ESP32 together using male to female wires, a breadboard, and male wires. 
2. Setup the ESP32 on the Arduino IDE
- Install the library esp32 by Espressif Systems 
- Set up the port and select ESP32 Dev Module
3. Test the Oled 
- Used an image and converted it to byte arrays to display on my Oled
- Using this website: https://javl.github.io/image2cpp/  
4. Set up Wi-Fi on ESP32
- Create a hotspot on a separate device with 2.4 Hz
- Added the name and password of the hotspot 
- Wrote code that displayed the IP address and whether the ESP32 was able to connect to the hotspot or not in the serial monitor. 
5. Got an API Key
- Created an account of OpenWeathermap which is an open source API 
- Got an API key and use it to get data for the temperature and description of the weather in my area
- Parsed the JSON file by storing it into doc and picking out what I wanted to display using float temp and const char description
Displayed the temperature and description onto the Oled

**Challenges:**
1. Connecting to Wi-Fi
- When trying to connect my ESP32 to wifi I first tried to use the hotspot on my phone however that did not work, so I had to use my laptop in order to create a 2.4 Hz mobile hotspot my ESP32 could connect too. I changed the name and password of my hotspot to be simpler so the ESP32 could be connected easier. In order to troubleshoot I made sure to display whether the ESP32 was in the process of connecting and whether it was unable or unable to connect on the serial monitor.
2. HTTP request 
- In order for my Oled to display real time data that frequently changes, I need to send a GET request to get the data. However I did not know how to approach this so with the help of my instructor I was able to store the string from the Json file into the variable payload then gets parsed in the doc object. 
3. Displaying the Temperature and Description on the Oled
- My temperature and description were displayed in the serial monitor however not on my Oled. The problem was because I didn’t set the color  of the text that was going to display on the Oled. So I couldn’t see what was being displayed on my Oled. However when I set the text color to white I was able to see what was being displayed on the Oled. 


# Schematics 
![Schematics](/45A01582-B902-47AD-BBE7-BE5A31A134A5.png)

# Code
Here's where you'll put your code. The syntax below places it into a block of code. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize it to your project needs. 

**ESP32 Sender Code**
```arduino ide
#include "ESP32_NOW.h"
#include "WiFi.h"
#include <esp_mac.h>  // For MACSTR and MAC2STR macros
#include "DHT.h"
#include "Adafruit_MQTT.h"
#include "Adafruit_MQTT_Client.h"
#include "WiFiClientSecure.h"


#define DHTPIN 4 
#define DHTTYPE DHT11
#define PhotoRes 35

const char* ssid = "ssid";
const char* password = "password";

#define WLAN_SSID "ssid"
#define WLAN_PASS "password"
#define AIO_SERVER      "io.adafruit.com"
#define AIO_SERVERPORT  8883
#define AIO_USERNAME  "username"
#define AIO_KEY       "aio key"
#define DHTPIN 4 
#define DHTTYPE DHT11

DHT dht(DHTPIN, DHTTYPE);
WiFiClientSecure client;
Adafruit_MQTT_Client mqtt(&client, AIO_SERVER, AIO_SERVERPORT, AIO_USERNAME, AIO_KEY);

const char* adafruitio_root_ca = \
      "-----BEGIN CERTIFICATE-----\n"
      "MIIEjTCCA3WgAwIBAgIQDQd4KhM/xvmlcpbhMf/ReTANBgkqhkiG9w0BAQsFADBh\n"
      "MQswCQYDVQQGEwJVUzEVMBMGA1UEChMMRGlnaUNlcnQgSW5jMRkwFwYDVQQLExB3\n"
      "d3cuZGlnaWNlcnQuY29tMSAwHgYDVQQDExdEaWdpQ2VydCBHbG9iYWwgUm9vdCBH\n"
      "MjAeFw0xNzExMDIxMjIzMzdaFw0yNzExMDIxMjIzMzdaMGAxCzAJBgNVBAYTAlVT\n"
      "MRUwEwYDVQQKEwxEaWdpQ2VydCBJbmMxGTAXBgNVBAsTEHd3dy5kaWdpY2VydC5j\n"
      "b20xHzAdBgNVBAMTFkdlb1RydXN0IFRMUyBSU0EgQ0EgRzEwggEiMA0GCSqGSIb3\n"
      "DQEBAQUAA4IBDwAwggEKAoIBAQC+F+jsvikKy/65LWEx/TMkCDIuWegh1Ngwvm4Q\n"
      "yISgP7oU5d79eoySG3vOhC3w/3jEMuipoH1fBtp7m0tTpsYbAhch4XA7rfuD6whU\n"
      "gajeErLVxoiWMPkC/DnUvbgi74BJmdBiuGHQSd7LwsuXpTEGG9fYXcbTVN5SATYq\n"
      "DfbexbYxTMwVJWoVb6lrBEgM3gBBqiiAiy800xu1Nq07JdCIQkBsNpFtZbIZhsDS\n"
      "fzlGWP4wEmBQ3O67c+ZXkFr2DcrXBEtHam80Gp2SNhou2U5U7UesDL/xgLK6/0d7\n"
      "6TnEVMSUVJkZ8VeZr+IUIlvoLrtjLbqugb0T3OYXW+CQU0kBAgMBAAGjggFAMIIB\n"
      "PDAdBgNVHQ4EFgQUlE/UXYvkpOKmgP792PkA76O+AlcwHwYDVR0jBBgwFoAUTiJU\n"
      "IBiV5uNu5g/6+rkS7QYXjzkwDgYDVR0PAQH/BAQDAgGGMB0GA1UdJQQWMBQGCCsG\n"
      "AQUFBwMBBggrBgEFBQcDAjASBgNVHRMBAf8ECDAGAQH/AgEAMDQGCCsGAQUFBwEB\n"
      "BCgwJjAkBggrBgEFBQcwAYYYaHR0cDovL29jc3AuZGlnaWNlcnQuY29tMEIGA1Ud\n"
      "HwQ7MDkwN6A1oDOGMWh0dHA6Ly9jcmwzLmRpZ2ljZXJ0LmNvbS9EaWdpQ2VydEds\n"
      "b2JhbFJvb3RHMi5jcmwwPQYDVR0gBDYwNDAyBgRVHSAAMCowKAYIKwYBBQUHAgEW\n"
      "HGh0dHBzOi8vd3d3LmRpZ2ljZXJ0LmNvbS9DUFMwDQYJKoZIhvcNAQELBQADggEB\n"
      "AIIcBDqC6cWpyGUSXAjjAcYwsK4iiGF7KweG97i1RJz1kwZhRoo6orU1JtBYnjzB\n"
      "c4+/sXmnHJk3mlPyL1xuIAt9sMeC7+vreRIF5wFBC0MCN5sbHwhNN1JzKbifNeP5\n"
      "ozpZdQFmkCo+neBiKR6HqIA+LMTMCMMuv2khGGuPHmtDze4GmEGZtYLyF8EQpa5Y\n"
      "jPuV6k2Cr/N3XxFpT3hRpt/3usU/Zb9wfKPtWpoznZ4/44c1p9rzFcZYrWkj3A+7\n"
      "TNBJE0GmP2fhXhP1D/XVfIW/h0yCJGEiV9Glm/uGOa3DXHlmbAcxSyCRraG+ZBkA\n"
      "7h4SeM6Y8l/7MBRpPCz6l8Y=\n"
      "-----END CERTIFICATE-----\n";

Adafruit_MQTT_Publish humidity = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/humidity");
Adafruit_MQTT_Publish light = Adafruit_MQTT_Publish(&mqtt, AIO_USERNAME "/feeds/light");


class ESP_NOW_Broadcast_Peer : public ESP_NOW_Peer {
public:
  ESP_NOW_Broadcast_Peer(uint8_t channel, wifi_interface_t iface, const uint8_t *lmk)
    : ESP_NOW_Peer(ESP_NOW.BROADCAST_ADDR, channel, iface, lmk) {}
  ~ESP_NOW_Broadcast_Peer() { remove(); }

  bool begin() {
    if (!ESP_NOW.begin() || !add()) {
      log_e("Failed to initialize ESP-NOW or register the broadcast peer");
      return false;
    }
    return true;
  }

  bool send_message(const uint8_t *data, size_t len) {
    if (!send(data, len)) {
      log_e("Failed to broadcast message");
      return false;
    }
    return true;
  }
};

/* Global Variables */
ESP_NOW_Broadcast_Peer* broadcast_peer = nullptr;

void setup() {
  Serial.begin(115200);
  delay(10);

  Serial.println(F("Adafruit IO MQTTS (SSL/TLS) Example"));
  Serial.println("DHT11 Sensor Initialization");
  dht.begin();

  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  WiFi.begin(WLAN_SSID, WLAN_PASS);
  Serial.print("Connecting to WiFi");
  Serial.println(WLAN_SSID);


  int attempts = 0;
  while (WiFi.status() != WL_CONNECTED && attempts < 100) {
    delay(500);
    Serial.print(".");
    attempts++;
  }

  client.setCACert(adafruitio_root_ca);

 if (WiFi.status() == WL_CONNECTED) {
    Serial.println("\nConnected to WiFi!");
    Serial.print("IP Address: ");
    Serial.println(WiFi.localIP());

  } else {
    Serial.println("\nFailed to connect to WiFi.");
  }
  // --- Step 2: Get Wi-Fi channel ---
  int wifiChannel = WiFi.channel();
  Serial.printf("Wi-Fi is on channel: %d\n", wifiChannel);

  // --- Step 3: Disconnect Wi-Fi and set channel for ESP-NOW ---
  WiFi.mode(WIFI_STA);
  WiFi.setChannel(wifiChannel);


  // --- Step 4: Init ESP-NOW ---
  broadcast_peer = new ESP_NOW_Broadcast_Peer(wifiChannel, WIFI_IF_STA, nullptr);
  if (!broadcast_peer->begin()) {
    Serial.println("Failed to initialize broadcast peer");
    delay(5000);
    ESP.restart();
  }

  Serial.printf("ESP-NOW version: %d, max data length: %d\n",
                ESP_NOW.getVersion(), ESP_NOW.getMaxDataLen());
  Serial.println("Setup complete. Broadcasting messages every 5 seconds.");
}

uint32_t x=0;

void loop() {
  MQTT_connect();
  float humidityValue = dht.readHumidity();
  int analogValue = analogRead(PhotoRes);

  if (! humidity.publish(humidityValue)) //Publish Humidity Reading!
 { 
   Serial.println(F("Failed"));
 } 
 else {
   Serial.print("Humidity Value = ");
   Serial.println(humidityValue);
 }

  delay(2000);
  const char* lightIntensity;
  if (analogValue < 40) {
    lightIntensity = "Dark";
  } else if (analogValue < 800) {
    lightIntensity = "Dim";
  } else if (analogValue < 3000) {
    lightIntensity = "Bright";
  } else {
    lightIntensity = "Very Bright";
  }

if (!light.publish(lightIntensity)) {  // Publish Light Reading
    Serial.println(F("Failed to publish light intensity"));
} else {
    Serial.print(F("Light Intensity: "));
    Serial.println(lightIntensity);
}

  delay(5000);
}

void MQTT_connect() {
  int8_t ret;

  // Stop if already connected.
  if (mqtt.connected()) {
    return;
  }

  Serial.print("Connecting to MQTT... ");

  uint8_t retries = 3;
  while ((ret = mqtt.connect()) != 0) { // connect will return 0 for connected
       Serial.println(mqtt.connectErrorString(ret));
       Serial.println("Retrying MQTT connection in 5 seconds...");
       mqtt.disconnect();
       delay(5000);  // wait 5 seconds
       retries--;
       if (retries == 0) {
         // basically die and wait for WDT to reset me
         while (1);
       }
  }

  Serial.println("MQTT Connected!");
}


```

**ESP32 Reciever Code**

```arduino ide
#include "WiFi.h"
#include <vector>
#include <ArduinoJson.h>
#include <Wire.h>
#include <Adafruit_GFX.h>
#include <Adafruit_SSD1306.h>
#include <HTTPClient.h>
#include <PubSubClient.h>

#define SCREEN_WIDTH 128
#define SCREEN_HEIGHT 64
#define OLED_RESET    -1
#define SCREEN_ADDRESS 0x3C

Adafruit_SSD1306 oled(SCREEN_WIDTH, SCREEN_HEIGHT, &Wire, OLED_RESET);

const char* ssid = "ssid";
const char* password = "password";
const char* apiKey = "api key";
const char* city = "Location"; 
const char* mqtt_broker = "io.adafruit.com";  // Adafruit IO broker
const int mqtt_port = 1883;                   // non-SSL port (simpler to start)
const char* mqtt_username = "username";        // your Adafruit username
const char* mqtt_password = "aio key"; // your AIO key
const char* humidtopic = "jen_dann/feeds/humidity";  // Case-sensitive!
const char* lighttopic = "jen_dann/feeds/light";  // Case-sensitive!



String apiTemp = "";
String apiDesc = "";
String receivedData = "";

WiFiClient espClient;       // Note: not WiFiClientSecure
PubSubClient client(espClient);

const char* adafruitio_root_ca = \
      "-----BEGIN CERTIFICATE-----\n"
      "MIIEjTCCA3WgAwIBAgIQDQd4KhM/xvmlcpbhMf/ReTANBgkqhkiG9w0BAQsFADBh\n"
      "MQswCQYDVQQGEwJVUzEVMBMGA1UEChMMRGlnaUNlcnQgSW5jMRkwFwYDVQQLExB3\n"
      "d3cuZGlnaWNlcnQuY29tMSAwHgYDVQQDExdEaWdpQ2VydCBHbG9iYWwgUm9vdCBH\n"
      "MjAeFw0xNzExMDIxMjIzMzdaFw0yNzExMDIxMjIzMzdaMGAxCzAJBgNVBAYTAlVT\n"
      "MRUwEwYDVQQKEwxEaWdpQ2VydCBJbmMxGTAXBgNVBAsTEHd3dy5kaWdpY2VydC5j\n"
      "b20xHzAdBgNVBAMTFkdlb1RydXN0IFRMUyBSU0EgQ0EgRzEwggEiMA0GCSqGSIb3\n"
      "DQEBAQUAA4IBDwAwggEKAoIBAQC+F+jsvikKy/65LWEx/TMkCDIuWegh1Ngwvm4Q\n"
      "yISgP7oU5d79eoySG3vOhC3w/3jEMuipoH1fBtp7m0tTpsYbAhch4XA7rfuD6whU\n"
      "gajeErLVxoiWMPkC/DnUvbgi74BJmdBiuGHQSd7LwsuXpTEGG9fYXcbTVN5SATYq\n"
      "DfbexbYxTMwVJWoVb6lrBEgM3gBBqiiAiy800xu1Nq07JdCIQkBsNpFtZbIZhsDS\n"
      "fzlGWP4wEmBQ3O67c+ZXkFr2DcrXBEtHam80Gp2SNhou2U5U7UesDL/xgLK6/0d7\n"
      "6TnEVMSUVJkZ8VeZr+IUIlvoLrtjLbqugb0T3OYXW+CQU0kBAgMBAAGjggFAMIIB\n"
      "PDAdBgNVHQ4EFgQUlE/UXYvkpOKmgP792PkA76O+AlcwHwYDVR0jBBgwFoAUTiJU\n"
      "IBiV5uNu5g/6+rkS7QYXjzkwDgYDVR0PAQH/BAQDAgGGMB0GA1UdJQQWMBQGCCsG\n"
      "AQUFBwMBBggrBgEFBQcDAjASBgNVHRMBAf8ECDAGAQH/AgEAMDQGCCsGAQUFBwEB\n"
      "BCgwJjAkBggrBgEFBQcwAYYYaHR0cDovL29jc3AuZGlnaWNlcnQuY29tMEIGA1Ud\n"
      "HwQ7MDkwN6A1oDOGMWh0dHA6Ly9jcmwzLmRpZ2ljZXJ0LmNvbS9EaWdpQ2VydEds\n"
      "b2JhbFJvb3RHMi5jcmwwPQYDVR0gBDYwNDAyBgRVHSAAMCowKAYIKwYBBQUHAgEW\n"
      "HGh0dHBzOi8vd3d3LmRpZ2ljZXJ0LmNvbS9DUFMwDQYJKoZIhvcNAQELBQADggEB\n"
      "AIIcBDqC6cWpyGUSXAjjAcYwsK4iiGF7KweG97i1RJz1kwZhRoo6orU1JtBYnjzB\n"
      "c4+/sXmnHJk3mlPyL1xuIAt9sMeC7+vreRIF5wFBC0MCN5sbHwhNN1JzKbifNeP5\n"
      "ozpZdQFmkCo+neBiKR6HqIA+LMTMCMMuv2khGGuPHmtDze4GmEGZtYLyF8EQpa5Y\n"
      "jPuV6k2Cr/N3XxFpT3hRpt/3usU/Zb9wfKPtWpoznZ4/44c1p9rzFcZYrWkj3A+7\n"
      "TNBJE0GmP2fhXhP1D/XVfIW/h0yCJGEiV9Glm/uGOa3DXHlmbAcxSyCRraG+ZBkA\n"
      "7h4SeM6Y8l/7MBRpPCz6l8Y=\n"
      "-----END CERTIFICATE-----\n";
void updateOLED();



void setup() {
  Serial.begin(115200);

  Serial.print("Username: "); Serial.println(mqtt_username);
  Serial.print("Key: "); Serial.println(mqtt_password);
  Serial.print("Humid Topic: "); Serial.println(humidtopic);
    Serial.print("Light Topic: "); Serial.println(lighttopic);


  // --- OLED init ---
  if (!oled.begin(SSD1306_SWITCHCAPVCC, SCREEN_ADDRESS)) {
    Serial.println("OLED init failed");
    while (true);
  }

  // --- Connect to Wi-Fi ---
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  Serial.print("Connecting to WiFi");

  oled.setTextSize(1);
  oled.setTextColor(SSD1306_WHITE);
  oled.clearDisplay();
  oled.setCursor(0, 0);
  oled.println("Connecting...");
  oled.display();

  int attempts = 0;
  while (WiFi.status() != WL_CONNECTED && attempts < 100) {
    delay(500);
    Serial.print(".");
    attempts++;
  }

  if (WiFi.status() == WL_CONNECTED) {
    Serial.println("\nConnected to WiFi!");
    Serial.print("IP Address: ");
    Serial.println(WiFi.localIP());

    oled.clearDisplay();
    oled.setCursor(0, 0);
    oled.println("Connected to WiFi");
    oled.display();
  } else {
    Serial.println("\nFailed to connect to WiFi.");
  }


  client.setServer(mqtt_broker, mqtt_port);
  client.setCallback(callback);

  while (!client.connected()) {
      String client_id = "esp32-client-";
      client_id += String(WiFi.macAddress());
      Serial.printf("The client %s connects to the public MQTT broker\n", client_id.c_str());
      if (client.connect(client_id.c_str(), mqtt_username, mqtt_password)) {
           Serial.println("Public EMQX MQTT broker connected");
       } else {
           Serial.print("failed with state ");
          Serial.print(client.state());
          delay(2000);
       }
    }
    // Publish and subscribe
    client.subscribe(humidtopic);
    client.subscribe(lighttopic);

  String serverPath = "http://api.openweathermap.org/data/2.5/weather?q=Los+Angeles,US&APPID=d6f778b1fecfcc2b8772b1767388b005&units=metric";
  HTTPClient http;
  http.begin(serverPath);
  int httpCode = http.GET();
  if (httpCode == 200) {
    String payload = http.getString();
    Serial.println(payload);

    StaticJsonDocument<1024> doc;
    DeserializationError error = deserializeJson(doc, payload);
    if (!error) {
      float temp = doc["main"]["temp"];
      const char* description = doc["weather"][0]["description"];
      Serial.printf("Temperature: %.1f C\n", temp);
      Serial.printf("Description: %s\n", description);

      apiTemp = String(temp);
      apiDesc = String(description);
      updateOLED();  // Show the data on OLED
    } else {
      Serial.println("JSON parse error");
    }
  } else {
    Serial.printf("Error getting data: %d\n", httpCode);
  }
  http.end();


}
void callback(char* topic, byte* payload, unsigned int length) {
    // Convert payload to string
    char message[length+1];
    for (int i = 0; i < length; i++) {
        message[i] = (char)payload[i];
    }
    message[length] = '\0';

    if (strcmp(topic, "jen_dann/feeds/humidity") == 0) {
        // Erase only the old humidity value area
        oled.fillRect(70, 35, 50, 10, SSD1306_BLACK);  // adjust width as needed

        oled.setCursor(0, 35);
        oled.print("Humidity:");
        oled.setCursor(70, 35);
        oled.print(message);
        oled.print(" %");
    } 
    else if (strcmp(topic, "jen_dann/feeds/light") == 0) {
        // Erase only the old light value area
        oled.fillRect(50, 45, 70, 10, SSD1306_BLACK);  // adjust width as needed

        oled.setCursor(0, 45);
        oled.print("Light:");
        oled.setCursor(50, 45);
        oled.print(message);
    }

    oled.display();
}

void updateOLED() {
  oled.clearDisplay();

  float tempC = apiTemp.toFloat();                // Convert to float
  float tempF = tempC * 9.0 / 5.0 + 32.0;         // Convert to Fahrenheit
  String tempFStr = String(tempF, 1);              // Fahrenheit string with 1 decimal

  oled.setCursor(0, 0);
  oled.setTextSize(1);
  oled.setTextColor(SSD1306_WHITE);
  oled.print("Temp: ");
  oled.print(tempFStr.c_str());   // Print Fahrenheit temp
  oled.println(" F");             // Add 'F' and go to new line

  oled.setCursor(0, 15);
  oled.print("Desc: ");
  oled.println(apiDesc.c_str());

  oled.display();
}

void loop() {
  // Print debug info every 10 seconds
  client.loop();


  delay(100);
}

```

# Bill of Materials
Here's where you'll list the parts in your project. To add more rows, just copy and paste the example rows below.
Don't forget to place the link of where to buy each component inside the quotation marks in the corresponding row after href =. Follow the guide [here]([url](https://www.markdownguide.org/extended-syntax/)) to learn how to customize this to your project needs. 

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| ESP32 | Get data from weather station website | $15.19 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/HiLetgo-ESP32-DevKitC-ESP32-WROOM-32U-ESP-WROOM-32U-Development/dp/B09KLS2YB3/ref=sr_1_2_sspa?crid=RZS0VO0FHLVG&dib=eyJ2IjoiMSJ9.UdLxS8engRob9RiEzo8GfUX_eb5SIivviByIHACD0jgBzVFD5MSKwRvt2HMUQ7jFwjmW8ZG0IeuXxh15af8FtOTBMtuZxzmPoijgCAnKnMBABOHZyD0edn4YZkFJbrA5RfjALOAtDYMc95a05cKxR9wKnwQd1YByAgxWGkl5UDc3XCV-nKV2pEM5FC9Wd_ZQUxuXoQkWv5tMsbM1aizGqFFphD4vJO6XYRx27X9L_yI.c3APPMEBNkLendl2C4CcAdNRThGAmdE_-whahkygZ4U&dib_tag=se&keywords=esp32&qid=1750359632&sprefix=esp32%2Caps%2C99&sr=8-2-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9hdGY&psc=1)"> Link </a> |
| Micro USB Port | Provide power to ESP32 | $5.49 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/dp/B098DW7485?ref=nb_sb_ss_w_as-reorder_k0_1_8&amp=&crid=1YRBOW66YBW2H&sprefix=microusb&th=1)"> Link </a> |
| SSD1306 | Displays weather | $6.99 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/HiLetgo-Serial-128X64-Display-Color/dp/B06XRBYJR8/ref=sr_1_3_pp?crid=3OI8K8ICZCT78&dib=eyJ2IjoiMSJ9.GZzyj9YeMmqRwkrAxnJLvxZmp8juEAPxvWaJNDTe-BnVwPIudvqRm4sZAc7IP9eih2k2UBI28kpvyqW1UdujgM7ySWO5widZQX7z_e3htFkprf94Axis4An0fMAB4kCuZi6Zommv2BNBf8I3SemVVES1kw97-VHHl9trS3F92p_tj5bDolfV3Lu9_eWo92TuVAy9uYZJX0iSqHhAfrsuyFppO90CLPFCNlSxerc6Sdw.EZ75vNhgBS6jhn1zFlxcrBuLhYbOK52ouKRiwIzu3pM&dib_tag=se&keywords=SSD1306&qid=1752420116&sprefix=ssd1306%2Caps%2C116&sr=8-3&th=1)"> Link </a> |
| Electronics Kit | Customize the device and wires to connect components | $13.49 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6](https://www.amazon.com/Smraza-Electronics-Potentiometer-tie-Points-Breadboard/dp/B0B62RL725/ref=sxts_b2b_sx_reorder_acb_business?content-id=amzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f%3Aamzn1.sym.f63a3b0b-3a29-4a8e-8430-073528fe007f&crid=2IC3T44H3U3WG&cv_ct_cx=breadboard%2Bkit&dib=eyJ2IjoiMSJ9.TUd5tu2T8rmms7ZuJ0UzmbtpLL1zsu93bQM0PzwnP4E.sT0V0vL_QtbYv8ymVTCcRkhFNgBtRvRiT7G4FT1oGTE&dib_tag=se&keywords=breadboard%2Bkit&pd_rd_i=B0B62RL725&pd_rd_r=67e1f4ff-e3b9-44e4-b441-b4ae282f036b&pd_rd_w=UjFaP&pd_rd_wg=0xRoC&pf_rd_p=f63a3b0b-3a29-4a8e-8430-073528fe007f&pf_rd_r=BFGP77H27ZN31W4PZAW6&qid=1715911733&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=breadboard%2Bkit%2Caps%2C109&sr=1-2-9f062ed5-8905-4cb9-ad7c-6ce62808241a&th=1)/"> Link </a> |
| DMM | Measure the voltage and resistance | $11 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/AstroAI-Digital-Multimeter-Voltage-Tester/dp/B01ISAMUA6/ref=sxin_17_pa_sp_search_thematic_sspa?content-id=amzn1.sym.e8da13fc-7baf-46c3-926a-e7e8f63a520b%3Aamzn1.sym.e8da13fc-7baf-46c3-926a-e7e8f63a520b&cv_ct_cx=digital+multimeter&dib=eyJ2IjoiMSJ9.5LQumrfBR8l0mKnJCJlRg73dxpou0gqYD_ffU3srgs0Utegwth8GcQCSVXVzeZeLSJx5J3itz5TLdmJHsrVITQ.-00jRPoT-bBy26YC4LzQ-S4cYdztgmSMGb83_WEm6HY&dib_tag=se&keywords=digital+multimeter&pd_rd_i=B01ISAMUA6&pd_rd_r=e1ff2570-7e4a-4906-bc55-6f819d48d1bc&pd_rd_w=h7HgL&pd_rd_wg=0ZcFH&pf_rd_p=e8da13fc-7baf-46c3-926a-e7e8f63a520b&pf_rd_r=R6YKX3NXTDQ1PQP4H8RM&qid=1715911879&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sr=1-1-7efdef4d-9875-47e1-927f-8c2c1c47ed49-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9zZWFyY2hfdGhlbWF0aWM&psc=1)"> Link </a> |
| USB-USBC | Adapter | $2.99 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6/](https://www.amazon.com/ENVEL-Transfer-Converter-Thunderbolt3-Compatible/dp/B0D3T2QDVJ/ref=sxin_17_pa_sp_search_thematic_sspa?content-id=amzn1.sym.70fcaece-2dd2-4653-bf00-fb6af1af1b93%3Aamzn1.sym.70fcaece-2dd2-4653-bf00-fb6af1af1b93&crid=2XKXL9JJ62FRH&cv_ct_cx=usba%2Bto%2Busbc&keywords=usba%2Bto%2Busbc&pd_rd_i=B0D3T2QDVJ&pd_rd_r=4d705a77-7d1c-4543-b61d-c95f071f99c3&pd_rd_w=zydwI&pd_rd_wg=Ra4PI&pf_rd_p=70fcaece-2dd2-4653-bf00-fb6af1af1b93&pf_rd_r=GA7XX674ZRQ1VKTH3HWX&qid=1750358432&sbo=RZvfv%2F%2FHxDF%2BO5021pAnSA%3D%3D&sprefix=usba%2Bto%2Busb%2Caps%2C106&sr=1-1-e169343e-09af-4d41-85b1-8335fe8f32d0-spons&sp_csd=d2lkZ2V0TmFtZT1zcF9zZWFyY2hfdGhlbWF0aWM&th=1)"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
