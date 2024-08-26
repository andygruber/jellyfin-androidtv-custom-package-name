# using this one instead of openjdk image, so we are in control of the Unbuntu base image
FROM ubuntu:24.04

# build-essential: compilers etc.
# devscripts: make command etc.
# java jdk: android sdk tools require it
RUN apt-get update && \
    apt-get install -y build-essential devscripts openjdk-17-jdk zip unzip curl

    
# # install android sdk
# # download URL: https://developer.android.com/studio#command-tools
ENV ANDROID_HOME=/opt/android-sdk
RUN mkdir -p $ANDROID_HOME/cmdline-tools && \
    curl -L https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip -o sdktools.zip && \
    unzip sdktools.zip -d $ANDROID_HOME/cmdline-tools && \
    mv $ANDROID_HOME/cmdline-tools/cmdline-tools $ANDROID_HOME/cmdline-tools/latest && \
    rm sdktools.zip

RUN yes | $ANDROID_HOME/cmdline-tools/latest/bin/sdkmanager "ndk-bundle" "platforms;android-21" "platforms;android-33" "build-tools;34.0.0" "platform-tools"

ENV PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin:$ANDROID_HOME/platform-tools:$ANDROID_HOME/build-tools/34.0.0
ENV ANDROID_SDK_PATH=$ANDROID_HOME \
    ANDROID_SDK_ROOT=$ANDROID_HOME \
    ANDROID_NDK_PATH=$ANDROID_HOME/ndk-bundle \
    ANDROID_NDK_ROOT=$ANDROID_HOME/ndk-bundle

# switch to bash
SHELL ["/bin/bash", "-c"]

RUN curl -s "https://get.sdkman.io" | bash && \
    source "$HOME/.sdkman/bin/sdkman-init.sh" && \
    sdk install maven && \
    sdk install gradle

# can run local builds with './gradlew assemble bundleRelease'