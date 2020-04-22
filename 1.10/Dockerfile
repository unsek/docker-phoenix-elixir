FROM ubuntu:18.04

WORKDIR /app

RUN apt-get update && apt-get install -y locales

# Set locale
RUN locale-gen en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV LANGUAGE=en_US:en
ENV LC_ALL=en_US.UTF-8
ENV HOME=/root

ENV OTP_VERSION="22.3.2-1"
ENV ELIXIR_VERSION="1.10.2-1"

# Install dependencies
RUN apt-get update -y && \
    apt-get install -y aptitude ca-certificates python3 python-dev python3-pip curl wget \
                    python-virtualenv \
                    git \
                    automake autoconf libreadline-dev libncurses-dev libssl-dev libyaml-dev libxslt-dev libffi-dev libtool unixodbc-dev unzip && \
    rm -rf /var/lib/apt/lists/*

# Install Erlang
RUN wget https://packages.erlang-solutions.com/erlang-solutions_2.0_all.deb && \
  dpkg -i erlang-solutions_2.0_all.deb && \
  apt update -y && \
  apt install esl-erlang=1:$OTP_VERSION -y

# Install Elixir
RUN apt install elixir=$ELIXIR_VERSION -y

# Add node source
RUN curl -sL https://deb.nodesource.com/setup_12.x -o nodesource_setup.sh && \
  bash nodesource_setup.sh

# Install NodeJS
RUN apt install nodejs -y

# Install hex and rebar
RUN mix local.hex --force
RUN mix local.rebar --force

# Set up ansible
RUN apt-get update && \
    apt-get install -y software-properties-common && \
    apt-add-repository ppa:ansible/ansible && \
    apt-get update && \
    apt-get install -y ansible