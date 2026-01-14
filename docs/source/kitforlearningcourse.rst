kit For Learning Course
======================

**In this section, we will use the components in this kit to expand our learning, gradually mastering the principles and functional characteristics of each component in order of depth, and completing the corresponding program writing.**

----

1. Breathing-Lamp
-----------------

Wiring diagram
~~~~~~~~~~~~~~

.. image:: _static/course/1.course.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

- RGB —— ESP32 IO15

----

Example code
~~~~~~~~~~~~

.. code-block:: cpp

    #include <Adafruit_NeoPixel.h>

    #define LED_PIN    15      // Data pin connected to GPIO15
    #define LED_COUNT  1       // Using 1 LED only

 Adafruit_NeoPixel strip(LED_COUNT, LED_PIN, NEO_GRB + NEO_KHZ800);

 // Breathing effect variables
 int brightness = 0;    // Current brightness (0-255)
 int fadeAmount = 1;    // How much to change brightness each time
 int delayTime = 15;    // Delay between updates (ms)

 void setup() {
  strip.begin();
  strip.show();  // Initialize all pixels to 'off'
  strip.setBrightness(255);  // Set maximum brightness for strip
  
  Serial.begin(115200);
  Serial.println("RGB Breathing LED Test");
  Serial.println("LED should slowly breathe (fade in and out)");
 }

 void loop() {
  // Set LED color with current brightness (red color)
  strip.setPixelColor(0, brightness, 0, 0);
  strip.show();
  
  // Change brightness for next time
  brightness = brightness + fadeAmount;
  
  // Reverse direction at the ends of fade
  if (brightness <= 0 || brightness >= 255) {
    fadeAmount = -fadeAmount;
  }
  
  delay(delayTime);
 }

----

Achieved Effect
~~~~~~~~~~~~~~~~

 - The first LED in the RGB light is red, and it goes from bright to dim and then bright again, just like breathing.

.. image:: _static/course/2.course.png
   :width: 800
   :align: center

----

2. Button_Count
-----------------

Wiring diagram
~~~~~~~~~~~~~~

.. image:: _static/course/1.course.png
   :width: 800
   :align: center

.. raw:: html

   <div style="margin-top: 30px;"></div>

- RGB —— ESP32 IO15
- Button —— ESP32 IO5

----

Example code
~~~~~~~~~~~~

.. code-block:: cpp

 #include <Adafruit_NeoPixel.h>

 // Pin definitions
 #define BUTTON_PIN 5      // Button connected to GPIO13
 #define LED_PIN    15      // LED strip data pin connected to GPIO15
 #define LED_COUNT  1       // Using 1 LED

 Adafruit_NeoPixel strip(LED_COUNT, LED_PIN, NEO_GRB + NEO_KHZ800);

 // Variables
 int pressCount = 0;       // Button press counter
 bool lastButton = false;  // Previous button state

 void setup() {
  // Configure button pin with internal pull-up
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  
  // Initialize LED strip
  strip.begin();
  strip.show();  // Turn off all LEDs
  strip.setBrightness(50);
  
  // Initialize serial communication
  Serial.begin(115200);
  Serial.println("Button Counter Started");
  Serial.println("Press button: LED turns RED, counter increases");
  Serial.println("Release button: LED turns OFF");
 }

 void loop() {
  // Read button state (LOW = pressed when using INPUT_PULLUP)
  bool buttonPressed = digitalRead(BUTTON_PIN) == LOW;
  
  // Control RGB LED
  if (buttonPressed) {
    strip.setPixelColor(0, strip.Color(255, 0, 0));  // Red when pressed
    strip.show();
  } else {
    strip.setPixelColor(0, strip.Color(0, 0, 0));    // Off when released
    strip.show();
  }
  
  // Detect button press (rising edge detection)
  if (buttonPressed && !lastButton) {
    pressCount++;  // Increment counter
    
    Serial.print("Button pressed! Total: ");
    Serial.println(pressCount);
  }
  
  // Save current state for next loop
  lastButton = buttonPressed;
  
  // Small delay for stability
  delay(50);
 }
----

Achieved Effect
~~~~~~~~~~~~~~~~

 - Press the button and the first LED of the RGB light will light up.
 - Open the serial monitor, which will record the number of times the button was pressed.

.. image:: _static/course/3.course.png
   :width: 800
   :align: center

----