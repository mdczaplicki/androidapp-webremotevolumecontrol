# androidapp-websoundcontrol
Adjust sound volume of your Android device from any device that has a web browser.
This includes non-Android devices such as Apple or Microsoft devices, maybe televisions or even maybe watches if these things have web browsers.

This Android application will be free and will not include ads.

How to use it :
===============

First, start this application on your Android device, it will display the internet address (URL, example : http://192.168.1.42:9000/) you have to connect to in order to control sound volume remotely.
Then, on any other device connected on the same local network (Wifi) as your android device, open a web browser (Chrome, Safari, Firefox, whatever, any web browser should work), and navigate to above address.
Finally, on the page that appears, press buttons to remotely adjust your android device's sound volume.

How it actually works :
=======================

On your Android device, this app will start a lightweight minimalistic and app-specific web server, as a foreground service.
This web server will listen on port 9000 (default) and serve a static html page (single page application).
That single page will display only two buttons, Raise Volume and Lower Volume, which when clicked will asynchronously tell the web server / Android device to adjust main sound volume.

The web server isn't really one : it does not list directories or serve any requested file from filesystem.
It only responds to a few commands (URLs below) (see the switch case in HttpServer.java, subclass ClientThread, method Run) :
/ : serves the web page
/volume-up.png : serves the volume-up.png image included in the web page
/volume-down.png : serves the volume-up.png image included in the web page
/volume-up : raises volume
/volume-down : lowers volume
any other URL will respond with a 404.

Also it is supposed to listen only on local address : it obtains your local IP address and listens only on that address.
As a consequence it should not be accesible from the Internet, evenif your internet box doesn't have a firewall with 9000 port closed by default.

I did not include a Mute button or a Slider input at the moment because to do so Android API seems to require me to point at a specific audio stream : I supposed this would not work in any situation, or make things complicated, so I gave up for now.