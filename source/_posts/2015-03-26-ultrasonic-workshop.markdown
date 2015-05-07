---
layout: post
title: "Ultrasonic Workshop"
date: 2015-03-26 20:32:37 +1000
comments: true
categories: Sessions
author: Chris Roberts, Ashley Gillman
---
### What is an Ultrasonic Sensor?
Ultrasonic sensors, like the HC-SR04, are hobbyist level distance sensors that use an ultrasonic burst of sound, or ping, to measure the distance to objects around it.
They are essentially like miniature sonars, as seen on your favourite submarine.
(Hunt For Red October, anyone?).

{% img /images/ultrasonic/2wire_ultrasonic.png %}
<!--more-->

### What can it be used for?
As a distance sensor, they’re best used when trying to measure the distance to something. That something could be, for example, a wall or it could be another robot inside a sumo dohyō. Once the location of the object is known, it’s much easier to target or avoid it.

### How does it work?
The HC-SR04 requires only two data pins, plus ground and +5V connections. The two data pins are used for a Trigger (which sends the ping out), and the Echo (which measures the return of the ping).

{% img floatl /images/ultrasonic/UltrasonicSensor-Operation.png 300 %}
The ping is sent via a tiny speaker on the front, at a frequency of 40kHz, well above our range of hearing. When it hits an object, the sound bounces back. A second speaker cone acts as a microphone to measure the returning signal.

Based on how long it takes for the sound wave to return, and equipped with a little bit of physics, we are able to calculate the distance the sound travelled in order to return to us, and hence the distance of the object.

### What are the limitations?
<p class="imgspan" style="height:250px">
{% img floatl /images/ultrasonic/hc-sr04_performance.jpg "50%" %}
{% img floatr /images/ultrasonic/SensorSignalDeflection.png 250 %}
</p>

Because the system relies on reflected sound, harder objects will return more of the sound burst, whilst softer objects will absorb more of it. (Can this be used to your advantage?).

The system relies on at least part of the soundwave being reflected back, it is only effective for a defined window (about ±15°), with best results obtained around the 0° mark. Similarly, at least part of the object must be facing towards the object in order to provide a surface capable of reflecting the ping wave.

### How do I use it?
[{% img floatr /images/ultrasonic/2wire_ultrasonic.png 300 %}](/images/ultrasonic/2wire_ultrasonic.png "Full Size")

1. Connect the Trigger and Echo pins, as per the schematic.
1. Connect the ground pin
1. Connect the +5V pin
1. Connect and program your Arduino
1. Open the Serial Monitor (terminal / console viewer)
1. Measure distances. Win!

{% codeblock lang:c++ "Ultrasonic Test Code" %}
/*
 Robo Club Workshop - Ultrasonics

 This simple code will allow you to measure the distance
 to an object using your HC-SR04 Ultrasonic distance sensor
 and your Arduino.
*/

#define trigPin 12 // blue
#define echoPin 11 // green

void setup() {
  Serial.begin (9600);              // allows terminal access
  pinMode(trigPin, OUTPUT);         // set up pin to trigger
  pinMode(echoPin, INPUT);          // set up pin to receive echo
}

void loop() {
  int duration, distance;           // set up variables
  digitalWrite(trigPin, HIGH);      // start of trigger sequence
  delayMicroseconds(1000);          // delay 1ms
  digitalWrite(trigPin, LOW);       // complete trigger sequence
  duration = pulseIn(echoPin, HIGH);  // see: http://arduino.cc/en/Reference/pulseIn
  distance = (duration/2) / 29.1;  // calculate distance from time
                                   // Divide the time in half (because
                                   // the pulse travelled to the object
                                   // AND back again.
                                   // Then divide the time by 29.1 microseconds/cm
                                   // (the speed of sound).
  if (distance >= 200 || distance <= 0){  // remove out-of-bounds readings
    Serial.println("Out of range");
  }
  else {                           // print distance if valid
    Serial.print(distance);
    Serial.println(" cm");
  }
  delay(500);                      // wait before repeating process
}
{% endcodeblock %}

### More information:
The datasheet is available in the project folder [here](https://drive.google.com/open?id=0B5CywTjAZK_nQ25HWFlFRDh0VU0&authuser=0).

### Advanced:
One problem with this code is that pulsein() will wait for the response, up to a certain amount of time. But what happens while the code is waiting? Nothing!
Advanced student may like to see the [NewPing library](https://code.google.com/p/arduino-new-ping/), which attempts to solve this issue.
