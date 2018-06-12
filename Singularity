#R in singularity
Bootstrap: docker
From: ubuntu:16.04

%environment
  R_VERSION=3.4.3
  export R_VERSION
  R_CONFIG_DIR=/etc/R/
  export R_CONFIG_DIR

%runscript
  "$@"

%post

%post
  NPROCS=`awk '/^processor/ {s+=1}; END{print s}' /proc/cpuinfo`
  apt-get update --fix-missing
  apt-get install -y wget
  cd $HOME
  wget https://cran.rstudio.com/src/base/R-3/R-3.4.3.tar.gz
  tar xvf R-3.4.3.tar.gz
  cd R-3.4.3

  apt-get update
  apt-get install -y libblas3 libblas-dev liblapack-dev liblapack3 curl 
  apt-get install -y libgmp10 libgmp-dev
  apt-get install -y gcc fort77 aptitude
  aptitude install -y g++
  aptitude install -y xorg-dev
  aptitude install -y libreadline-dev
  aptitude install -y gfortran
  gfortran --version
  apt install -y libssl-dev libxml2-dev libpcre3-dev liblzma-dev libbz2-dev libcurl4-openssl-dev


  ./configure --enable-R-static-lib --with-blas --with-lapack --enable-R-shlib=yes
  echo "Will use make with $NPROCS cores."
  make -j${NPROCS}
  make install
  R --version

  R --slave -e 'install.packages(c( "devtools"), repos = "http://cran.wu.ac.at/") '
  R --slave -e 'install.packages(c( "tidyverse", "plotly", "Cairo"), repos = "http://cran.wu.ac.at/") '
  R --slave -e 'options(unzip = "internal"); devtools::install_github("tidyverse/ggplot2") '
  R --slave -e 'install.packages(c( "cowplot" ), repos = "http://cran.wu.ac.at/") '
  R --slave -e 'source("http://bioconductor.org/biocLite.R"); biocLite("remotes"); biocLite("pachterlab/sleuth@v0.29.0")'
  R --slave -e 'source("http://bioconductor.org/biocLite.R"); biocLite("COMBINE-lab/wasabi")'
  
  mkdir /groups
  mkdir /scratch
  mkdir /scratch-ii2
  mkdir /clustertmp


%test
  R --version
  Rscript --version
	