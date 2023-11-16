<!-- docs/troubleshooting/README.md -->

# Troubleshooting

Here is a list of frequently encountered problems with proposed solutions:

- Radio does not transmit at all.<br>
Does the green `PTT` LED light on when PTT button on the microphone-speaker is pressed?<br>
If not - your microphone-speaker might be faulty or there is some continuity problem along the cable.<br>
If the LED lits up, please check if the PTT signal reaches the radio (connection problem).<br>
Is the radio configured properly?

- Radio transmits carrier only.<br>
Probably the connection between Module17 and the radio is unreliable or the TX potentiometer is set incorrectly (too low baseband level).<br>
Double check continuity over all wires and make sure that the baseband is being output from the Module. Using an oscilloscope may help a lot, but using it is not necessary.

- Radio transmits something, but I can not decode it using MD-3x0/SDR++/anything.<br>
Make sure the deviation is set correctly. The best way to check that is to use [SDR++](https://github.com/AlexandreRouma/SDRPlusPlus/releases/tag/nightly). Use the M17 decoder to tune to your signal.<br>
Take care that your RF signal reaching the SDR is not too high - you want to see a *single* peak in the spectrum, about 20 to 50 dB above noise floor, not more than that. Check if the symbol plot matches 4 reference lines - **image needed**.<br>
If the symbol plot matches with reference lines, but there is no decode, try inverting TX phase in your Module.<br>
Go to `Menu -> Settings -> Module 17 -> TX Phase`, toggle it.<br>
