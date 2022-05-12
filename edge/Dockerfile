FROM alpine:edge
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories
RUN echo http://mirrors.ustc.edu.cn/alpine/edge/testing >> /etc/apk/repositories \
  && apk --no-cache add alpine-sdk coreutils sudo \
  && adduser -G abuild -g "Alpine Package Builder" -s /bin/ash -D builder \
  && echo "builder ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers \
  && mkdir /packages \
  && chown builder:abuild /packages \
  && mkdir -p /var/cache/apk \
  && ln -s /var/cache/apk /etc/apk/cache
COPY /abuilder /bin/
USER builder
RUN mkdir /home/builder/.pip
COPY /pip.conf /home/builder/.pip/
RUN mkdir /home/builder/.cargo
COPY /config /home/builder/.cargo/
ENTRYPOINT ["abuilder", "-r"]
WORKDIR /home/builder/package
ENV RSA_PRIVATE_KEY_NAME ssh.rsa
ENV PACKAGER_PRIVKEY /home/builder/${RSA_PRIVATE_KEY_NAME}
ENV REPODEST /packages
VOLUME ["/home/builder/package"]
