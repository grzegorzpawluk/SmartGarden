# SmartGarden
SmartGarden: weather station, lighting control, garden irrigation.


## Here I will describe me project.

 This project is based on Raspberry Pi Zero WH 512MB RAM - WiFi + BT 4.1. There is a server, website and stm32 handler (weather station, valve controller,led controller).
 Throuh web site users can controll lighting, garden sprinklers and receive weather informations.
 Connections between Raspberry Pi and STM are established via Bluetooth.

## To configure the system you should:
(in case of Raspberry Pi)
1.	System instalation
[Polski](https://botland.com.pl/pl/content/60-instalacja-systemu-operacyjnego-raspberry-pi)
[English](https://www.youtube.com/watch?v=RQ6JvnXwDCM)

2.	VNC (Virtual Network Computing)
[Polski](https://www.youtube.com/watch?v=LVKVGmP9hxQ)
[English](https://www.raspberrypi.org/documentation/remote-access/vnc/)

3.	LAMP
https://howtoraspberrypi.com/how-to-install-web-server-raspberry-pi-lamp/
in my case I don't use 
```
DROP USER 'root'@'localhost';
```

4.	No-IP: Free Dynamic DNS 
[noip](https://www.noip.com/support/knowledgebase/install-ip-duc-onto-raspberry-pi/)

5.	SSL
[ltes encrypt](https://pimylifeup.com/raspberry-pi-ssl-lets-encrypt/)
[How to fix missing dirmngr](https://blog.sleeplessbeastie.eu/2017/11/02/how-to-fix-missing-dirmngr/)
sudo apt-get install dirmngr --install-recommends

6. Download web page
-install git
-sudo rm -rf /var/www
-git clone https://github.com/grzesiek1406/SmartGarden-web.git (in /var)
 -modify html/includes/connect.php to your database
-git clone https://github.com/grzesiek1406/SmartGarden-pilothouse.git (in /home/user)
 -in logs/ execute chmod 777 * in case to allow www user to modify these files

7. Configure database, create tables
TODO: SQL script

8. CRON
[cron instruction](https://www.raspberrypi.org/documentation/linux/usage/cron.md)

past this to crontab -e
```
@reboot sleep 60 && /home/pi/serwer_raspberry_pi/rebootNotification.py
*/5 * * * * /home/pi/serwer_raspberry_pi/connections/reconnect_led_controller.sh
*/5 * * * * /home/pi/serwer_raspberry_pi/connections/reconnect_valve_controller.sh
*/5 * * * * /home/pi/serwer_raspberry_pi/connections/reconnect_weather_device.sh
*/5 * * * * /home/pi/serwer_raspberry_pi/valveAutoOff.py
2 * * * * /usr/bin/php /home/pi/serwer_raspberry_pi/connections/send_data_to_db.php
```

9. Configuration of /etc/rc.local
paste this to /etc/rc.local
```
/home/pi/serwer_raspberry_pi/connections/WEATHER_INFO_connect &
/home/pi/serwer_raspberry_pi/connections/LED_CTRL_connect &
/home/pi/serwer_raspberry_pi/connections/VALVE_CTRL_connect &
/home/pi/serwer_raspberry_pi/connections/change_permission_all &
/home/pi/serwer_raspberry_pi/connections/gpio_config.sh &
/home/pi/serwer_raspberry_pi/connections/periodically_receive_run_all.sh &
```

10. Watchdog
[Setting up the raspberry pi watchdog](https://www.domoticz.com/wiki/Setting_up_the_raspberry_pi_watchdog)

11. Fail2Ban (in case of some attacks)
[Main page](https://www.fail2ban.org/wiki/index.php/Main_Page)
[How to install](https://blog.rapid7.com/2017/02/13/how-to-protect-ssh-and-apache-using-fail2ban-on-ubuntu-linux/)
is error in sudo nano /etc/fail2ban/jail.local, should be
```
##To stop DOS attack from remote host.
[http-get-dos]

```
however, solution is in comments

TODO: fill this pages
## The source code will be avalible here:
 - [Web site](https://github.com/grzesiek1406/SmartGarden-web)
 - [Pilthouse](https://github.com/grzesiek1406/SmartGarden-pilothouse)
 - [Weather station](https://github.com/grzesiek1406/SmartGarden-weather-station)
 - [Valve controller](https://github.com/grzesiek1406/SmartGarden-valve-controller)
 - [Led controller](https://github.com/grzesiek1406/SmartGarden-led-controller)
