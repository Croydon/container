FROM ubuntu:24.04

ENV DEBIAN_FRONTEND=noninteractive \
    ANDROID_SDK_ROOT=/opt/android-sdk \
    ANDROID_HOME=/opt/android-sdk \
    PATH=/opt/android-sdk/cmdline-tools/latest/bin:/opt/android-sdk/platform-tools:/opt/android-sdk/build-tools/35.0.0:$PATH

ARG CMDLINE_TOOLS_URL=https://dl.google.com/android/repository/commandlinetools-linux-13114758_latest.zip
ARG ANDROID_BUILD_TOOLS=35.0.0
ARG APKTOOL_VERSION=3.0.3

RUN apt-get update && apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        wget \
        unzip \
        zip \
        git \
        python3 \
        python3-pip \
        openjdk-17-jdk \
    && rm -rf /var/lib/apt/lists/*

# Install Node.js. Pin this version in production if reproducibility matters.
RUN curl -fsSL https://deb.nodesource.com/setup_22.x | bash - \
    && apt-get update \
    && apt-get install -y --no-install-recommends nodejs \
    && rm -rf /var/lib/apt/lists/* \
    && node --version \
    && npm --version

# Install Android SDK command-line tools and Build-Tools.
RUN mkdir -p "${ANDROID_SDK_ROOT}/cmdline-tools" \
    && curl -fsSL "${CMDLINE_TOOLS_URL}" -o /tmp/cmdline-tools.zip \
    && unzip -q /tmp/cmdline-tools.zip -d /tmp/cmdline-tools \
    && mv /tmp/cmdline-tools/cmdline-tools "${ANDROID_SDK_ROOT}/cmdline-tools/latest" \
    && rm -rf /tmp/cmdline-tools /tmp/cmdline-tools.zip \
    && yes | sdkmanager --licenses >/dev/null || true \
    && sdkmanager --sdk_root="${ANDROID_SDK_ROOT}" \
        "platform-tools" \
        "build-tools;${ANDROID_BUILD_TOOLS}" \
    && rm -rf /root/.android

RUN curl -fL \
        "https://github.com/iBotPeaches/Apktool/releases/download/v${APKTOOL_VERSION}/apktool_${APKTOOL_VERSION}.jar" \
        -o "/opt/apktool_${APKTOOL_VERSION}.jar" \
    && printf '%s\n' \
        '#!/bin/sh' \
        'exec java -jar /opt/apktool_'"${APKTOOL_VERSION}"'.jar "$@"' \
        > /usr/local/bin/apktool \
    && chmod +x /usr/local/bin/apktool

RUN npm install --global apk-mitm \
    && mv /usr/bin/apk-mitm /usr/bin/apk-mitm-original \
    && printf '%s\n' \
        '#!/bin/sh' \
        'exec /usr/bin/apk-mitm-original --apktool /opt/apktool_'"${APKTOOL_VERSION}"'.jar "$@"' \
        > /usr/bin/apk-mitm \
    && chmod +x /usr/bin/apk-mitm

RUN ln -sf "$(command -v python3)" /usr/local/bin/python

# Verify the required commands.
RUN java -version \
    && node --version \
    && python --version \
    && python3 --version \
    && apktool --version \
    && command -v zipalign >/dev/null \
    && apksigner --version

WORKDIR /work
ENTRYPOINT ["/bin/bash"]
