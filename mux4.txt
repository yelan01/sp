.lib '~/cic018.l' tt

.subckt inv in out vdd vss
M1 out in vdd vdd P_18 W=2u L=180n
M0 out in vss vss N_18 W=1u L=180n
.ends inv

.subckt and2 a b y vdd vss
M1 n1 a vdd vdd P_18 W=2u L=180n
M2 n1 b vdd vdd P_18 W=2u L=180n
M3 n1 a mid vss N_18 W=1u L=180n
M4 mid b vss vss N_18 W=1u L=180n
M5 y n1 vdd vdd P_18 W=2u L=180n
M6 y n1 vss vss N_18 W=1u L=180n
.ends and2

.subckt or2 a b y vdd vss
M1 mid a vdd vdd P_18 W=2u L=180n
M2 n1 b mid vdd P_18 W=2u L=180n
M3 n1 a vss vss N_18 W=1u L=180n
M4 n1 b vss vss N_18 W=1u L=180n
M5 y n1 vdd vdd P_18 W=2u L=180n
M6 y n1 vss vss N_18 W=1u L=180n
.ends or2

.subckt mux4 d0 d1 d2 d3 s0 s1 y vdd vss
Xinv0 s0 ns0 vdd vss inv
Xinv1 s1 ns1 vdd vss inv

Xand00 d0 ns0 t00 vdd vss and2
Xand01 t00 ns1 term0 vdd vss and2

Xand10 d1 s0 t10 vdd vss and2
Xand11 t10 ns1 term1 vdd vss and2

Xand20 d2 ns0 t20 vdd vss and2
Xand21 t20 s1 term2 vdd vss and2

Xand30 d3 s0 t30 vdd vss and2
Xand31 t30 s1 term3 vdd vss and2

Xor01 term0 term1 y01 vdd vss or2
Xor23 term2 term3 y23 vdd vss or2
XorF y01 y23 y vdd vss or2
.ends mux4


Vdd vdd 0 DC 1.8
Vss vss 0 DC 0

Vs0 s0 0 pulse(0 1.8 0 0.1n 0.1n 0.5u 1u)
Vs1 s1 0 pulse(0 1.8 0 0.1n 0.1n 1u 2u)

Vd0 d0 0 DC 0
Vd1 d1 0 DC 1.8
Vd2 d2 0 pulse(0 1.8 0 0.1n 0.1n 0.25u 0.5u)
Vd3 d3 0 pulse(1.8 0 0 0.1n 0.1n 0.25u 0.5u)

Xmux d0 d1 d2 d3 s0 s1 y vdd vss mux4

.option post
.tran 0.01u 4u
.end
