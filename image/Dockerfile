FROM mcr.microsoft.com/vscode/devcontainers/base:bookworm

ENV DEBIAN_FRONTEND=noninteractive \
    TERM=xterm-256color \
    PATH=${PATH}:/home/vscode/.nix-profile/bin \
    EDITOR=micro

RUN apt-get -y update \
    && apt-get -y install --no-install-recommends \
        curl \
        ca-certificates \
        sudo \
        xz-utils \
        bash-completion \
        micro \
    && apt-get -y autoclean \
    && apt-get -y clean \
    && rm -rf /var/lib/apt/lists/* \
    && update-alternatives --install /usr/bin/nano nano /usr/bin/micro 1 \
    && update-alternatives --install /usr/bin/vim vim /usr/bin/micro 1


USER vscode
WORKDIR /home/vscode

SHELL [ "/bin/bash", "-c" ]
RUN curl -fsSL https://get.jetify.com/devbox | bash -s -- -f \
    && echo ". /home/$(id -un)/.nix-profile/etc/profile.d/nix.sh" >> ~/.bashrc \
    && mkdir -p temp \
    && cd temp \
    && devbox init \
    && echo '\n' | devbox shell || true \
    && cd .. \
    && rm -rf temp
