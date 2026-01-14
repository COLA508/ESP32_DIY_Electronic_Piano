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
   :width: 1000
   :align: center

----

3. Rgb_Color_Lamp
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

 #define BUTTON_PIN 5
 #define LED_PIN    15
 #define LED_COUNT  8

 Adafruit_NeoPixel strip(LED_COUNT, LED_PIN, NEO_GRB + NEO_KHZ800);

 // Simple variables
 int mode = 0;           // 0=Off, 1=Red, 2=Green, 3=Blue, 4=Rainbow, 5=Wave
 int brightness = 100;
 bool lastButton = HIGH;
 unsigned long pressTime = 0;
 int animationStep = 0;

 void setup() {
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  strip.begin();
  strip.setBrightness(brightness);
  Serial.begin(115200);
  Serial.println("8-LED Interactive Light Ready");
  updateAllLEDs();
 }

 void loop() {
  bool button = digitalRead(BUTTON_PIN);
  
  // Button pressed
  if (button == LOW && lastButton == HIGH) {
    pressTime = millis();
  }
  
  // Button released
  if (button == HIGH && lastButton == LOW) {
    unsigned long holdTime = millis() - pressTime;
    
    if (holdTime < 500) {
      // Short press: change mode
      mode = (mode + 1) % 6;
      if (mode == 0) mode = 1;  // Skip off mode when cycling
      Serial.print("Mode: ");
      Serial.println(mode);
      updateAllLEDs();
    } else {
      // Long press: change brightness
      brightness += 80;
      if (brightness > 255) brightness = 30;
      strip.setBrightness(brightness);
      Serial.print("Brightness: ");
      Serial.println(brightness);
      updateAllLEDs();
    }
  }
  
  lastButton = button;
  
  // Run animations for mode 4 and 5
  if (mode == 4 || mode == 5) {
    runSimpleAnimation();
  }
  
  delay(10);
 }

 void updateAllLEDs() {
  strip.clear();
  
  if (mode == 0) {
    // All off
    for (int i = 0; i < LED_COUNT; i++) {
      strip.setPixelColor(i, 0, 0, 0);
    }
  }
  else if (mode == 1) {
    // All red
    for (int i = 0; i < LED_COUNT; i++) {
      strip.setPixelColor(i, 255, 0, 0);
    }
  }
  else if (mode == 2) {
    // All green
    for (int i = 0; i < LED_COUNT; i++) {
      strip.setPixelColor(i, 0, 255, 0);
    }
  }
  else if (mode == 3) {
    // All blue
    for (int i = 0; i < LED_COUNT; i++) {
      strip.setPixelColor(i, 0, 0, 255);
    }
  }
  else if (mode == 4) {
    // Rainbow (will be animated)
    return;
  }
  else if (mode == 5) {
    // Chase effect (will be animated)
    return;
  }
  
  strip.show();
 }

 void runSimpleAnimation() {
  strip.clear();
  animationStep++;
  
  if (mode == 4) {
    // Rainbow animation
    for (int i = 0; i < LED_COUNT; i++) {
      int hue = (animationStep * 10 + i * 30) % 360;
      hue = hue * 65536L / 360;
      strip.setPixelColor(i, strip.gamma32(strip.ColorHSV(hue, 255, brightness)));
    }
  }
  else if (mode == 5) {
    // Chase animation
    for (int i = 0; i < LED_COUNT; i++) {
      if ((i + animationStep) % 3 == 0) {
        strip.setPixelColor(i, 255, 0, 0);
      } else if ((i + animationStep) % 3 == 1) {
        strip.setPixelColor(i, 0, 255, 0);
      } else {
        strip.setPixelColor(i, 0, 0, 255);
      }
    }
  }
  
  strip.show();
  delay(100);
 }
----

Achieved Effect
~~~~~~~~~~~~~~~~

 - Press the button, and the RGB lights will switch between different lighting effects.

----

3. Rgb_Color_Lamp
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

 #define BUTTON_PIN 5
 #define LED_PIN    15
 #define LED_COUNT  8

 Adafruit_NeoPixel strip(LED_COUNT, LED_PIN, NEO_GRB + NEO_KHZ800);

 // Simple variables
 int mode = 0;           // 0=Off, 1=Red, 2=Green, 3=Blue, 4=Rainbow, 5=Wave
 int brightness = 100;
 bool lastButton = HIGH;
 unsigned long pressTime = 0;
 int animationStep = 0;

 void setup() {
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  strip.begin();
  strip.setBrightness(brightness);
  Serial.begin(115200);
  Serial.println("8-LED Interactive Light Ready");
  updateAllLEDs();
 }

 void loop() {
  bool button = digitalRead(BUTTON_PIN);
  
  // Button pressed
  if (button == LOW && lastButton == HIGH) {
    pressTime = millis();
  }
  
  // Button released
  if (button == HIGH && lastButton == LOW) {
    unsigned long holdTime = millis() - pressTime;
    
    if (holdTime < 500) {
      // Short press: change mode
      mode = (mode + 1) % 6;
      if (mode == 0) mode = 1;  // Skip off mode when cycling
      Serial.print("Mode: ");
      Serial.println(mode);
      updateAllLEDs();
    } else {
      // Long press: change brightness
      brightness += 80;
      if (brightness > 255) brightness = 30;
      strip.setBrightness(brightness);
      Serial.print("Brightness: ");
      Serial.println(brightness);
      updateAllLEDs();
    }
  }
  
  lastButton = button;
  
  // Run animations for mode 4 and 5
  if (mode == 4 || mode == 5) {
    runSimpleAnimation();
  }
  
  delay(10);
 }

 void updateAllLEDs() {
  strip.clear();
  
  if (mode == 0) {
    // All off
    for (int i = 0; i < LED_COUNT; i++) {
      strip.setPixelColor(i, 0, 0, 0);
    }
  }
  else if (mode == 1) {
    // All red
    for (int i = 0; i < LED_COUNT; i++) {
      strip.setPixelColor(i, 255, 0, 0);
    }
  }
  else if (mode == 2) {
    // All green
    for (int i = 0; i < LED_COUNT; i++) {
      strip.setPixelColor(i, 0, 255, 0);
    }
  }
  else if (mode == 3) {
    // All blue
    for (int i = 0; i < LED_COUNT; i++) {
      strip.setPixelColor(i, 0, 0, 255);
    }
  }
  else if (mode == 4) {
    // Rainbow (will be animated)
    return;
  }
  else if (mode == 5) {
    // Chase effect (will be animated)
    return;
  }
  
  strip.show();
 }

 void runSimpleAnimation() {
  strip.clear();
  animationStep++;
  
  if (mode == 4) {
    // Rainbow animation
    for (int i = 0; i < LED_COUNT; i++) {
      int hue = (animationStep * 10 + i * 30) % 360;
      hue = hue * 65536L / 360;
      strip.setPixelColor(i, strip.gamma32(strip.ColorHSV(hue, 255, brightness)));
    }
  }
  else if (mode == 5) {
    // Chase animation
    for (int i = 0; i < LED_COUNT; i++) {
      if ((i + animationStep) % 3 == 0) {
        strip.setPixelColor(i, 255, 0, 0);
      } else if ((i + animationStep) % 3 == 1) {
        strip.setPixelColor(i, 0, 255, 0);
      } else {
        strip.setPixelColor(i, 0, 0, 255);
      }
    }
  }
  
  strip.show();
  delay(100);
 }
----

Achieved Effect
~~~~~~~~~~~~~~~~

 - Press the button, and the RGB lights will switch between different lighting effects.

----

4. Single ToneGenerator
-----------------------

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
 #define BUTTON_PIN 5      // Button to change notes
 #define SPEAKER_PIN 19    // Speaker positive pin (negative to GND)
 #define LED_PIN    15     // LED for visual feedback

 #define LED_COUNT 1

 Adafruit_NeoPixel strip(LED_COUNT, LED_PIN, NEO_GRB + NEO_KHZ800);

 // Musical notes frequencies (Hz) - C Major Scale
 const int notes[] = {
  262,  // C4 - Do
  294,  // D4 - Re
  330,  // E4 - Mi
  349,  // F4 - Fa
  392,  // G4 - Sol
  440,  // A4 - La
  494   // B4 - Si
 };

 const char* noteNames[] = {
  "Do (C4)",
  "Re (D4)", 
  "Mi (E4)",
  "Fa (F4)",
  "Sol (G4)",
  "La (A4)",
  "Si (B4)"
 };

 // Colors for each note
 uint32_t noteColors[] = {
  strip.Color(255, 0, 0),     // Do - Red
  strip.Color(255, 128, 0),   // Re - Orange
  strip.Color(255, 255, 0),   // Mi - Yellow
  strip.Color(0, 255, 0),     // Fa - Green
  strip.Color(0, 128, 255),   // Sol - Light Blue
  strip.Color(0, 0, 255),     // La - Blue
  strip.Color(128, 0, 255)    // Si - Purple
 };

 // Variables
 int currentNote = 0;          // Current note index (0-6)
 bool lastButtonState = HIGH;
 unsigned long lastPressTime = 0;

 // Speaker setup
 #define SPEAKER_CHANNEL 0

 void setup() {
  Serial.begin(115200);
  
  // Setup button
  pinMode(BUTTON_PIN, INPUT_PULLUP);
  
  // Setup speaker with LEDC
  ledcSetup(SPEAKER_CHANNEL, 2000, 8);
  ledcAttachPin(SPEAKER_PIN, SPEAKER_CHANNEL);
  
  // Setup LED
  strip.begin();
  strip.show();
  strip.setBrightness(100);
  
  // Show current note
  updateDisplay();
  
  Serial.println("Tone Generator with Speaker");
  Serial.println("Short press: Change to next note");
  Serial.println("----------------------");
 }

 void loop() {
  bool buttonState = digitalRead(BUTTON_PIN);
  
  // Button pressed (detect falling edge)
  if (buttonState == LOW && lastButtonState == HIGH) {
    // Simple debounce
    delay(50);
    
    if (digitalRead(BUTTON_PIN) == LOW) {
      // Change to next note
      changeNote();
      
      // Play the new note briefly
      playNoteBriefly();
    }
  }
  
  lastButtonState = buttonState;
  delay(10);
 }

 void changeNote() {
  // Move to next note
  currentNote = (currentNote + 1) % 7;
  
  updateDisplay();
  
  // Visual feedback
  for (int i = 0; i < 2; i++) {
    strip.setPixelColor(0, strip.Color(255, 255, 255));
    strip.show();
    delay(50);
    strip.setPixelColor(0, noteColors[currentNote]);
    strip.show();
    delay(50);
  }
 }

 void playNoteBriefly() {
  Serial.print("Playing: ");
  Serial.print(noteNames[currentNote]);
  Serial.print(" (");
  Serial.print(notes[currentNote]);
  Serial.println(" Hz)");
  
  // Play note on speaker
  ledcWriteTone(SPEAKER_CHANNEL, notes[currentNote]);
  
  // Visual feedback - bright LED
  strip.setPixelColor(0, noteColors[currentNote]);
  strip.setBrightness(200);
  strip.show();
  
  // Play for 300ms
  delay(300);
  
  // Stop speaker
  ledcWriteTone(SPEAKER_CHANNEL, 0);
  
  // Dim LED back
  strip.setBrightness(100);
  strip.show();
 }

 void updateDisplay() {
  Serial.println("----------------------");
  Serial.print("Current note: ");
  Serial.println(noteNames[currentNote]);
  Serial.print("Frequency: ");
  Serial.print(notes[currentNote]);
  Serial.println(" Hz");
  Serial.println("----------------------");
  
  // Update LED color
  strip.setPixelColor(0, noteColors[currentNote]);
  strip.setBrightness(100);
  strip.show();
 }
----

Achieved Effect
~~~~~~~~~~~~~~~~

 - Press the button, and the speaker will play the Do-Si syllables in sequence and light up different colored lights.

----