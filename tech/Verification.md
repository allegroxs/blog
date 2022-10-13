# Formal
- State space explosion


## Basic formal closure
- Inconclusives can be due to
  - design state space and complexity
  - property state space and complexity
  - initial state
  - number of inputs
  - constraints 
- Other options
  - use other methods to analyze the property: simulation/Emulation
  - use different tool options
  - modify the property
  - reduce design state space
### 2 main ways to resolve an inconclusive
- using tools/resouces options
  - ...
- reduce state space
  - reduce property state space
  - reduce design state space
  - 4 ways to minimize design state space
    - lower hierarchy
    - more assumptions
    - easy abstractions
      - alter parameter/generic settings
        - typically done hen changing the parameter doesn't effect the control logic. datapath can be affected
        - reduce the width of busses!
        - reduce the depth of FIFOs!
      - constrain the design with constants
        - scan/test/clock enable pins
        - mode/select pins
        - data pathe reduction
        - internal configuration registers
      - blackbox parts
        - outputs become control points
        - proofs obtained will be good
      - add cutpoints
        - cut the complex parts from the Cone of Influence(COI)
        - proofs obtained will be good
    - advanced abstractions


## Formal Model Checking
Simulation costs time, but formal does not require a testbench or input stimulus. It cost more memory.

No vectors. Can begin very early.

Terminology:
Property
- Description of design behavior
Assertion
- Property being targeted by formal analysis
Proof
- Property is true across the full design space
Counterexample
- Stimuli that violates a property, usually shown as a waveform
Assumption
- Property used to limit the input conditions considered by formal

3 ways to use a property with formal
Assertion (assert property)
- Property is targeted for formal analysis
- Need at least one of these (or cover)
Assumption (assume property)
- Property limits formal analysis
- Formal can not violate the assumption
Cover Statement (cover property)
- Show stimulus which reaches the property/sequence

Requirements befor run formal
- plan
- assertion
- synthesizable design (gete level representations ok)
- knowledge
  - clocks
  - modes of operation: normal test scan
  - complex logic
  - reset logic

ABC's of formal
Assurance
Bug hunting
coverage closure

## PropCheck
Questa ProCheck
Leverage UCDB/VM or Interactive Debug

# Intermediate-level formal

# IP for Formal


# UVM
## SVA
s1 |-> s2: 如果s1 是match的， 那么s2也必然要match； 如果s1不匹配， 那么这个squence的结果就是true
s1 |=> s2: 代表的是s1在下一个clock在进行判断，这种写法等价于下面的描述

## 