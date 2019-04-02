FROM alpine:edge
LABEL maintainer="Viraj Trivedi <inf3cti0n95@gmail.com>"

ENV LANG C.UTF-8
ENV WATCHMAN_VERSION "4.9.0-r2"
ENV NODEJS_VERSION "10.15.3-r0"
ENV YARN_VERSION "1.15.2-r0"
ENV OPENJDK_VERSION "8.201.08-r0"
ENV SDK_TOOLS "4333796"
ENV ANDROID_HOME "/opt/sdk"
ENV GLIBC_VERSION "2.28-r0"

ENV JAVA_HOME=/usr/lib/jvm/java-1.8-openjdk

ENV PATH=$PATH:$JAVA_HOME/jre/bin:$JAVA_HOME/bin
ENV PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/platform-tools

RUN apk add -X http://dl-cdn.alpinelinux.org/alpine/edge/testing --no-cache bash git unzip wget nss \
    watchman="$WATCHMAN_VERSION" \
    nodejs="$NODEJS_VERSION" \
    openjdk8="$OPENJDK_VERSION" \
    yarn="$YARN_VERSION" && \
    wget -q https://alpine-pkgs.sgerrand.com/sgerrand.rsa.pub -O /etc/apk/keys/sgerrand.rsa.pub && \
    wget -q https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VERSION}/glibc-${GLIBC_VERSION}.apk -O /tmp/glibc.apk && \
    wget -q https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VERSION}/glibc-bin-${GLIBC_VERSION}.apk -O /tmp/glibc-bin.apk && \
    apk add --no-cache /tmp/glibc.apk /tmp/glibc-bin.apk && \
    apk add --virtual .rundeps $runDeps && \
    rm -rf /tmp/* && \
    rm -rf /var/cache/apk/*

RUN wget -q http://dl.google.com/android/repository/sdk-tools-linux-${SDK_TOOLS}.zip -O /tmp/tools.zip && \
    mkdir -p ${ANDROID_HOME} && \
    unzip -qq /tmp/tools.zip -d ${ANDROID_HOME} && \
    rm -v /tmp/tools.zip

RUN mkdir -p ~/.android/ && touch ~/.android/repositories.cfg && \
    yes | ${ANDROID_HOME}/tools/bin/sdkmanager "--licenses" && \
    ${ANDROID_HOME}/tools/bin/sdkmanager "--update" && \
    ${ANDROID_HOME}/tools/bin/sdkmanager \
    "platform-tools" \
    "extras;android;m2repository" \
    "extras;google;m2repository" \
    "extras;google;instantapps" \
    "extras;google;google_play_services" \
    "extras;google;market_apk_expansion" \
    "extras;google;market_licensing" \
    "extras;google;simulators" \
    "extras;google;webdriver" \
    "extras;m2repository;com;android;support;constraint;constraint-layout;1.0.2" \
    "extras;m2repository;com;android;support;constraint;constraint-layout;1.0.1"
