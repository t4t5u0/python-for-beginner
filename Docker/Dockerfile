FROM python:latest
# USER root

ARG USER=fuga
ARG PASSWD=pass


RUN useradd -m ${USER}
RUN gpasswd -a ${USER} sudo
RUN echo "${USER}:${PASSWD}" | chpasswd


RUN apt-get update && apt-get -y install vim
RUN apt-get update \
    && apt-get install -y locales \
    && locale-gen ja_JP.UTF-8 \
    && echo "export LANG=ja_JP.UTF-8" >> ~/.bashrc
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8
ENV TZ JST-9

ENV HOME=/home/${USER}

RUN mkdir $HOME/work
WORKDIR $HOEM/work

# USER ${USER}

COPY python-for-beginner/work/requirements.txt $HOME/work/
RUN pip install -r ${HOME}/work/requirements.txt
COPY python-for-beginner/work ${HOME}/work/

RUN yes | jupyter lab --generate-config 
RUN sed -i \ 
    -e '2ic = get_config()' \
    -e "s/^# \(c.ServerApp.ip = '127.0.0.1'\)/\1/" \
    -e 's/^# \(c.ServerApp.open_browser = False\)/\1/' \
    -e 's/^# \(c.ServerApp.port = 8888\)/\1/' \
    -e "s/^# \(c.ServerApp.token = \).*/\1u''/" \ 
    $HOME/.jupyter/jupyter_lab_config.py

EXPOSE 8888

USER ${USER}