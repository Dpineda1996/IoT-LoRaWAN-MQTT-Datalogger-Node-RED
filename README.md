# IoT-LoRaWAN-MQTT-Datalogger-Node-RED

Node-Red full open access programming for IoT-LoRaWAN-Datalogger-Node-RED field station automated using Raspberry Pi and MQTT communication.

*This Project was developed as part of the Alliance of Bioversity International & CIAT IoT initiatives.

**This is a full Node-Red flow that was initially developed for automatically monitoring and storing data from LoRaWAN sensors in the field of Palmira Campus in Colombia. A project led by the Bean Physiology Team.

***This is one of the projects developed focused to support crop monitoring (Beans) in remote areas initially in Colombia.

***You will find control logic and specific programming based on many years of real experience working with plants of different crops (mainly Beans).

***Developed for beginners and experts in Node-Red programming to have quick interaction between Software and Hardware with easy-to-implement IoT web applications.

***The programming uses MQTT and MySQL connections. Developed for any Raspberry Pi use. In addition, a user-friendly dashboard was developed for all users in the world can easily interact remotely and set their own parameters according to their plants' needs.

*This project involved collaboration with many plant scientists in the Alliance of Bioversity International & CIAT. 

*Special thanks to Drs. Milan Urban, Ph.D.; Steve Beebe, Ph.D.; Clare Mukankusi, Ph.D.; for their invaluable support and funding. And Eng Harold Diaz, my colleague who began whit these implementations and developed the first version used as a basic template for this project. 

*In addition, our technician Fabricio Soto and the Electronical Engineer trainees Alejandra Guzman and Sebastian Pinzon contributed to this project.

# Before starting:
- The JSON programming code in Node-Red is able to plot and store the data from LoRaWAN sensors connected through MQTT Gateway to Raspberry Pi instead that using TTN. 
- This system was tested in Raspberry Pi 4, Zero, and Zero W. However you can deploy it on Raspberry Pi 3 also.
- The version of the Node-RED was 1.3.5. However, it is possible to use it in recent versions of Node-Red. Note to in recent versions (newest NodeJs versions), the variables must be declared with the type before the name (example: var name_variable;).
- This project was deployed in a remote area in the field. Thus, internet access and electrical source were not available. Therefore, to implement the system was used a 12V 100W solar panel and a 12V 70Ah battery.
- We use the DLS08 outdoor gateway from Dragino which is able to send and receive data from sensors using LoRa. But also, is capable of creating a local Wi-Fi network where the Raspberry was connected to establish the MQTT communication. The gateway also allows SIMCARD use, so it is possible to use an internet connection through implement this chip. See the gateway here(https://www.dragino.com/products/lora-lorawan-gateway/item/160-dlos8.html).
- Note that in cases where Raspberry Pi has no internet connection, the date and time will be lost in case of some rebooting. To solve this, the Node-Red programming includes nodes that take automatically this information from the device that the user is using to be connected to the developed web application. In addition, you will see that information at the head of every tab.
- Raspberry Pi and gateway must be connected to the same Wi-Fi network for the MQTT communication work. And the Raspberry Pi will be used as a broker.
- The sensors used in the programming were the LSE01 soil moisture and EC sensor. However, by implementing the proper decoding function other LoRa sensors can be used. See the sensor used here: https://www.dragino.com/products/lora-lorawan-end-node/item/159-lse01.html and how to communicate the sensor using ABP (our case) http://wiki.dragino.com/xwiki/bin/view/Main/Communicate%20with%20ABP%20End%20Node%20on%20the%20LPS8-V2%20Gateway/.
- Every sensor must be set using the proper AT commands. Keep into account that every device must be in the same Frequency Sub Band as the gateway. Remember to extract from the sensor the Network Session Key, the App Session Key, and the Device address. These parameters are essential in the configuration so they can send the data to the gateway. see the sensor manual: (https://www.dragino.com/downloads/downloads/LoRa_End_Node/LSE01/LSE01_AT_Commands_v1.0.pdf).
- In the gateway you must configure the LoRa, and MQTT parameters. Every sensor must be registered to the communication works. Do not forget to use the MQTT communication instead TTN proposed by the manufacturer. See how to configure the gateway: https://www.youtube.com/watch?v=AmRbabOOaIs and the user manual: https://www.dragino.com/downloads/downloads/LoRa_Gateway/DLOS8/DLOS8_LoRaWAN_Gateway_User_Manual_v1.3.pdf
- Remember that it is necessary to install the mosquitto broker on the Raspberry Pi (https://randomnerdtutorials.com/how-to-install-mosquitto-broker-on-raspberry-pi/).
- Necessary to install MariaDB server in Raspberry Pi for MySQL use (https://raspberrytips.com/install-mariadb-raspberry-pi/).
- Remember that you need to install the missing modules that Node-Red will notify you after importing the flows.
- In addition by setting personalized parameters is possible to store the data in a MySQL database and download them in a CSV format.
- Please navigate to the center of every flow where nodes are programmed.
- A dynamic storing of data in MySQL and the download between selectable date limits are innovative features that I implemented in this version of programming. Therefore my repository https://github.com/Dpineda1996/IoT-Greenhouse-Temperature-and-Irrigation-Controller-Node-Red.git does not include these characteristics.

# The Node-Red Flows:

All flows and some nodes include comments inside to easily understand the code.

- Initialize

Start the flows and control the parameters change made by the user through the web application.- Alarms

You will find here:

The nodes are necessary to alert the user when temperature or soil humidity is over the previously set reference limits.

- User Parameters Flow

![alt text](https://github.com/Dpineda1996/IoT-LoRaWAN-MQTT-Datalogger-Node-RED/main/settings or parameters user tab.png?raw=true)
Web User Application - Tab settings configuration

You will find here: 

Nodes that in case of some unexpected electrical reboot the system must be initialized with previous parameters set by the user, this information is extracted from the database directly.

The flows for setting the experiment information and email where alerts will be sent.

Apply the configuration selected and extracted from MySQL after an unexpected reboot


- Sensors LoRaWAN

![Appimg](https://github.com/Dpineda1996/IoT-LoRaWAN-MQTT-Datalogger-Node-RED/assets/77678151/f2d2e626-34e6-455b-b0a3-9647e49d2ec6)
Web User Application - Tab monitoring

You will find here:

Programming logic to receive and decode sensor data using MQTT. You can add as many as sensors you need only by adding the MQTT node and implementing the specific decoding function.

- Database

![app_save](https://github.com/Dpineda1996/IoT-LoRaWAN-MQTT-Datalogger-Node-RED/assets/77678151/397bec85-b29b-4803-aa8a-e3ffcb1bf15f)
Web User Application - Tab download data

You will find here:

A new feature is used to dynamically store data in a MySQL table. With this programming, is not necessary to modify the initially created table if we need to store a new variable non-stored before. Because the nodes automatically will check if the column to store the new variable exists. If not, it will create it and then data are stored.

The implementation for downloading data based on date limits that the user can choose using the dashboard or web application. 

Nodes to automatically set the date and time into the Raspberry Pi.

We have tested Raspberry Pi under field environment conditions (high temperatures, humidity, sunlight, air) and we are totally sure that these ones are robust enough to work perfectly in these hard environments. This enables the possibility to have a trustable sophisticated datalogger at a low-cost using IoT.

Hope this will be useful for your projects. Let´s improve the plant research process together!!!

Developed by Dpineda.

Feel free to contact me at duvanpineda09@gmail.com
