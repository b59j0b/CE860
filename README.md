java cAdvanced Embedded Systems Design CE323/860 Weiyong Si Mini project
CE323/CE860 Advanced Embedded Systems Design Assignment
The assignment is worth 30% of the total mark for this module. This assignment is to design a home 
alarm system based on the mbed NXP LPC1768 development board.
The assignment is based on the ARM mbed NXP LPC1768 microcontroller. Some of the details about 
the mbed will be provided in the lecture slides, but full details can be found from the website (Website
link). The extension board was developed by the lab technician. The board details can be found from 
the module website. In the following, a brief description on online compiler, UART link to PC, and the 
extension board is provided first. Some exercises related to the assignment are then provided to 
practice. These exercises are helpful for you to better solve the two assignments.
1. The ARM mbed and Online compiler
Each student will be assigned with anmbed and its extension board. The mbed uses an online compiler 
tool (arm KEIL studio ) to edit and compile the source code. You have to create an mbed user account 
in order to access to the mbed website and resource (use this link to sign up). You must search and 
select “mbed LPC1768” from the search section.
After log in your account, you can select the compiler menu to go to the workspace. Once going to the 
workspace, you are able to create new project (from File menu), and edit and compile the source code. 
The generated bin file can be moved into the mbed folder to execute. The mbed starts to run the latest 
the bin file once you reset it. The details of using the online compiler can be found here (link).
Fig. 1 Physical extension board
Advanced Embedded Systems Design CE323/860 Weiyong Si Mini project
2. Home Alarm System
The home alarm system specification is given below. The sensors mentioned below will be replaced 
with the switches in the virtual extension board and the alarm is replaced with a LED in your design. 
Based on the specification, you need to develop a formal specification using a graphicalrepresentation 
(preferably Unified Modeling Language (UML)). The UML specification should include structure 
diagrams (class diagrams and/or object diagrams), and behaviour diagrams (state machine diagram, 
sequence diagram). The formal specification should be written in a report. Then you must write a
C/C++ program to implement the design of the home alarm system. You also need to make a
demonstration in the lab.
3. The report:
The report should be in PDF and must include a cover page with the module name, student name and 
ID. The source code should be included in the report. The maximum length of the report is of 5 pages
for CE323 Students and 10 pages for CE860 students (without appendix).
An important note for CE860 Students:
The program and functionality are similar. However, higher level of technical explanations is expected
from CE860 students. Therefore, the length of the report is considered to be 10 pages.
The report should include:
▪ Requirement form
▪ UML graphical representation of the system 
• class diagrams
• state machine diagrams 
• sequence diagrams
When necessary, you should provide explanation. Please note that this is the open end and student oriented part of the project! the diagrams will not be taught in the class, and you need to self-study 
through internet to learn them.
4. Source code
The parts of code you are developing in the labs can be used in your program. You must complete the 
program and deliver required functionalities.
Full source code should be provided in the report as an appendix.
Full source code should also be provided as a separate document, which can be run in the ARM 
mbed NXP LPC1768 . 
Advanced Embedded Systems Design CE323/860 Weiyong Si Mini project
5. Submission:
The report should be submitted through the online coursework submission system (FASER). You 
should submit a single pdf file called “CE323 Assignment 2 (your name)” for CE323 students and 
“CE860 Assignment 2 (your name)” for CE860 students. Full source code should also be uploaded as 
a separate document, which can be run in the ARM mbed NXP LPC1768 . 
Assessment criteria:
• Following the rules mentioned in this document for the submission and preparation of the 
assignment 10%
• Class diagram 10%
• State machine diagram 10%
• Sequence diagram 10%
• Correctness of program 30%
• Quality and Clarity of program 30%
6. Late submission and plagiarism
This assignment is to be done individually, i.e. whatever you submit must be your own individual work. 
Any software or any other materials that you use in this assignment, whether previously published or 
not, must be referred to and properly acknowledged. Please be aware that the module supervisor may 
ask students for an interview to explain their submitted work. Please refer to the Undergraduate 
Students’ Handbook for details of the school policy
regarding late submission and University regulations regarding plagiarism:
http://www.essex.ac.uk/ldev/resources/plagiarism/default.aspx
http://www.essex.ac.uk/about/governance/policies/academic-offences.aspx
Advanced Embedded Systems Design CE323/860 Weiyong Si Mini project
Home Alarm System
1. Introduction to alarm systems
A home alarm system is constructed from various separate parts, which all connect to a central control 
unit (in this link you can find a DIY example of a home alarm system). Your task is to implement a basic 
control unit and the functionality behind its user interface. To complete this task you must first 
understand the specification of the external parts provided and the required behaviour of the overall 
system.
A conventional 代 写CE860、C/C++
代做程序编程语言alarm system normally contains:
• Central control unit
• Keypad entry point / user interface 
• Sounder unit
• Sensors such as: magnetic contacts (reed switches), pressure mats, movement sensors like 
passive infrared sensors (PIRs).
In the case of the sensors, they all contain switches that are “closed” circuit under normal 
circumstances. This type of switch is known as a Normally Closed (NC) switch, the converse is known 
as Normally Open (NO). Magnetic contacts (The principle of Magnetic door contact switch refer to 
Link) are used to detect when doors and windows are opened. When a door is opened a magnet is
moved away from the reed switch which causes the switch contacts to open. Likewise, when a
movement sensor is triggered or a pressure mat depressed the circuit is broken (the switch is
opened), see Figure 1. The opening of a contact can be detected by using one of
the microcontroller's parallel port inputs, connected to the top of the switch whose lower end is
connected to GND. The top of the switch is also connected through a resistor to 3.3v (V+). When the
switch is closed the port, input is connected to 0v (GND), and it will be pulled-up to 3.3v (V+) by the
resistor when the sensor's switch opens. Therefore, the microcontroller input is logic low (0v) 
when a door is closed / there is no movement, and logic high (3.3v) when the sensor is activated. The 
alarm controller must detect these events and respond as specified later in this document. In this 
assignment, the sensors are emulated using normally closed push to break switches.
Figure 1: Alarm sensors
Normal home alarm systems include multiple sensors located in various locations, each location can 
have its own circuit which allows identification of the triggered switch, i.e. it lets the user know where 
the break-in occurred. Each sensor circuit is known as a zone, and zones can have different behaviour.
Advanced Embedded Systems Design CE323/860 Weiyong Si Mini project
2. Functional Specification - Zone Behaviour
The board will be used for demonstration. This alarm has SIX states of operation set, unset, entry, exit, 
alarm, and report. Initially it is in the unset state.
• Unset state:
▪ In the unset state, activation of any of the sensors should not cause the alarm-LED to 
blink.
▪ Entry of the correct four-digit code in the user interface (described later) followed by 
“B” should cause the system to change to the exit state.
▪ If the user enters an invalid code three times, the alarm should change to the alarm 
state.
• Exit state:
▪ When in the exit state, the user has a time interval called the exit period (you can 
choose any time e. g. 1 minute) in which to evacuate their home.
▪ In the exit state, activation of any sensors in a full set zone should cause the alarm to 
enter the alarm state.
▪ Whilst in the exit state the alarm-LED should blink.
▪ If the user enterstheirfour-digit code followed by “B” within the period, the exitstate 
should change to the unset state.
▪ If an invalid code is entered three times, the exit state should change to the alarm 
state.
▪ If all the zones are inactive when the exit period (e. g. 1 minutes) expires, the exit 
state should enter the set state.
• Set state:
▪ In the set state, activation of any sensors in the set state zone should cause the 
system to enter the alarm state.
▪ Activation of the entry / exit zone should change the state to entry state. 
• Entry state:
▪ The purpose of the entry state is to allow the user a period of time to gain access to 
their home so that they can unset the alarm, this duration is known as the entry 
period (e. g. 2 minutes).
▪ Whilst in the entry state, the alarm-LED should sound blink.
▪ If the user enters their four-digit code followed by “B” within the period, the state 
should change to the unset state.
▪ If the user fails to enter their correct code within the entry period, the state should 
change to the alarm state.
▪ In the entry state, activation of any sensors in a full set zone should cause the state 
to enter the alarm state.
• Alarm state
▪ When in the alarm state, the alarm-LED should be on all the time. 
▪ After 2 minutes, the alarm-LED unit should be disabled.
▪ If the user enters the correct code followed by “B”, the alarm should change to the 
report state, otherwise stay in the alarm state.
• Report state
Advanced Embedded Systems Design CE323/860 Weiyong Si Mini project
▪ When in the report state, the LCD should show the zone numbers or code error 
information in the first line (e. g. ‘code error 1’). In the second line it should show “C 
key to clear”.
▪ When an “C” is entered, the alarm should change to the unset state.
When a sensor in a zone is active (circuit broken), the corresponding LED should be illuminated. When 
the system leaves the alarm state, the alarm-LED should be off.
The LCD should display the state of the system on the first line of the LCD display.
• When the user enters the first digit of their code in the unset state, the second line of the 
display should additionally show left aligned “Code: * _ _ _” with the “_” characters being 
replaced with each successive digit entered.
• If the user pressesthe “C” key, then the last entered charactershould be deleted and replaced 
with “_” unless there are no characters left, in which case the display should only display the 
state, i.e. be the same as when none of the code had been entered.
• When all four digits of the code have been entered, the second line of the display should 
display “Press B to set”.
• Any other key should cause the code entry procedure to abort without checking the user 
code.

         
加QQ：99515681  WX：codinghelp  Email: 99515681@qq.com
