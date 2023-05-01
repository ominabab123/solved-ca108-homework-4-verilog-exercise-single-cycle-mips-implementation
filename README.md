Download Link: https://assignmentchef.com/product/solved-ca108-homework-4-verilog-exercise-single-cycle-mips-implementation
<br>



<h1>1. Introduction</h1>

Microprocessor without Interlocked Pipeline Stages (MIPS) is a widely used instruction set architecture. We’ve already had comprehensive understandings of it in our Computer Architecture course. In this exercise, we’re going to implement the single-cycle MIPS architecture using Verilog. This exercise will give you a hardware viewpoint to this architecture.

The MIPS architecture is shown in Fig. 1. In this exercise, the instruction memory and data memory are implemented in the testbench. Except the memories, you need to implement everything else by yourself.




Figure1: Single-cycle MIPS architecture.

<strong>             </strong>

<h1>2. Specification</h1>

The input/output pins are defined in Table. 1. The required instructions you need to support in the baseline are listed in Table. 2. The complete machine codes for the required instructions are not listed in this document. Please refer to your textbook or MIPS Green Sheet for the full machine code, our testbench follows the standard machine code rules. (We also provide you MIPS Green Sheet with marker on the instructions that we asked you to do. The password is same as course slides)




Tabel1: I/O pins specification

<table width="553">

 <tbody>

  <tr>

   <td width="114"><strong>Signal name </strong></td>

   <td width="46"><strong>I / O </strong></td>

   <td width="76"><strong>Bit width </strong></td>

   <td width="317"><strong>Description </strong></td>

  </tr>

  <tr>

   <td width="114"><strong>clk </strong></td>

   <td width="46">I</td>

   <td width="76">1</td>

   <td width="317">Clock signal.Positive edge trigger.</td>

  </tr>

  <tr>

   <td width="114"><strong>rst_n </strong></td>

   <td width="46">I</td>

   <td width="76">1</td>

   <td width="317">Active low <strong>synchronous</strong> reset signal.</td>

  </tr>

  <tr>

   <td width="114"><strong>IR_addr </strong></td>

   <td width="46">O</td>

   <td width="76">32</td>

   <td width="317">Output address of the instruction memory.</td>

  </tr>

  <tr>

   <td width="114"><strong>IR </strong></td>

   <td width="46">I</td>

   <td width="76">32</td>

   <td width="317">Instruction read from instruction memory</td>

  </tr>

  <tr>

   <td width="114"><strong>ReadDataMem </strong></td>

   <td width="46">I</td>

   <td width="76">32</td>

   <td width="317">Signals from data memory.</td>

  </tr>

  <tr>

   <td width="114"><strong>CEN </strong></td>

   <td width="46">O</td>

   <td width="76">1</td>

   <td width="317">Chip-enable; set it low to read/write data.</td>

  </tr>

  <tr>

   <td width="114"><strong>WEN </strong></td>

   <td width="46">O</td>

   <td width="76">1</td>

   <td width="317">Write-enable; set it low to write data into memory.</td>

  </tr>

  <tr>

   <td width="114"><strong>A </strong></td>

   <td width="46">O</td>

   <td width="76">7</td>

   <td width="317">Address for data memory.</td>

  </tr>

  <tr>

   <td width="114"><strong>Data2Mem </strong></td>

   <td width="46">O</td>

   <td width="76">32</td>

   <td width="317">Registers output signal.</td>

  </tr>

  <tr>

   <td width="114"><strong>OEN </strong></td>

   <td width="46">O</td>

   <td width="76">1</td>

   <td width="317">Read-enable; set it low to read data from memory.Note that OEN should be the inverse of WEN.</td>

  </tr>

 </tbody>

</table>




Table 2: Your MIPS needs to support the instructions in this table to pass baseline.

<table width="453">

 <tbody>

  <tr>

   <td width="114"><strong>Instrucion </strong></td>

   <td width="46"><strong>Type </strong></td>

   <td width="113"><strong>Op code (Hex) </strong></td>

   <td width="179"><strong>Func code (Hex) </strong></td>

  </tr>

  <tr>

   <td width="114"><strong>sll </strong></td>

   <td width="46">R</td>

   <td width="113">0</td>

   <td width="179">00</td>

  </tr>

  <tr>

   <td width="114"><strong>srl </strong></td>

   <td width="46">R</td>

   <td width="113">0</td>

   <td width="179">02</td>

  </tr>

  <tr>

   <td width="114"><strong>add </strong></td>

   <td width="46">R</td>

   <td width="113">0</td>

   <td width="179">20</td>

  </tr>

  <tr>

   <td width="114"><strong>sub </strong></td>

   <td width="46">R</td>

   <td width="113">0</td>

   <td width="179">22</td>

  </tr>

  <tr>

   <td width="114"><strong>and </strong></td>

   <td width="46">R</td>

   <td width="113">0</td>

   <td width="179">24</td>

  </tr>

  <tr>

   <td width="114"><strong>or </strong></td>

   <td width="46">R</td>

   <td width="113">0</td>

   <td width="179">25</td>

  </tr>

  <tr>

   <td width="114"><strong>slt </strong></td>

   <td width="46">R</td>

   <td width="113">0</td>

   <td width="179">2A</td>

  </tr>

  <tr>

   <td width="114"><strong>addi </strong></td>

   <td width="46">I</td>

   <td width="113">8</td>

   <td width="179">N/A</td>

  </tr>

  <tr>

   <td width="114"><strong>lw </strong></td>

   <td width="46">I</td>

   <td width="113">23</td>

   <td width="179">N/A</td>

  </tr>

  <tr>

   <td width="114"><strong>sw </strong></td>

   <td width="46">I</td>

   <td width="113">2B</td>

   <td width="179">N/A</td>

  </tr>

  <tr>

   <td width="114"><strong>beq </strong></td>

   <td width="46">I</td>

   <td width="113">4</td>

   <td width="179">N/A</td>

  </tr>

  <tr>

   <td width="114"><strong>bne </strong></td>

   <td width="46">I</td>

   <td width="113">5</td>

   <td width="179">N/A</td>

  </tr>

  <tr>

   <td width="114"><strong>j </strong></td>

   <td width="46">J</td>

   <td width="113">2</td>

   <td width="179">N/A</td>

  </tr>

  <tr>

   <td width="114"><strong>jal </strong></td>

   <td width="46">J</td>

   <td width="113">3</td>

   <td width="179">N/A</td>

  </tr>

  <tr>

   <td width="114"><strong>jr </strong></td>

   <td width="46">R</td>

   <td width="113">0</td>

   <td width="179">8</td>

  </tr>

 </tbody>

</table>

<strong> </strong>

<h1>3. Timing Diagram for Memory</h1>

In the MIPS architecture shown in Fig. 1, there are two memories needed. Here we use a ROM for the instruction memory. As soon as you give it an address, the instruction memory sends the instruction immediately.

As for data memory, we use a simulated SRAM for it in this exercise. Fig. 2 shows the timing diagram for reading data from a memory. Fig. 3 shows the timing diagram for writing data into a memory.




Figure2: Read data from a memory.




Figure3: Write data from a memory.




<h1>4. Floating-Point Unit</h1>

For more advanced exercise, you can add some function to your MIPS to support some floating-point instructions. You need to extend from your baseline code, adding new registers $FR0 – $FR31 to store floating-point numbers ($FR0 is not always 0 !).

Since some floating-point operations are too complex to implement, you have to use DesignWare modules to accomplish this part. DesignWare modules are stored in the following directory.




/usr/cad/synopsys/synthesis/cur/dw/sim_ver/




During RTL Simulation, include these modules by adding the following in command line :

-y /usr/cad/synopsys/synthesis/cur/dw/sim_ver/  

+libext+.v +incdir+/usr/cad/synopsys/synthesis/cur/dw/sim_ver/




Table 3 are the instructions that will be tested in our testbench, where double precision instructions are marked with darker background. You can also just do single precision part to get partial points.




Table 3: floating-point instructions that your MIPS have to support

<table width="553">

 <tbody>

  <tr>

   <td width="106"><strong>Instrucion </strong></td>

   <td width="46"><strong>Type </strong></td>

   <td width="113"><strong>Op code (Hex) </strong></td>

   <td width="130"><strong>FMT(Hex) </strong></td>

   <td width="158"><strong>Func code (Hex) </strong></td>

  </tr>

  <tr>

   <td width="106"><strong>add.s </strong></td>

   <td width="46">FR</td>

   <td width="113">11</td>

   <td width="130">10</td>

   <td width="158">00</td>

  </tr>

  <tr>

   <td width="106"><strong>sub.s </strong></td>

   <td width="46">FR</td>

   <td width="113">11</td>

   <td width="130">10</td>

   <td width="158">01</td>

  </tr>

  <tr>

   <td width="106"><strong>mul.s </strong></td>

   <td width="46">FR</td>

   <td width="113">11</td>

   <td width="130">10</td>

   <td width="158">02</td>

  </tr>

  <tr>

   <td width="106"><strong>div.s </strong></td>

   <td width="46">FR</td>

   <td width="113">11</td>

   <td width="130">10</td>

   <td width="158">03</td>

  </tr>

  <tr>

   <td width="106"><strong>lwcl </strong></td>

   <td width="46">I</td>

   <td width="113">31</td>

   <td width="130">N/A</td>

   <td width="158">N/A</td>

  </tr>

  <tr>

   <td width="106"><strong>swcl </strong></td>

   <td width="46">I</td>

   <td width="113">39</td>

   <td width="130">N/A</td>

   <td width="158">N/A</td>

  </tr>

  <tr>

   <td width="106"><strong>c.eq.s </strong></td>

   <td width="46">FR</td>

   <td width="113">11</td>

   <td width="130">10</td>

   <td width="158">32</td>

  </tr>

  <tr>

   <td width="106"><strong>bclt </strong></td>

   <td width="46">FI</td>

   <td width="113">11</td>

   <td width="130">8</td>

   <td width="158">N/A</td>

  </tr>

  <tr>

   <td width="106"><strong>add.d</strong></td>

   <td width="46">FR</td>

   <td width="113">11</td>

   <td width="130">11</td>

   <td width="158">00</td>

  </tr>

  <tr>

   <td width="106"><strong>sub.d</strong></td>

   <td width="46">FR</td>

   <td width="113">11</td>

   <td width="130">11</td>

   <td width="158">01</td>

  </tr>

  <tr>

   <td width="106"><strong>ldcl</strong></td>

   <td width="46">I</td>

   <td width="113">35</td>

   <td width="130">N/A</td>

   <td width="158">N/A</td>

  </tr>

  <tr>

   <td width="106"><strong>sdcl</strong></td>

   <td width="46">I</td>

   <td width="113">3D</td>

   <td width="130">N/A</td>

   <td width="158">N/A</td>

  </tr>

 </tbody>

</table>

<strong> </strong>