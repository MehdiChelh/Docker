FROM ubuntu

MAINTAINER Winston Chang "winston@rstudio.com"

# =====================================================================
# R
# =====================================================================

# Need this to add R repo
RUN apt-get update && apt-get install -y software-properties-common

# Add R apt repository
RUN apt-get install -y software-properties-common
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E084DAB9
RUN add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/'
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9
RUN apt update
#RUN add-apt-repository "deb http://cran.rstudio.com/bin/linux/ubuntu $(lsb_release -cs)/"
#RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9

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
# Shiny Server Pro
# =====================================================================

RUN apt-get install -y \
    gdebi-core \
    wget \
    #pandoc \
    #pandoc-citeproc \
    libcurl4-gnutls-dev \
    libcairo2-dev \
    libxt-dev

# Download and install libssl 0.9.8
#RUN wget --no-verbose http://ftp.us.debian.org/debian/pool/main/o/openssl/libssl2.9.8_0.9.8o-4squeeze14_amd64.deb && \
#    dpkg -i libssl0.9.8_0.9.8o-4squeeze14_amd64.deb && \
#    rm -f libssl0.9.8_0.9.8o-4squeeze14_amd64.deb
RUN apt-get install libssl1.0.0 libssl-dev
# Download and install shiny server pro
RUN wget "https://s3.amazonaws.com/rstudio-shiny-server-pro-build/ubuntu-14.04/x86_64/shiny-server-commercial-1.5.11.994-amd64.deb"  && \
    gdebi -n shiny-server-commercial-1.5.11.994-amd64.deb && \
    rm shiny-server-commercial-1.5.11.994-amd64.deb

RUN echo "password" | /opt/shiny-server/bin/sspasswd /etc/shiny-server/passwd "admin"

RUN R -e "install.packages(c('shiny', 'rmarkdown'), repos='http://cran.rstudio.com/')"

#RUN apt-get install -y systemd

EXPOSE 3838


#COPY shiny-server.sh /usr/bin/shiny-server.sh

#CMD ["/usr/bin/shiny-server.sh"]


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
