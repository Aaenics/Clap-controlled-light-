# sound_sensor_with_relay
This project uses the sound sensor and the relay to control a higher voltage bulb, when you clap.

#code explanation
This code controls an LED light based on claps detected by a sensor. Let's break it down step by step:

**1. Variable Definitions:**

* `Ledpin`: This integer variable stores the pin number (8 in this case) connected to the LED.
* `Sensorpin`: This integer variable stores the pin number (7 in this case) connected to the sensor that detects claps.
* `clap`: This integer variable keeps track of the number of claps detected in a short period.
* `detection_range_start`: This long variable stores the time (in milliseconds) when a clap is first detected.
* `detection_range`: This long variable is used to calculate the time difference between claps.
* `light_status`: This boolean variable indicates whether the LED is currently on (true) or off (false).

**2. Setup Function:**

* `pinMode(Sensorpin, INPUT)`: This line configures the sensor pin as an input, meaning it will receive signals from the sensor.
* `pinMode(Ledpin, OUTPUT)`: This line configures the LED pin as an output, allowing the code to control the LED state.
* `Serial.begin(9600)`: This line initializes serial communication at a baud rate of 9600. This might be used for debugging by printing messages to a serial monitor.

**3. Loop Function (main program loop):**

This loop runs continuously, monitoring the sensor and controlling the LED.

* `int sensor_value = digitalRead(Sensorpin)`: This line reads the current value from the sensor pin and stores it in the `sensor_value` variable.
* `if(sensor_value == 0)`: This conditional statement checks if a clap is detected. A value of 0 might indicate a sudden change in the sensor reading, potentially caused by a clap.
    * `if(clap == 0)`: This inner condition checks if this is the first clap detected in a sequence.
        * `detection_range_start = detection_range = millis()`: If it's the first clap, the current time (in milliseconds) is stored in both `detection_range_start` and `detection_range` variables. This marks the beginning of the clap detection window.
        * `clap++`: The `clap` counter is incremented to indicate a clap has been detected.
    * `else if(clap > 0 && millis() - detection_range >= 50)`: This condition checks for subsequent claps within a short time window (50 milliseconds in this case).
        * `detection_range = millis()`: The current time is assigned to `detection_range` to update the window for detecting the next clap.
        * `clap++`: The `clap` counter is incremented again.
* `if (millis() - detection_range_start >= 400)`: This condition checks if 400 milliseconds have passed since the first clap was detected.
    * `if (clap == 1)`: This inner condition verifies if only one clap was detected within the 400ms window.
        * `if(!light_status)`: This checks if the LED is currently off (not `light_status`).
            * `light_status = true; digitalWrite(Ledpin, HIGH);`: If the LED is off, it's turned on by setting `light_status` to true and sending a HIGH signal to the LED pin.
        * `else if(light_status)`: This checks if the LED is currently on (`light_status`).
            * `light_status = false; digitalWrite(Ledpin, LOW);`: If the LED is on, it's turned off by setting `light_status` to false and sending a LOW signal to the LED pin.
        * `Serial.println("Clap Detected");`: This line might be for debugging purposes, printing "Clap Detected" to the serial monitor when a single clap is recognized.
    * `clap = 0;`: After processing the single clap, the `clap` counter is reset to zero.

In summary, this code turns the LED on or off depending on whether a single clap is detected within a short time window (400ms). Multiple claps within that window are ignored. You might need to adjust the sensitivity of the sensor and the time windows based on your specific hardware and desired behavior.
