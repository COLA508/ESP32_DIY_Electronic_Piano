kit For Learning Course
======================

**In this section, we will use the components in this kit to expand our learning, gradually mastering the principles and functional characteristics of each component in order of depth, and completing the corresponding program writing.**

**The sixth code is the complete code for this kit "ESP32 DIY Electronic Piano," which you can click here to view.**

:ref:`ESP32 DIY Electronic Piano`

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

- RGB —— ESP32 IO15
- Button —— ESP32 IO5
- Speaker —— ESP32 IO33
----

Example code
~~~~~~~~~~~~

.. code-block:: cpp

 #include <Adafruit_NeoPixel.h>

 // Pin definitions
 #define BUTTON_PIN 5      // Button to change notes
 #define SPEAKER_PIN 33    // Speaker positive pin (negative to GND)
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

5. Triple Bond Piano
--------------------

Wiring diagram
~~~~~~~~~~~~~~

- RGB —— ESP32 IO15
- Button1 —— ESP32 IO5
- Button2 —— ESP32 IO18
- Button3 —— ESP32 IO5
- Speaker —— ESP32 IO33

----

Example code
~~~~~~~~~~~~

.. code-block:: cpp

 #include <Adafruit_NeoPixel.h>

 #define BTN1 5   // Do
 #define BTN2 18   // Mi  
 #define BTN3 19   // Sol
 #define SPEAKER 33
 #define LED_PIN 15

 Adafruit_NeoPixel leds(3, LED_PIN, NEO_GRB + NEO_KHZ800);

 // Notes for C major scale
 int notes[] = {262, 294, 330, 349, 392, 440, 494};
 // Our buttons: Do(0), Mi(2), Sol(4)
 int buttonNotes[] = {0, 2, 4};  // Indexes in notes array

 // Chord definitions
 #define CHORD_C_MAJOR 0     // Do + Mi + Sol
 #define CHORD_F_MAJOR 1     // Fa + La + Do
 #define CHORD_G_MAJOR 2     // Sol + Si + Re

 int currentChord = -1;      // -1 = no chord
 bool buttons[3] = {false, false, false};
 unsigned long buttonTime[3] = {0, 0, 0};

 void setup() {
  Serial.begin(115200);
  
  pinMode(BTN1, INPUT_PULLUP);
  pinMode(BTN2, INPUT_PULLUP);
  pinMode(BTN3, INPUT_PULLUP);
  
  ledcSetup(0, 2000, 8);
  ledcAttachPin(SPEAKER, 0);
  
  leds.begin();
  leds.setBrightness(100);
  
  Serial.println("Chord Piano - Three Keys");
  Serial.println("Do+Mi = Interval");
  Serial.println("Do+Mi+Sol = C Major Chord");
  Serial.println("Mi+Sol = Another interval");
 }

 void loop() {
  // Read buttons with debounce
  readButton(0, BTN1);
  readButton(1, BTN2);
  readButton(2, BTN3);
  
  // Check what's being pressed
  int pressedCount = 0;
  for (int i = 0; i < 3; i++) {
    if (buttons[i]) pressedCount++;
  }
  
  // Play appropriate sound
  if (pressedCount == 0) {
    ledcWriteTone(0, 0);  // Stop sound
    currentChord = -1;
  }
  else if (pressedCount == 1) {
    // Single note
    for (int i = 0; i < 3; i++) {
      if (buttons[i]) {
        playSingleNote(i);
        break;
      }
    }
    currentChord = -1;
  }
  else {
    // Multiple buttons - check for chords
    checkChord();
  }
  
  // Update LEDs
  updateLEDs();
  
  delay(10);
 }  

 void readButton(int index, int pin) {
  bool state = digitalRead(pin);
  
  if (state == LOW && !buttons[index]) {
    // Button just pressed
    if (millis() - buttonTime[index] > 50) {  // Debounce
      buttons[index] = true;
      buttonTime[index] = millis();
      Serial.print("Button ");
      Serial.print(index + 1);
      Serial.println(" pressed");
    }
  }
  else if (state == HIGH && buttons[index]) {
    // Button just released
    buttons[index] = false;
    Serial.print("Button ");
    Serial.print(index + 1);
    Serial.println(" released");
  }
 }

 void playSingleNote(int buttonIndex) {
  int noteIndex = buttonNotes[buttonIndex];
  ledcWriteTone(0, notes[noteIndex]);
  
  // LED feedback
  for (int i = 0; i < 3; i++) {
    if (i == buttonIndex) {
      setLEDColor(i, true);
    } else {
      setLEDColor(i, false);
    }
  }
 }

 void checkChord() {
  // Check which buttons are pressed
  bool doPressed = buttons[0];  // Button 1
  bool miPressed = buttons[1];  // Button 2
  bool solPressed = buttons[2]; // Button 3
  
  // Check for C Major chord (Do + Mi + Sol)
  if (doPressed && miPressed && solPressed) {
    if (currentChord != CHORD_C_MAJOR) {
      currentChord = CHORD_C_MAJOR;
      Serial.println("C Major Chord!");
      playChord(CHORD_C_MAJOR);
    }
  }
  // Check for Do+Mi interval
  else if (doPressed && miPressed) {
    if (currentChord != 10) {  // Custom code for this interval
      currentChord = 10;
      Serial.println("Do-Mi Interval");
      // Play alternating between Do and Mi
      static bool alt = false;
      if (alt) {
        ledcWriteTone(0, notes[0]);  // Do
      } else {
        ledcWriteTone(0, notes[2]);  // Mi
      }
      alt = !alt;
    }
  }
  // Check for Mi+Sol interval
  else if (miPressed && solPressed) {
    if (currentChord != 11) {
      currentChord = 11;
      Serial.println("Mi-Sol Interval");
      // Play alternating
      static bool alt = false;
      if (alt) {
        ledcWriteTone(0, notes[2]);  // Mi
      } else {
        ledcWriteTone(0, notes[4]);  // Sol
      }
      alt = !alt;
    }
  }
  // Check for Do+Sol interval
  else if (doPressed && solPressed) {
    if (currentChord != 12) {
      currentChord = 12;
      Serial.println("Do-Sol Interval (Perfect 5th)");
      ledcWriteTone(0, notes[0]);  // Play Do (root)
    }
  }
 }

 void playChord(int chordType) {
  switch(chordType) {
    case CHORD_C_MAJOR:
      // Play C Major chord (Do, Mi, Sol)
      // We'll play the root note, but could also cycle through chord notes
      ledcWriteTone(0, notes[0]);  // Do
      
      // Chord animation on LEDs
      static unsigned long lastBlink = 0;
      if (millis() - lastBlink > 200) {
        lastBlink = millis();
        for (int i = 0; i < 3; i++) {
          leds.setPixelColor(i, 
            buttons[i] ? leds.Color(255, 255, 255) : leds.Color(0, 0, 0));
        }
        leds.show();
        delay(50);
        updateLEDs();
      }
      break;
  }
 }

 void setLEDColor(int index, bool pressed) {
  int colors[][3] = {{255,0,0}, {0,255,0}, {0,0,255}};
  
  if (pressed) {
    leds.setPixelColor(index, colors[index][0], colors[index][1], colors[index][2]);
  } else {
    leds.setPixelColor(index, 
      colors[index][0]/5, colors[index][1]/5, colors[index][2]/5);
  }
 }

 void updateLEDs() {
  for (int i = 0; i < 3; i++) {
    setLEDColor(i, buttons[i]);
  }
  leds.show();
 }

----

Achieved Effect
~~~~~~~~~~~~~~~~

 - The three buttons represent Do, Mi, and Sol, allowing you to use your imagination to play some simple music.

----


.. _ESP32 DIY Electronic Piano:

6. ESP32 DIY Electronic Piano
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~