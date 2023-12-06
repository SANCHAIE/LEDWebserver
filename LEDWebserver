/*
 * Created by ArduinoGetStarted.com
 *
 * This example code is in the public domain
 *
 * Tutorial page: https://arduinogetstarted.com/tutorials/arduino-uno-r4-wifi-controls-led-via-web
 */

#include <WiFiS3.h>

const int LED_PIN = 9;  // Arduino pin connected to LED's pin

const char ssid[] = "YOUR_WIFI";          // change your network SSID (name)
const char pass[] = "YOUR_WIFI_PASSWORD"; // change your network password (use for WPA, or use as key for WEP)

int status = WL_IDLE_STATUS;

WiFiServer server(80);

void setup() {
  //Initialize serial and wait for port to open:
  Serial.begin(9600);
  pinMode(LED_PIN, OUTPUT);  // set arduino pin to output mode

  String fv = WiFi.firmwareVersion();
  if (fv < WIFI_FIRMWARE_LATEST_VERSION) {
    Serial.println("Please upgrade the firmware");
  }

  // attempt to connect to WiFi network:
  while (status != WL_CONNECTED) {
    Serial.print("Attempting to connect to SSID: ");
    Serial.println(ssid);
    // Connect to WPA/WPA2 network. Change this line if using open or WEP network:
    status = WiFi.begin(ssid, pass);

    // wait 10 seconds for connection:
    delay(10000);
  }
  server.begin();
  // you're connected now, so print out the status:
  printWifiStatus();
}

void loop() {
  // listen for incoming clients
  WiFiClient client = server.available();
  if (client) {
    // read the first line of HTTP request header
  String HTTP_req = "";
    while (client.connected()) {
      if (client.available()) {
        Serial.println("New HTTP Request");
        HTTP_req = client.readStringUntil('\n');  // read the first line of HTTP request
        Serial.print("<< ");
        Serial.println(HTTP_req);  // print HTTP request to Serial Monitor
        break;
      }
    }

    // read the remaining lines of HTTP request header
    while (client.connected()) {
      if (client.available()) {
        String HTTP_header = client.readStringUntil('\n');  // read the header line of HTTP request

        if (HTTP_header.equals("\r"))  // the end of HTTP request
          break;

        Serial.print("<< ");
        Serial.println(HTTP_header);  // print HTTP request to Serial Monitor
      }
    }

    if (HTTP_req.indexOf("GET") == 0) {  // check if request method is GET
      // expected header is one of the following:
      // - GET led1/on
      // - GET led1/off
      if (HTTP_req.indexOf("led1/on") > -1) {  // check the path
        digitalWrite(LED_PIN, HIGH);           // turn on LED
        Serial.println("Turned LED on");
      } else if (HTTP_req.indexOf("led1/off") > -1) {  // check the path
        digitalWrite(LED_PIN, LOW);                    // turn off LED
        Serial.println("Turned LED off");
      } else {
        Serial.println("No command");
      }
    }

    // send the HTTP response
    // send the HTTP response header
    client.println("HTTP/1.1 200 OK");
    client.println("Content-Type: text/html");
    client.println("Connection: close");  // the connection will be closed after completion of the response
    client.println();                     // the separator between HTTP header and body
    // send the HTTP response body
    client.println("<!DOCTYPE HTML>");
    client.println("<html>");
    client.println("<head>");
    client.println("<link rel=\"icon\" href=\"data:,\">");
    client.println("</head>");
    
    client.println("<body><h1>ESE BUU Web Server</h1>")
    client.println("<a href=\"/led1/on\">LED ON</a>");
    client.println("<br><br>");
    client.println("<a href=\"/led1/off\">LED OFF</a>");
    client.println("</body></html>");
   
    client.flush();

    // give the web browser time to receive the data
    delay(10);

    // close the connection:
    client.stop();
  }
}

void printWifiStatus() {
  // print your board's IP address:
  Serial.print("IP Address: ");
  Serial.println(WiFi.localIP());

  // print the received signal strength:
  Serial.print("signal strength (RSSI):");
  Serial.print(WiFi.RSSI());
  Serial.println(" dBm");
}
