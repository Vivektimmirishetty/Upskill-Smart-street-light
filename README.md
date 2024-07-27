# Smart iot street light 

First we have to setup a AWS account. 

In AWS we get S3 buckets to store the data.

Create a new bucket and get access to your "ACCESS_KEY_ID", "ACCESS_SECRET_KEY", "BUCKET_NAME"

Enter the same details in "boto_v2.py". boto is the API used to send data from raspberry pi to AWS s3 bucket.

Also take care of proper path names in the codes. do remember that all the codes have to be in the same folder as "cam.py" and "boto_v2.py" are subroutines called from main program.

Once all the connections are made run the "smart_light_main.py" and the application should keep running.
Project Overview
Welcome to the Smart Lighting and Detection System project! This project utilizes a Raspberry Pi to create an intelligent lighting system that can respond to environmental changes and user interactions. The system integrates various sensors and components, including an LDR (Light Dependent Resistor), an infrared sensor, and a button, to control the brightness of an LED and trigger other actions.

Features
Automatic Brightness Control: The system adjusts the LED brightness based on the ambient light detected by the LDR.
Infrared Detection: The infrared sensor detects objects' proximity and adjusts the LED brightness accordingly.
Manual Control: A button allows manual toggling of the LED and initiates other processes.
Script Integration: The system can run external Python scripts based on sensor inputs.
Components
Raspberry Pi: The brain of the system.
LDR (Light Dependent Resistor): Detects ambient light levels.
Infrared Sensor: Detects the proximity of objects.
Button: Provides manual control.
LED: Indicates the system's status and responds to sensor inputs.
Installation
Hardware Setup:

Connect the LDR to GPIO pin 21.
Connect the infrared sensor to GPIO pin 18.
Connect the button to GPIO pin 23.
Connect an LED to GPIO pin 24.
Connect another LED to GPIO pin 26 for PWM control.
Software Setup:

Ensure you have Python installed on your Raspberry Pi.
Install necessary libraries:
bash
Copy code
sudo apt-get update
sudo apt-get install python3-gpiozero
sudo pip3 install RPi.GPIO
Clone the Repository:

bash
Copy code
git clone https://github.com/yourusername/smart-lighting-detection-system.git
cd smart-lighting-detection-system
Usage
Running the Main Script:

Execute the main script to start the system:
bash
Copy code
python3 main.py
Button Interaction:

Press the button connected to GPIO pin 23 to toggle the LED and initiate external scripts.
Infrared Sensor:

The LED brightness will increase if the infrared sensor detects an object far away and decrease if an object is near.
LDR Functionality:

The system will adjust the LED brightness based on the ambient light level detected by the LDR.
Code Explanation
The main script (main.py) includes the following functionalities:

PWM Control:

python
Copy code
pwm1 = GPIO.PWM(LED1, 1000)  # Activate PWM on LED1 at 1000 Hz
pwm1.start(0)
Brightness Adjustment Functions:

python
Copy code
def dim(bright):
    bright = bright / 2
    pwm1.ChangeDutyCycle(20)

def brighten(bright):
    bright = bright * 2
    pwm1.ChangeDutyCycle(100)
Sensor and Button Handling:

python
Copy code
def infrared():
    if(GPIO.input(18) == False):  # Object is far away
        brighten(1)
        time.sleep(3)
    if(GPIO.input(18) == True):  # Object is near
        dim(1)

def button():
    button_state = GPIO.input(23)
    if button_state == False:
        GPIO.output(24, True)
        return 1
    else:
        GPIO.output(24, False)
        return 2
Main Loop:

python
Copy code
while 1:
    time.sleep(0.000001)
    if (button() == 1):
        pwm1.start(0)
        brighten(1)
        subprocess.call(["python", "cam.py"])
        subprocess.call(["python3", "boto_v2.py"])
    
    elif (ldr.value < 0.4):
        pwm1.start(0)
    else:
        pwm1.stop(0)
GPIO.cleanup()
Contribution
Feel free to fork this repository, submit issues, and send pull requests. Any contributions to improve the system are welcome!

License
This project is licensed under the MIT License - see the LICENSE file for details.

Happy coding and enjoy creating your Smart Lighting and Detection System! If you have any questions or need further assistance, please open an issue or contact me at your-email@example.com.

Contact
Author: Timmirishetty Vivek
Email: timmirishettyvivek@gmail.com
LinkedIn: Vivek Timmirishetty
This project was created with the intention of making a smart, responsive lighting system using simple components and a Raspberry Pi. I hope it serves as a helpful guide and inspires further innovation.
