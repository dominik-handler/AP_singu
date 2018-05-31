#R in singularity

BootStrap: debootstrap
OSVersion: xenial
MirrorURL: http://us.archive.ubuntu.com/ubuntu/

%runscript
  "$@"

%post

  apt-get update
  apt-get -y install build-essential wget git binutils binutils-dev cmake gcc g++ gfortran bzip2  xz-utils liblzma-dev make libcurl4-openssl-dev libreadline-dev libpcre3-dev libbz2-dev zlib1g-dev libssl-dev libxml2-dev

  mkdir /R/
  cd /R/
  wget https://cran.r-project.org/src/base/R-latest.tar.gz
  tar zxf R-latest.tar.gz
  rm -rf R-latest.tar.gz
  cd R-*
  ./configure --enable-R-shlib --with-x=no
  make
  make install

  apt-get update
  R --slave -e 'install.packages(c( "devtools"), repos = "http://cran.wu.ac.at/") '
  R --slave -e 'devtools::install_github("tidyverse/ggplot2") '
  R --slave -e 'install.packages(c( "tidyverse", "plotly"), repos = "http://cran.wu.ac.at/") '
  R --slave -e 'source("http://bioconductor.org/biocLite.R"); biocLite("remotes"); biocLite("pachterlab/sleuth@v0.29.0")'
  R --slave -e 'source("http://bioconductor.org/biocLite.R"); biocLite("COMBINE-lab/wasabi")'

  mkdir /groups
  mkdir /scratch
  mkdir /scratch-ii2
  mkdir /clustertmp


%test
  R --version
  Rscript --version
	