FROM mcr.microsoft.com/devcontainers/base:ubuntu
# 
RUN apt-get update && apt-get install -y openjdk-17-jdk-headless

USER vscode

VOLUME fvm /home/vscode/fvm
VOLUME android-sdk /home/vscode/android-sdk

WORKDIR /home/vscode

RUN curl -fsSL https://fvm.app/install.sh | bash

RUN mkdir -p android-sdk/cmdline-tools/latest
RUN curl -o cmdline-tools.zip https://dl.google.com/android/repository/commandlinetools-linux-14742923_latest.zip
RUN unzip cmdline-tools.zip -d android-sdk/cmdline-tools
RUN mv android-sdk/cmdline-tools/cmdline-tools/* android-sdk/cmdline-tools/latest/
RUN rmdir android-sdk/cmdline-tools/cmdline-tools
RUN rm cmdline-tools.zip

ENV ANDROID_HOME="/home/vscode/android-sdk"
ENV FVM_HOME="/home/vscode/fvm"
ENV PATH="${FVM_HOME}/bin:${FVM_HOME}/default/bin:${ANDROID_HOME}/cmdline-tools/latest/bin:${ANDROID_HOME}/platform-tools:${PATH}"

RUN yes | sdkmanager --licenses
RUN sdkmanager "platform-tools" "platforms;android-36" "build-tools;36.0.0"

RUN fvm install stable && fvm global stable
RUN flutter bash-completion
RUN flutter --disable-telemetry
RUN dart --disable-analytics