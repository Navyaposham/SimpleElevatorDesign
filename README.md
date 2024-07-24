# SimpleElevatorDesign

## Introduction 

Design of System Verilog FSM for an elevator control system. The system controls the movement of an elevator in a multi-story
building with the following requirements:

**1. Floor Selection:** The elevator should be able to stop on multiple floors. Users can select the desired floor from inside the elevator.

**2. Priority Handling:** The elevator should prioritize serving passengers based on the order in which they press the buttons. For example,
if a user inside the elevator selects floor 5, and another user on floor 3 presses the button to go to floor 4, the elevator should first go to
floor 3 to pick up the new passenger before proceeding to floor 5.

**3. Emergency Stop:** Include an emergency stop button inside the elevator. When pressed, the elevator should stop at the next available
floor and remain there until the emergency stop button is reset.

**4. Limit Sensors:** Implement sensors to detect the elevator's position and ensure it does not exceed the top or bottom floors.

**5. State Transitions:** Design a finite state machine to manage the elevator's states, such as moving up, moving down, idle, and
emergency stop.

**6. User Interface:** Create a simple user interface or testbench to simulate user input and elevator operation, including floor selections
and emergency stop requests.

**7. Testing and Simulation:** Develop a test bench to verify the correctness of your FSM design. Simulate various scenarios to ensure
the elevator operates as expected.

**8. Documentation:** Provide documentation that includes state diagrams, state transition tables, and explanations of the FSM's operation
