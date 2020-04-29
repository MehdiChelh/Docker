FROM ubuntu

MAINTAINER Winston Chang "winston@rstudio.com"
# Source : https://gist.github.com/smartinsightsfromdata/d53d54deef687662789e

# =====================================================================
# R
# =====================================================================

# Need this to add R repo
RUN apt-get update && apt-get install -y software-properties-common

# Add R apt repository
# RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E084DAB9
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 51716619e084dab9
#     -> Link below to update if necessary
RUN add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/'
RUN apt update

# Install basic stuff and R
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y \
    vim-tiny \
    less \
    wget \
    r-base \
    r-base-dev \
    r-recommended


# =====================================================================
# Shiny Server
# =====================================================================

RUN apt-get install -y \
    gdebi-core \
    pandoc \
    pandoc-citeproc \
    libcurl4-gnutls-dev \
    libcairo2-dev \
    libxt-dev

# Je pense que cette partie n'est pas nécessaire en version "commerciale" (ie. non pro)
RUN apt-get install libssl1.0.0 libssl-dev

# Download and install shiny server
RUN wget "https://download3.rstudio.org/ubuntu-14.04/x86_64/shiny-server-1.5.9.923-amd64.deb"  && \
    gdebi -n shiny-server-1.5.9.923-amd64.deb && \
    rm shiny-server-1.5.9.923-amd64.deb


RUN R -e "install.packages(c('shiny', 'rmarkdown'), repos='http://cran.rstudio.com/')"

RUN R -e "install.packages('remotes')"

RUN apt-get install -y libgsl-dev \
    libxml2-dev \
    libssl-dev

# RUN R -e "remotes::install_github('Thinkr-open/golem')"
# Standard packages from CRAN
#RUN R -e "install.packages(read.delim('requirements-std.txt', sep='\n', header=F)[, 1])"
# Remplacer cette ligne par un script bash
#RUN R -e "remotes::install_github('MehdiChelh/triangle.tlbx')"

# TODO : recuperer le nom du package et la source via un fichier de config a cote du Dockerfile 
RUN mkdir /srv/shiny-server/app 

# Remplacer le dépôt github par le dépôt gitlab
RUN mkdir /tmp_docker
COPY requirements.txt /tmp_docker/requirements.txt
RUN apt-get install -y git && \
    R -e "install.packages(read.csv('/tmp_docker/requirements.txt', stringsAsFactor = FALSE)[, 1])"
RUN R -e "install.packages('shinytest')"
RUN R -e "shinytest::installDependencies()"
RUN R -e "install.packages('devtools')"
RUN rm -r -d /tmp_docker
#    R -e "remotes::install_github('MehdiChelh/triangle.tlbx')"
#    git clone http://github.com/MehdiChelh/triangle.tlbx /srv/shiny-server/app/triangle.tlbx && \

# COPY shiny-server.conf /etc/shiny-server/shiny-server.conm

# Set the locale
RUN apt-get update && apt-get install -y locales
RUN locale-gen en_US.UTF-8  
ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8 


EXPOSE 3838

# Ajouter l'exécution d'un bash contenant les commandes pour l'installation du package et la configuration du serveur

# mettre ca dans un fichier bash ?
#RUN shiny-server
#COPY shiny-server.sh /usr/bin/shiny-server.sh
#CMD ["/usr/bin/shiny-server.sh"]
#CMD [“/usr/sbin/init”]

# =====================================================================
# Shiny examples
# =====================================================================

# Get examples from git repo. This downloads the files from the commit just
# before Garrett deleted them from the repo:
#   https://github.com/rstudio/shiny-examples/commit/41ff00e
#RUN cd /srv/shiny-server && \
#    wget https://github.com/rstudio/shiny-examples/archive/230acb1990f1ea6049885837eac8e58ed8405f80.zip && \
#    unzip 230acb1990f1ea6049885837eac8e58ed8405f80.zip && \
#    rm 230acb1990f1ea6049885837eac8e58ed8405f80.zip && \
#    mv shiny-examples-230acb1990f1ea6049885837eac8e58ed8405f80/* . && \
#    rm -rf shiny-examples-230acb1990f1ea6049885837eac8e58ed8405f80

# Packages needed for specific examples
#RUN R -e "install.packages(c('ggplot2', 'lubridate', 'dplyr', 'RSQLite'), repos='http://cran.rstudio.com/')"
#
