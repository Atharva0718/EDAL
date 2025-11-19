Edge-to-Cloud Data Analytics
Objective

Send sensor data from a Python edge device to Azure IoT Hub, process it using Stream Analytics, and show results live in Power BI.

Steps
1. Create Azure Resources

Make a Resource Group.

Create an IoT Hub (Free Tier).

2. Register Device

Add new device in IoT Hub.

Copy the device connection string.

3. Python Edge Simulator

Install:

pip install azure-iot-device


Code:

from azure.iot.device import IoTHubDeviceClient, Message
import random, time

CONNECTION_STRING = "YOUR_CONNECTION_STRING"

client = IoTHubDeviceClient.create_from_connection_string(CONNECTION_STRING)

while True:
    temp = random.uniform(20, 35)
    hum = random.uniform(40, 80)
    msg = Message(f'{{"temperature": {temp}, "humidity": {hum}}}')
    client.send_message(msg)
    print(msg)
    time.sleep(3)


Run:

python edge_device.py

4. Stream Analytics Job

Add Input → IoT Hub.

Add Output → Power BI.

Query:

SELECT AVG(temperature) AS avg_temp,
       AVG(humidity) AS avg_humidity,
       System.Timestamp AS event_time
INTO pbioutput
FROM iotinput
GROUP BY TumblingWindow(second, 10)


Start the job.

5. Power BI Dashboard

Open dataset → Create report.

Add charts to see live data every 10 seconds.

Summary

A Python edge device sends sensor data → Azure IoT Hub → Stream Analytics → Power BI live dashboard.
