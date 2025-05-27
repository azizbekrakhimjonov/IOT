# IoT Architecture prototype
## Technologies
This project was created with:
* Python 3.9.6
  * Django 3.2.9
  * Requests 2.26.0
* Arduino 1.8.15
* Mu 1.1.0

## Hardware
* 2x  PIR sensors
* 2x  NodeMCU circuit board
* 1x  LED

### Setup
```
virtualenv venv
```
Linux/Mac:
```
source venv/bin/activate
```
Windows:
```
venv/Scripts/activate
```

```
pip install Django==3.2.9
pip install requests==2.26.0
```
### Django Settings
On a terminal run ```ipconfig``` or ```ip a``` to know your computer's IP address which
we will use to update the ```ALLOWED HOSTS``` in ```gateway/gateway/setting.py```.
```python
ALLOWED_HOSTS = ['localhost', '<Insert your gateway/computer IP>']
```

``` python
def actuator_get_request_update(status):
    actuator_url = f"http://<enter actuator's IP>/actuator={status}"
    # Example: actuator_url = f"http://192.168.1.100/actuator={status}"
    requests.get(
            actuator_url, 
            headers = {'Content-Type': 'text/html'}
        )
```

```python
ssid = "Your WiFi network name" 
password = "Your Wifi network password"
```

This request.find reading relates to the **```GET```** message sent by the Gateway, where ON/OFF were part of the request message as ```status```
```python
if request.find('/actuator=on') == 6:
        print('Actuator ON')
        actuator.value(1)
    if request.find('/actuator=off') == 6:
        print('Actuator OFF')
        actuator.value(0)
    response = "OK"
```

## Sensor
```cpp

const char* ssid = "Your_Router_Name";
const char* password = "Your_WiFi_Password";

void setup(){
    Serial.begin(115200);
    Serial.println("\nWiFi station setting");
    WiFi.mode(WIFI_STA);
    WiFi.begin(ssid, password);
    Serial.print("Connecting");
    while (WiFi.status() != WL_CONNECTED){
        delay(500);
        Serial.print(".");
    }
    Serial.println("\nWiFi connection established");
    Serial.print("Device ip address: ");
    Serial.println(WiFi.localIP());
}
```