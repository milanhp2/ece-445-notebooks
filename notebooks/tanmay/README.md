Tanmay Mittal

9-30-2024: 
Together as a team we worked on the design document. We also divided the work for the project between all the team members.


10-7-2024
We had a review of our design document along with a professor. They gave feedback on the power subsystem and we assigned the power subsystem to Kyle.


10-8-2024
I talked to the machine shop and had them review our initial design for an automatic tea machine. We initially designed to have all the components lie in the back of the dispensing unit but the machine shop advised me to place it on the side so that the device doesn't tip over when opening or while we are working on it. Plus they mentioned that they will take care of the dispensing using a metal rotating disk that will scoop the tea. 


10-15-2024
I submitted a drawing to the machine shop with the top, front, side view of our automatic tea machine. When they looked at my drawing they wanted to tweak a few dimensions according to the metal disks they had for dispensing the storage unit. Since the dimensions for the storage and dispensing unit weren’t that pressing, only the functionality mattered. I also delivered them all the sensors like the motors and the weight sensor. I didn’t have the air and temperature and humidity sensors today so I am planning on giving it to them as soon as I get it. I marked the spot where I would want to add the sensor. 

![Front View](https://github.com/user-attachments/assets/ccf7f9f6-7ff4-40bf-b22c-807982a9cc98)

![Top and Side View](https://github.com/user-attachments/assets/aef00b66-1762-4d3f-972c-ff9a858e2176)

10-23-2024
The machine shop had the design built and we went to the machine shop to look at the design. We brought them the humidity and temperature sensors and their sensors. The machine shop expected a smaller temperature and humidity sensor but they fixed the problem by placing the sensor at the back of the machine panel by drilling in small holes. The air sensor didn’t arrive on time so we were still waiting for them to come in. 


10-21-2024
I had trouble connecting my device esp32 from my laptop. I resolved this issue by manually hard resetting my esp32 by 
1) press on the boot button and hold 
2) press the reset button while pressing the boot button
3) release the reset button
4) continue holding the boot button for a bit and then release.

This is used to remove error states that the esp32 sometimes gets stuck into. I also needed to manually download the device drivers like specific port drivers for the esp32 from the official driver website as when I downloaded the esp32 library drivers didn't automatically download. I needed to go to office hours to resolve this issue.

10-25-2024
The IR sensors arrived so I gave them to the machine shop and they updated the design to get the sensors attached to the device.


10-28-2024
The machine shop was done with the automatic tea machine build and we collected the machine and started on the programming task. I worked on setting up the esp32 dev board.

I was having trouble connecting my esp32 with the school wifi. The thing worked great with apartment wifi and cellular hotspots but wouldn't connect to the university wifi.


10-29-2024
I asked about this issue with the TA and he mentioned he'll ask other group members if they had similar problems. And the ta mentioned that the other groups are using Bluetooth connection rather than wifi. I looked into Bluetooth connection and the app created by the official esp32 company was not that well designed for ios devices. So I decided to continue researching about this and still used wifi connection.


11-1-2024
I resolved the school wifi connection issue by finding a youtube video of a professor from michigan showing his class how to connect microcontrollers to their university wifi. He showed that they need to register the device on the university wifi to have the university grant access to that microcontroller by sending in the ID of that microcontroller. Then I checked the linked github to that youtube video and used that code as the starter code.
The code was still not able to connect so I did a little more research and found that I needed to add a sort of certificate to get the microcontroller connected so I downloaded a certificate from a different microcontroller. The addition of that certificate removed some errors.
But then I found out that the functions for from the michigan professor github are no longer supported so I googled online and found a replacement for these key functions and those functions are included below-

esp_eap_client_set_ca_cert((uint8_t *)incommon_ca, strlen(incommon_ca) + 1);
  esp_eap_client_set_identity((uint8_t *)EAP_IDENTITY, strlen(EAP_IDENTITY));
  esp_eap_client_set_username((uint8_t *)EAP_IDENTITY, strlen(EAP_IDENTITY));
  esp_eap_client_set_password((uint8_t *)EAP_PASSWORD, strlen(EAP_PASSWORD));


11-3-2024
I worked on setting up the website. I set up a website using a react framework. Download all the dependencies required for the setup. Then I worked on adding buttons, playing around with various styles. Watched youtube videos to learn the basics of html, javascript, and css. 

11-4-2024
I switched my website development efforts to using html and css primarily in arduino ide as Milan was using it to test the motor using the wifi component using his mobile network. I set up code for developing buttons, creating a 3 panel screen, being able to read the url and adding a submit button to be able to record values sent by the user. 

11-6-2024
I worked on setting up the load sensor. The load sensor came with a long metal brick shaped piece and a pcb with a chip to control the load sensor. The wires on the load sensor were really tiny and I was having a really hard time connecting them to the jumper wires to get the setup working. 
I was able to get everything connected to the microcontroller by wrapping the wire around the jumper wires exposed metal part. This was not an ideal situation and that I will need to solder this component.

11-7-2024
I went to the lab and worked on soldering the load sensor with a store bought PCB. Then after soldering I worked on programming the load sensor. I worked on the initial calibration code to get the sensor initialized. For the calibration I had to generate a number which will act as a constant multiple for all the future readings.
	To do that downloaded a load cell library and used its functions to tear the scale by placing an object on the scale and haved the code generate a constant. Then I took that generated constant and generated a calibration factor by -

calibration factor = (reading)/(known weight)

Then I added this calibration factor to a function from the library and that function took care of using the calibration factor to get the readings. Then I called the scale.read function and took an average of 5 consecutive readings as my real reading and printed it to the console. The code isn't working correctly as the example had some code on changing the cpu speed and all those functions were not supported esp32 so I had to comment them out. 
	Then I ran all the code to get the reading but the readings were changing quite a lot and the device was having a hard time stabilizing the output.


11-9-2024
Today I worked on soldering the load sensor attached to our machine. I also worked on correcting the clock speed code. It was really hard to find this answer as all the stackoverflow questions on this topic were a bit old so whenever I would try a suggested method I would get errors just to find out that that method is not supported by esp32 anymore. Then in the end I went to the official website and looked into the esp32 manual and found the clock speed section. I corrected the error by adding this code -

rtc_cpu_freq_config_t config;
  int current_freq = rtc_clk_cpu_freq_value(rtc_clk_cpu_freq_get());


  rtc_clk_cpu_freq_get_config(&config);
  rtc_clk_cpu_freq_mhz_to_config(80, &config);
  rtc_clk_cpu_freq_set_config_fast(&config);
  scale.begin(LOADCELL_DOUT_PIN, LOADCELL_SCK_PIN);

This code worked by slowing down the clock to 80M hertz. I found that the original clock speed of the esp32 is too fast for the load sensor and that that was causing the inconsistencies. After this was done I added this code to our master sensor code and integrated it with the motor code. I used this website for the starter load sensor code.

https://randomnerdtutorials.com/esp32-load-cell-hx711/


11-11-2024
I worked on setting up the ir sensor. I wired the sensor on a breadboard and wrote code to get the sensor working. The ir sensor has two sensors where one is the transmitter and one is the receiver. And the transmitter emits a beam of infrared light and if that light receives the receiver the sensor detects that as a digital one. The beam to the receiver is broken in some way that is registered as a 0. I set the wiring by using a pullup resistor to get the ir sensor working. I used this website to get the starter code for the IR sensor. 
https://learn.adafruit.com/ir-breakbeam-sensors/arduino

11-12-2024
I met up with Milan in the ece 445 lab in eceb and ran integration tests with the sensors attached to the actual machine to make sure everything still works. I wrote code for all three IR sensors working together and the code will alert which ir sensor broke and in which storage cell. I integrated the IR sensor code into our master sensor code.


11-13-2024
I met up with Milan and tested the overall machine with the tea.

IR sensor problem
The tea would get stuck in front and the sensor and it would cause the IR sensor to always read broken. I tried to fix this problem by trying to add a piece of paper on the side of the sensor in the storage cell. In my testing I found that the IR sensor worked great even when there was a piece of paper in front of it. That worked but the paper kept detaching itself. So to resolve this problem I used a translucent scotch tape and added that in front of the receiver and transmitter of the IR sensor.
Load Sensor
We did some preliminary testing and found that the load sensor wasn't that great at measuring tea as tea turned out to be too light for the sensor. We change our measurement error tolerances to upto to 15%. We tested it by placing a cup, having the weight sensor zero out the cup after initializing first before the cup was added. Then we would dispense the tea from the dispensing unit and measure the output.

Motors
After repeated trials the tea would get stuck on the sides of the motor and that would jam the motors. We came up with a few strategies to resolve this problem. First we decided to have the motor rotate back and forth whenever it got jammed in one direction. Next we added a piece of thick paper in front of the motor as the screen in front of the storage cell isn't flush with the motor so some of the tea was leaking from the front. Adding the paper makes the screen flush with the motor removing that problem.

11-18-2024
I worked on setting up the HiLetgo FT232RL Mini USB to TTL Serial Converter Adapter Module 3.3V/5.5V. I had a hard time setting up this component as there weren't any instructions to set up a dev board with a serial converter as that's already built into the dev module. But after looking into various websites I was able to get the system wired up and it required the use of 2 capacitors to automatically boot the esp32 dev board.

11-25-2024 and the whole week
I worked on setting up the website. I wrote code to add a drop down menu on the app so that the user can see all the tea blend options when they want to choose one removing the need for them to type that in or have multiple buttons. The code for the dropdown menu is below-

client.println(".dropdown-container {margin: 20px auto; width: 300px;}");
            client.println("select {width: 100%; padding: 12px; font-size: 18px; border: 2px solid #4CAF50; border-radius: 5px; background-color: white; cursor: pointer;}");
            client.println( "select:hover { border-color: #45a049;}");
            client.println( "select:focus { outline: none; box-shadow: 0 0 5px rgba(76, 175, 80, 0.5);}");
            client.println("option { padding: 12px; font-size: 16px;}");


client.println("<div class=\"dropdown-container\">");
              client.println("<form action='/select' method='get'>");
              client.println("<select name=\"tea-options\" id=\"tea-options\">");
              client.println("<option value=\"\" disabled selected>Select your tea preference</option>");
              client.println("<option value=\"1g\">Light steep 1</option>");
              client.println("<option value=\"2g\">Light steep 2</option>");
              client.println("<option value=\"3g\">Medium steep 1</option>");
              client.println("<option value=\"4g\">Medium steep 2</option>");
              client.println("<option value=\"5g\">Dark steep 1</option>");
              client.println("<option value=\"6g\">Dark steep 2</option>");
              client.println("</select>");
              client.println("<input type=\"submit\" value=\"Confirm Selection\" class=\"button\">");  // Add submit button
              client.println("</form>");
              client.println("</div>");


Then I also wrote code to add a timer so that when the user dispenses tea into their cup a timer will automatically start. The stop time of the timer will depend on the type of tea blend and tea type selected. The code for the timer is included below-

client.println("<script>");
            client.println("let timerInterval;");
            client.println("let timeLeft;");
            client.println("let isRunning = false;");
            client.println("function startTimer() {");
            client.println("    if (!isRunning) {");
            client.println("        isRunning = true;");
            client.println("        const minutes = parseInt(document.getElementById('minutes').value) || 0;");
            client.println("        const seconds = parseInt(document.getElementById('seconds').value) || 0;");
            client.println("        timeLeft = minutes * 60 + seconds;");
            client.println("        timerInterval = setInterval(function() {");
            client.println("            if (timeLeft <= 0) {");
            client.println("                clearInterval(timerInterval);");
            client.println("                isRunning = false;");
            client.println("                alert('Timer finished!');");
            client.println("                return;");
            client.println("            }");
            client.println("            timeLeft--;");
            client.println("            updateDisplay();");
            client.println("        }, 1000);");
            client.println("    }");
            client.println("}");
            client.println("function stopTimer() {");
            client.println("    clearInterval(timerInterval);");
            client.println("    isRunning = false;");
            client.println("}");
            client.println("function resetTimer() {");
            client.println("    stopTimer();");
            client.println("    const minutes = parseInt(document.getElementById('minutes').value) || 0;");
            client.println("    const seconds = parseInt(document.getElementById('seconds').value) || 0;");
            client.println("    timeLeft = minutes * 60 + seconds;");
            client.println("    updateDisplay();");
            client.println("}");
            client.println("function updateDisplay() {");
            client.println("    const minutes = Math.floor(timeLeft / 60);");
            client.println("    const seconds = timeLeft % 60;");
            client.println("    document.getElementById('timer').textContent = ");
            client.println("        `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;");
            client.println("}");
            client.println("resetTimer();");
            client.println("</script>");

I also added buttons and other components. I worked on integrating the sensor code and the app code together so that the user inputs can be accessed in the code and used by the sensors or have the sensor's reading be displayed on the app.





11-30-2024

Worked on subsystem testing:

Dispensing Subsystem:
Our requirement was that it must be able to receive signals from the motor controller and rotate stepper motors a full 360 degrees while also allowing control over varying step sizes.

THe way we checked for it was by - Test that the motors can receive signals by sending the motor driver signals from the software side. Repeat this with varying step amounts and visually verify that the motor 
moves the intended amount.

Our requirement was that it must be able to stop when notified by the microcontroller that the dispensing process has been completed. There should be minimal latency with 0 extra rotations.
We tested it by -Send a request for some amount of tea to be dispensed. Observe print statements from the software side showing the readings from the load sensor as tea is dispensed. Verify that the motors stop rotating (with no extra rotations) once the requested amount of tea has been dispensed.

![Dispensing_subsystem_table](https://github.com/user-attachments/assets/2c179d34-2137-4b6c-8761-f6e7e8fd4728)

![Dispensing_subsystem_graph](https://github.com/user-attachments/assets/3a4640db-b694-43fe-bd28-deb04b0fc3c4)


Measuring Subsystem:
Our requirement was that it must be able to accurately make readings using a load sensor from 300-500 grams with 15% accuracy. 
We test this by i-nclude code on the microcontroller to output readings from the load sensor
Add different amounts of dry tea leaves, ranging from 300-500 grams to the load cell and note values output from the microcontroller
Manually weigh the same amount on a smaller, cheap weighing scale
Compare the two values and ensure that readings from the microcontroller are within 5% of the expected value. 

Our requirement was that it must be able to register fine changes in measurements and allow users to calibrate a steeping spoon to "zero" out the scale. We test this by adding a cup to the load sensor and note the weight of it before adding tea.
Add small amounts of tea and take repeated measurements to verify that the reading increases as more is added.
Verify that the software side is able to note the tare weight of the cup before dispensing tea. Check that the weight of the cup and the weight of the requested tea add up to an amount that falls within 1 gram of the amount requested.

![Measuring_subsystem_table](https://github.com/user-attachments/assets/f2b426df-e4f8-4743-a8c9-004dc6e4fdd4)



12-2-2024- whole week
We worked on the presentation slides for our mock presentation and work on getting everything working for the mock Demo for thai whole week

