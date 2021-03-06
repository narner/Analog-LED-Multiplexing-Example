# Analog-LED-Multiplexing-Example
Illustrating the use of a multiplexer to control multiple analog LED strips

## Background

<a href="https://github.com/commodorewafflejack">Ray Brooks </a> and I were recently working on prototyping a design project that invovled some analog LED strips.

We needed to be able to alter the color of <a href="https://www.adafruit.com/products/285">strips from Adafruit </a>  between red and blue. Since we wanted to control 10 
LED strips, and there wasn't enough inputs in the Arduino for all of these strips, we decided to try
using two multiplexers so that we would be able to control all of the 10 required LED strips.

We used a <a href="https://www.sparkfun.com/products/9056">multiplexer from Sparkfun </a>, that 
ended up working quite well for our purpsoes. This <a href="http://bildr.org/2011/02/cd74hc4067-arduino/">guide from Bildr </a> was extremely helpful for getting the multiplexer setup. 

## Circuit

### Circuit Diagram

![Alt Text](https://github.com/narner/Analog-LED-Multiplexing-Example/raw/master/ReadmeFiles/AnalogLEDStripCircuitDiagram.png)


### Circuit Photos   

![Alt Text](https://github.com/narner/Analog-LED-Multiplexing-Example/raw/master/ReadmeFiles/AnalogPhoto1.png)

![Alt Text](https://github.com/narner/Analog-LED-Multiplexing-Example/raw/master/ReadmeFiles/AnalogPhoto2.png)

![Alt Text](https://github.com/narner/Analog-LED-Multiplexing-Example/raw/master/ReadmeFiles/AnalogPhoto3.png)

![Alt Text](https://github.com/narner/Analog-LED-Multiplexing-Example/raw/master/ReadmeFiles/AnalogPhoto4.png)



## Arduino Sketch

The heart of the Arduino sketch is the method below:

```
int writeMux(int channel){
  //array corresponding to our muxs' control pins
  int controlPin[] = {s0, s1, s2, s3, s4, s5, s6, s7};

  int muxChannel[20][8]={
    //Mux 1 
    {0,0,0,0,0,0,0,0}, //channel 0
    {1,0,0,0,0,0,0,0}, //channel 1
    {0,1,0,0,0,0,0,0}, //channel 2
    {1,1,0,0,0,0,0,0}, //channel 3
    {0,0,1,0,0,0,0,0}, //channel 4
    {1,0,1,0,0,0,0,0}, //channel 5
    {0,1,1,0,0,0,0,0}, //channel 6
    {1,1,1,0,0,0,0,0}, //channel 7
    {0,0,0,1,0,0,0,0}, //channel 8
    {1,0,0,1,0,0,0,0}, //channel 9

    //Max 2
    {1,1,1,1,0,0,0,0}, //channel 0
    {1,1,1,1,1,0,0,0}, //channel 1
    {1,1,1,1,0,1,0,0}, //channel 2
    {1,1,1,1,1,1,0,0}, //channel 3
    {1,1,1,1,0,0,1,0}, //channel 4
    {1,1,1,1,1,0,1,0}, //channel 5
    {1,1,1,1,0,1,1,0}, //channel 6
    {1,1,1,1,1,1,1,0}, //channel 7 
    {1,1,1,1,0,0,0,1}, //channel 8
    {1,1,1,1,1,0,0,1}, //channel 9
  }; 


  //loop through the 8 sig pins
  for(int i = 0; i < 8; i ++){
    digitalWrite(controlPin[i], muxChannel[channel][i]);
    Serial.println("CHANNEL IS ");
    Serial.println(muxChannel[channel][i]);

    Serial.println("Control pin IS ");
    Serial.println(controlPin[i]);
  }

  //write the value at the SIG pin
  analogWrite(SIG_pin, testValue);  
  Serial.println("Test value IS ");
  Serial.println(testValue);

  //return the value
  return 0;
}
```

What this does is create an array that holds each of our multiplexer's control pins, and an array
which holds the output pins of the muxes'. The for loop determines which channel of the mux we want
to write to, and the `analogWrite` call after it writes our "on" value (a voltage value of 255) to 
the appropriate `SIG_pin`.


Here's a what this looks like in action:

![Alt Text](https://github.com/narner/Analog-LED-Multiplexing-Example/raw/master/ReadmeFiles/AnalogLEDCircuit.gif)


## Parts list:

* 1 x <a href="https://www.arduino.cc/en/Main/ArduinoBoardUno">Arduino Uno </a>
* 1 x <a href="https://www.adafruit.com/products/285">RGB LED weatherproof flexi-strip </a>
* 2 x <a href="https://www.sparkfun.com/products/9056">Multiplexer from Sparkfun </a>
* 1 x <a href="https://www.adafruit.com/products/352">12V 5A Switching Supply </a>
* 20 x <a href="https://www.sparkfun.com/products/10213">N-Channel MOSFET 60V 30A </a>
* 20 x <a href="https://www.sparkfun.com/products/11508">10K Ohm Resitors </a>

## Admendum

We realized that for the particular project we're working on prototyping, we need more fine-grained
control over the individual LEDs, and will be moving to use digial LED strips rather than analog.
We hope this is helpful for anyone working with analog LED strips and Arduinos!
