### 复旦微
- FPGA工程师
  
    1、微电子、电子工程、计算机等相关专业硕士及以上学历；
    2、熟悉**FPGA开发流程**，熟悉VerilogHDL/**VHDL**，熟悉**C语言**；
    3、能熟练使用**示波器**等仪器进行调试和测试；
    4、有**硬件电路原理图**识图能力"

    负责或参与芯片**测试系统**FPGA逻辑的设计开发工作及相关项目的芯片测试分析工作。

- 数字前端工程师
  
    1、电子学、微电子学或相关专业硕士及以上学历；
    2、具备数字电路知识，了解**半导体物理、半导体器件**知识；
    3、了解数字**前端设计EDA**工具、方法和流程，会使用**verilog**，**shell**，**SystemVerilog**等语言；
    4、熟悉UNIX、Windows、Office等软件的使用；
    5、具有良好的沟通、学习能力，具备较强的逻辑分析能力。

    1、参与芯片设计项目中数字电路的设计开发，实现芯片产品对数字电路的功能、性能要求；
    2、进行RTL设计，并编写芯片设计，验证相关文档；
    3、为内外部客户提供芯片相关技术支持。
			
			
			
### Reasil
- 数字IC设计工程师

    1、电子、通信、计算机、微电子相关专业，硕士学历；
    2、有扎实的数字电路基础；
    3、熟悉**verilogHDL**，有数字前端设计经验或者FPGA设计经验优先；
    4、熟悉数字前端设计flow和相关EDA工具者优先；
    5、熟悉perl或**tcl或shell等**脚本语言，有makefile经验者优先；
    6、熟悉**网络通信相关协议**者优先。
    
    负责芯片的架构设计、算法设计、数字电路设计、验证，FPGA平台，芯片整合，芯片Tapeout，量产测试。

- 嵌入式软件开发工程师

1、计算机、通信、自动化及相关专业，硕士学历；
2、熟练掌握C语言和汇编语言，熟悉FreeRTOS/uCOS/等实时操作系统，具有良好的编程基础。
3、熟悉计算机体系架构和嵌入式开发技术，具有一定的硬件背景。
4、具有良好的团队合作能力、理解能力和沟通能力；
5、良好的英文技术文档阅读撰写能力。


# 2230 MLP
1. Serial neuron
   1. One-neuron design.Generic M × N MLP modeling (M layers, N neurons per layer) with one single neuron
unit (M, N are generic parameters). This means that the generic M × N MLP shallbe modeled with a single neuron unit, and the control path takes care of the repetitive computation using the single neuron to achieve the desired functionality of the neural network. This is one extreme case of fully serial implementation at the neuron level.
2. Semi neuron
   1. N-neuron design. Generic M × N MLP modeling (M layers, N neurons per layer) with N neuron units (M, N are generic). This can be viewed as another extreme that uses a parallel architecture (as many per-layer neurons as needed). Still you need to consider how to re-use the one-layer neurons to implement the M layers of neuron computations
1. parallel neuron
2. Nonlinear function
   1. relu
   2. sigmoid: n-segments fitting
3. DC_shell
   1.  Wire_load_mode is top. 
   2.  Operating condition is NCCOM from tcbn90gtc.



# 2203 Hardware description languages
1. ALU
   1. Opcode
2. Register File
   1. Test: patterns like “1010101” followed by “0101010” also checks if the bits are leaking to their neighbors)
3. Datapath
   1. Create a clock divider component. It should divide the 100 MHz clock on the board to an internal clock of approximately 1 Hz. The easiest way to achieve that is to make an incrementer (add 1), and use the MSB of it as the Clk output. 
   2.  add-1 test
4. FSM
   1. a Program Counter and a bypass mux for jump/branch instructions. The Program counter is either part of the registers in the register file (typically the last one), but it can also be a separate PC register, external to the register file. In our case, we will not be incrementing the PC externally, but use the last register (reg 7 in our case) for the PC
   2.  Microcontroller ROM for issuing microcode instructions
5. cpu
   1. Complete the Microcontroller FSM and add a R_Wn signal (or use two separate signals, RDEN and WREN) for controlling accesses to an external external memory. 
   2. ROM.  We complete the **Microcontroller ROM** and include R/W_n signals to control reading/writing from/to external memories. 
      1. Modelsim will convert your assembly program to hex-codes. Look on the RAM signal in the simulation window of Modelsim when you test the fake memory architecture, and write down the hex-codes in the memory.mif file. From now on, you should use the generated memory in your simulations so you get the memory latency right. 
   3. We also build a **General Purpose IO** unit for writing data to LEDS, and write the **program (assembly code)** that should be stored in the external memory. We then build a **testbench** to run the system with the assembly code in it to test it
   4. Finally, we download the Microcontroller component on the prototype FPGA board and verify that it works.



