# G-AthArt-MicrobitGestureTrainer
A program that analyzes different gestures with Microbit, and displays in wekinator how closely you reproduced the gesture (green output bars).

## Names
Stacia Near and Markus Hudobnik

## What your goal is
We were thinking about how you could use Microbit, and tape it to the end of a bat or tennis racket to collect accelerometer data. Using wekinator's dynamic time warping, you can measure how close the user got to the correct form with the stroke of a bat or tennis racket.
This would greatly help to improve people's form using a digital training system.
Becase I don't have the right sports equipment on hand, and I don't have a large enough work area to practice baseball swings, I used a Harry Potter themed approach to increase the entertainment factor, and the ease of training. More information is below.

## How you approached the problem
The wand motions in Harry Potter require small, precise actions, which was feasible with a USB cable connected to my microbit. I rolled up two pieces of paper to create a fake "wand" and taped the mirobit inside it, at the tip. I routed the USB cable through, and it came out at the back end of the tube with room to spare to plug into the laptop.

### Sensors and Software
Microbit - For gesture capture

Computer microphone - For speech capture

BBC Microbit Hex File - Found on Wekinator site

BBC Microbit Processing File - On Wekinator site, for feeding info from Microbit through OSC

MFCC Speech Inputs - Sends 13 OSC messages relating to voice quality at continuous time stamps

Wekinator - 2 Instances, one to process sound and one to process gestures on the Microbit

### Feature extractor approach
We extracted MFCC values from a voice feed. MFCC's, as mentioned in the Kadenze videos, are a good way to capture the quality of speech, and give a unique set of values to each uttered phrase. It was not necessary to extract other values, such as pitch or centroid, as they would not give much more information about which word is being uttered, and would clutter the model with excess inputs.
For the gestural portion, we simple grabbed the 3 raw accelerometer values from the Microbit processing file, while the Microbit was plugged into the computer. Wekinator was able to process these values quite efficiently with dynamic time warping.

### ML model structure
At the beginning of each gesture, I held down the plus key on the Wekinator interface, only releasing it when the gesture was over. To increase fidelity of matching, I included a category for noise, which would be trained on the microphone and microbit sitting stationary with no input. Without this noise category, which turns green when nothing is happening, Wekinator may get false positives more often.
The categories were as followed:

#### Voice

Category 1: Wingardium Leviosa. Trained on me saying "Leviosa," because passing in too long a string of words reduced fidelity.

Category 2: Stupefy. Trained on me saying "Stupefy."

Category 3: Noise. Trained on ambient microphone input.

#### Gesture

Category 1: Swish and Flick. Trained on me making an arc that forms a shallow "U" shape from left to right, then flicking downwards. This category corresponds to the "Wingardium Leviosa" voice input.

Category 2: Small circle. Trained on me making a small circle with the tip of the paper "wand."

Category 3: Noise. Trained on the microbit lying on the table, and on holding the wand in your hand as steadily as possible.

I had to retrain the model a couple of times, because it had difficulty with some of the gestures if either too few or too many samples were given. Other than that, we simply used the defaults for DTM in Wekinator, and it worked with proficient success rates.

## Ideas about how other might improve upon your work


## Demo video
