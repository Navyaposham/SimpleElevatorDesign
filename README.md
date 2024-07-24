# SimpleElevatorDesign

## Introduction 

Design of System Verilog FSM for an elevator control system. The system controls the movement of an elevator in a multi-story
building with the following requirements:

**1. Floor Selection:** The elevator is able to stop on multiple floors. Users can select the desired floor from inside the elevator.

**2. Priority Handling:** The elevator prioritize serving passengers based on the order in which they press the buttons.

**3. Emergency Stop:** Including an emergency stop button inside the elevator. When pressed, the elevator stops at the next available
floor and remain there until the emergency stop button is reset.

**4. Limit Sensors:** Implementation of sensors to detect the elevator's position and ensure it does not exceed the top or bottom floors.

**5. State Transitions:** Design of a finite state machine to manage the elevator's states, such as moving up, moving down, idle, and
emergency stop.

**6. User Interface:** A simple user interface or testbench to simulate user input and elevator operation, including floor selections
and emergency stop requests.

**7. Testing and Simulation:** A test bench to verify the correctness of your FSM design. Simulation of various scenarios to ensure
the elevator operates as expected.

**8. Documentation:** Documentation that includes state diagrams, state transition tables, and explanations of the FSM's operation.


## Assumptions Made 

### 1. Direction Persistence 

The first assumption, about the elevator not changing direction until it completes all requests in that direction. This means that if the elevator is moving upward, it will continue to serve requests going upward until there are no more requests in that direction. The same applies when moving downward. This behavior aligns with the concept of the elevator prioritizing requests in the direction it's currently moving. 

### 2. Idle State 

The assumption that the elevator stops and becomes idle after serving all requests. It  ensures  that  the  elevator  doesn't  keep  moving unnecessarily once all passenger requests have been fulfilled. The idle state is a crucial part of the system's operation as it signifies that the elevator is available for new requests. 

### 3. Door Control Signal 

The assumption that arrival sensors generate a control signal for opening the lift door for a clock timer upon reaching the destination floor. It ensures that the doors remain open for a sufficient time to allow passengers to enter and exit the elevator  safely. This  safety  feature  is  essential  for preventing  accidents  and  ensuring passenger convenience. 


## Priority Algorithm Implementation and Optimization

The implementation of the modified priority algorithm within the Elevator Control System  represents  a  substantial  improvement  in  elevator  operations.  By  intelligently prioritizing  requests  based  on  proximity  and  direction,  the  system  reduces  travel  times, conserves energy, and enhances the overall passenger experience. The seamless integration of an emergency system ensures that safety is not compromised while maximizing efficiency in elevator operations. The Elevator Control System's optimization and achievement of adaptability to varying floor counts showcase its versatility, scalability, and resource efficiency. This accomplishment positions our system as a versatile and future-ready solution, ready to meet the diverse needs of the vertical transportation industry, from small-scale buildings to high- rises with numerous floors.

## State Tables 

### Table 1 

Table 1 provides a detailed overview of the finite state machine transitions within our elevator control system.

<table><tr><th colspan="1">Current State</th><th colspan="1">Input</th><th colspan="1">Next State</th><th colspan="1">Output</th></tr>
<tr><td colspan="1" valign="bottom">Reset</td><td colspan="1">reset = 1</td><td colspan="1">Reset</td><td colspan="1">Idle =1,door=0, Up=1, Down=0,requests=0, estop=0, current_floor=0</td></tr>
<tr><td colspan="1"></td><td colspan="1">reset = 0</td><td colspan="1">Door Closed & Idle (1)</td><td colspan="1">Idle =1,door=0, Up=1, door_timer=0</td></tr>
<tr><td colspan="1"></td><td colspan="1">Checker : requests[current_floor] = 1</td><td colspan="1">Door Open & Idle (1)</td><td colspan="1">door = 1; idle = 1; requests[current_floor] = 0; door_timer = 1</td></tr>
<tr><td colspan="1" rowspan="2">Door Closed & Idle (1)</td><td colspan="1">max_request > current_floor  && Up = 0</td><td colspan="1">Moving Up</td><td colspan="1">idle = 0; current floor +=1</td></tr>
<tr><td colspan="1">min_request > current_floor && Down = 0</td><td colspan="1">Moving Down</td><td colspan="1">idle = 0; current floor -=1</td></tr>
<tr><td colspan="1"></td><td colspan="1">max_request = current_floor</td><td colspan="1">Down Direction Setter</td><td colspan="1">Up=0; Down=1</td></tr>
<tr><td colspan="1"></td><td colspan="1">min_request = current_floor</td><td colspan="1">Up Direction Setter</td><td colspan="1">Up=1; Down=0</td></tr>
<tr><td colspan="1">Door Open & Idle (1)</td><td colspan="1">door_timer = 1</td><td colspan="1">Door Closed & Idle (1)</td><td colspan="1">Idle =1,door=0, Up=1, door_timer=0</td></tr>
<tr><td colspan="1"></td><td colspan="1" valign="bottom">Checker = 1</td><td colspan="1" valign="bottom">Door Open & Idle (1)</td><td colspan="1" valign="bottom">door = 1; idle = 1; requests[current_floor] = 0; door_timer = 1</td></tr>
<tr><td colspan="1" valign="top">Moving Up</td><td colspan="1" valign="bottom">Checker = 0</td><td colspan="1" valign="bottom">Moving Up</td><td colspan="1" valign="bottom">idle = 0; current floor +=1</td></tr>
<tr><td colspan="1"></td><td colspan="1">estop = 1</td><td colspan="1">Door Closed & Idle (1)</td><td colspan="1">Idle =1,door=0, Up=1, door_timer=0</td></tr>
<tr><td colspan="1"></td><td colspan="1" valign="bottom">Checker = 1</td><td colspan="1" valign="bottom">Door Open & Idle (1)</td><td colspan="1" valign="bottom">door = 1; idle = 1; requests[current_floor] = 0; door_timer = 1</td></tr></table>

### Table 2 

Table 2 presents a concise representation of state transitions related to the update of elevator requests, max\_request, min\_request and how the elevator system handles new floor requests and maintains these key variables.


<table><tr><th colspan="1">Current State</th><th colspan="1">Input</th><th colspan="1">Next State</th><th colspan="2">Output</th></tr>
<tr><td colspan="1" rowspan="2">UpdateRequests</td><td colspan="1" valign="top">NewFloorRequested</td><td colspan="1" valign="top">NewFloorRequested</td><td colspan="2" rowspan="2">x</td></tr>
<tr><td colspan="1">No NewFloorRequested</td><td colspan="1">UpdateRequests</td></tr>
<tr><td colspan="1"></td><td colspan="1">max_req < req_floor</td><td colspan="1">UpdateMaxReq</td><td colspan="2">max_req = req_floor</td></tr>
<tr><td colspan="1"></td><td colspan="1">min_req > req_floor</td><td colspan="1">UpdateMinReq</td><td colspan="2">min_req = req_floor</td></tr>
<tr><td colspan="1" valign="top">NewFloorRequested</td><td colspan="1">req[max_req] == 0 & req_floor>currFloor</td><td colspan="1">UpdateMaxReq</td><td colspan="2">max_req = req_floor</td></tr>
<tr><td colspan="1"></td><td colspan="1">req[min_req] == 0 & req_floor<currFloor</td><td colspan="1">UpdateMinReq</td><td colspan="2">min_req = req_floor</td></tr>
<tr><td colspan="1">UpdateMaxReq UpdateMinReq</td><td colspan="1">x</td><td colspan="1">UpdateRequests UpdateRequests</td><td colspan="1">x</td></tr>
</table>
