# IoT-LoRaWAN-Datalogger-Node-RED

Node-Red full programming for IoT-LoRaWAN-Datalogger-Node-RED field station automated using Raspberry Pi and MQTT communication.

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
- The JSON programming code in Node-Red is able to plot and store the data from LoRaWAN sensors connected through MQTT Gateway to Raspberry Pi. 
- This system was tested in Raspberry Pi 4, Zero, and Zero W. However you can deploy it on Raspberry Pi 3 also.
- The version of the Node-RED was 1.3.5. However, it is possible to use it in recent versions of Node-Red. Note to in recent versions (newest NodeJs versions), the variables must be declared with the type before the name (example: var name_variable;).
- This project was deployed in a remote area in the field. Thus, internet access and electrical source were not available. Therefore, to implement the system was used a 12V 100W solar panel and a 12V 70Ah battery.
- We use the DLS08 outdoor gateway from Dragino which is able to send and receive data from sensors using LoRa. But also, is capable of creating a local Wi-Fi network where the Raspberry was connected to establish the MQTT communication. The gateway also allows SIMCARD use, so it is possible to use an internet connection through implement this chip. See the gateway here(https://www.dragino.com/products/lora-lorawan-gateway/item/160-dlos8.html).
- Note that in cases where Raspberry Pi has not internet connection, the date and time will be lost in case of some rebooting. To solve this, the Node-Red programming includes nodes that take automatically this information from the device that the user is using to be connected to the developed web application. In addition, you will see that information at the head of every tab.
- Raspberry Pi and gateway must be connected to the same Wi-Fi network for the MQTT communication work. And the Raspberry Pi will be used as a broker.
- The sensors used in the programming were the LSE01 soil moisture and EC sensor. However, by implementing the proper decoding function other LoRa sensors can be used. See the sensor used here: https://www.dragino.com/products/lora-lorawan-end-node/item/159-lse01.html.
- Every sensor must be set using the proper AT commands. Keep into account that every device must be in the same Frequency Sub Band than the gateway. see the sensor manual: (https://www.dragino.com/downloads/downloads/LoRa_End_Node/LSE01/LSE01_AT_Commands_v1.0.pdf) and 
- and must be registered in the gateway previously.
- However, it is possible to use this system with any MQTT sensor.
- Remember that it is necessary to install the mosquitto broker on the Raspberry Pi (https://randomnerdtutorials.com/how-to-install-mosquitto-broker-on-raspberry-pi/).
- Necessary to install MariaDB server in Raspberry Pi for MySQL use (https://raspberrytips.com/install-mariadb-raspberry-pi/).
- Remember that you need to install the missing modules that Node-Red will notify you after importing the flows.
- In addition by setting personalized parameters is possible to store the data in a MySQL database and download them in a CSV format.
- Please navigate to the center of every flow where nodes are programmed.

# The Node-Red Flows:

All flows and some nodes include comments inside to easily understand the code.

- User Parameters Flow


Web User Application - Tab settings configuration

You will find here: 

Nodes that in case of some unexpected electrical reboot the system must be initialized with previous parameters set by the user, this information is extracted from the database directly.

The flows for setting the experiment information and email where alerts will be sent.

Apply the configuration selected and extracted from MySQL after an unexpected reboot

- Alarms

You will find here:

The nodes are necessary to alert the user when temperature or humidity is over the previously set reference limits.

- Sensors & Data Integration


Web User Application - Tab monitoring

You will find here:

Sensors data provisional replace for precise the control logic in case of some sensor fails (because of electrical or network failure), it will take data from the other one placed closely.

Calculate the average data of the Temperature, Humidity, Dew point & VPD.

- Control

You will find here:

This is based on a system controlled by relays connected to the Raspberry Pi and an additional electrical power system to control motors.

These motors are working as heat extractors (extractors/fans) and will be controlled regarding the data from sensors and references set by the user in the dashboard.

There are four extractors (in our project) connected to pins 29,31,33 and 36 of the Raspberry Pi. We called them:

Fan left 1 (fl1), Fan left 2 (fl2), Fan right 1 (fr1), Fan right 2 (fr2).

Users can set the reference values for temperature control

fref1 = Temperature during the day. 

fref2 = Temperature to step change and prevent high gradient rate. 

fref3 = Temperature during the night.

The day and night limits were created to control the temperature in parameters near real behavior. This means that in real conditions in the field, there are higher temperatures during the day than at night. So, this affects the plants and for that reason, the programming includes the possibility for the user can set the reference temperatures to be controlled automatically in these periods daily and simulate an environment close to real scenarios.

Note to:

Between fTime2 and fTime 3 is the day; Between fTime4 and fTime1 is night; Between fTime1 and fTime2 & fTime3 and fTime4 are the steps, where the temperature should be set in a middle point to not apply high changes directly to the plants.

The heat extractors will be turned on if the temperature is 1°C above reference (tref) in that period otherwise will be turned off.

Users can also select in the dashboard if every extractor works with every tref  (day, night, step) then, the control action will use or not that specific extractor to increase or decrease the temperature.

Remember that fref1 is the temperature during the day the day and night limits can be changed in the "fTime&fRef limits control" node.

The Cooling (irrigation) flow is based on a system controlled by electro-valves which control the flow of water to irrigate the soil or the plants inside the greenhouse.

The irrigation/cooling control is connected to the pin35 in the Raspberry Pi. However, you can change it as well as you need.

- Database

You will find here:

These flows are for storing in the database.

Note that data will be stored every ten minutes. You can change this in the "Store interval" node.

Data will be saved in the table DATA (you can change this on every DataSensor node).

Remember that data must coincide with the columns in the created table.

Nodes that will help you to create your personalized MySQL table and will guide you in the process of storing data.

Nodes to download data from the MySQL table directly.

Remember that this will only download your information related to the current trial in the course selected by the user in the dashboard; This was done in that way to not overload the Raspberry Pi with downloading very heavy files.

Note that are created two files to be downloaded. One file is from the DATA database and contains all information from sensors stored. The second one is from the SETTINGS database, which stored all configurations that the user set before. Then is possible to download both files with different information but depending on the current trial name.

PLEASE MAKE SURE THAT THE TWO FILES (FROM DATA AND CONFIG) ARE NAMED CORRECTLY AND DIFFERENTLY.

We have tested Raspberry Pi under greenhouse conditions (high temperatures and humidity) and we are totally sure that these ones are robust enough to work perfectly in these hard environments. This enables the possibility to have trustable sophisticated datalogger and controllers at low-cost using IoT.

Hope this will be useful for your projects. Let´s improve the plant research process together!!!

Developed by Dpineda.

Feel free to contact me at duvanpineda09@gmail.com
