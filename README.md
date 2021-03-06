# Reverse-Engineering-Tenda-CP3


Hello, today im going to teach  you how to reverse engineer your Tenda CP3 camera.

First you need to connect your ip camera to your wifi. Then with Nmap (https://github.com/nmap/nmap) you can enumerate the open ports on the camera. To do a scan the command is simply nmap ip_of_your_camera

![image_2022-04-17_191717940](https://user-images.githubusercontent.com/64932654/163725335-ca33bbaa-51ba-4e0a-96f0-bcd1ccd92082.png)


We can see that 4 ports are showing up: 
Telnet(terminal network), An unknown service, h323hostcallsc(H323 Host Call Secure), sd(	Session Director)

From that we can try to connect to the camera via Telnet:

![image_2022-04-17_192005782](https://user-images.githubusercontent.com/64932654/163725417-2a241acb-85a7-4565-808d-609476ccdb4f.png)

> I didnt try to bruteforce it but the login admin and password admin didnt worked so I went ahead. 

When I was looking at the hardware I noticed two marks on the mother board: Rx and Tx. This two little ports are made to receive and send data.

![IMG-6992](https://user-images.githubusercontent.com/64932654/163731843-e46b4aca-fb68-499a-b7f3-3f6cf35c2ac2.jpg)

 I took my UART to USB connector (https://letmeknow.fr/fr/convertisseurs/1673-convertisseur-uart-usb-cp2102--4894479454459.html) and I plugged the GND and the VCC to an arduino and I soldered the RX and the TX of the board to my connector.
 
 From that I was able to receive data from the camera via this script https://github.com/devttys0/baudrate/blob/master/baudrate.py made by devttys0.
 
 When we try to connect to the camera, at the third false try the camera send this data to the TX port:
 
 ![image_2022-04-18_003230163](https://user-images.githubusercontent.com/64932654/163734678-1086a38c-2af8-42e5-bef4-e950d009c5a2.png)
 I was able to determined that if the user that you have input does not exist on the machine it will say UNKNOWN (like in the screenshot)
 but if it is a known user it will print the name of the user:
 
 ![image](https://user-images.githubusercontent.com/64932654/163734724-88f6c9b3-1fe3-4c19-b781-2ed4ad42855f.png)

> In both screenshot "pts/0" means pseudo terminal (the user logged in)

We can now determine that the user for the camera is root.

I will continue as soon as I will be able to make a working exploit.
