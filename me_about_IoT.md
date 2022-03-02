The IoT (Internet of Things) technology has a wide practical use all over the world nowadays. Every electronic device have the control options, like wired, radio, bluetooth, wi-fi network and internet. And the last one is the most interesting and it gives ability for a wide range of applications, starting from simple online smart sockets to complex systems that rely on each other and work together, as it is shown in Industry4.0 made robots, factory automation systems, drones, etc.

The IoT has several different approaches and solutions, like Lora-WAN, MQTT, API.
Building an IoT Smart Home system from scratch gives the wide range of development abilities. The aim is to create a progressive programming interface, where anyone could integrate additional device and make use of the API to control it or get sensor data from it.

The main components, that were dedicated to connect to a network, store and share states, send actions to control any electronic device, are:
- Microcomputer with a Linux operation system (example: RaspberryPi, OrangePi, or any other computer)
- ESP8266, NodeMCU for connecting circuits to Wi-Fi
- Arduino for controlling electronics.

Server-side application.
The interface of handling requests and storing the database of all devices and sensors can be written in any programming language for backend development.
Examples of languages and frameworks:
- JavaScript. Frameworks: Express, Next
- Python. Frameworks: Flask, Django, FastApi
- Go. Frameworks: Gin, Gorilla
- C#. ASP
- PHP. Laravel
- Ruby.
- Rust.
And many others.

Using relational database will be very helpful for further development, and surely it will contain a one to many and many to many relationship templates.
Authentication layer is covered by User table. API should use one of the standards of Authentication for client-side security.
Device table will store each of our ESP8266 modules, this table contain a detailed info of a controller and it's IP address in a network. When connected, a RaspberryPi acts like the same IoT device in whole system, sharing and transfering info accross the web to our modules.
The state of our electronic devices is stored in Device and Pin tables. The Pin table will be connected with device using one-to-many relaioship.
A lot more tables could be added for expanding features and security of the IoT application.

Afterwards the URL routes should be created. These routes will handle request an work on it, by storing data or running an action dedicated to that data.
The sample Device controlling route will look like following:
"/device/" methods: GET, POST
When "Get" request is done, the API will retrieve information about state of device, and "Post" will be used to update info on server and database, and send action to an IoT device.

Retrieving and sending data is done, but how about ESP8266 controllers?
Arduino Library of ESP8266 has a wide range of libraries so we will use it. Each microprocessor will store a data and share it with a main cloud, so it could be programmed as a mini-server for itself and for devices around. Creating a route "/control/" will handle the states that were send from API server and control our device.
For a lamp or wall socket devices, it requires to send "true" and "false" (1 & 0) values to it. For more complex devices, like a temperature controlling systems (coolers, heaters) it requires to send structured arguments in URL or in body of the request.
Now, when the API set and device and client-side application connected to it, changing state will update the microcontroller and therefore will manage the electric circuit.
In the whole overview, separate electronic devices, that running ESP8266 or any other controller, are connected to main hub, that handles local database and system of a Smart Home. Furthermore this hub can be connected to a remote cloud server, to update it's own states through internet connection.
How about security?
In every data exchanging and remote controlling systems, it is required to have a security layer.
As for client-side application, the security is done by "Basic" and "Bearer" authentication, as known as JWT tokens, with added SSL encryption of cloud server.
What about electronic devices, they have secret keys, that are registered on system using QR codes that are unique and attached to device.
They also validate time, connection with Home system. Therefore another more complex levels of security could be added.
Smart home systems and IoT is still adopting, more complex tasks and projects are developing on it, with added Artificial Intelligence and Blockchain.

