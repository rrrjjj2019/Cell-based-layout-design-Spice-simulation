<h1 align="center">
  <br>
    Introduction of Integrated Circuit Design - Final project
    Cell-based layout design & Spice simulation
  <br>
</h1>


## Project Goal
* Use inverter, NAND gate, OR gate design a multiplexer, full adder, and 3-bit carry select adder.
* After designing the layout, we need to pass Design Rule Check (DRC), as well as Layout Versus Schematic (LVS)
* Use spice simulation to extract timing information and verify functional correctness

## Layout
* 3-bit carry select adder
    - The design of our carry select adder is based on the picture below. The comparison of the cell is as follows.

<br>
<p align="center">
<img src="https://github.com/rrrjjj2019/Cell-based-layout-design-Spice-simulation/blob/master/carry_select_adder.JPG" width="500" style="margin-right:5px; border: 1px solid #ccc;" />
</p>
<br>

<br>
<p align="center">
<img src="https://github.com/rrrjjj2019/Cell-based-layout-design-Spice-simulation/blob/master/carry_select_adder_layout.JPG" width="500" style="margin-right:5px; border: 1px solid #ccc;" />
</p>
<br>

## Spice simulation - input vector waveform

<br>
<p align="center">
<img src="https://github.com/rrrjjj2019/Cell-based-layout-design-Spice-simulation/blob/master/spice_simulation1.JPG" width="500" style="margin-right:5px; border: 1px solid #ccc;" />
</p>
<br>

<br>
<p align="center">
<img src="https://github.com/rrrjjj2019/Cell-based-layout-design-Spice-simulation/blob/master/spice_simulation2.JPG" width="500" style="margin-right:5px; border: 1px solid #ccc;" />
</p>
<br>

## DRC summary report

<br>
<p align="center">
<img src="https://github.com/rrrjjj2019/Cell-based-layout-design-Spice-simulation/blob/master/DRC.JPG" width="500" style="margin-right:5px; border: 1px solid #ccc;" />
</p>
<br>

## LVS summary report

<br>
<p align="center">
<img src="https://github.com/rrrjjj2019/Cell-based-layout-design-Spice-simulation/blob/master/LVS.JPG" width="500" style="margin-right:5px; border: 1px solid #ccc;" />
</p>
<br>

## LVS schematic
.subckt ADDER3 A0 A1 A2 B0 B1 B2 CIN S0 S1 S2 COUT VDD GND
x_fa1_fa1 A0 B0 GND cout_up1 _S0 VDD GND FA
x_fa2_fa2 A1 B1 cout_up1 cout_up2 _S1 VDD GND FA
x_fa3_fa3 A2 B2 cout_up2 cout_up3 _S2 VDD GND FA
x_fa4_fa4 A0 B0 VDD cout_down1 __S0 VDD GND FA
x_fa5_fa5 A1 B1 cout_down1 cout_down2 __S1 VDD GND FA
x_fa6_fa6 A2 B2 cout_down2 cout_down3 __S2 VDD GND FA
x_mux1_mux1 __S0 _S0 CIN S0 VDD GND MUX21
x_mux2_mux2 __S1 _S1 CIN S1 VDD GND MUX21
x_mux3_mux3 __S2 _S2 CIN S2 VDD GND MUX21
x_mux4_mux4 cout_up3 cout_down3 CIN COUT VDD GND MUX21
.ends and

.subckt FA a b c_in c_out sum VDD GND
x_inv1_inv1 a a_bar VDD GND INV_2X
x_nand1_nand1 a_bar b out1 VDD GND NAND_2X
x_inv2_inv2 b b_bar VDD GND INV_2X
x_nand2_nand2 b_bar a out2 VDD GND NAND_2X
x_nand3_nand3 out1 out2 out3 VDD GND NAND_2X
x_inv3_inv3 out3 out3_bar VDD GND INV_2X
x_nand4_nand4 c_in out3_bar out4 VDD GND NAND_2X
x_inv4_inv4 c_in c_in_bar VDD GND INV_2X
x_nand5_nand5 c_in_bar out3 out5 VDD GND NAND_2X
x_nand6_nand6 out4 out5 sum VDD GND NAND_2X
x_nand7_nand7 a b nand_ab VDD GND NAND_2X
x_inv5_inv5 nand_ab nand_ab_bar VDD GND INV_2X
x_nand8_nand8 a c_in nand_acin VDD GND NAND_2X
x_inv6_inv6 nand_acin nand_acin_bar VDD GND INV_2X
x_nand9_nand9 b c_in nand_bcin VDD GND NAND_2X
x_inv7_inv7 nand_bcin nand_bcin_bar VDD GND INV_2X
x_nor1_nor1 nand_ab_bar nand_acin_bar nor1 VDD GND NOR_2X
x_inv8_inv8 nor1 nor1_bar VDD GND INV_2X
x_nor2_nor2 nor1_bar nand_bcin_bar nor2 VDD GND NOR_2X
x_inv9_inv9 nor2 c_out VDD GND INV_2X
.ends and

.subckt MUX21 in1 in2 ctrl out VDD GND
x_inv1_inv1 ctrl ctrl_bar VDD GND INV_2X
x_nand1_nand1 ctrl_bar in1 out1 VDD GND NAND_2X
x_nand2_nand2 in2 ctrl out2 VDD GND NAND_2X
x_nand3_nand3 out2 out1 out VDD GND NAND_2X
.ends and

.subckt INV_2X IN OUT VDD GND
mp1 OUT IN VDD VDD P_18 w=0.67u l=0.18u
mn1 OUT IN GND GND N_18 w=0.67u l=0.18u
.ends

.subckt NAND_2X IN1 IN2 OUT VDD GND
mp1 OUT IN1 VDD VDD P_18 w=0.67u l=0.18u
mp2 OUT IN2 VDD VDD P_18 w=0.67u l=0.18u
mn1 NET IN1 GND GND N_18 w=0.67u l=0.18u
mn2 OUT IN2 NET GND N_18 w=0.67u l=0.18u
.ends

.subckt NOR_2X IN1 IN2 OUT VDD GND
mp1 OUT IN1 NET VDD P_18 w=0.67u l=0.18u
mp2 NET IN2 VDD VDD P_18 w=0.67u l=0.18u
mn1 OUT IN1 GND GND N_18 w=0.67u l=0.18u
mn2 OUT IN2 GND GND N_18 w=0.67u l=0.18u
.ends
