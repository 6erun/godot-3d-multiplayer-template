# syntax=docker/dockerfile:1
FROM fedora:latest

# Environment Variables
ARG GODOT_VERSION="4.3"
ARG SERVER_PORT=1370
ENV GODOT_FILE_NAME="Godot_v${GODOT_VERSION}-stable_linux.x86_64"
# Name of the PCK file you want to run on the server
ENV GODOT_GAME_NAME="Godot3DMultiplayer.linux" 
ENV HTTPS_GIT_REPO=""

RUN dnf update -y
RUN dnf install -y wget
RUN dnf install -y unzip
RUN dnf install -y wayland-devel

# Download Godot, version is set from environment variables
RUN wget https://github.com/godotengine/godot/releases/download/${GODOT_VERSION}-stable//${GODOT_FILE_NAME}.zip
RUN mkdir -p ~/.cache \
    && mkdir -p ~/.config/godot \
    && unzip ${GODOT_FILE_NAME}.zip \
    && mv ${GODOT_FILE_NAME} /usr/local/bin/godot \
    && rm -f ${GODOT_FILE_NAME}.zip

#RUN git clone ${HTTPS_GIT_REPO} .

RUN cd ./project \
    && godot --headless --export-pack "Linux/DedicatedServer" ../build/${GODOT_GAME_NAME}.pck

# Make directory to run the app from and then run the app
RUN mkdir /godotapp
WORKDIR /godotapp
COPY ./build/${GODOT_GAME_NAME}.pck .

EXPOSE ${SERVER_PORT}/tcp
EXPOSE ${SERVER_PORT}/udp
CMD godot --main-pack ${GODOT_GAME_NAME}.pck