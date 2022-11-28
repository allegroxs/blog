# Introduction
This is a review note for one of my courses.

Network-on-Chip


## L1 Many-core

### Trend:
single-core ===> multi-core  ===> many-cores microprocessors
MPSoC

Why is it happening? 
- Moore’s law
- Dennard scaling:
  - Same area, double transistors (smaller and faster), same power, same cost.
  - VDD (no more) scaling: The CMOS power crisis
  - Why CPU frequency stalled?
    - • Power consumption
    - • Heat dissipation
- Pollack’s rule : Microprocessor’s performance grows with the square root of area (power, if proportional to area)

### Bus
Properties
- Serialization
- Broadcast: send a message to other components without an extra cost


Bus Transmit: ET (Enable Transmit) active

Bus Receive: ER (Enable Receive) active

Message: Logical unit of information transferred from a transmitter to a set of receivers (e.g. a read message contains an address and control signals for read)

Cycles: A message requires a number of cycles to be sent from sender to receiver over the bus

Transaction: A transaction consists of a sequence of messages that are causally related (e.g. a memory read requires a memory read message and a reply with the requested data)


Synchronous Bus

- Advantage: involves very little logic and can run very fast
- Disadvantages: Every device on the bus must run at the same clock rate. To avoid clock skew, wires cannot be long for fast operation.

Asynchronous Bus
- requires accommodate a wide range of devices
- handshaking protocol
- lengthened without worrying about clock skew

To transmit a message
- Arbitration
- Addressing
- Data transfer
- Acknowledgement

memory read transaction = 1 read cycle + 1 reply cycle

memory white transaction = 1 write address + 1 write data + 1 ack

### Bus Arbitration

Req --> Arbiter  

Grant <-- Arbiter


Centralized bus arbitration: A central arbiter collects the requests of all bus masters and gives only one module the right to access the bus (bus grant)  
Distributed arbitration: daisy-chain arbitration, which however incurs high delay of running carry chain across many modules.

Fairness is a key property of an arbiter

- Fixed-Priority Arbiter
- Fair Arbiters
- Oblivious Arbiter
- Round-Robin Arbiter
- Queuing Arbiter

### Bus Protocols

Category
- Low-performance bus protocol
  - Non-pipelined bus
- High-performance
  - Bus pipelining: hide arbitration overhead
  - Split-transaction bus: intend to eliminate bus idling time during memory read
  - Burst transfer

Bus Pipelining  

AR:   Arbitration Request  
ARB:  Arbitration  
AG:   Arbitration Grant  
**RQ**:   Put message (address/data) on Bus   
**ACK**:  Acknowledgement on bus   
**RPLY**: Replied data on bus
P:    Processing cycle, which - can vary in length. **block write rq**  

Split-Transaction Bus
- The advantages of the split-transaction bus are evident, if there is a variable delay for requests.
- But replies may be out of order. Need ”tag” to match sender/receiver. 

Burst Message: The overhead can be reduced, if messages are sent as blocks (bursts)

Case study: The PCI bus

## L2 Network Topology

Traditional bus interconnect & Traditional crossbar interconnect

- Topology (static arrangement of channels and nodes) 
  - The network topology refers to the static arrangement of channels and nodes in the network.
- Routing Techniques (selection of a path through the network)
- Switching Techniques (How a route is traversed)
- Flow Control (how are network resources allocated, when packets traverse the network)
- Router Architecture (buffers and crossbar)
- Traffic Pattern




- Channel bisection
        BC = 4 (2 bidirectional channels go through the bisection)
- Bandwidth bisection
        BB = 4b (b is the bandwidth of each channel


The ideal throughput is defined as the throughput assuming a perfect routing and flow control  
        Perfect routing: Load is balanced over alternate paths
        Perfect flow control: No idle cycles on bottleneck channels

The channel load (γ) of a channel is the amount of traffic that must cross the channel (or bisection channel)

Bisection bound of N/2Bc only apply under uniform traffic

hop count boubd of HminN/C under any traffic but perfect balance on all channels

### Latency

Head latency Th : Time required for the head of a message to traverse the network
Serialization latency Ts= L/b : Time required for the tail to catch up (time for a message of length L to cross a channel with bandwidth b)
Tr (time spent in the routers) 
Tw (time spent on wires)
ideal average latency: T0 = Hmin tr + Dmin / v + L / b (no congestion)

# L3 Routing Algorithms and Mechanics

Properties
- In terms of adaptivity: Deterministic, oblivious, adaptive
- In terms of shortest path: Minimal, non-minimal

Dimension-Order Routing in Tori and Meshes( Also called e-cube routing

backpressure

## Performance analysis

- Greedy algorithm
- Uniform random algorithm
- Weighted random algorithm
- Adaptive routing algorithm

## Routing Mechanics
- Source-table routing
- Node-table routing 


# L4 Flow control
Resources and allocation units
- Network-level flow control
  - Bufferless flow control
    - dropped: protocol that informs the sending node that the packet has been dropped
      - N acks
      - Retransmission. timeout
    - misrouted
    - Circuit switching
      - two weaknesses:
        - High zero-load (non-contention) latency: T = 3 H tr + L/b
        - Low throughput, 


  - Buffered flow control:   Packets that cannot be routed via the desired channel are stored in buffers
    - Packet-Buffer Flow Control
      - Store-And-Forward (SAF)
        - Packet-sized buffer in the switch
        - Exclusive use of the outgoing channel during the full packet transmission period.
        - Very high latency
      - (Virtual) Cut-Through
        - reduces the latency; Very high channel utilization
        - Not so good utilization of buffers, requiring packet-sized buffers; Contention latency is increased, since packets must wait until a whole packet leaves the occupied channel.
    - Flit-Buffer Flow Control
      - Wormhole flow control
      - Virtual Channel (VC) flow control
- Link-level (switch-to-switch) flow control
      - Credit-Based Flow Control
      - On/off Flow control :tries to reduce the amount of upstream signaling
      - Ack/Nack Flow Control

RI: Routing info.  
SN: Sequence number

4 types: Head flit, body flit, tail flit, head/tail flit.  
A flit (flow control digits)  
A phit (physical transfer digits) 

    Typical values
      Phits: 1 bit to 64 bits (8B)  
      Flits: 16 bits (2B) to 512 bits (64B)  
      Packets: 128 bits (16B) to 1024 bits (128B)  

The minimum time between the credit being sent at time t1 and a credit being sent for the same buffer at time  t5 is the credit round-trip delay tcrt

where Lf is the length of a flit in bits


# L5 Router Architecture

- Wormhole router ( (one virtual channel (VC) per physical channel))
- __Virtual channel router__

- functional blocks
  - Data path
    - Input buffers
    - Switch (crossbar)
    - Output buffers
  - Control Plane
    - Route Computation (RC)
    - VC Allocator (VA)
    - Switch Allocator (SA)


Canonical 4-stage pipeline  
 RC  
 VA  
 SA   
 ST    (Switch Traversal)
WT(wire/link traversal)

- Packet Rate
  - Route computation
  - Virtual-channel (VC) allocation
- Flit Rate
  - Switch allocation
  - Pointer and credit count update


- Pipeline Stalls
  - Packet stalls
  - Flit stalls


The credit loop can be viewed by means of a token that

## Network Interface

- Message passing: symmetric
- Shared memory: un-symmetric


# L6 Deadlock and Livelock

Network deadlock vs. Protocol deadlock

solutions
- Deadlock avoidance [*]
- Deadlock recovery

