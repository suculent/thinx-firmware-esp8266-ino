# thinx-firmware-esp8266-ino

[![Codacy Badge](https://api.codacy.com/project/badge/Grade/656ce88b25864d8b87d5c41848364253)](https://www.codacy.com/app/suculent/thinx-firmware-esp8266-ino?utm_source=github.com&utm_medium=referral&utm_content=suculent/thinx-firmware-esp8266-ino&utm_campaign=badger)

Arduino firmware for THiNX providing device management and automatic (webhook-based) OTA (over-the-air) firmware builds and updates.

Provides example implementations for ESP8266 with Arduino IDE.

* This is a work in progress.
* 100% functionality is not guaranteed for all the time.

# Requirements


### IDE

- Arduino IDE (currently tested with 1.8.5 but works even with older versions)

### Arduino C development

> WARNING! Adding Arduino Library Manager dependencies is supported through the thinx.yml file, however this library already contains all required dependencies, because your local Arduino Libraries are not located on the CI server. 

> Copy dependencies from the `lib` folder to your Arduino libraries to compile locally.

> Use the thinx.yml to add more dependencies for THiNX CI, but be aware that those will be merged with libraries from lib folder next to .ino file.

- Arduino IDE or Platform.io
- Arduino libraries: THiNXLib 2.0.99 or newer; ArduinoJSON, WiFiManager, ESP8266httpUpdate (to be replaced)
- Open this folder using Atom with installed Platform.io or thinx-firmware-esp8266/thinx-firmware-esp8266.ino using Arduino IDE.
- Run prerelease.sh to bake your commit ID into the Thinx.h file.

# Installation

[![Instruction video for OS X](https://img.youtube.com/vi/N5abfKJzsd0/0.jpg)](https://www.youtube.com/watch?v=N5abfKJzsd0 "Installation")

## Arduino IDE

Search for `THiNXLib` in Arduino Library Manager and install all other dependencies... or you can just copy then from the `lib` folder if you prefer tested versions before the latest.

## Board support

For properly configuring the `.board` file (preset to ESP8266/Wemos D1 Mini) see the [Arduino CLI Docs](https://github.com/arduino/Arduino/blob/master/build/shared/manpage.adoc) docs for the `-board package:arch:board[:parameters]` option.


# Usage

1. Create account on the [http://rtm.thinx.cloud/](http://rtm.thinx.cloud/) site
2. Create an API Key
3. Clone [repository](https://github.com/suculent/thinx-firmware-esp8266-ino) 
4. Pull all the dependencies using `git submodule update --init --recursive`
5. Copy all included libraries to your `~/Documents/Arduino/libraries` folder (on Mac)
6. Run the bash ./prerelease.sh to create Thinx.h file; you can edit this with your custom information but the file will be overwritten when building on the server
7. You can store Owner ID and API Key in Thinx.h file in case your project is not stored in public repository. Otherwise make sure WiFiManager is enabled in THiNXLib.h to copy-paste those values to Captive Portal later.
8. Build and upload the code to your device.
9. After restart, connect with some device to WiFi AP 'AP-THiNX' and enter the API Key / Owner ID if you haven't hardcoded those in step 4.
10. Device will connect to WiFi and register itself. Check your thinx.cloud dashboard for new device.

... Then you can add own git source, add ssh-keys to access those sources if not public, attach the source to device to dashboard and click the last icon in row to build/update the device. 

Note: In case you'll build/upload your project (e.g. the library) using thinx.cloud, API key will be injected automatically and you should not need to set it up anymore.

# Environment Variable Support

You provide callback receiving String using `setPushConfigCallback()` method. Whenever device receives MQTT update with `configuration` key, it will provide all environment variables to you.

> This may be also used for the WiFi Migration procedure.


# Security

Because all the traffic from ESP8266 is usually HTTP-only and not all devices can handle SSL, you can install our side-kick project [THiNX-Connect](https://github.com/suculent/thinx-connect). Install this proxy to your home environment and it will encrypt HTTP traffic to HTTPS and will tunnel your device communication directly to thinx.cloud.

Library queries the `thinx` MDNS service on `_tcp` protocol on local network upon connection. When such service is available, library will forward all HTTP requests through this service. 
