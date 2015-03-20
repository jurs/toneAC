toneAC Arduino Library
Replacement to the standard tone library with many advantages
Nearly twice the volume (because it uses two out of phase pins in push/pull fashion)
Higher quality (less clicking)
Capability of producing higher frequencies (even if running at a lower clock speed)
Nearly 1.5k smaller compiled code
Bug fixes (standard tone library can generate some odd and unpredictable results)
Can set not only the frequency but also the sound volume
Less stress on the speaker so it will last longer and sound better
Disadvantages are that it must use certain pins and it uses two pins instead of one. But, if you're flexible with your pin choices, this is a great upgrade. It also uses timer 1 instead of timer 2, which may free up a conflict you have with the tone library. It exclusively uses port registers for the fastest and smallest code possible.

Connection
Connection is very similar to a piezo or standard speaker. Except, instead of connecting one speaker wire to ground you connect both speaker wires to Arduino pins. The pins you connect to are specific, as toneAC lets the ATmega microcontroller do all the pin timing and switching. This is important due to the high switching speed possible with toneAC and to make sure the pins are alyways perfectly out of phase with each other (push/pull). See the below list for which pins to use for different Arduinos. Just as usual when connecting a speaker, make sure you add an inline 100 ohm resistor between one of the pins and the speaker wire.

Pins 9 & 10 - ATmega328, ATmega128, ATmega640, ATmega8, Uno, Leonardo, etc.
Pins 11 & 12 - ATmega2560/2561, ATmega1280/1281, Mega
Ping 12 & 13 - ATmega1284P, ATmega644
Pins 14 & 15 - Teensy 2.0
Pins 25 & 26 - Teensy++ 2.0

Example sketch
#include <toneAC.h>

void setup() {} // Nothing to setup, just start playing!

void loop() {
  for (unsigned long freq = 150; freq <= 15000; freq += 10) {  
    toneAC(freq); // Play the frequency (150 Hz to 15 kHz in 10 Hz steps).
    delay(1);     // Wait 1 ms so you can hear it.
  }
  toneAC(0); // Turn off toneAC, can also use noToneAC().

  while(1); // Stop (so it doesn't repeat forever driving you crazy--you're welcome).
}

Syntax
toneAC( frequency [, volume [, length [, background ]]] ) - Play a note.
frequency - Play the specified frequency indefinitely, turn off with toneAC().
volume - [optional] Set a volume level. (default: 10, range: 0 to 10 [0 = off])
length - [optional] Set the length to play in milliseconds. (default: 0 [forever], range: 0 to 2^32-1)
background - [optional] Play note in background or pause till finished? (default: false, values: true/false)
toneAC() - Stop output.
noToneAC() - Same as toneAC().

How does it work?
The library is named toneAC because it produces an alternating push/pull between two pins. It's not really AC (alternating current) as in-wall electrical wiring because it's a square wave and never produces a negative voltage. However, the effect of the alternating push/pull creates an effective double voltage differential which produces the higher volume level.

The ATmega's PWM takes care of the alternating push/pull so the accuracy is exact. When you send a tone to a speaker with the standard tone library, the loudest is at 50% duty cycle (only on half the time). Which at 5 volts, is like sending only 2.5v to the speaker. With toneAC, we're sending out of phase signals on two pins. So in effect, the speaker is getting 5 volts instead of 2.5, making it nearly twice as loud.

The sound quality difference has to do with allowing the Arduino's PWM to take care of everything and careful programming. Longer piezo life happens because instead of driving the transducer disc only ever in one direction (deforming the disc and reducing sound and quality), it drives it in both directions keeping the disc uniform.
