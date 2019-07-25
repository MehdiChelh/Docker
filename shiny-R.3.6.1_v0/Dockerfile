FROM ubuntu:18.04

ARG DEBIAN_FRONTEND=noninteractive

# install R 3.6.1
RUN apt-get update
RUN apt-get install -y software-properties-common
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E084DAB9
RUN add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu bionic-cran35/'
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E084DAB9
RUN  apt update

RUN apt -y install r-base r-base-core r-recommended r-base-dev

RUN apt-get install -y gdebi-core wget
RUN wget https://download3.rstudio.org/ubuntu-14.04/x86_64/shiny-server-1.5.9.923-amd64.deb
RUN gdebi shiny-server-1.5.9.923-amd64.deb

EXPOSE 80
EXPOSE 22
EXPOSE 443
