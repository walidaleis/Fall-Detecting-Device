# Fall-Detector-Term-Project
Authors: Walid Al-Eisawi, Firas Darwish, Hamdan Zoghbor

Description: 

This source code has been designed for the M5Stack Core2 using Arduino in C++, to develop a fall-detecting device known as the "Guardian Angel". 
The device, which is equipped with an accelerometer and built-in WiFi module, makes use of these to detect when the user has fallen.
By implementing a variety of functions, the source code shown here prints out a title screen on the display, followed by multiple functions. 
The source code utilizes a library sourced through an online site for voice recognition and classification in order to detect verbal user response post-fall.
Other than verbal response, the code also provides the user with the option to manually input their response through built-in buttons on the display.
If the device detects a fall and the user indicates they are not okay, a function is called to cue a WhatsApp message to be sent through Wi-Fi to an emergency contact.
The user's emergency contact and Wi-Fi information must be hardcoded due to challenges with written user input on the M5Stack Core 2 Display.

How to Install and Run the Project

Make sure to download the library included in this GitHub page initially as well as the M5Core2 Library from Arduino.
These libraries are required for the code to operate. 
The code can then be compiled on Arduino.

How to Use the Project

An M5Stack Core 2 is needed for the project, as the syntax is specific to this device.
The user will need to hardcode their WiFi information and emergency contact number, as well as the API key that will be sent to their emergency contact as they set this up.
Simply replace current emergency contact information with your respective emergency contact info.
After this, the code can be compiled onto the device through Arduino and the device can be used as a fall-detecting device, perhaps with a shoulder strap to enhance accuracy.


Challenges and errors faced with the project:

One of the most significant errors we faced was attempting to reconcile the example codes we had employing the M5’s microphone to store audio inputs (and the functions associated with doing so) with the integrated functions associated with the EdgeImpulse voice recognition library.  
A critical part of our debugging process consisted of referring to various forums (whether the M5’s or EdgeImpulse’s respective library). 
Here, we had to look through a forum discussion between various users on how to run interference on the M5Core2 for audio classification. 
One of the users, Arjan, noted that “The EI audio examples uses a callback pdm_data_ready_inference_callback() that is called periodically when an audio buffer is full. 
The M5Core2 example calls the audio driver directly and waits for the buffer to be filled.”
This indicated to us that we simply had to refer to other examples provided with M5Core2 to see in what sequence the functions are called in order to fill the buffer correctly. 
Our debugging process went well because we realized there was a function in the sample code provided to us:

microphone_inference_record();

that did not correctly employ the M5’s microphone. 
We came to this conclusion because, once the code encountered this function, the whole program would loop–starting from the setup function. 
From working with arduino and the M5, we knew that, once the set-up function is employed and the program enters void loop(), the program–if functioning correctly–ought to remain in the loop function unless otherwise exited/sent to another function.
Because the M5’s screen would turn black and the program would loop all the way back to the beginning of the set-up function, we knew the above function microphone_inference_record(); was not being interpreted correctly by our program.

By looking to the example code associated with the Record example on Arduino, we were able to find the following function:

InitI2SSpeakOrMic(MODE_MIC)

Once implemented within our code in place of the above function, the program was now able to run regularly and accurately detect yes/no from the user.


Another frequent error we faced was attempting to connect the device to a reliable internet connection with which we can ping the user’s emergency contact’s WhatsApp to inform them of the user’s potential emergency. 
This initially arose because of the security tied to NYU’s internet connection and the multi-factored authentication necessary to access it. 
In light of that, we considered switching the network we’re using to a hotspot of some sort so we can evade the supplementary security checks associated with the NYU network. 
After attempting to use the hotspot on one of our iPhones and speaking to a few TAs about why this may be happening, we realized the connection was being blocked internally–likely due to Apple’s security exacerbating the process with which the M5 was trying to connect to the hotspot. 
Ultimately, this lead us to the Engineering Design Studio where we can use the Wi-Fi available there to test the emergency contact system’s functionality.


A common procedure when debugging our code was printing values (such as counts or booleans) in order to see if functions were being implemented and employed correctly by the program. 
In one instance, when we were coding the concussion/cognitive test, printing the count–which indicated the user’s score–helped show us that the device was never reassigning 0 to the score achieved. 
This meant the program would run and the user would always score higher than the baseline (because the score they would achieve on each individual test would be added to a preexisting score–rather than 0–so would always exceed the maximum score achievable on the baseline [20]). 
Flags also allowed us to easily navigate which functions were to be used by the program and which were to be neglected (depending on what situation the user is in/the responses and inputs they’ve provided the program).


Another acute error we faced concerned our decision-making as a group and the sacrifices we often had to make when evaluating different implementable. 
Two critical examples will be evaluated in reference to this:

1. Fall-detection mechanism and what sensors’ data to use. 
Initially, we considered using the M5’s gyroscope unit in order to determine if the use rhsa suffered a fall or not. 
However, we had to logistically and theoretically break down what that would entail. 
The gyroscope unit would be used to detect if there is an added acceleration from the user–allowing us to determine if the user is moving or not. 
Initially, we thought of implementing this to complement the way we read accelerometer data, such that we can determine if the user is immobile following a potential fall. 
However, we realized that this may lead to falls going undetected if the user tries to struggle and move following a fall. 
The gyroscope unit will determine some movement, which it may incorrectly interpret as indicating the user is alright. 
This may also occur if a user takes a tumble, where their fall is prolonged and may not end statically. 
After testing out the fall detection system using solely the accelerometer unit and weighing it up against this theoretical argument against employing the gyroscope, we realized the current fall detection mechanism works near flawlessly to detect falls and neglect non-falls from an elderly user.

2. The second error is one that still afflicts our code. 
It is concerned with how Edge Impulse classifies the audio input. 
Whether or not the user is able to respond within the time limit given for them to provide their audio, the classifier will try and determine if the user indicated yes/no. 
This may lead to situations where the user does not respond at all yet the classifier picks up noise that it assumes is responding with “yes” to “are you okay?”. 
This leads to a persistent problem where the user may be incapable of receiving medical help or attention once they’ve taken a fall.


What we would have done differently:

Alter concussion test to accommodate for color-blind users

One element that can considerably enhance the device is the inclusion of additional baseline examinations that can aid every individual uniquely. For instance, we need to consider colorblind individuals, which influences roughly 300 million individuals. We would incorporate more tests like "reaction speed" or "recollection" reevaluations in the future. As indicated in the figure below, there are three types of questions/tests that are frequently in use in cognitive/concussion tests: symbol matching, design memory, and color match. By providing sample concussion tests that fall under the category of symbol matching and design memory, we may be able to create a more holistic device that takes into account the visual impairments suffered by users. At the same time, some of these tests could be expanded to the auditory field for blind users who may not be able to navigate the visual interface as easily–or at all. The only concern is the physical impediments of the M5 Stack Core 2. The device has an undersized  300-pixel screen and can only display primary colors within its LED lights. However, acquiring or cultivating a more evolved technological gadget would include the aforementioned improvements.


Incorporate GPS functionality to alert emergency contact of the user’s location as well after taking a fall

As previously forecasted in the above paragraph, the inclusion of some GPS trackers can significantly improve the functional application of the "Guardian Angel." We would implement these GPS trackers over IP addresses as they are communed to a series of satellites to define a precise location. We would employ the M5stack ATOM GPS Kit M8030-KT which uses an approach anointed ‘trilateration’ that operates the assignment of satellites from the Global Navigation Satellite System Network and computes their exact distance to define longitude, latitude, elevation, and time.

Add touch-screen buttons for more user-friendly interface

Moreover, we would need to add the functionality of a touch screen, which might yield it to be more user-friendly for some. This is necessary to consider since the target users are less technologically literate and might require some nominal practices to get accustomed to the new commands. 
This watch emulates a fall detecting watch well but is nowhere near a usable, marketable device that can be launched to the general public. In its current state, one needs to Connect mini-keyboard for M5Stack to permit the user to input their mandated contact information, API key, WiFi SSID, and password. And also better acclimate the code to entitle Wi-Fi information to be modified after set-up, rather than having the user reset the device when switching networks. This will overall make the device more dynamic and less static in nature.

Multi-Language Interface: This device’s universality is marred by the fact its voice detection system can only recognize responses provided in the English language. Osteoporosis does not discriminate and, thus, we would hope to create several embedded machine learning algorithms that can take into account the multiplicity of languages that exist in today’s world.


If you would like to create your own voice recognition software, you may do so using the following:
https://www.digikey.com/en/maker/projects/how-to-use-embedded-machine-learning-to-do-speech-recognition-on-arduino/1d5dd38c05d9494180d5e5b7b657804d

How to create your own personzlied message to be sent to WhatsApp:
https://www.callmebot.com/blog/free-api-whatsapp-messages/
