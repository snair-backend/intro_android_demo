# Earthly Build Script for our Android App

FROM adoptopenjdk/openjdk11:alpine AS builder

# Set up build environment
WORKDIR /app
RUN apk update && apk add --no-cache \
    git \
    curl \
    unzip

# Install Android SDK
ARG ANDROID_SDK_VERSION=commandlinetools-linux-6858069_latest.zip
ARG ANDROID_BUILD_TOOLS_VERSION=30.0.3
ARG ANDROID_PLATFORM_VERSION=android-30
ARG ANDROID_HOME=/opt/android-sdk

RUN curl -L https://dl.google.com/android/repository/${ANDROID_SDK_VERSION} -o /tmp/android-sdk.zip \
    && unzip -q /tmp/android-sdk.zip -d ${ANDROID_HOME} \
    && rm /tmp/android-sdk.zip

ENV PATH=${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/platform-tools:${PATH}

# Accept Android SDK licenses
RUN yes | sdkmanager --licenses

# Download necessary Android SDK components
RUN sdkmanager "build-tools;${ANDROID_BUILD_TOOLS_VERSION}" "platforms;${ANDROID_PLATFORM_VERSION}"

# Copy the app source code
COPY . /app

# Build the Android app
RUN make release
