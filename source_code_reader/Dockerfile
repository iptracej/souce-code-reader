FROM alpine:latest

RUN apk add global --update-cache --repository http://dl-4.alpinelinux.org/alpine/edge/testing/ --allow-untrusted && \
apk add py-pip ctags && \
apk add apache2 perl && \
pip install Pygments

RUN mkdir -p /opt/code && \
mkdir -p /run/apache2

COPY httpd.conf /etc/apache2/httpd.conf
COPY ./source /opt/code/source
COPY gtags.conf /etc/gtags.conf

WORKDIR /opt/code/source

RUN gtags -v && \
htags -aosnfvF

RUN sed -i 's/\/opt\/local\/bin\/perl/\/usr\/bin\/perl/' HTML/cgi-bin/global.cgi
RUN sed -i 's/\/usr\/local\/bin\/global/\/usr\/bin\/global/' HTML/cgi-bin/global.cgi

CMD /usr/sbin/httpd -D FOREGROUND -f /etc/apache2/httpd.conf
