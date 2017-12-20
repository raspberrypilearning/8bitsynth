## How it works

Back to the sine wave generator: we’ll be breaking down the elements of this sketch fairly thoroughly, as knowing the basics of how Mozzi works will enable you to make more exciting changes later. 

First, we’ll take a look at the includes. This is where we add in the required files from the Mozzi library, and you should see three includes: ‘MozziGuts.h’, ‘Oscil.h’, and ‘tables/sin2048_int8.h’. 

MozziGuts.h is the main library required for doing anything with Mozzi. This file will adapt your Arduino for use as a synth, by taking over some timers and setting up some fast sampling methods. 

Oscil.h is simply a template for an oscillator. Any sound requires a repeated change in voltage, or air pressure; an oscillation. This file tells Mozzi how to create an oscillator from a lookup table. 

tables/sin2048_int8.h is the lookup table we’ll be using to make a sine wave. A lookup table is often used where calculating the values of a function (in this case, a sine wave) would take too long. We simply pre-calculate all the values and store them in memory. 

When we need them, we can simply ‘look them up’, hence the name lookup table. We then have a line:

~~~
Oscil <SIN2048_NUM_CELLS, AUDIO_RATE> aSin(SIN2048_DATA);
~~~

This is a little like saying, “Create a sine wave oscillator called aSin, using the table I mentioned before.” We also have the line:

~~~
#define CONTROL_RATE 64 
~~~

Which means we intend to update our controls (our potentiometers and buttons) 64 times per second. Mozzi asks for control rates to be powers of two (e.g. 2, 4, 8, 16, …) 

To continue to our main functions: in setup() you’ll see two commands. The first, startMozzi(CONTROL_ RATE), will start the Mozzi engine, and the second, aSin.setFreq(440), will set the frequency (or pitch) of our oscillator. 440 Hz is middle A (so if you only get this far, at least you can get your band in tune). 

Typically, when writing a Mozzi sketch, you’ll avoid putting anything in loop(), except the function audioHook(). This function will calculate samples (little chunks of audio data) ready to be written to our output. So where do we put our code? You’ll notice, apart from the usual setup() and loop() functions that we have two more: updateControl() and updateAudio(). 

updateControl() is where we put the changes we want to happen at our control rate (64 times per second). This will be things like reading our potentiometer values, button states and other tasks that don’t need to happen too often. In this sketch, nothing like this is required, so the function is empty. 

updateAudio() is the function that audioHook() will run repeatedly – it calculates our audio samples and stores them in a buffer to be sent to pin 9 later. You can see within this sketch the code: 

return aSin.next(); which simply means to send the next sample for this oscillator to the buffer. 