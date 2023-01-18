Troubleshooting Installing R Packages on Quest and Quest Analytics
==================================================================

Installing R Packages on the RStudio instance on the QUEST analytics
node and QUEST Log-In Nodes.

.. container:: table-responsive

   +-----------------------------------------------------------------------+
   | Content                                                               |
   +-----------------------------------------------------------------------+
   | `1. Introduction <#Preamble>`__                                       |
   | `2. Install <#Install>`__                                             |
   | `2.1 Seurat <#Seurat>`__                                              |
   | `2.2 Monocle3 <#Monocle3>`__                                          |
   | `2.3 SF <#SF>`__                                                      |
   | `2.4 RGDAL <#RGDAL>`__                                                |
   | `2.5 RJAGS <#RJAGS>`__                                                |
   | `2.6 V8/RSTAN/RSTANARM <#RSTAN>`__                                    |
   | `2.7 DiffBind <#DiffBind>`__                                          |
   +-----------------------------------------------------------------------+

Introduction
------------

Due to technical limitations in RStudio Server, some R packages
installed on Quest login nodes may not work when loaded in RStudio
Server on the Quest Analytics nodes. If you would still like to use the
graphical interface of RStudio but are unable to load all the R packages
you need on Quest Analytics, you will need to launch RStudio within a
graphical session on Quest. The simplest way is to use FastX:
`https://kb.northwestern.edu/quest-login <https://services.northwestern.edu/TDClient/30/Portal/KB/ArticleDet?ID=1541>`__.

The R environment of the Quest Analytics nodes includes the following
modules:

.. code:: code

   R/4.1.1
   gdal/3.1.3-R-4.1.1
   proj/7.1.1
   geos/3.8.1

.. container:: panel panel-default

   .. container:: panel-heading

      Installing R Packages for R 4.1.1 On Quest

   .. container:: panel panel-body js-panelnormalswitches0 collapse

      Please check for files where you may have at some point
      accidentally hardcoded paths before starting these instructions.
      This includes ``~/.bash_profile``, ``~/.bashrc``,
      ``~/.local/share/rstudio/Renviron``,
      ``~/.local/share/rstudio/``\ ``.Rprofile``, or ``~/.R/Makevars``.

      If issues arise, you can delete the folder where R/4.1.x packages
      get installed, which is (usually)
      ``~/R/x86_64-pc-linux-gnu-library/4.1``, and start again.

      .. rubric:: Seurat
         :name: seurat

      *Seurat is an R package designed for QC, analysis, and exploration
      of single-cell RNA-seq data.*

      Note: the commands can be run within an interactive R session,
      either on a Quest login node or an RStudio Server session.

      On the Quest log-in node, create a script called **build.R** which
      contains the following lines or on the RStudio Server simply copy
      and paste them into your R command window:

      .. code:: code

         Sys.setenv(INCLUDE = "/software/geos/3.8.1/include:/software/gcc/8.4.0/include")
         old_path <- Sys.getenv("PATH")
         Sys.setenv(PATH = paste("/software/geos/3.8.1/bin", old_path, sep = ":"))
         install.packages("BiocManager", repos="https://cloud.r-project.org/")
         install.packages("Rcpp", repos="https://cloud.r-project.org/")
         install.packages("png", repos="https://cloud.r-project.org/")
         BiocManager::install("multtest")
         install.packages("Seurat", repos="https://cloud.r-project.org/")

      On the Quest log-in node, run the follow commands on the
      command-line to install Seurat.

      .. code:: code

         module purge all
         module load R/4.1.1
         module load geos/3.8.1
         Rscript --vanilla "build.R"

      .. rubric:: Monocle3
         :name: monocle3

      *The monocle3 package provides a toolkit for analyzing single-cell
      gene expression experiments. Single-cell transcriptome sequencing
      (sc-RNA-seq) experiments help discover new cell types and
      understand how they arise in development.*

      Note: the commands below should be run within an interactive R
      session, either on a Quest login node or an RStudio Server
      session:

      .. code:: code

         Sys.setenv(INCLUDE = "/software/geos/3.8.1/include:/software/gcc/8.4.0/include:/software/proj/7.1.1/include")
         Sys.setenv(PKG_CONFIG_PATH = "/software/sqlite/3.27.2/lib/pkgconfig:/software/proj/7.1.1/lib/pkgconfig:/software/gdal/3.1.3-R-4.1.1/lib/pkgconfig")
         old_path <- Sys.getenv("PATH")
         Sys.setenv(PATH = paste("/software/geos/3.8.1/bin", old_path, sep = ":"))
         old_path <- Sys.getenv("PATH")
         Sys.setenv(PATH = paste("/software/proj/7.1.1/bin", old_path, sep = ":"))
         old_path <- Sys.getenv("PATH")
         Sys.setenv(PATH = paste("/software/gdal/3.1.3-R-4.1.1/bin", old_path, sep = ":"))
         install.packages("devtools", repos="https://cloud.r-project.org/")
         install.packages("Rcpp", repos="https://cloud.r-project.org/")
         install.packages("grr", repos="https://cloud.r-project.org/")
         install.packages("https://cran.r-project.org/src/contrib/Archive/Matrix.utils/Matrix.utils_0.9.7.tar.gz",
             repos = NULL, type="source")
         install.packages(c("shiny", "RCurl", "matrixStats", "futile.logger", "snow", "ggbeeswarm", "viridis", "RcppAnnoy",
             "RcppHNSW", "irlba", "rsvd", "igraph"), repos="https://cloud.r-project.org/")
         install.packages(c("dplyr", "ggrepel", "lmtest", "pbapply", "pbmcapply", "pheatmap", "plotly", "plyr",
             "proxy", "pscl", "RANN", "reshape2", "rsample", "RhpcBLASctl", "Rtsne", "slam",
             "spdep", "speedglm", "uwot", "tidyr"), repos="https://cloud.r-project.org/")
         install.packages("BiocManager", repos="https://cloud.r-project.org/")
         BiocManager::install('DelayedArray', update = FALSE, ask = FALSE)
         BiocManager::install(c("BiocNeighbors", "BiocSingular", "scater"), update = FALSE, ask = FALSE)
         BiocManager::install(c('BiocGenerics', 'DelayedMatrixStats', 'limma', 'S4Vectors', 'SingleCellExperiment', 'SummarizedExperiment', 'batchelor'), update = FALSE, ask = FALSE)
         devtools::install_github('cole-trapnell-lab/leidenbase')
         devtools::install_github('cole-trapnell-lab/monocle3')

      .. rubric:: SF
         :name: sf

      *The sf package provides support for*\ `simple
      features <https://en.wikipedia.org/wiki/Simple_Features>`__\ *in
      R. It binds to ‘GDAL’ for reading and writing data, to ‘GEOS’ for
      geometrical operations, and to ‘PROJ’ for projection conversions
      and datum transformations. This package can also use the ‘s2’
      package for spherical geometry operations on geographic
      coordinates.*

      Note: the commands below should be run within an interactive R
      session, either on a Quest login node or an RStudio Server
      session:

      .. code:: code

         Sys.setenv(INCLUDE = "/software/geos/3.8.1/include:/software/gcc/8.4.0/include:/software/proj/7.1.1/include")
         Sys.setenv(PKG_CONFIG_PATH = "/software/sqlite/3.27.2/lib/pkgconfig:/software/proj/7.1.1/lib/pkgconfig:/software/gdal/3.1.3-R-4.1.1/lib/pkgconfig")
         old_path <- Sys.getenv("PATH")
         Sys.setenv(PATH = paste("/software/geos/3.8.1/bin", old_path, sep = ":"))
         old_path <- Sys.getenv("PATH")
         Sys.setenv(PATH = paste("/software/proj/7.1.1/bin", old_path, sep = ":"))
         old_path <- Sys.getenv("PATH")
         Sys.setenv(PATH = paste("/software/gdal/3.1.3-R-4.1.1/bin", old_path, sep = ":"))
         install.packages("sf", repos="https://cloud.r-project.org/")

      .. rubric:: RGDAL
         :name: rgdal

      *Provides bindings to the ‘Geospatial’ Data Abstraction Library
      (’GDAL’) (>= 1.11.4) and access to projection/transformation
      operations from the ‘PROJ’ library. Use is made of classes defined
      in the ‘sp’ package. Raster and vector map data can be imported
      into R, and raster and vector ‘sp’ objects exported. The ‘GDAL’
      and ‘PROJ’ libraries are external to the package, and, when
      installing the package from source, must be correctly installed
      first; it is important that ‘GDAL’ < 3 be matched with ‘PROJ’ < 6.
      From ‘rgdal’ 1.5-8, installed with to ‘GDAL’ >=3, ‘PROJ’ >=6 and
      ‘sp’ >= 1.4, coordinate reference systems use ‘WKT2_2019’ strings,
      not ‘PROJ’ strings.*

      Note: the command below should be run within an interactive R
      session, either on a Quest login node or an RStudio Server
      session:

      .. code:: code

         Sys.setenv(INCLUDE = "/software/geos/3.8.1/include:/software/gcc/8.4.0/include:/software/proj/7.1.1/include")
         Sys.setenv(PKG_CONFIG_PATH = "/software/sqlite/3.27.2/lib/pkgconfig:/software/proj/7.1.1/lib/pkgconfig:/software/gdal/3.1.3-R-4.1.1/lib/pkgconfig")
         old_path <- Sys.getenv("PATH")
         Sys.setenv(PATH = paste("/software/geos/3.8.1/bin", old_path, sep = ":"))
         old_path <- Sys.getenv("PATH")
         Sys.setenv(PATH = paste("/software/proj/7.1.1/bin", old_path, sep = ":"))
         old_path <- Sys.getenv("PATH")
         Sys.setenv(PATH = paste("/software/gdal/3.1.3-R-4.1.1/bin", old_path, sep = ":"))
         install.packages("rgdal", repos="https://cloud.r-project.org/",)

      .. rubric:: RJAGS
         :name: rjags

      *The rjags package provides an interface from R to the JAGS
      library for Bayesian data analysis. JAGS uses Markov Chain Monte
      Carlo (MCMC) to generate a sequence of dependent samples from the
      posterior distribution of the parameters*

      Note: the commands below must be run in a terminal session on a
      Quest login node.

      On the Quest log-in node, create a script called **build.R** which
      contains the following lines:

      .. code:: code

         install.packages("Rcpp", repos="https://cloud.r-project.org/")
         install.packages("rjags", repos="https://cloud.r-project.org/")

      On the Quest log-in node, run the follow commands on the
      command-line to install RJAGS.
      .. code:: code

         module purge all
         module load R/4.1.1
         module load jags
         Rscript --vanilla "build.R"

      .. rubric:: **V8/RSTANARM/RSTAN**
         :name: v8rstanarmrstan

      .. container::

         *The rstan package is the R interface
         to*\ `Stan <http://mc-stan.org/>`__\ *. User-facing R functions
         are provided to parse, compile, test, estimate, and analyze
         Stan models by accessing the header-only Stan library provided
         by the ‘StanHeaders’ package. The Stan project develops a
         probabilistic programming language that implements full
         Bayesian statistical inference via Markov Chain Monte Carlo
         (MCMC), rough Bayesian inference via ‘variational’
         approximation, and (optionally penalized) maximum likelihood
         estimation via optimization. In all three cases, automatic
         differentiation is used to quickly and accurately evaluate
         gradients without burdening the user with the need to derive
         the partial derivatives.*

         Note: the commands below must be run in a terminal session on a
         Quest login node.

         On the Quest log-in node, create a script called **build.R**
         which contains the following lines:

         .. code:: code

            install.packages("V8", repos="https://cloud.r-project.org/")
            install.packages(c("rstanarm", "rstan"), repos="https://cloud.r-project.org/")

         On the Quest log-in node, run the follow commands on the
         command-line to install V8/RSTANARM/RSTAN.

         .. code:: code

            module purge all
            module load R/4.1.1
            DOWNLOAD_STATIC_LIBV8=1 Rscript --vanilla "build.R"

         .. rubric:: DiffBind
            :name: diffbind

         .. container::

            *DiffBind is an R package that is used for identifying sites
            that are differentially enriched between two or more sample
            groups. It helps to compute differentially bound sites from
            multiple ChIP-seq experiments using affinity (quantitative)
            data and allows occupancy (overlap) analysis and plotting
            functions.*

            Note: the commands below must be run in a terminal session
            on a Quest login node.

            On the Quest log-in node, create a script called **build.R**
            which contains the following lines:

            .. code:: code

               if (!requireNamespace("BiocManager", quietly = TRUE))
                   install.packages("BiocManager")
               BiocManager::install("V8")
               BiocManager::install("DiffBind")

            On the Quest log-in node, run the follow commands on the
            command-line to install DiffBind.

            .. code:: code

               mkdir -p ~/.R/
               echo CC=gcc >> ~/.R/Makevars 
               DOWNLOAD_STATIC_LIBV8=1 Rscript --vanilla "build.R"

.. container:: panel panel-default

   .. container:: panel-heading

      Installing R Packages for R 4.0.3 On Quest

   .. container:: panel panel-body js-panelnormalswitches1 collapse

      Please check for files where you may have at some point
      accidentally hardcoded paths before starting these instructions.
      This includes ``~/.bash_profile``, ``~/.bashrc``,
      ``~/.local/share/rstudio/Renviron``,
      ``~/.local/share/rstudio/``\ ``.Rprofile``, or ``~/.R/Makevars``.

      If issues arise, you can delete the folder where R/4.0.x packages
      get installed, which is (usually)
      ``~/R/x86_64-pc-linux-gnu-library/4.0``, and start again.

      .. rubric:: Seurat
         :name: seurat-1

      *Seurat is an R package designed for QC, analysis, and exploration
      of single-cell RNA-seq data.*

      Note: the commands below must be run in a terminal session on a
      Quest login node.

      .. code:: code

         module purge all
         module load anaconda3
         module load R/4.0.3
         conda create --name Seurat-dependecies python=3.7 --yes
         source activate Seurat-dependecies
         pip install leidenalg numpy python-igraph
         module purge all
         module load R/4.0.3
         Rscript --vanilla "build.R"

      Where **build.R** is a file containing the following:

      .. code:: code

         install.packages("BiocManager", repos="https://cloud.r-project.org/")
         install.packages("Rcpp", repos="https://cloud.r-project.org/")
         install.packages("png", repos="https://cloud.r-project.org/")
         BiocManager::install("multtest")
         install.packages("Seurat", repos="https://cloud.r-project.org/")

      .. rubric:: Monocle3
         :name: monocle3-1

      *The monocle3 package provides a toolkit for analyzing single-cell
      gene expression experiments. Single-cell transcriptome sequencing
      (sc-RNA-seq) experiments help discover new cell types and
      understand how they arise in development.*

      Note: the commands below should be run within an interactive R
      session, either on a Quest login node or an RStudio Server
      session:

      .. code:: code

         install.packages("devtools", repos="https://cloud.r-project.org/")
         install.packages("Rcpp", repos="https://cloud.r-project.org/")
         install.packages("grr", repos="https://cloud.r-project.org/")
         install.packages("https://cran.r-project.org/src/contrib/Archive/Matrix.utils/Matrix.utils_0.9.7.tar.gz",
             repos = NULL, type="source")
         install.packages(c("shiny", "RCurl", "matrixStats", "futile.logger", "snow", "ggbeeswarm", "viridis", "RcppAnnoy",
             "RcppHNSW", "irlba", "rsvd", "igraph"), repos="https://cloud.r-project.org/")
         install.packages(c("dplyr", "ggrepel", "lmtest", "pbapply", "pbmcapply", "pheatmap", "plotly", "plyr",
             "proxy", "pscl", "RANN", "reshape2", "rsample", "RhpcBLASctl", "Rtsne", "slam",
             "spdep", "speedglm", "uwot", "tidyr"), repos="https://cloud.r-project.org/")
         install.packages("BiocManager", repos="https://cloud.r-project.org/")
         BiocManager::install('DelayedArray', update = FALSE, ask = FALSE)
         BiocManager::install(c("BiocNeighbors", "BiocSingular", "scater"), update = FALSE, ask = FALSE)
         BiocManager::install(c('BiocGenerics', 'DelayedMatrixStats', 'limma', 'S4Vectors', 'SingleCellExperiment', 'SummarizedExperiment', 'batchelor'), update = FALSE, ask = FALSE)
         devtools::install_github('cole-trapnell-lab/leidenbase')
         devtools::install_github('cole-trapnell-lab/monocle3')

      .. rubric:: SF
         :name: sf-1

      *The sf package provides support for*\ `simple
      features <https://en.wikipedia.org/wiki/Simple_Features>`__\ *in
      R. It binds to ‘GDAL’ for reading and writing data, to ‘GEOS’ for
      geometrical operations, and to ‘PROJ’ for projection conversions
      and datum transformations. This package can also use the ‘s2’
      package for spherical geometry operations on geographic
      coordinates.*

      Note: the commands below should be run within an interactive R
      session, either on a Quest login node or an RStudio Server
      session:

      .. code:: code

         Sys.setenv(INCLUDE = "/software/geos/3.8.1/include:/software/gcc/8.4.0/include:/software/proj/7.1.1/include")
         Sys.setenv(PKG_CONFIG_PATH = "/software/sqlite/3.27.2/lib/pkgconfig:/software/proj/7.1.1/lib/pkgconfig:/software/gdal/3.1.3/lib/pkgconfig")
         old_path <- Sys.getenv("PATH")
         Sys.setenv(PATH = paste("/software/geos/3.8.1/bin", old_path, sep = ":"))
         old_path <- Sys.getenv("PATH")
         Sys.setenv(PATH = paste("/software/proj/7.1.1/bin", old_path, sep = ":"))
         old_path <- Sys.getenv("PATH")
         Sys.setenv(PATH = paste("/software/gdal/3.1.3/bin", old_path, sep = ":"))
         install.packages("sf", repos="https://cloud.r-project.org/")

      .. rubric:: RGDAL
         :name: rgdal-1

      *Provides bindings to the ‘Geospatial’ Data Abstraction Library
      (’GDAL’) (>= 1.11.4) and access to projection/transformation
      operations from the ‘PROJ’ library. Use is made of classes defined
      in the ‘sp’ package. Raster and vector map data can be imported
      into R, and raster and vector ‘sp’ objects exported. The ‘GDAL’
      and ‘PROJ’ libraries are external to the package, and, when
      installing the package from source, must be correctly installed
      first; it is important that ‘GDAL’ < 3 be matched with ‘PROJ’ < 6.
      From ‘rgdal’ 1.5-8, installed with to ‘GDAL’ >=3, ‘PROJ’ >=6 and
      ‘sp’ >= 1.4, coordinate reference systems use ‘WKT2_2019’ strings,
      not ‘PROJ’ strings.*

      Note: the command below should be run within an interactive R
      session, either on a Quest login node or an RStudio Server
      session:

      .. code:: code

         Sys.setenv(INCLUDE = "/software/geos/3.8.1/include:/software/gcc/8.4.0/include:/software/proj/7.1.1/include")
         Sys.setenv(PKG_CONFIG_PATH = "/software/sqlite/3.27.2/lib/pkgconfig:/software/proj/7.1.1/lib/pkgconfig:/software/gdal/3.1.3/lib/pkgconfig")
         old_path <- Sys.getenv("PATH")
         Sys.setenv(PATH = paste("/software/geos/3.8.1/bin", old_path, sep = ":"))
         old_path <- Sys.getenv("PATH")
         Sys.setenv(PATH = paste("/software/proj/7.1.1/bin", old_path, sep = ":"))
         old_path <- Sys.getenv("PATH")
         Sys.setenv(PATH = paste("/software/gdal/3.1.3/bin", old_path, sep = ":"))
         install.packages("rgdal", repos="https://cloud.r-project.org/",)

      .. rubric:: RJAGS
         :name: rjags-1

      *The rjags package provides an interface from R to the JAGS
      library for Bayesian data analysis. JAGS uses Markov Chain Monte
      Carlo (MCMC) to generate a sequence of dependent samples from the
      posterior distribution of the parameters*

      Note: the commands below must be run in a terminal session on a
      Quest login node.

      .. code:: code

         module purge all
         module load R/4.0.3
         module load jags
         Rscript --vanilla "build.R"

      Where **build.R** is a file containing the following:

      .. code:: code

         install.packages("Rcpp", repos="https://cloud.r-project.org/")
         install.packages("rjags", repos="https://cloud.r-project.org/")

      .. rubric:: **V8/RSTANARM/RSTAN**
         :name: v8rstanarmrstan-1

      .. container::

         *The rstan package is the R interface
         to*\ `Stan <http://mc-stan.org/>`__\ *. User-facing R functions
         are provided to parse, compile, test, estimate, and analyze
         Stan models by accessing the header-only Stan library provided
         by the ‘StanHeaders’ package. The Stan project develops a
         probabilistic programming language that implements full
         Bayesian statistical inference via Markov Chain Monte Carlo
         (MCMC), rough Bayesian inference via ‘variational’
         approximation, and (optionally penalized) maximum likelihood
         estimation via optimization. In all three cases, automatic
         differentiation is used to quickly and accurately evaluate
         gradients without burdening the user with the need to derive
         the partial derivatives.*

         Note: the commands below must be run in a terminal session on a
         Quest login node.

         .. code:: code

            module purge all
            module load R/4.0.3
            DOWNLOAD_STATIC_LIBV8=1 Rscript --vanilla "build.R"

         Where **build.R** is a file containing the following:

         .. code:: code

            install.packages("V8", repos="https://cloud.r-project.org/")
            install.packages(c("rstanarm", "rstan"), repos="https://cloud.r-project.org/")

         .. rubric:: DiffBind
            :name: diffbind-1

         .. container::

            *DiffBind is an R package that is used for identifying sites
            that are differentially enriched between two or more sample
            groups. It helps to compute differentially bound sites from
            multiple ChIP-seq experiments using affinity (quantitative)
            data and allows occupancy (overlap) analysis and plotting
            functions.*

            Note: the commands below must be run in a terminal session
            on a Quest login node.

            .. code:: code

               mkdir -p ~/.R/ 
               echo CC=gcc >> ~/.R/Makevars 
               DOWNLOAD_STATIC_LIBV8=1 Rscript --vanilla "build.R"

            Where **build.R** is a file containing the following:

            .. code:: code

               if (!requireNamespace("BiocManager", quietly = TRUE))
                   install.packages("BiocManager")

               BiocManager::install("V8")
               BiocManager::install("DiffBind")
