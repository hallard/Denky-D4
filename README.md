# Denky D4 ESP32 Teleinfo

<img src="https://github.com/hallard/Denky-D4/blob/main/pictures/Denky-D4-red.png" alt="Denky D4">

This board is used to get French energy meter called Teleinfo (AKA TIC) and has the following features:

- Steroid ESP32 using ESP32-PICO-V3-02 chip with 8Mb Flash and 2Mb PSRAM
- Serial to USB onboard converter
- Teleinfo Reader interface
- RGB User Led
- Blue LED to see if Teleinfo signal is received
- QWIIC/STEMMA I2C header sensors connector
- USB-C Connector

It is a plug and play board, nothing to solder or assemble.

# History

**New in v1.3a**

- Changed TIC trimmer from 1K to 2K
- Changed TIC Led (blue) resistor to decrease light intensity
- Placed easy cutting trace to disable Teleinfo Blue LED
- Added exposed pad for 5V and GND to external powering out of USB connector


**v1.3**

- Changed some silk for better reading

**v1.2**

- Replaced R3 by a trimmer to allow TIC sensitivity change without soldering resistors

**v1.1**

- Changed CPU now use ESP32-PICO-V3-02 with 8Mb Flash and 2Mb PSRAM
- Changed USB/SERIAL chip to CH9102 due to shortage
- Added footprint for U-FL connector for external antenna 
- Removed headers pins
- Reduced RGB LED size using SK6805-1515
- Added small visual blue LED on Teleinfo receive signal
- All components on same side (nothing on bottom side)

**v1.0**

- First fully working prototype

# Detailed Description

Look at the schematics for more informations, easy to understand. Wiring is as follow:

| Pin Function | ESP32-PICO | 
|  :---        |  :---:  | 
| Téléinfo Rx  |  GPIO8  | 
| RGB Led      |  GPIO14 | 
| I2C SDA      |  GPIO21 | 
| I2C SDL      |  GPIO22 | 


# Schematics

<img src="https://github.com/hallard/Denky-D4/blob/main/pictures/Denky-D4-sch.png" alt="Denky D4 Schematics">

# Boards

<img src="https://github.com/hallard/Denky-D4/blob/main/pictures/Denky-D4.jpg" alt="Denky D4 Size">

<img src="https://github.com/hallard/Denky-D4/blob/main/pictures/Denky-D4-Boards.png" alt="Denky D4 Boards">


# Firmware 

It's compatible with Arduino IDE, Visual Studio Code and also Micro Python since this board use same CPU than Adafruit [QT-PY-ESP32-PICO](https://learn.adafruit.com/adafruit-qt-py-esp32-pico)

You can write your own and use with [LibTeleinfo](https://github.com/hallard/LibTeleinfo) library I wrote if you want to get rid of driving teleinfo stuff (chekout some [examples](ttps://github.com/hallard/LibTeleinfo/tree/master/examples)).

## Tasmota

But I strongly suggest using amazing [Tasmota](https://tasmota.github.io/docs/) firmware, all is already done and well done.

Please check Teleinfo official tasmota [documentation](https://tasmota.github.io/docs/Teleinfo/) so see how to configure your device depending on smartmeter type and what options you need.

### Unofficial builds

Teleinfo is not member of official Tasmota builds. So you need to build your own with `USE_TELEINFO` define. 
But Tasmota team agreed it's not simple to build for end users, and they now provide unofficial build that follow developement branch, this mean you always have an up to date build with latest code available. Of course we added Teleinfo (ESP8266 and ESP32) builds in this unofficial build process so you have nothing to install and compile/link (building). You can't go easier.

And as a cherry on the cake, easy flasher tools (web version and executable one) will present Teleinfo firmware so you are able to flash teleinfo firmware in less than 1 minute. You can check detail [here](https://github.com/Jason2866/Tasmota-specials) but here how to do that.

- Launch [Web Flasher here](https://jason2866.github.io/Tasmota-specials/) 
- Select Teleinfo 
- Select Serial port, and click `install`

<img src="https://github.com/hallard/WeMos-TIC/raw/master/pictures/WeMos-TIC-web_flasher.png">

Once done something like that

<img src="https://github.com/hallard/WeMos-TIC/raw/master/pictures/WeMos-TIC-web_flasher_ok.png">

After flashed, you should now see a new access point named `tasmota_aabbcc_xxxx` where you can connect to configure your WiFi for the device to connect on.

Alternatively, if you connect serial console and reset the device you should see Serial logs like that
```
00:00:00.003 HDW: ESP32-D0WDQ6 
00:00:00.037 UFS: FlashFS mounted with 308 kB free
00:00:00.109 CFG: Loaded from File, Count 12
00:00:00.124 QPC: Count 1
00:00:00.257 CFG: no '*.autoconf' file found
00:00:00.263 BRY: Berry initialized, RAM used=3620
00:00:00.282 BRY: no 'preinit.be'
00:00:00.291 Project tasmota - Tasmota Version 10.0.0.3(teleinfo)-2_0_1_1(2021-11-30T14:22:47)
00:00:00.379 BRY: no 'autoexec.be'
00:00:00.447 WIF: WifiManager active for 3 minutes
00:00:01.139 HTP: Web server active on tasmota-090F8C-3980 with IP address 192.168.4.1
00:00:06.827 QPC: Reset
```

If you want to deep into this process or just curious, you can check out it's [here](https://github.com/Jason2866/Tasmota-specials)

:memo: If you have some issues flashing with Web Flasher, do not hesitate to use another awesome tool [ESP Flasher](https://github.com/Jason2866/ESP_Flasher), with this one you can see exactly what's going on in case of issue because it has built in console. Usefull also after reboot of the device because console still active. This is the one I'm using day by day. In this case you need to download firmware to flash first [here](https://github.com/Jason2866/Tasmota-specials/tree/firmware/firmware/tasmota/other). Download firmware `tasmota-teleinfo`

You can take a look on `autoconf` [folder](https://github.com/tasmota/autoconf/tree/main/raw/esp32/Wemos_Teleinfo) to see some of init commands used on Denky D4.

:memo: Don't forget to reset Energy counters on first boot with console command `EnergyTotal 0` if you have erratic values. 


### I2C Display

You can add fancy I2C display (or even I2C sensors), using the on board QWIIC sensor, please refer to section [I2C Display](https://github.com/hallard/WeMos-TIC/blob/master/README.md#i2c-display).

### Autoconf 

Another awesome feature of Tasmota is the ability to download configuration profile, and guess what, we done it for Denky D4, just go to configuration option, select Autoconfig and then choose in the list `Denky D4 V1.1` and here you are, ne need to copy/paste template, it's done by autoconfig.
If you want to deep into this process or just curious, you can check out it's [here](https://github.com/tasmota/autoconf)


### Tasmota template

I strongly suggest using awesome Tamsota autoconf feature but if you need Tasmota template for Denky D4, here it it

#### V1.1 and up
```json
{"NAME":"Denky D4 (v1.1)","GPIO":[32,0,0,0,1,0,0,0,0,1,1376,1,0,0,0,0,0,640,608,0,0,0,0,0,0,0,5632,0,0,0,0,0,0,0,0,0],"FLAG":0,"BASE":1}
```

#### V1.0
```json
{"NAME":"Denky D4 (v1.0)","GPIO":[32,0,0,0,1,0,0,0,0,1,0,1,0,0,0,0,0,640,608,0,0,450,449,448,0,0,5632,0,0,0,0,0,0,0,0,0],"FLAG":0,"BASE":1}
```

## Berry Scripting 

Now you can personalize code with [Berry language](https://tasmota.github.io/docs/Berry/). Check out some Berry samples [here](https://github.com/arendst/Tasmota/blob/development/tasmota/berry/examples/)

You can do that by going to Berry console from Tasmota WEB user interface.

### Drive RGB LED depending on actual power

Here is a Berry example, goal is to follow real time consumption driving on board RGB Led depending on current Power consumption (low green then going to red when reaching maximum current of your contract)

```python
#-
# example of using Berry script to change the led color
# accordingly to power consumption
# using Denky or WeMos Teleinfo (French Teleinfo reader)
-#

#- define the global symbol for reference -#
runcolor = nil

def runcolor()
  var max_contrat = 30 # contrat 30A
  var i = energy.current
  #print(i)
  var red = tasmota.scale_uint(int(i), 0, max_contrat, 0, 255)
  var green = 255 - red
  var channels = [red, green, 0]
  light.set({"channels":channels, "bri":64, "power":true})
  tasmota.set_timer(2000, runcolor)
end

#- run animation -#
runcolor()
```

### Send data to Emoncms with Berry 

What's magic with Berry is the ability to do basic stuff with data, in this example we will intercept MQTT send message by Energy driver, do some calc and send data to Emoncms every 15 seconds and also to drive RGB Led from Green (low load) to Red (approach max subscription)

Modifiy API key with your, and copy paste the following code into Berry Console. Tst and validate if all is okay for you.

Once all is fine, you paste the code into a file named `autoexec.be` on the Tasmota Filesystem so it will be executed each time Tasmota device starting.

#### Mode Historique (contrat heures creuses)

```python
import json

var api_url = "https://emoncms.org/input/post"
var api_key = "YOUR_EMON_API_WRITE_KEY"
var node_name = "NODE_NAME"
var post_every = 15000 # post evert 15 seconds
var payload = {}

def send_emoncms()
  # Convert JSON object to string 
  var obj_json = json.dump(payload)

  # Create URL to call
  var param="?fulljson="+obj_json + "&node="+node_name + "&apikey="+api_key 
  # Post Data to EMONCMS
  var cl = webclient()
  cl.begin( api_url + param)
  var r =  cl.GET()
  tasmota.set_timer(post_every, send_emoncms)
end

def setcolor(iinst, isousc)
  var red = tasmota.scale_uint(iinst, 0, isousc, 0, 255)
  var green = 255 - red
  var channels = [red, green, 0]
  light.set({"channels":channels, "bri":64, "power":true})
end

# set global payload the field we need
def rule_tic(value, trigger)
  # Got Heures Creuses contract so I will calculate total consumption
  # adding Heures Creuses (HCHC) + Heures Pleines (HCHP) and create new value for emoncms 
  # Change label depending on name for your contract type
  var htot = value['HCHP'] + value['HCHC'] # mode historique
  # Create new value HTOT converted to kWH
  payload['HTOT'] = htot / 1000.0
  # Calculate current percent Load 
  var iinst = value['IINST']  # mode historique
  var isousc= value['ISOUSC'] # mode historique

  if iinst != nil && isousc != nil 
    # Drive RGB LED
    setcolor(iinst, isousc)
    if isousc > 0
      load = 100 * iinst / isousc
      payload['LOAD'] = load
    end
  end

  # Set values we need to send to emoncms
  payload['ADCO']  = value['ADCO']
  payload['HCHP']  = value['HCHP']
  payload['HCHC']  = value['HCHC']
  payload['ISOUSC']= isousc
  payload['PAPP']  = value['PAPP']
  payload['IINST'] = iinst

end

def start()
  # Callback on each frame interception
  tasmota.add_rule("TIC",rule_tic)
  # fire 1st post in 5s 
  tasmota.set_timer(5000, send_emoncms)
end

# delay start to have time to get full frame
tasmota.set_timer(10000, start)

```


#### Mode Standard (contrat heures creuses)

```python
import json

var api_url = "https://emoncms.org/input/post"
var api_key = "YOUR_EMON_API_WRITE_KEY"
var node_name = "NODE_NAME"
var post_every = 15000 # post evert 15 seconds
var payload = {}

def send_emoncms()
  # Convert JSON object to string 
  var obj_json = json.dump(payload)

  # Create URL to call
  var param="?fulljson="+obj_json + "&node="+node_name + "&apikey="+api_key 
  # Post Data to EMONCMS
  var cl = webclient()
  cl.begin( api_url + param)
  var r =  cl.GET()
  tasmota.set_timer(post_every, send_emoncms)
end

def setcolor(iinst, isousc)
  var red = tasmota.scale_uint(iinst, 0, isousc, 0, 255)
  var green = 255 - red
  var channels = [red, green, 0]
  light.set({"channels":channels, "bri":64, "power":true})
end

# set global payload the field we need
def rule_tic(value, trigger)
  # Got Heures Creuses contract so I will calculate total consumption
  # Calculate current percent Load 
  var iinst = value['IRMS1']  
  var isousc= value['PREF']*5 

  if iinst != nil && isousc != nil 
    # Drive RGB LED
    setcolor(iinst, isousc)
    if isousc > 0
      load = 100 * iinst / isousc
      payload['LOAD'] = load
    end
  end

  # Here I keep name of historique mode
  payload['ADCO']  = value['ADSC']
  payload['HTOT']  = value['EAST'] / 1000 # I need kWh
  payload['HCHP']  = value['EASF01']
  payload['HCHC']  = value['EASF02']
  payload['ISOUSC']= isousc
  payload['PAPP']  = value['SINSTS']
  payload['IINST'] = iinst

end

def start()
  # Callback on each frame interception
  tasmota.add_rule("TIC",rule_tic)
  # fire 1st post in 5s 
  tasmota.set_timer(5000, send_emoncms)
end

# delay start to have time to get full frame
tasmota.set_timer(10000, start)

```


### Customn WEB Interface

If you are not happy with values on the WEB interface you can totaly change it to fit your needs.

You can add new values to display or even disable all values displayed and use you own or even some calculated ones.

To disable all current values displayed please use the following command in console (not the berry one)

```
backlog0 WebSensor0 0 ; WebSensor1 0 ; WebSensor2 0 ; WebSensor3 0
```

Then you can add Teleinfo driver (this one as example) into a file named `teleinfo.be`

This is just an example please fill it to your needs. This one display Total `EAST` index, `URMS1` and `IRMS1` on wab interface


```python
# Teleinfo driver written in Berry
# Support for Teleinfo custom values from choosen fields returned by smart meter
 
class TELEINFO: Driver

  # Global var Needed
  var east
  var irms
  var urms

  def init()
    # initialize globals

    # Don't display original sensors on WebUI, we want just our ones
    tasmota.cmd("backlog0 WebSensor0 0 ; WebSensor1 0 ; WebSensor2 0 ; WebSensor3 0")

    # create rules to trigger when TIC values updates 
    # Use Teleinfo Etiquette Names to get whatever value you need
    tasmota.add_rule("TIC#EAST", /value -> self.trigger_east(value))
    tasmota.add_rule("TIC#IRMS1", /value -> self.trigger_irms(value))
    tasmota.add_rule("TIC#URMS1", /value -> self.trigger_urms(value))
  end

  def trigger_east(index)
    self.east = index
  end

  def trigger_irms(i)
    self.irms = i
    # DEBUG print("irms:", i)
  end

  def trigger_urms(u)
    self.urms = u
    # DEBUG print("urms:", u)
  end

  # trigger a read every second 
  def every_second()
    # DEBUG print("sensors:",tasmota.read_sensors())
  end

  # display sensor value in the web UI
  def web_sensor()
    import string
    var msg = string.format(
            "{s}Consommation{m}%d Wh{e}"
            "{s}Tension RMS{m}%d V{e}"
            "{s}Intensité RMS{m}%d A{e}", 
             self.east, self.urms, self.irms )

    tasmota.web_send_decimal(msg)
  end
end

# Instantiate the driver
teleinfo = TELEINFO()
# add it to Tasmota
tasmota.add_driver(teleinfo)


```

Then into your `autoexec.be` file just add

```python
# Auto start teleinfo driver
load("teleinfo.be")
```


<img src="https://github.com/hallard/Denky-D4/blob/main/pictures/Denky-D4-tasmota-custom-ui.png" alt="Denky D4 Custom User Interface">


# Support and discussion

If you have any issue or just want to discuss on this project, please use community [forum](https://community.ch2i.eu/category/20)

# License

<img alt="Creative Commons Attribution-NonCommercial 4.0" src="https://i.creativecommons.org/l/by-nc/4.0/88x31.png">   

This work is licensed under a [Creative Commons Attribution-NonCommercial 4.0 International License](http://creativecommons.org/licenses/by-nc/4.0/)    
If you want to do commercial stuff with this project, please contact [CH2i company](https://ch2i.eu/en#support) so we can organize an simple agreement.

# Lazy building your own? 

You can order this module (V1.1 only) fully assembled with some extra on [tindie][1]

<a href="https://www.tindie.com/products/28907/"><img src="https://d2ss6ovg47m0r5.cloudfront.net/badges/tindie-mediums.png" alt="I sell on Tindie" width="150" height="78"></a>

# Misc

See news and other projects on my [blog][2] 
 
[1]: https://www.tindie.com/products/25467/
[2]: https://hallard.me










