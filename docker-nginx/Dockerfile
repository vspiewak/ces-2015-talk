#
# NGiNX + Supervisor Dockerfile
#

# Define the initial base image
FROM ubuntu:latest

# Maintainer Informaion
MAINTAINER vspiewak@thefamounsgooglemailerifyouknowwhatimean.com

# Update the base image
RUN apt-get update 

# Install nginx
RUN apt-get install -qq nginx

# Install Supervisord
RUN apt-get install -qq supervisor

# Install psmisc for killall command
RUN apt-get install -qq psmisc

# Add the base configuration file for Supervisord
#ADD supervisor.conf /etc/supervisor.conf

# Will run supervisord not as daemon 
RUN sed -i 's/^\(\[supervisord\]\)$/\1\nnodaemon=true/' /etc/supervisor/supervisord.conf

RUN echo '[program:nginx]' > /etc/supervisor/conf.d/nginx.conf
RUN echo 'command=/usr/sbin/nginx' >> /etc/supervisor/conf.d/nginx.conf

EXPOSE 80

CMD ["supervisord", "-c", "/etc/supervisor/supervisord.conf"]
