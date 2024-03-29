## Repository:

PRED_PREY_SIM_FD2D is a collection of MATLAB codes for Simulating Predator-Prey Interactions in 2D.

## General description:

PRED_PREY_SIM_FD2D is a collection of simple MATLAB routines using finite element / difference methods for simulating the dynamics of predator-prey interactions modelled in two space dimensions and time by nonlinear reaction-diffusion systems. 

The MATLAB code is mostly self explanatory, with the names of variables and parameters corresponding to the symbols used in the finite element / difference methods described in the paper referenced below. 

The code employs the sparse matrix facilities of MATLAB when solving the linear systems, which provides advantages in both matrix storage and computation time. The code is vectorized to minimize the number of "for-loops" and conditional "if-then-else" statements, which again helps speed up the computations. There is just one "for-loop" and two "if-then-else" statements in the codes.

The linear systems are solved using MATLAB's built in function gmres.m. The gmres.m algorithm in MATLAB requires a number of input arguments. For our simulations we found it acceptable to use no "restarts" of the iterative method, or, preconditioners, and a tolerance for the relative error of 1E-6. In practise, the user will need to experiment with the restart value, tolerance, and "maxit" (maximum number of outer iterations) in order to achieve satisfactory rates of convergence of the gmres.m function. For definitions of these input arguments (and others) see the description in MATLAB's Help pages. We remark that a pure C or FORTRAN code is likely to be faster than our codes, but with the disadvantage of much greater complexity and length.

The user is prompted for all the necessary parameters, time and space-steps, and initial data. Due to a limitation in MATLAB, vector indices cannot be equal to zero; thus the nodal indices 0, . . . ,J are shifted up one unit to give i = 1, . . . ,(J + 1) so xi = (i - 1)*h + a.

## Format of initial data

The initial data is entered by the user as a string, and the functions depend on a grid of x and y values denoted by X and Y (as a common MATLAB convention, a variable name using capital letters indicates that the variable is a matrix). Functions are entered using the same element-by-element rules for the arithmetic operation as in the 1-D code.

An example with an acceptable format is the following:

        >> Enter initial prey function U0(X,Y)  0.2*exp(-(X.^2+Y.^2))
        >> Enter initial predator function V0(X,Y)  1.0
      
We can also define functions in a piecewise fashion. For example, with Omega=[-500,500]x[-500,500], in order to choose an initial prey density of 0.2 within the circle with radius 10 and center (-50,200), and a density of 0.01 elsewhere on Omega we input the following:
      
        >> Enter initial predator function V0(X,Y)     
        0.2*(((X+50).^2+(Y-200).^2)<100)+0.01*(((X+50).^2+(Y-200).^2)>=100)
      
## Some practical issues

Firstly, bear in mind that if you run a simulation with a large domain size and large final time T, coupled with small temporal and spatial discretization parameters, then the run-time in MATLAB can be prohibative.

Another point concerns the choice of parameters alpha, beta, and gamma used to run the code. In order for the local kinetics of the systems (Kinetics (i) or Kinetics (ii)) to be biologically meaningful, there are restrictions on the parameters that need to be satisfied (for further details see the reference below).

## Nonconvergence of GMRES

The GMRES algorithm does not always converge, or it may converge very slowly. One approach that can be tried to overcome this problem is to use MATLAB's 'incomplete lu factorization' algorithm LUINC. This provides a preconditioner for GMRES. See MATLAB's Help page under GMRES for an example.

If this still doesn't help you can try other preconditioners, or use a less sophisticated iterative method for solving the linear systems. For example, the Jacobi and Gauss-Seidel algorithms are widely used, although the run-times will likely be considerably longer than for GMRES. As these methods are not part of the standard suite of MATLAB functions they are provided in this repository. The algorithms require that all diagonal elements aii of the coefficient matrix A are non-zero, and they will not always converge even then.

## Download codes for FD2D

Files you may copy include:

    fd2d.m,    MATLAB code for Scheme 2 applied to Kinetics (i) in 2D.
    fd2dKin2.m    MATLAB code for Scheme 2 applied to Kinetics (ii) in 2D.
    fd2dx.m    MATLAB code for Scheme 1 applied to Kinetics (i) in 2D.
    fd2dxKin2.m    MATLAB code for Scheme 1 applied to Kinetics (ii) in 2D.
    fd2d.inp,    a text file containing the input that a user might enter to define a simple problem.
    fd2d.out,    printed output from a run of the program with the sample input.
    fd2d_v.jpg,    a JPEG file containing an image of the v component of the solution generated from fd2d.m.
    copyright.txt,    PRED_PREY_SIM copyright details.
    jacobi.m,    MATLAB code for iteratively solving the (square) system of linear equations Ax = b.
    gauss_seidel.m    A variation (usually an improvement) of JACOBI.

## Reference

Garvie M.R. , "Finite difference schemes for reaction-diffusion equations modeling predator-prey interactions in MATLAB," Bulletin of Mathematical Biology (2007) 69:931-956. 

PRED_PREY_SIM_FD2D is distributed under the GNU GPL; see the copyright.txt file for more information.
