FROM python:latest
LABEL maintainer="https://github.com/t4t5u0"

# USER root

# このUSERとPASSWDは上書きされる
ARG USER=fuga
ARG PASSWD=pass
ENV HOME=/home/${USER}

RUN useradd -m ${USER}
RUN gpasswd -a ${USER} sudo
RUN echo "${USER}:${PASSWD}" | chpasswd

RUN apt-get update && apt-get -y install vim fonts-ipafont
RUN apt-get update \
    && apt-get install -y locales \
    && locale-gen ja_JP.UTF-8 \
    && echo "export LANG=ja_JP.UTF-8" >> ~/.bashrc

RUN curl -fsSL https://deb.nodesource.com/setup_15.x | bash -
RUN apt-get install -y nodejs
RUN fc-cache -fv

ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8
ENV TZ JST-9
# ENV HOME=/home/${USER}

# RUN mkdir ${HOME}/work
WORKDIR ${HOME}/work

# USER ${USER}

COPY python-for-beginner/work/requirements.txt ${HOME}/work/
RUN pip install -r ${HOME}/work/requirements.txt
COPY python-for-beginner/work ${HOME}/work/

RUN yes | jupyter lab --generate-config 
RUN sed -i \ 
    -e '2ic = get_config()' \
    -e "s/^# \(c.ServerApp.ip = '127.0.0.1'\)/\1/" \
    -e 's/^# \(c.ServerApp.open_browser = False\)/\1/' \
    -e 's/^# \(c.ServerApp.port = 8888\)/\1/' \
    -e "s/^# \(c.ServerApp.token = \).*/\1u''/" \ 
    ${HOME}/.jupyter/jupyter_lab_config.py

EXPOSE 8888

# RUN echo ${USER}
RUN chown -R ${USER}:${USER} ${HOME}
USER ${USER}