# Formal (verificationacademy)
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


# JasperGold (Cadence)

## Formal friendly SVA

### Inefficient SVA example
```
P_BAD:  assert property ( @(posedge CLK) (req |-> s_eventually gnt) );
P_GOOD: assert property ( @(posedge CLK) ( $rose(req) |-> s_eventually gnt))
```
property s_eventually p is true iff property p is true in someclock tick.

`$rose(req) |-> req[*] ##0 ack` equals to `$rose(req) |-> req[+] ##0 ack `. If req went high stayed high forever, but ack never occurred, will they fail?  It depends. Use: `req && !ack |=> req`

When PULSE, SIG cannot change:
```
P20: assert property( $rose(PULSE) |-> $stable(SIG) throughout (##[1:&] $fell(PULSE)));
P21: assert property( $rose(PULSE) |-> $stable(SIG) throughout (!PULSE[->1]) );
P21: assert property( PULSE |=> $stable(SIG) || !PULSE );
```

B stable when A is 1 until A changes to 0:
```
P3: assert property( A |=> strong( $stable(B)[*0:$] ##1 ($stable(B) &&!A) ) );
P3: assert property( A |=> strong( $stable(B)[*0:$] ##1 ($stable(B) &&!A) ) );
P1: assert property( A |=> ( $stable(B) s_until_with !A) ) );
```
**Creat infinite length sequences? should or shouldn't ?**

```
P4: assert property(A |=> $stable(B));
P5: assert property($fell(A) |-> $stable(B));
```

Not to use some expressions.

### Overlapping example and auxiliary code
If requests can overlap,
```
A_WRONG: assert property ( @(posedge CLK) REQ |-> ##[2:8] GNT)
A_RIGHT: assert property ( GNT |-> $countones(MAIN_SHIFT_REG) != 0);
```

Auxiliary code locate in:
- verification components/checkers
- normal verilog modules with only inputs
  - v comps are only verification code
  - SV `bind` statement
  - just instance and connect them where you need them, like UVC's in a UVM testbench
- in RTL, surrounded with `ifdef

Sometimes you need aux code to do global counting. No way to write this in just SVA... like for handeshake solution with max number of requests.

**Cover statements?**


## Formal analysis
### Terminology
- Invariant
- Temporal sequence
- Property
- verification directives
- assert
- assume/assumption
- cover: possible to be true
- restrict: readability intent statement, =assume

Property can be SVA, PSL or OVL.

Dynamic verification=Simulation

Jaspergold consider your design as a giant FSM. State on nect clock deoends only on inputs, current state and the design.

How many states? mostly << 2^(no. of FFs)

Unreachable states may depend on the initial state. So we need to inform Jaspergold what the initial state is.

- state space
- reachable state space
- diameter = how many steps from initial state to all reachable states

Assertions pass if they are true in all reachable states

- invariant assertion
- temporal assertion

assume property is not assert property

### over-constrained environment
unintentionally reduced state space bew legal and desired state space

### COI =cone of influence

### JG Expert system



# UVM
## SVA
s1 |-> s2: 如果s1 是match的， 那么s2也必然要match； 如果s1不匹配， 那么这个squence的结果就是true
s1 |=> s2: 代表的是s1在下一个clock在进行判断

## Constrained random verification
automate stimulus generation

constrains: header payload checksum?

driver - scoreboard - printer

Our verification environments will use the following principles
1. Constrained random stimulus
2. Functional coverage
3. Layered testbench using transactors
4. Common testbench for all tests
5. Test-specific code kept separate from testbench


## Steps for a counter
1. write tb
2. add uvm_test
3. add uvm_env
4. add if
5. build the testbench in a task env (like a dirver)
6. agent: connect env to dut
   1. uvm_config_db
   2. my_dut_config
   3. agent
      1. my_driver
      2. my_sequencer
      3. my_monitor
   4. transaction
   5. sequencer
   6. sequence
7. package my_sequences;



# Q
- What's the difference between main_phase and run_phase