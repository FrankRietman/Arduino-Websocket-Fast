## Websocket client for Arduino, with fast data send

This is a simple library that implements a Websocket client running on an Arduino or ESP32

### Rationale

For a IOT project I needed a websocket client wrapped around a client so i could use SSL via WiFi or ethernet on it. This was needed because I was using WSS. The websocket server I connected to required me to ping back so this is now implemented. It send a ping back with opcode 10 and the data is ping request sended.

### Features
I added the following:

- Ping support
```cpp
if (opcode != NULL)
    {
        *opcode = msgtype & ~WS_FIN;
        if (*opcode == WS_OPCODE_PING)
        {
            sendEncodedData(data, WS_OPCODE_PONG);
            strcpy(data, "");
            return false;
        }
    }
    return true;
```
### Tests
The optimized code was tested for:

- `WiFiClient` (`<WiFi.h>` and `<WiFi101.h>`) and Ethernet seems to work fine
- Arduino UNO, ZERO and ESP32
- WiFi shield (retired) on Arduino UNO and WiFi shield 101 on Arduino ZERO
- `ws` as Node.js websocket server and `wss` works fine too if using a SSL client such as ESPSSLClient

### MCU compatibility
- Tested: Arduino UNO, ZERO, ESP32
- Not tested: Arduino DUE; howerer, by searching similar C++ repos on GitHub (`arduino websocket due in:readme,name,description fork:true`), it seems that the conditional inclusion (in src/sha1.cpp) of `#include <avr/io.h>` and `#include <avr/pgmspace.h>` needed for ZERO board, would also fix compilation for DUE board. Any good-soul tester is welcome to feedback.

### Notes
See the original code from Branden for additional notes.

### Credits
This is an optimized version of the client code from the excellent job in <https://github.com/brandenhall/Arduino-Websocket>. Most of the credit goes to Branden. And much credits to u0078867 from who 
I forked this libary
