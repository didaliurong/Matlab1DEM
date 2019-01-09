# Matlab1DEM
Brief descriptions of the Matlab codes included in this package are given below. 
Most of the routines have extensive header comments describing their specific usage, 
so go there for further details. These routines do not use any special Matlab toolboxes. 
The only possible requirement is that you have a version of Matlab which supports variable-precision arithmetic. 

The  	QWE algorithm is based on the FHTvsQWE by Key 2012
Key, K., 2012, Is the fast Hankel transform faster than quadrature? : 
Geophysics, 77, no. 3, F21-F30, doi: 10.1190/geo2011-0237.1.

Rong Liu, Zhuo Liu, Jianxin Liu, Rongwen Guo
School of Geosciences and Info-Physics, Central South University
liurongkaoyan@126.com,zhuoliu@mymail.mines.edu,ljx6666@126.com, rongwenguo@csu.edu.cn
August 2018


The algorithm is formulated closely related to the Section 2. We choose the marineCSEM_Current.m 
which used in 3.3. Application to get the electrical response results of x-directed electric dipole 
source (E_XED) as example to illustrate the program.

**********************************Input and output**********************************
%%background model
%input*************model description  *******************
bc=[1d-12,   3.3                1]; %conducutivity from topmost layer to deepest layer
h=[1d60,    1000             1d60];%thickness from topmost layer to deepest layer
z=[      0       1000            ];%interface 
%*************design the observation point *******************
TX=[0,0,950];                      %position of the transmitter
T0=2;                             % the transmitter is the second layer 
[Tmodel0,TmodelM,TmodelN]= select_model(T0,bc,h,z); % Rearrange the order of layers 
f=1;                              %frequency of the transmitter
%positions of the receivers 
xxB=-8200:400:8200;
yyB=0;
zzB=0.1:200:4000.1;
P=length(xxB)*length(yyB)*length(zzB);
PS=zeros(P,3);
for i=1:length(xxB)
 for j=1:length(yyB)
     for k=1:length(zzB)
        m=(i-1)*length(yyB)*length(zzB)+(j-1)*length(zzB)+k;
        PS(m,1)=xxB(i);
        PS(m,2)=yyB(j);
        PS(m,3)=zzB(k);
     end
 end
end%end input*************

The model description are inputs for the program, we can obtain the electric response of 
x-directed unit electric dipole by arbitrary Tx-Rx geometries by modifying the transmitter 
and receiver positions. Meanwhile, we can obtain the response on a large number of 1D models
at different frequencies by varying the conductivity, thickness, interface and frequency. 
In the marineCSEM_Current.m, we get two output files: 1Hzmarine 1D electric vector in background model.txt 
and 1Hzmarine 1D electric vector in reservoir model.txt by varying the 1D model.

**********************************Vector potential in layers**********************************
CAL_EDabcd.m returns the coefficients of the potential in each layer caused by unit electric dipole
located at arbitrary layer. For unit electric dipole located at the bottom layer CAL_EDabcd.m calls 
CAL_EDabcdM0.m. For the unit electric dipole located at the bottom layer CAL_EDabcd.m calls POTENTIAL_EDabcdN0.m.

**********************************The electric fields by QWE**********************************
E_XED.m: the relTol (α), absTol(β), nQuad(m) and nIntervalsMax(n) are accuracy 
control factors for the QWE evaluation in the 2.5. QWE for EM fields evaluation. 
Then, the function getBesselWeights.m returns the quadrature intervals and Bessel 
function weights for E_qweXED usage. 
E_qweXED.m: firstly, the expression of the EM fields with integrations of J0 and J1 
Bessel functions are obtained by calling the inline function getCSEM1DKernelsVX.m. 
Then, the convergence rates of the integrations are accelerated by the QWE algorithm.
