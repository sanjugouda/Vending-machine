# Vending-Machine


## 1 Introduction
In this project, the objective was to build a simple chip to control a cans vending machine.

### 1.1 Machine Operation
The machine operates as follows:
- The price of the can is 1.5 LE.
- The machine only accepts 1 LE and 0.5 LE coin, with a maximum of one coin entered at each cycle.
- The machine returns the change if there is any.

### 1.2 Chip Specifications
The chip must have the following specs:
- Two input bits; **one_in** is high if the entered coin is 1 LE, and **half_in** is high if the entered coin is 0.5 LE. If no coin is inserted, both inputs are low.
- Two output bits; **can_out** is high when a can is to be output, and **change_out** is high if there is a change.
- An externally supplied **Reset** signal, which is also triggered after the can/change is ready. **Reset** is applied at the first falling clock edge after the can/change is out.
- Works at 10MHz.

## 2 Design Process
### 2.1 FSM Design
The FSM design process was straightforward. The tables below show the state transition and output table, while the figure shows the state transition diagram.
The chosen FSM is a Moore FSM, with 5 states that transition as shown. Depending on the input, the FSM either transitions to another state or stays at the current state waiting for input to come. A reset signal, that is supplied externally, is used to bring the FSM back to the known initial state IDLE.

|State|state[2]|state[1]|state[0]|ne_in|half_in|nstate[2]|nstate[1]|nstate[0]|
|-----|--------|--------|--------|-----|-------|---------|---------|---------|
|**IDLE**|0|0|0|0|1|0|1|0|
|**IDLE**|0|0|0|1|0|0|0|1|
|**ONELE**|0|0|1|0|1|0|1|1|
|**ONELE**|0|0|1|1|0|1|0|0|
|**HALFLE**|0|1|0|0|1|0|0|1|
|**HALFLE**|0|1|0|1|0|0|1|1|
|**CANOUT**|0|1|1|X|X|0|0|0|
|**CHNGOUT**|1|0|0|X|X|0|0|0|

|State|state[2]|state[1]|state[0]|can_out|change_out|
|-----|--------|--------|--------|-------|----------|
|**IDLE**|0|0|0|0|0|
|**ONELE**|0|0|1|0|0|
|**HALFLE**|0|1|0|0|0|
|**CANOUT**|0|1|1|1|0|
|**CHNGOUT**|1|0|0|1|1|

![FSM Diagram](https://i.imgur.com/Pn0OESe.png "FSM Diagram")

### 2.2 High Level RTL Design
The RTL description of the FSM is coded in Verilog. The code describes the above FSM and allows for functional testing and simulation.
The simulation is carried out using ModelSim. The result of the simulation is shown in the next figure.

![FSM Testing Results](https://i.imgur.com/tUaPP7I.png "FSM Testing Results")
