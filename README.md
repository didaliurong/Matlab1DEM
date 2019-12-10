Release notes for MatlabCSEM1D

Rong Liu, email: liurongkaoyan@126.com
Jianxin Liu, email: ljx6666@126.com 
Jianxin Wang, email: jxwang@csu.edu.cn
Zhuo Liu, email: zhuoliu@mymail.mines.edu
Rongwen Guo*, email: rongwenguo@csu.edu.cn
Rong Liu and Jianxin Wang are with School of Computer Science and Engineering, Central South University, Changsha, 410083, China
Jianxin Liu and Rongwen Guo are with School of Geosciences and Info-Physics, Central South University, Changsha, 410083, China
Zhuo Liu is with Center for gravity, Electrical and Magnetic Studies, Department of Geophysics, Colorado School of Mines, Golden, CO 80401, USA

Brief descriptions of the Matlab codes included in this package are given below. Most of the routines have extensive header comments describing their specific usage, input, output and code structures, so go there for further details. These routines are running on Matlab 2016a and no specific Matlab toolboxes are used. The only requirement is that you have a version of Matlab which supports variable-precision arithmetic. 
The codes developed are used to calculate 1D EM responses due to electric and magnetic dipoles with different orientations, allowing for an arbitrary transmitter-receiver (Tx-Rx) geometry in the multi-layered earth. The examples in the manuscript are used to better explain how to use the code, and all the colorful results in the manuscript can be reproduced by the codes. Most of the routines have extensive comments describing their specific usage, input and output parameters and relationships with each other. 
	Basic variables
In the Matlab code, the layered model is described by the conductivity (bc), thickness (h) and interface position (z). The dipole source is described by the frequency (f) and position (TX), and the layer with the source is indicated by T0. The measurement point is described by position (PS).
	Basic functions
To calculate the response for a dipole source, we need the calculation of ecoefficiencies of the vector potential at measurement point and the integration of Bessel function in the explicit expression of EM field at measurement point by QWE.
2.1 Functions to calculate the EM response by unit dipole source：
E_XED.m     -calculate the electric fields for x-directed unit electric dipole.
E_ZED.m     -calculate the electric fields for z-directed unit electric dipole.
E_XMD.m     -calculate the electric fields for x-directed unit magnetic dipole.
E_ZMD.m     -calculate the electric fields for z-directed unit magnetic dipole.
H_XED.m     -calculate the magnetic fields for x-directed unit electric dipole.
H_ZED.m     -calculate the magnetic fields for z-directed unit electric dipole.
H_XMD.m     -calculate the magnetic fields for x-directed unit magnetic dipole.
H_ZMD.m     -calculate the magnetic fields for x-directed unit magnetic dipole.
2.2 Functions to obtain coefficients of vector potential：
CAL_EDabcdpq.m   -For both horizontal and vertical electric dipoles, return the coefficients of vector potentials in each layer. 
CAL_MDabcdpq.m   -For both horizontal and vertical magnetic dipoles, return the coefficients of vector potentials in each layer.
The makeED_M0.m and makeED_N0.m are called by CAL_EDabcdpq.m to return the coefficients of vector potential for electric dipole in the bottom layer and upmost layer, respectively. makeMD_M0.m and makeMD_N0.m are called by CAL_MDabcdpq.m to return the coefficients of vector potential for magnetic dipole in the bottom layer and upmost layer, respectively.
2.3 Functions to calculate Bessel integration by QWE：
E_qweXED.m   - return the electric fields for x-directed electric dipole source by QWE .   
E_qweZED.m   - return the electric fields for z-directed electric dipole source by QWE.
E_qweXMD.m   - return the electric fields for x-directed magnetic dipole source by QWE.
E_qweZMD.m   - return the electric fields for z-directed magnetic dipole source by QWE.
H_qweXED.m   - return the magnetic fields for x-directed electric dipole source by QWE.
H_qweZED.m   - return the magnetic fields for z-directed electric dipole source by QWE.
H_qweXMD.m   - return the magnetic fields for x-directed magnetic dipole source by QWE.
H_qweZMD.m   - return the magnetic fields for z-directed magnetic dipole source by QWE.
These functions are modified from Key (2012, FFTvsQWE), in which getBesselWeights.m is called to return the quadrature intervals and Bessel function weights. 
	Main programs
Here, we show the main programs associate with each colorful picture in the manuscript, the details including input, output, structure and running time.
3.1 Validation
3.1.1 Swidinsky_validation.m
Swidinsky_validation.m is used to calculate vertical current density and magnetic density kernels at different frequencies and compare with published results as in figure 2.
Input:
bc, h, z                 -layered model description.
TX, T0, ff               -parameters of dipole source. 
PS                     -position of the measurement point.
image11, image12, image21, image22, image31, image32, real11, real12, real21, real22, real31, real32
-the data from Swidinsky et al. (2018).
Output:
Jx, Jy, Jz                -the electric current density in x-, y- and z- direction.
Bx, By, Bz              -the magnetic density in x-, ¬y- and z- direction .
Code structure:
call: slect_model   -to rearrange the layered model parameters according to the position of the dipole source.
call:CAL_EDabcdpq_validation_vpa   -use the variable-precision floating-point arithmetic (VPA) to evaluate the coefficient with accuracy more than default 32 digits.
call: validation_E  -to return the value of kernel function (electric field) for each lambda.
call: validation_H  -to return the value of kernel function (magnetic field) for each lambda.
Running time:
47.5 seconds.
3.1.2 Reid_validationXMD.m and Reid_validationZMD.m
Reid_validationXMD.m and Reid_validationZMD.m are used to calculate both the analytical and numerical solutions for a thin sheet model for x-directed magnetic dipole and z-directed magnetic dipole, respectively. Then, plot_validation_Reid.m is used to plot figure 3.  
Input:
bc, h, z                -layered model description.
TX, T0, ff              -parameters of dipole source. 
xxP, yyP, zzP            -position of the elements discretized from the thin sheet.
RC           -position of the measurement point for both analytical and numerical solutions.
Output:
XEx, XEy, XEz      -the electric field at the center of each elements for magnetic dipole source.
AHZ                -the magnetic field at RC by unite vector electric dipole in each element.
PZHx11, PZHy, PZHz  -the analytical magnetic field for magnetic dipole in the three layered model.
PZHz0               -the analytical magnetic field for magnetic dipole in whole space model.
Code structure:
call: slect_model     -to rearrange the layered model parameters according to the position of the dipole source.
If run Reid_validationXMD.m, firstly calculate the numerical solution.
call: E_XMD        -to return the electric fields in each element for x-directed magnetic dipole.
call: wholeHX       -to calculate the numerical solution for x-directed secondary field at RC by the currents in each element.
Then, calculate the analytical solution
call: H_XMD        -to calculate the analytical field at RC for x-directed magnetic dipole on the three-layered model.
call: XH_whole      -to calculate the analytical field at RC for x-directed magnetic dipole on whole space model.
If run Reid_validationZMD.m, firstly calculate the numerical solution.
call: E_ZMD        -to return the electric fields in each element by z-directed magnetic dipole.
call: wholeHZ       -to calculate the numerical solution of z-directed secondary field at RC by the currents in each element.
Then, calculate the analytical solution.
call: H_ZMD        -to calculate the analytical field at RC for z-directed magnetic dipole on three layered model.
call: ZH_whole       -to calculate the analytical field at RC for z-directed magnetic dipole on whole space model.
Running time:
158535.5 seconds.
3.2 Experiments
example_XED_Vector.m, example_ZED_Vector.m, example_XMD_Vector.m and example_ZMD_Vector.m are used to test our code on a 7-layer model. Then, plot_example_XED_Vector.m, plot_example_ZED_Vector.m, plot_example_XMD_Vector.m and plot_example_ZMD_Vector.m are used to plot Figure 5, 6, 7 and 8, respectively.
Input:
bc, h, z                -layered model description.
TX, T0, f              -parameters of dipole source.
xxB, yyB, zzB          -position of the measurement points along bisector x=y.
Output:
XEx, XEy, XEz       -the electric field at measurement points.
XHx, XHy, XHz      -the magnetic field at measurement points.
Code structure:
call: slect_model      -to rearrange the layered model parameters according to the position of the dipole source.
If example_XED_Vector.m
call: E_XED         -to return the electric fields for x-directed electric dipole.
call: H_XED         -to return the magnetic fields for x-directed electric dipole.
If example_ZED_Vector.m
call: E_ZED         -to return the electric fields for z-directed electric dipole.
call: H_ZED         -to return the magnetic fields for z-directed electric dipole.
If example_XMD_Vector.m
call: E_XMD         -to return the electric fields for x-directed magnetic dipole.
call: H_XMD         -to return the magnetic fields for x-directed magnetic dipole.
If example_ZMD_Vector.m
call: E_ZMD         -to return the electric fields for z-directed magnetic dipole.
call: H_ZMD         -to return the magnetic fields for z-directed magnetic dipole.
Running time:
845.7 seconds.
3.3 Application
3.3.1 marineCSEM_Analysis.m
marineCSEM_Analysis.m is used to calculate the electric field for inline system based on marine models to examine the EM responses at different Rx depths close to the seafloor. The receivers are placed at 0.1 m above and beneath the seafloor. Then, plot_marineCSEM_Analysis.m is used to plot figure 9.
Input:
bc1, h1, z1                -1D reservoir model description.
bc2, h2, z2                -lD background model description.
TX, T0, f                  -parameters of dipole source.
RX                       -position of the measurement points for the inline system.
Output:
XEx,XEy,XEz      -the electric field at measurement points for x-directed electric dipole source.
Code structure:
call: select_model     -to rearrange the layered model parameters according to the position of the dipole source.
call: E_XED         -to return the electric fields for x-directed electric dipole.
Running time:
58.9 seconds.
3.3.2 marineCSEM_Current.m
marineCSEM_Current.m is used to calculate the electric current distribution for sections. Then, plot_marineCSEM_Current.m is used to plot figure 10.
Input:
bc, h, z                   -layered model description.
TX, T0, f                 -parameters of dipole source.
xxB, yyB, zzB            -position of the measurement points for 1D background model.
xx, yy, zz                 -position of the measurement points for 1D reservoir model.
Output:
XEx, XEy, XEz     -the electric field at measurement points by x-directed electric dipole source.
Code structure:
call: select_model       -to rearrange the layered model parameters according to the position of the dipole source.
call: E_XED            -to return the electric fields for x-directed electric dipole.
Running time:
543.3 seconds
3.3.3 marineCSEM_Sensitivity.m
marineCSEM_Sensitivity.m is used to calculate the sensitivity of Ex relative to the discrete cells with uniform dimension of 400×400×200 m3 in the background model. Then, plot_marineCSEM_Sensitivity.m is used to plot figure 11.
Input:
bc, h, z                -layered model description.
TX, T0, f              -parameters of dipole source.  
RC                   -position of the receiver .
xxP, yyP, zzP           -position of the elements at one plan view.
xxD, yyD, zzD          -position of the elements at one section view.
Output:
XEx, XEy, XEz    -the electric field at measurement points for x-directed magnetic dipole source.
AEX    -the electric field at the receiver for the vector electric dipole source within the elements.
Code structure:
call: slect_model   -to rearrange the layered model parameters according to the position of the dipole source.
call: E_XED       -to return the electric fields at the discrete element.
call: CAL_AEX    -to calculate the x-component electric field at the receiver caused by vector electric dipole source at the central of each the element.
Running time:
278.7 seconds.
3.3.4 AEM_CurrentXMD.m and AEM_CurrentZMD.m
AEM_CurrentXMD.m and AEM_CurrentZMD.m are used to calculate the current distribution in the uniform half-space model for x-directed horizontal magnetic dipole and z-directed magnetic dipole, respectively. Then, plot_AEM_CurrentXMD.m and plot_AEM_CurrentZMD.m are used to plot figure 12 and 13.
Input:
bc, h, z                -layered model description.
TX, T0, f              -parameters of dipole source.  
xxP, yyP, zzP           -position of the measurement points at one plan.
xxD,yyD,zzD           -position of the measurement points at one section.
Output:
XEx,XEy,XEz     -the electric field at measurement points for x-directed magnetic dipole source.
Code structure:
call: slect_model   -to rearrange the layered model parameters according to the position of the dipole source.
If run AEM_CurrentXMD.m
call: E_XMD      -to return the electric fields for x-directed magnetic dipole.
If run AEM_CurrentZMD.m
call: E_ZMD      -to return the electric fields for z-directed magnetic dipole.
Running time:
651.8 seconds.
