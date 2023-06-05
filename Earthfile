# Earthly Build Script for our Android App
VERSION 0.7
FROM adoptopenjdk/openjdk11:alpine
RUN apk update && apk add --no-cache \
    git \
    curl \
    unzip
WORKDIR /app

build:
    # Install Android SDK
    ARG ANDROID_SDK_VERSION=tools_r25.2.3-linux.zip
    ARG ANDROID_BUILD_TOOLS_VERSION=30.0.3
    ARG ANDROID_PLATFORM_VERSION=android-30
    ARG ANDROID_HOME=/opt/android-sdk

    RUN curl -L https://dl.google.com/android/repository/${ANDROID_SDK_VERSION} -o /tmp/android-sdk.zip \
        && unzip -q /tmp/android-sdk.zip -d ${ANDROID_HOME} \
        && rm /tmp/android-sdk.zip

    ENV PATH=${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/platform-tools:${PATH}

    # Accept Android SDK licenses
    RUN yes | sdkmanager --licenses

    # Download necessary Android SDK components
    RUN sdkmanager "build-tools;${ANDROID_BUILD_TOOLS_VERSION}" "platforms;${ANDROID_PLATFORM_VERSION}"

    # Copy the app source code
    COPY . /app

    # Build the Android app
    RUN make release

    # Copy the built APK from the builder stage
    # COPY --from=builder /app/app/build/outputs/apk/release/app-release.apk /app/app-release.apk

    # Set up the Google Play API credentials
    # ARG GOOGLE_PLAY_API_KEY
    # ENV GOOGLE_PLAY_API_KEY=${GOOGLE_PLAY_API_KEY}

    # Upload the APK to Google Play Store
    # RUN curl --fail \
    #    -X POST \
    #    -H "Authorization: Bearer ${GOOGLE_PLAY_API_KEY}" \
    #    -H "Content-Type: application/octet-stream" \
    #    --data-binary "@/app/app-release.apk" \
    #    "https://www.googleapis.com/upload/androidpublisher/v3/applications/[PACKAGE_NAME]/edits/[EDIT_ID]/apks"

    # Define the build target for Earthly
    # FROM scratch AS earthly
    # COPY --from=builder /app /app
    # WORKDIR /app
    # ENTRYPOINT ["/usr/local/bin/earthly"]

    # Default Earthly target
    # FROM earthly AS default
    # CMD +build
