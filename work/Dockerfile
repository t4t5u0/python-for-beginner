FROM python:latest
USER root


RUN apt-get update
RUN apt-get -y install vim
# RUN apt-get -y install language-pack-ja
# RUN apt-get -y install locale && \
#     localedef -f UTF-8 -i _JP ja_JP.UTF-8
ENV LANG ja_JP.UTF-8
ENV LANGUAGE ja_JP:ja
ENV LC_ALL ja_JP.UTF-8
ENV TZ JST-9
# RUN echo | pwd

RUN mkdir /root/work
WORKDIR /root/work

ADD . /root/work

RUN pip install -r requirements.txt

RUN echo y | jupyter lab --generate-config /root/
# RUN ls /root/.jupyter
RUN sed -i \ 
    -e '2ic = get_config()' \
    -e "s/^# \(c.ServerApp.ip = '127.0.0.1'\)/\1/" \
    -e 's/^# \(c.ServerApp.open_browser = False\)/\1/' \
    -e 's/^# \(c.ServerApp.port = 8888\)/\1/' \
    -e "s/^# \(c.ServerApp.token = \).*/\1u''/" \ 
    /root/.jupyter/jupyter_lab_config.py

EXPOSE 8888
