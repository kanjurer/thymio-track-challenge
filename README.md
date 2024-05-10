# Thymio Track Challenge

In this project, we had to program a Thymio II robot to navigate itself through a track that had several barriers (figure 1) including black taped lines on the ground, Duplo blocks that enclosed some areas and barcodes that could let the robot know which turns to take. The robot was finally supposed to reach the "treasure room" and stop in the middle of the circle as shown in figure 1. There was an additional task given for us to do, which was to program the robot to beep and stop at the 3 black taped lines for 5 seconds. As this program consisted of so many small programs, we wrote multiple subroutines and then combined them all to form one big program that could do all the tasks required. The result of our project was a massive success with the maze being completed in just 47 seconds.

![image](https://github.com/kanjurer/thymio-track-challenge/assets/66438237/670a18eb-a8ae-46cf-b393-a78a89b51980)
_Figure 1: Temple Layout_

## Introduction

The Thymio II robot contains many kinds of actuators. The main three actuators that are being used in this project are its motors, LEDS, and speakers. The motors will allow us to move the robot around the temple. The LEDs and speakers will allow us to show when important events have occurred. The robot also has many sensors. The ones being used in this project are its ground vision sensors and horizontal vision sensors. These sensors will let us do many important tasks in the context of this project. Of these tasks some notable ones are following a line to arrive at a desired location, reading a barcode and storing the data, and reading distance from a wall. Finally, the Thymio robot contains controllers, the controllers help process the data received from the sensors using an on-board CPU. This can rarely impact the effectiveness of the robot's performance if the CPU is too slow.

To complete this temple, we used the ASEBA programming language to give the robot the set of necessary instructions. We also used the hammer emulator so we could test our program without needing the robots. The main issue we faced during our project lay in the difference between the hammer simulation and reality. We started developing most of our project over the reading week break using the hammer simulator. After getting the program to effectively finish the temple in hammer we assumed that there wouldn;t be any drastic change in the effectiveness of our program. Unfortunately, we were very far off in our assumption, The tap feature, which we based a majority of our program off, didn;t work on the actual robots. The reason the tap event was broken was due to its sensitivity. The tap event was sensitive to such a degree that it would end up tapping itself, thus causing an infinite feedback loop. This meant we had to re-design a large majority of our program.

As stated previously, the task we were assigned was to program the robot to find its way to the end of a temple (figure 1). In order to do this the robot needed to overcome a few hurdles. Firstly, the robot needed to read three barcodes, read their information, store it, and act accordingly. The actions based off the barcodes are which directions it turns and which color it will turn at the end of the temple. The direction the robot turns is important as some of the temple walls will be removed (see figure 2. and figure 3.) to allow the robot to traverse to the end of the temple.

![image](https://github.com/kanjurer/thymio-track-challenge/assets/66438237/23a14abe-2572-4ff8-907c-b42abc23fe0a)

_Figure 2: First Walls_

![image](https://github.com/kanjurer/thymio-track-challenge/assets/66438237/5d3bcbed-8711-4302-b564-fbe476859af4)

_Figure 3: Second Walls_

Secondly, the robot needed to find the "hazard line"" indicated by 3 black taped lines (see figure 4.). The robot also needed to stop on these three lines for 5 seconds, flash red, and beep.

![image](https://github.com/kanjurer/thymio-track-challenge/assets/66438237/390179eb-ee16-4e12-b16f-2cb9b0e17c44)

_Figure 4: "Hazard Lines"_

Finally, the robot needed to follow a black taped path to the "treasure room", then stop on top of the black taped bullseye (see figure 5.).

![image](https://github.com/kanjurer/thymio-track-challenge/assets/66438237/ca79e119-757d-4c81-a705-8cd73e8c2a9f)

_Figure 5: "Treasure Room"_

## Background

Since this is our final project, it is cumulative. This means our project is an amalgamation of all our past works and concepts. The most prevalent past works in our program include: the line follow program (Kane, 2021), object recognition (Kane, 2021), and the angular speed program (Kane, 2021). The line follow program used twice in our program. It's first use is with scanning the barcodes where it serves to align the robot with the barcode, thus allowing for a precise scan. The second use for the line follow program is to guide the robot towards the "treasure room" as seen in figure 5. Both programs make use of Thymio's actuators. These actuators, however, are not without their limitations. The motors can never perfectly go the same speed which can cause inconsistencies when going straight. The object recognition concept is the most used and most vital concept for our project. As our project has 3 bar codes that need to be read accurately it is very important that our bar code reading subroutine is accurate. This concept uses Thymio's sensors which, like the actuators, have their own limitations. One of the limitations is how the sensors have a relatively short distance and an even shorter distance for an accurate reading. The final program that we used in this project is the angular speed program. Near the end of the temple the robot will need to make a precise abrupt turn and we used the angular speed program to get a rough idea of what value would be best to accomplish this. During the development of this project, we made one big assumption. This assumption was that the course would not undergo any changes besides the ones already defined being the walls opening and closing.

## Program Description

Overall, when developing the design for our program we based it off this one important fact: the temple would stay the same. This meant the robot would never have to adapt to any unknown situations. Knowing this, we could program the robot to do a hardcoded motion for each hurdle in the temple. We opted for this design strategy as we thought it was more practical than a design in which the robot would adapt to various situations. This problem is relatively complex, so making it adapt, when perfected, while being more consistent, would take far more time to perfect than we had available. One downside to this design is that the robot would need to use a counter to keep track of where it was in the temple, this makes it susceptible to errors. The way we kept track of the robot's location was by incrementing the counter variable each time the robot completed a hurdle.

The first hurdle was reading and finding the first three barcodes. We started off by designing a subroutine that would allow the robot to get into a position where it could read the barcode efficiently, consistently, and accurately. The way we did this was by making the robot align itself with the barcode when it detected the black line by following it with slow turns. This happens between the FORWARD and SENSE states (see figure. 6). We also needed the robot to be at a distance where its sensors could accurately read the barcode. To do this we made the robot read the barcode and turn when it was about ¼ of an inch away from the wall. This happens between the SENSE and REVERSE states. We opted for this solution as it made reading the barcodes more consistent. Consistency is important when reading the barcode simply due to the shear precision required. One of the downsides to this approach is that it is very slow, when the robot is aligning itself with the barcode it takes slow turns to increase precision.

![image](https://github.com/kanjurer/thymio-track-challenge/assets/66438237/82831c91-7123-44af-9bd3-17f9c4fb06b0)

_Figure 6: First Hurdle STD_

The second hurdle was making the robot go from the last barcode to the hazard lines, detect them, and use the correct actuators (LEDs and speakers). To complete this task, we programmed a hardcoded turn that was dependent on which way the robot turned on the initial barcode. Assuming the counter variable is the correct value after the last barcode we will know the next time the robot senses black tape it will be the hazard line. From this, we can call the LEDs and speakers. The hardcoded motion is done with two states. The first state is HARDCODED_MOTION_STRAIGHT (see figure 7.), this state is when the robot will be driving in a straight line for the duration of a 1.7s timer. The next state is HARDCODED_MOTION_TURN, this is when the robot will complete its gradual turn until its ground sensors detect the black line. We chose this solution as we wanted to eliminate the use of sensors as much as we possibly could as each sensor adds more unpredictability to the program. One downside of this approach is that it is reliant on the counter variable which can cause problems if the robot fails to register a turn earlier.

![image](https://github.com/kanjurer/thymio-track-challenge/assets/66438237/c112b559-0e31-49af-96fc-9c5abd0c18c3)

_Figure 7: Second Hurdle STD_

The final hurdle was the robot finding its way to the end of the temple. To do this we first needed a method for the robot to drive to the "treasure room" accurately and consistently. The method we developed for this was using a line follow program to guide the robot to the entrance of the "treasure room". This line following subroutine happens in the DETECTIVE subroutine (see figure 8.). Upon reaching the entrance the guiding black tape will reach an end which will signal the robot to make an abrupt sharp turn facing the robot towards the bullseye where it will drive in a straight line until it reaches the black middle dot of the bullseye, upon reaching the bullseye the robot enters the STOPPED state. We chose this approach because it was both the fastest and most reliable out of our options. The main downside to our approach is how the line following subroutine uses short timers every time the robot detects the black tape to see if the robot should start moving forward towards the bullseye. Because the abrupt turn is triggered by the timer event, the turn can happen at any point during the line follow subroutine therefore the turn is inconsistent.

![image](https://github.com/kanjurer/thymio-track-challenge/assets/66438237/5cd9daf5-5624-4475-b8ad-ba2725052bb0)

_Figure 8: Third Hurdle STD_

## Results

The result of our program was the robot successfully completing each hurdle without any issues on its first run. The program we designed was a successful solution to the problem we were given. Our robot completed the maze in 47 seconds which was the fastest time in our lab section (see figure 9.). The reason our program was able to compete in a competitive time was because of our overall design philosophy. We opted to create a program that would complete the tasks more consistently rather than quickly. Although our program, on a successful run, is slower than our classmates, it has a far higher success rate which ultimately outweighs this downside.

![image](https://github.com/kanjurer/thymio-track-challenge/assets/66438237/907f6e2f-ca61-495d-804f-102aee6f46eb)

_Figure 9: Competition Results_

The main reason we were able to achieve a noteworthy level of consistency was due to our constant optimizing of subroutines. There were a few main subroutines where our optimization added a big boost to the performance. The first of these subroutines was the barcode detection subroutine. This subroutine required a lot of precision, because if the robot registered the wrong color, it would cause an error only noticeable much farther down the temple. There were two important variables when optimizing the barcode detection subroutine: the threshold values (value to tell the robot if what it's sensing is black or white) and the distance when reading the barcode. We discovered the optimal values for these variables by manually moving the robot and gathering live values. We used the average of these values to arrive at the most consistent, optimal values.

The second subroutine that was essential to the success of our robot was the "hazard-line" subroutine. This subroutine would cause the robot to turn into the hazard lines, stop, and beep. While many groups in our lab opted to make a sharp 90 degree turn into the lines, we decided to do a more optimal approach. This approach was making a more gradual continual turn as seen in figure 10.

![image](https://github.com/kanjurer/thymio-track-challenge/assets/66438237/68b52388-1b4b-4832-af1e-f42369ef73a7)

_Figure 10: Turn Paths_

This approach is both better than many of our classmate's approaches in two ways. Firstly, our approach is faster, this is visible in figure 10 as our robot takes a shorter path. Secondly, our approach is more consistent as it does not rely on sensors to function.

## Conclusion and Future work

In Conclusion, our program is a combination of the line follow, bar scanner, and angular speed programs. The overall design philosophy of our program was to prioritize consistency over speed. This helped us get first in our lab section. Our program could've been improved by optimizing the speed of the black taped lines before the bar codes. This area is exceptionally slow and could've cut at least 10 seconds off our time. While designing our program we chose to not implement a failure mechanism as we thought it would be a waste of time. This is something, had time been allowing, we would've liked to add.
