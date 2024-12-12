Kyle Humphries ECE 445 Lab notebook

9-30-24
Our group meet together to get the design document finished. When we divided the project into different subsytems it seems most likely I will be doing the power subsystem and helping out with the pcb creation. I will need to figure out how we will power the device and how much voltage/current.

10-3-24
My inital thoughts are that we use some batteries to power the device. The motors my teammates picked look like they will take 5V and sensors might be ok with 5V. Will need to test both though. Need to make the pack easy to access so we can replace batteries. Based on the picture that my team gave me I drew roughly where i'd want it to be.
https://github.com/milanpatel9/ece-445-notebooks/blob/main/notebooks/kyle/IMG_1097.jpg?raw=true![image](https://github.com/user-attachments/assets/b4147702-41b8-41e4-b969-cfe248fb63a6)

10-4-24
Machine shop agreed with a lot of design ideas except you suggested doing wall power or soemthing like that. The design would still stay similar to the original picture but depending on the weight we might need to move the power source to the side of the machine. This is because it currently might make the machine fall over but I still suggested having it close to the pcb.

10-8-24
Had a meeting with the TA and professor yesderday about our design. I misunderstood the machine shop when he said we could do 12V from the wall outlet. This would require a circuit to go with it to step down multiple times to what we want. Professor recomeneded the power supply from our lab kit has a 12V and a 5V sources. Heat dissapation was also a big issue because stepping down from those voltages might cause problems. I will need to start looking at actual parts to do the stepping down soon.

10-11-24
I found one LDO part that will step down the 12V to 5V and then I found another to step down the 5V to 3.3V. https://www.digikey.com/en/products/detail/shenzhen-slkormicro-semicon-co-ltd/AMS1117-3-3-SOT-223/21853079 AMS1117 does 5v to 3.3v. LD1117S50TR works for the 12V to 5V

10-14-24
It doesn't seem like we will make the first round of pcb orders because we are having a few errors in the PCB editor. Currently have only one storage compartnant worth of design done for the PCB so far but it would be enough to test some of the quesitons. We will look to fix the errors and get something ordered so we can test after next round.

10-18-24
I worked on the PCB sechematic again today adding an LED light and a solderjumper so we can test and do some things manually on the pcb if needed. Read the datasheets for the sensors and some list themselves as 5V and others at 3.3V even though the ranges all say they can do both. Will need to do testing for this. It looks like digikey has both of the LDOs that I was considering.

10-19-24
I got the errors to go away on the PCB editor, had to learn how to use ground vias to hook up all the pin connections since we use the majority of ours on the ESP32. Still just have one of each component since we will just use this one for testing.

10-21-24



