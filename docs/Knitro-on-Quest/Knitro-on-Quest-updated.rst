Knitro on Quest
===============

Instructions for using Knitro on Quest with `MATLAB <#matlab>`__,
`R <#r>`__, `Python <#python>`__, and `AMPL <#ampl>`__.

Knitro Overview
---------------

Knitro is a software library used to find solutions of continuous and
discrete optimization models. It is designed for general, nonlinear
optimization problems and can be used for a wide range of cases such as
systems of non-linear equations, least square problems, convex and
non-convex quadratic problems etc. Please refer to the `user
manual <https://www.artelys.com/tools/knitro_doc/index.html>`__ for a
complete set of instructions on how to use Knitro. Knitro is not a
standalone program. It provides a programmable API to be used within
other software such MATLAB, Python, and R.

Knitro is installed on Quest. There are multiple versions, with 12.4
being the latest. You can check by typing the following command on
terminal:

::

   $ module spider knitro

   ------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    knitro:
   ------------------------------------------------------------------------------------------------------------------------------------------------------------------------
     Versions:
       knitro/8
       knitro/9.1.orig
       knitro/9.1
       knitro/10.0
       knitro/10.3
       knitro/12.4

If users need a newer version of knitro then is available on Quest
current, please fill out the `Software Installation
Request <https://services.northwestern.edu/TDClient/30/Portal/Requests/ServiceOfferingDet?ID=66>`__
form.

MATLAB
------

Knitro is loaded by each version of MATLAB. The path to the Knitro
installation is automatically added when the application starts. If
users want to have a different version of knitro loaded they must modify
the search path within their MATLAB code. Instructions on how to do this
are included at the end of this section.

knitromatlab is the interface used to call Knitro from the MATLAB
environment. To test that the installation exists, and to find out the
version of knitro loaded, a user can run a test function on MATLAB’s
command line as shown (find the minimum value of cos(x)):

.. code:: code

   >> [x fval] = knitromatlab(@(x)cos(x),1)

   =======================================
              Academic License
          (NOT FOR COMMERCIAL USE)
            Artelys Knitro 10.0.0
   =======================================

   Knitro presolve eliminated 0 variables and 0 constraints.

   algorithm:            1
   gradopt:              4
   hessopt:              2
   honorbnds:            1
   maxit:                10000
   outlev:               1
   par_concurrent_evals: 0
   The problem is identified as unconstrained.
   Knitro changing bar_initpt from AUTO to 3.
   Knitro changing bar_murule from AUTO to 4.
   Knitro changing bar_penaltycons from AUTO to 1.
   Knitro changing bar_penaltyrule from AUTO to 2.
   Knitro changing bar_switchrule from AUTO to 1.
   Knitro changing linsolver from AUTO to 2.

   Problem Characteristics                    ( Presolved)
   -----------------------
   Objective goal:  Minimize
   Number of variables:                     1 (         1)
       bounded below:                       0 (         0)
       bounded above:                       0 (         0)
       bounded below and above:             0 (         0)
       fixed:                               0 (         0)
       free:                                1 (         1)
   Number of constraints:                   0 (         0)
       linear equalities:                   0 (         0)
       nonlinear equalities:                0 (         0)
       linear inequalities:                 0 (         0)
       nonlinear inequalities:              0 (         0)
       range:                               0 (         0)
   Number of nonzeros in Jacobian:          0 (         0)
   Number of nonzeros in Hessian:           1 (         1)

   EXIT: Locally optimal solution found.

   Final Statistics
   ----------------
   Final objective value               =  -1.00000000000000e+00
   Final feasibility error (abs / rel) =   0.00e+00 / 0.00e+00
   Final optimality error  (abs / rel) =   2.37e-09 / 2.37e-09
   # of iterations                     =          7
   # of CG iterations                  =          0
   # of function evaluations           =         22
   # of gradient evaluations           =          0
   Total program time (secs)           =       0.46135 (     0.092 CPU time)
   Time spent in evaluations (secs)    =       0.04807

   ===============================================================================


   x =

       3.1416
   fval =

      -1.0000

Users can access different versions of kntiromatlab by updating their
path to the relevant location. Please query the knitro module to find
the installation directory. For example, MATLAB 2020b loads by default
``knitro/10.0``. To use version 12.4 type in the command line:

.. code:: code

   module load matlab/r2020b
   module load knitro/12.4 

Querying the 12.4 knitro module shows:

.. code:: code

   $ module show knitro/12.4
   ------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    /software/Modules/3.2.9/modulefiles/knitro/12.4:
   ------------------------------------------------------------------------------------------------------------------------------------------------------------------------
   whatis("Artelys Knitro is a commercial software package for solving large scale nonlinear mathematical optimization problems. ")
   whatis("KNITRO – (the original solver name) short for 'Nonlinear Interior point Trust Region Optimization' (the 'K' is silent) – was co-created by Richard Waltz, Jorge Nocedal, Todd Plantenga and Richard Byrd. ")
   whatis("It was first introduced in 2001, as a derivative of academic research at Northwestern University (through Ziena Optimization LLC), and has since been continually improved by developers at Artelys. ")
   prepend_path("KNITRODIR","/software/knitro/12.4")
   prepend_path("PATH","/software/knitro/12.4/knitroampl:/software/knitro/12.4/knitromatlab")
   prepend_path("LD_LIBRARY_PATH","/software/knitro/12.4/lib")
   setenv("ARTELYS_LICENSE_NETWORK_ADDR","129.105.119.173:8349")
   help([[Optimization problems must be presented to Knitro in mathematical form, and should provide a way of computing function derivatives using sparse matrices (Knitro can compute derivatives approximation but in most cases providing the exact derivatives is beneficial).
   An often easier approach is to develop the optimization problem in an algebraic modeling language.
   ]]) 

Therefore the path to the ``knitromatlab`` API is available on Quest in:
``/software/knitro/12.4/knitromatlab``. You can now start MATLAB. At the
MATLAB command prompt you can type:

.. code:: code

   >> addpath('/hpc/software/knitro/12.4/knitromatlab')
   >> savepath

The knitro interface for the user is now updated to version 12.4.

More information on using the MATLAB/Knitro interface can be found in
the `Knitro
Documentation <https://www.artelys.com/tools/knitro_doc/2_userGuide/gettingStarted/startMatlab.html>`__.

R
-

The knitro API for R can be installed by any user for any version of R
running the following commands:

.. code:: code

   module load knitro/12.4
   module load R/4.1.1
   rm -rf ~/R_knitro
   mkdir -p ~/R_knitro
   cp -r /software/knitro/12.4/examples/R ~/R_knitro/
   cd ~/R_knitro/R
   R CMD INSTALL --build KnitroR 

You can access use the ``KnitroR`` package by running the following:

::

   [quser24 R]$ module purge
   [quser24 R]$ module load knitro/12.4
   [quser24 R]$ module load R/4.1.1
   [quser24 R]$ R

   R version 4.1.1 (2021-08-10) -- "Kick Things"
   Copyright (C) 2021 The R Foundation for Statistical Computing
   Platform: x86_64-pc-linux-gnu (64-bit)

   R is free software and comes with ABSOLUTELY NO WARRANTY.
   You are welcome to redistribute it under certain conditions.
   Type 'license()' or 'licence()' for distribution details.

    Natural language support but running in an English locale

   R is a collaborative project with many contributors.
   Type 'contributors()' for more information and
   'citation()' on how to cite R or R packages in publications.

   Type 'demo()' for some demos, 'help()' for on-line help, or
   'help.start()' for an HTML browser interface to help.
   Type 'q()' to quit R.

   > library("KnitroR")
   > lsf.str("package:KnitroR")
   knitro : function (nvar = numeric(0), ncon = numeric(0), objective = NULL, objGoal = 0,
     gradient = NULL, gradientIndexVars = numeric(0), constraints = NULL,
     constraintsIndexCons = numeric(0), jacobian = NULL, jacIndexCons = numeric(0),
     jacIndexVars = numeric(0), jacBitMap = numeric(0), hessianLag = NULL,
     hessIndexVars1 = numeric(0), hessIndexVars2 = numeric(0), hessBitMap = numeric(0),
     objLinearStruct = numeric(0), objLinearStructIndexVars = numeric(0),
     conLinearStruct = numeric(0), conLinearStructIndexCons = numeric(0),
     conLinearStructIndexVars = numeric(0), objQuadraticStruct = numeric(0),
     objQuadraticStructIndexVars1 = numeric(0), objQuadraticStructIndexVars2 = numeric(0),
     conQuadraticStruct = numeric(0), conQuadraticStructIndexCons = numeric(0),
     conQuadraticStructIndexVars1 = numeric(0), conQuadraticStructIndexVars2 = numeric(0),
     xL = numeric(0), xU = numeric(0), x0 = numeric(0), cL = numeric(0),
     cU = numeric(0), xTypes = numeric(0), xTypesIndexVars = numeric(0),
     xStrategies = numeric(0), xStrategiesIndexVars = numeric(0), xPriorities = numeric(0),
     xPrioritiesIndexVars = numeric(0), xHonorBnds = numeric(0), xHonorBndsIndexVars = numeric(0),
     ccTypes = numeric(0), ccIdxList1 = numeric(0), ccIdxList2 = numeric(0), 
   knitrolsq : function (dimp = numeric(0), par0 = numeric(0), dataFrameX, dataFrameY,
     residual, jacobian = NULL, jacIndexRsds = numeric(0), jacIndexVars = numeric(0),
     rsdLinearStruct = numeric(0), rsdLinearStructIndexRsds = numeric(0),
     rsdLinearStructIndexVars = numeric(0), parL = numeric(0), parU = numeric(0),
     xScaleFactors = numeric(0), xScaleCenters = numeric(0), objScaleFactor = numeric(0),
     options = list(), optionsFile = NULL, env = NULL) 

To verify access to the knitroR interface a user can run a simple
example of the minimization of the Rosenbrock function: f(x,y) =
(1-x^2)+100*(y-x^2)^2.

At the R prompt, cut and paste the following commands that evaluate the
function and its derivatives and then call the KnitroR interface
calculate the location and value of the global minimum based on an
initial estimate.

.. code:: code

   > eval_f <- function(x) {
       return( 100 * (x[2] - x[1] * x[1])^2 + (1 - x[1])^2 )}

   > eval_grad_f <- function(x) {
       grad_f <-rep(0, length(x))
       grad_f[1] <- 2*x[1]-2+400*x[1]^3-400*x[1]*x[2]
       grad_f[2] <- 200*(x[2]-x[1]^2)
       return( grad_f )}

   > x0 <- c( -1.2, 1 )

   > sol <- knitro(x0 = x0, objective = eval_f)

Knitro then returns:

::

   =======================================
        Academic License
      (NOT FOR COMMERCIAL USE)
       Artelys Knitro 12.4.0
   =======================================

   Knitro performing finite-difference gradient computation with 1 thread.
   Knitro presolve eliminated 0 variables and 0 constraints.
   Knitro performing finite-difference gradient computation with 1 thread.
   Knitro presolve eliminated 0 variables and 0 constraints.

   gradopt:                 2
   hessopt:                 2
   outlev:                  1
   par_concurrent_evals:    0
   The problem is identified as unconstrained.
   Knitro changing algorithm from AUTO to 1.
   Knitro changing bar_initpt from AUTO to 3.
   Knitro changing bar_murule from AUTO to 4.
   Knitro changing bar_penaltycons from AUTO to 1.
   Knitro changing bar_penaltyrule from AUTO to 2.
   Knitro changing bar_switchrule from AUTO to 1.
   Knitro changing linesearch from AUTO to 1.
   Knitro changing linsolver from AUTO to 2.

   Problem Characteristics                    ( Presolved)
   -----------------------
   Objective goal:  Minimize
   Number of variables:                           2 (         2)
       bounded below only:                        0 (         0)
       bounded above only:                        0 (         0)
       bounded below and above:                   0 (         0)
       fixed:                                     0 (         0)
       free:                                      2 (         2)
   Number of constraints:                         0 (         0)
       linear equalities:                         0 (         0)
       nonlinear equalities:                      0 (         0)
       linear one-sided inequalities:             0 (         0)
       nonlinear one-sided inequalities:          0 (         0)
       linear two-sided inequalities:             0 (         0)
       nonlinear two-sided inequalities:          0 (         0)
   Number of nonzeros in Jacobian:                0 (         0)
   Number of nonzeros in Hessian:                 3 (         3)

   EXIT: Locally optimal solution found.

   Final Statistics
   ----------------
   Final objective value               =   2.00694602943133e-11
   Final feasibility error (abs / rel) =   0.00e+00 / 0.00e+00
   Final optimality error  (abs / rel) =   1.54e-08 / 1.54e-08
   # of iterations                     =         18
   # of CG iterations                  =          2
   # of function evaluations           =         71
   # of gradient evaluations           =          0
   Total program time (secs)           =       0.00355 (     0.004 CPU time)
   Time spent in evaluations (secs)    =       0.00062

   ===============================================================================

   > sol$x
   [1] 0.9999955 0.9999910

The theoretical location of the minimum is at (1,1) and the global
minimum value of 0. Knitro returns:

.. code:: code

   Final objective value               =   2.00694602943133e-11

Further examples are provided on Quest in
``/software/knitro/12.4/examples/R``. Further information is available
in the `Knitro
documentation <https://www.artelys.com/tools/knitro_doc/3_referenceManual/knitroRreference.html#knitrorreference%0A>`__.

Python
------

Knitro provides a Python interface for the Knitro callable library
functions. Knitro’s Python API is thread-safe where a Python code can
launch multiple instances of the knitro solver in different threads,
each solving a different problems. Examples are provided on Quest in
/software/knitro/12.4/examples/Python for users to gauge the structure
of a Knitro enabled python code. To use knitro/Python on Quest, you will
first create an anaconda virtual environment with the Python
distribution of your choice and then load the knitro module. In the
example below, we create a virtual environment with the name
``knitro-py38`` and install Python 3.8 into it.

.. code:: code

   % module load knitro/12.4
   % module load python-miniconda3
   % conda create --name knitro-py38 python=3.8 --yes
   % source activate knitro-py38
   % mkdir ~/python_knitro
   % cp -r /software/knitro/12.4/examples/Python ~/python_knitro/
   % cd ~/python_knitro/Python
   % python setup.py install

After the initial step of creating the virtual environment and
installing Knitro into it, each subsequent time you would like to use
Knitro and/or the virtual environment, you need only add the following
to your shell script or run the following on the command line

.. code:: code

   module load knitro/12.4
   module load python-miniconda3
   source activate knitro-py38

If this is done on the command line, you can test that importing Knitro
works by launching the Python command line and importing ``knitro``.

::

   (knitro-py38) $ python
   Python 3.8.13 (default, Mar 28 2022, 11:38:47)
   [GCC 7.5.0] :: Anaconda, Inc. on linux
   Type "help", "copyright", "credits" or "license" for more information.
   >>> import knitro
   >>> 

For more information please refer to the `Knitro
documentation <https://www.artelys.com/docs/knitro/2_userGuide/gettingStarted/startPython.html#startpython>`__.

AMPL
----

AMPL is a modeling language to solve optimization problems. AMPL is
installed on Quest and can be loaded as:

.. code:: code

   % module load ampl

There are only 2 licenses available on Quest at present. Users can check
if any there are any licenses available by running the following
command:

.. code:: code

   % ampl_lic status

Loading the AMPL module also loads knitro/10.3 which sets the path to
the knitroampl executable found on Quest in
/software/knitro/10.3/knitroampl.

To invoke knitro within AMPL one must either run or include in their
script the following line:

.. container:: code

   option solver knitroampl;

For more information and examples please see the `Knitro
documentation <https://www.artelys.com/tools/knitro_doc/2_userGuide/gettingStarted/startAmpl.html>`__.
