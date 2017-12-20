## Making adjustments

Let’s make a couple of changes to the way this works. We’ll add one pot (potentiometer) to control frequency, and a second pot to control volume. Connect two pots to your Arduino (see below). Each pot will have one side connected to 5V, the other side connected to GND and the middle (wiper) to an analogue input. We’ll use analogue inputs A0 and A1.


Add the following lines of code before void setup(): 
```
int pot0, pot1; 
int volume,frequency; 
```

These will be the variables where we’ll store the pot values, and the frequency and volume values they will control. 
