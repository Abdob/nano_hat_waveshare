# nano_hat_waveshare
LTE module for jetson nano

Walkthrough
__________

First I follow these instructions and they work well:

Software setup

Open a terminal and install libraries by the following commands
        sudo apt-get update
        sudo apt-get python3-pip
        sudo pip3 install pyserial
        sudo apt-get install p7zip
        Download demo codes
        mkdir -p ~/Documents/SIM7600X_4G_for_JETSON_NANO
        wget -P ~/Documents/SIM7600X_4G_for_JETSON_NANO/ https://www.waveshare.com/wiki/File:SIM7600X_4G_for_JETSON_NANO.7z
        cd ~/Documents/SIM7600X_4G_for_JETSON_NANO/

My First problem I encounter is this instruction: 

sudo p7zip --uncompress SIM7600X_4G_for_JETSON_NANO.7z
sudo p7zip --uncompress File:SIM7600X_4G_for_JETSON_NANO.7z
/usr/bin/p7zip: cannot read SIM7600X_4G_for_JETSON_NANO.7z
ERRORS:
Is not archive
    
Can't open as archive: 1
Files: 0
Size:       0
Compressed: 0

So I download directly from website: https://www.waveshare.com/wiki/File:SIM7600X_4G_for_JETSON_NANO.7z

These next instructions work:
sudo p7zip --uncompress SIM7600X_4G_for_JETSON_NANO.7z
sudo pip3 install Jetson.GPIO
sudo groupadd -f -r gpio
sudo usermod -a -G gpio your_user_name
sudo apt-get install minicom -y

These next ones didn't work instructions work:
echo 200 > /sys/class/gpio/export
echo out > /sys/class/gpio200/direction
echo 1 > /sys/class/gpio200/value
echo 0 > /sys/class/gpio200/value
echo 0 > /sys/class/gpio200/valuenano@nano:~/Documents/SIM7600X_4G_for_JETSON_NANO$ echo 200 > /sys/class/gpio/export
nano@nano:~/Documents/SIM7600X_4G_for_JETSON_NANO$ echo out > /sys/class/gpio200/direction
-bash: /sys/class/gpio200/direction: No such file or directory
nano@nano:~/Documents/SIM7600X_4G_for_JETSON_NANO$ echo 1 > /sys/class/gpio200/value
-bash: /sys/class/gpio200/value: No such file or directory
nano@nano:~/Documents/SIM7600X_4G_for_JETSON_NANO$ echo 0 > /sys/class/gpio200/value
-bash: /sys/class/gpio200/value: No such file or directory
This seems to worked:
sudo echo 200 > /sys/class/gpio/export
sudo echo out > /sys/class/gpio/gpio200/direction
sudo echo 1 > /sys/class/gpio/gpio200/value
sudo echo 0 > /sys/class/gpio/gpio200/value
Next I made sure the NET led is blinking, the hardware is connected properly and I made sure the DIP switch is ON as shown in the image.

On one terminal I do:
sudo minicom -D /dev/ttyTHS1 -b 115200
sudo minicom -D /dev/ttyUSB2 -b 115200
On another terminal I do:
pip3 install pyserial
sudo python3 SIM7600X_4G_for_JETSON_NANO/AT/AT.py
Please input the AT command:AT+CSPN  
Upon typing AT+CSPN I get this on the ttyTHS1 and ttyUSB2 terminal respectively:
Port /dev/ttyTHS1, 22:17:39

Press CTRL-A Z for help on special keys


ERROR
Compiled on Aug 13 2017, 15:25:34.
Port /dev/ttyUSB2, 23:30:08

Press CTRL-A Z for help on special keys


+CGREG: 2,1,3468,173A502

OK

+CNSMOD: 1,8

OK

+CGREG: 2,1,3468,173A502
Next I plug headphones to the headphones jack and I add my number to PhoneCall.py

sudo python3 SIM7600X_4G_for_JETSON_NANO/PhoneCall/PhoneCall.py
I get:
Traceback (most recent call last):
  File "SIM7600X_4G_for_JETSON_NANO/PhoneCall/PhoneCall.py", line 52, in <module>
    send_at('ATD'+phone_number+';','OK',1)
  File "SIM7600X_4G_for_JETSON_NANO/PhoneCall/PhoneCall.py", line 21, in send_at
    if back not in rec_buff.decode():
AttributeError: 'str' object has no attribute 'decode'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "SIM7600X_4G_for_JETSON_NANO/PhoneCall/PhoneCall.py", line 57, in <module>
    except keyboardInterrupt:
NameError: name 'keyboardInterrupt' is not defined
Then I edit PhoneCall.py again I replace 'rec_buff.decode()' with 'rec_buff'  and try again, I tried many times, I received a call once, I didn't get anything on the ttyTHS1 terminal, I get:
SIM7600X is starting:
SIM7600X is ready
ATD14087149600; ERROR
ATD14087149600; back:	
Call disconnected

I try the SMS example  with similar procedure as the PhoneCall example I get:
SIM7600X is loging off:
Good bye
SIM7600X is starting:
SIM7600X is ready
Sending Short Message Test:
Setting SMS mode...
AT+CMGF=1 ERROR
AT+CMGF=1 back:	
Sending Short Message
AT+CMGS="14087149600" ERROR
AT+CMGS="14087149600" back:	
error0
Receive Short Message Test:

Please send message to phone 14087149600
Setting SMS mode...
AT+CMGF=1 ERROR
AT+CMGF=1 back:	
AT+CPMS="SM","SM","SM" ERROR
AT+CPMS="SM","SM","SM" back:	
AT+CMGR=1 ERROR
AT+CMGR=1 back:	
error0
SIM7600X is loging off:
Good bye

The other examples did not work as well.

My SIM CARD is from Tmobile it worked on another LTE dongle. 

Are there any suggestions for getting the samples to work as a starting point, are there demo code written in C/C++ for this module I can try?
