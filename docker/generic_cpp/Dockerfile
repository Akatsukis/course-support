FROM ubuntu:22.04

# Set up environment variables used during the build and
# when the container is run afterward.
ENV DEBIAN_FRONTEND=noninteractive
ENV LANG=C.UTF-8
ENV LC_ALL=C.UTF-8
ENV TZ="America/Los_Angeles"

# Update software
RUN apt -y update && apt -y upgrade

# Basic software that should be available in all containers.
RUN apt -y install vim build-essential git wget tmux

# Useful C++ Tools
RUN apt -y install valgrind doxygen cmake bison flex libboost-all-dev

# Command to run when a container starts
CMD ["sleep", "infinity"]

