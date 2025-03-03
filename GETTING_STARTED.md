# Getting Started with Smart IoT Security Framework

This guide will help you set up and deploy the Smart IoT Security Framework on your IoT devices.

## Prerequisites

Before you begin, ensure you have the following:

1. **AWS Account** with access to AWS IoT Core
2. **Python 3.7+** installed on your development machine and target IoT devices
3. **MQTT Broker** (like Mosquitto) for local communication (optional but recommended)
4. **IoT Device** capable of running Python (Raspberry Pi, ESP32, etc.)

## Step 1: Set Up AWS IoT Core

1. **Create a Thing** in AWS IoT Core:
   ```
   AWS Console → IoT Core → Manage → Things → Create things
   ```

2. **Generate Certificates**:
   - During thing creation, choose to generate certificates
   - Download the certificate, private key, and root CA certificate
   - Activate the certificate

3. **Create a Policy** and attach it to your certificate:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": "iot:Connect",
         "Resource": "arn:aws:iot:your-region:your-account-id:client/${iot:Connection.Thing.ThingName}"
       },
       {
         "Effect": "Allow",
         "Action": "iot:Subscribe",
         "Resource": "arn:aws:iot:your-region:your-account-id:topicfilter/devices/${iot:Connection.Thing.ThingName}/*"
       },
       {
         "Effect": "Allow",
         "Action": "iot:Receive",
         "Resource": "arn:aws:iot:your-region:your-account-id:topic/devices/${iot:Connection.Thing.ThingName}/*"
       },
       {
         "Effect": "Allow",
         "Action": "iot:Publish",
         "Resource": "arn:aws:iot:your-region:your-account-id:topic/devices/${iot:Connection.Thing.ThingName}/*"
       }
     ]
   }
   ```

## Step 2: Install the Framework

1. **Clone the repository**:
   ```bash
   git clone https://github.com/yourusername/smart-iot-security-framework.git
   cd smart-iot-security-framework
   ```

2. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

3. **Configure the framework**:
   - Copy the example configuration file:
     ```bash
     cp config.example.json config.json
     ```
   - Edit the configuration with your AWS IoT Core details and device information:
     ```bash
     nano config.json
     ```
   - Update the following fields:
     - `device_id`: Your device's unique identifier
     - `aws.endpoint`: Your AWS IoT Core endpoint
     - `aws.cert_path`: Path to your device certificate
     - `aws.key_path`: Path to your device private key
     - `aws.root_ca_path`: Path to the AWS root CA certificate

## Step 3: Deploy to Your IoT Device

1. **Transfer files to your device**:
   - If using Raspberry Pi or similar:
     ```bash
     scp -r smart-iot-security-framework pi@your-device-ip:~/
     ```

2. **Set up certificates**:
   - Place your AWS IoT certificates in the specified paths
   - Ensure proper file permissions:
     ```bash
     chmod 400 /path/to/private.pem.key
     ```

3. **Install local MQTT broker** (if using local communication):
   - On Raspberry Pi:
     ```bash
     sudo apt-get update
     sudo apt-get install -y mosquitto mosquitto-clients
     sudo systemctl enable mosquitto.service
     sudo systemctl start mosquitto.service
     ```

## Step 4: Run the Security Monitor

1. **Basic run**:
   ```bash
   cd smart-iot-security-framework
   python security_monitor.py
   ```

2. **Run as a service** (recommended for production):
   - Create a systemd service:
     ```bash
     sudo nano /etc/systemd/system/iot-security.service
     ```
   - Add the following content:
     ```
     [Unit]
     Description=IoT Security Monitor
     After=network.target

     [Service]
     Type=simple
     User=pi
     WorkingDirectory=/home/pi/smart-iot-security-framework
     ExecStart=/usr/bin/python3 /home/pi/smart-iot-security-framework/security_monitor.py
     Restart=always
     RestartSec=10

     [Install]
     WantedBy=multi-user.target
     ```
   - Enable and start the service:
     ```bash
     sudo systemctl enable iot-security.service
     sudo systemctl start iot-security.service
     ```

## Step 5: Verify Operation

1. **Check logs**:
   ```bash
   sudo journalctl -u iot-security.service -f
   ```

2. **Verify AWS IoT connection**:
   - Go to AWS IoT Core → Monitor → MQTT test client
   - Subscribe to `devices/your-device-id/status`
   - You should see status messages from your device

3. **Testing anomaly detection**:
   - The system needs some time to learn normal behavior patterns
   - Initially, it will operate in learning mode
   - After collecting enough data (typically a few hours of operation), it will start detecting anomalies

## Step 6: Integration with Your IoT Application

To integrate the security framework with your existing IoT application:

1. **Import the security monitor**:
   ```python
   from iot_security import IoTSecurityMonitor
   ```

2. **Initialize and connect**:
   ```python
   security_monitor = IoTSecurityMonitor(
       device_id="your-device-id",
       aws_endpoint="your-aws-endpoint.amazonaws.com",
       cert_path="/path/to/certificate.pem.crt",
       key_path="/path/to/private.pem.key",
       root_ca_path="/path/to/AmazonRootCA1.pem"
   )
   security_monitor.connect()
   ```

3. **Add to your main loop**:
   ```python
   try:
       while True:
           # Your IoT application code
           time.sleep(1)
   except KeyboardInterrupt:
       security_monitor.disconnect()
   ```

## Troubleshooting

### AWS Connection Issues
- Verify your AWS IoT Core endpoint
- Check certificate and key permissions
- Ensure your policy allows the necessary MQTT operations
- Test connectivity with the AWS IoT Device SDK test client

### Anomaly Detection Issues
- The model needs sufficient data to work effectively
- Adjust the anomaly threshold in the configuration
- Check that feature extraction is appropriate for your device

### Local MQTT Issues
- Verify the MQTT broker is running: `systemctl status mosquitto`
- Test MQTT connectivity: `mosquitto_sub -t "test/topic"`
- Check firewall settings if using multiple devices

## Next Steps

- Set up CloudWatch alerts for security events
- Configure automated responses to security incidents
- Integrate with your existing monitoring infrastructure
- Customize the anomaly detection for your specific device behavior

For more advanced configurations and features, see the [ADVANCED_CONFIGURATION.md](ADVANCED_CONFIGURATION.md) document.
