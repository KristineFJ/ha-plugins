ARG BUILD_FROM
FROM $BUILD_FROM
ENV LANG C.UTF-8

RUN apt-get update
RUN apt-get install -y build-essential git swig python3 python3-dev python3-pip ffmpeg

RUN pip3 install pydub requests PyYAML typing_extensions

WORKDIR /tmp
RUN set -xe \
    && git clone --depth 1 --branch 2.13 https://github.com/pjsip/pjproject.git \
    && cd pjproject \
    && ./configure --enable-shared --disable-libwebrtc \
    && make -j 8 \
    && make dep \
    && make install \
    && cd pjsip-apps/src/swig \
    && make python \
    && make -C python install \
    && ldconfig \
    && rm -rf /tmp/pjproject

COPY run.sh /
RUN chmod a+x /run.sh

COPY src/ /ha-sip/

CMD [ "/run.sh" ]
