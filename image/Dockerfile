FROM mcr.microsoft.com/vscode/devcontainers/base:bookworm AS base

ENV DEBIAN_FRONTEND=noninteractive \
    PATH=${PATH}:/home/vscode/.nix-profile/bin

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


FROM base AS nix

USER vscode
WORKDIR /srv/test-nix

RUN curl -fsSL https://get.jetify.com/devbox | bash -s -- -f
RUN echo ". /home/$(id -un)/.nix-profile/etc/profile.d/nix.sh" >> ~/.bashrc

SHELL [ "/bin/bash", "-c" ]
RUN devbox init
RUN echo '\n' | devbox shell || true


FROM base

COPY --link=true --from=nix --chown=vscode:vscode /nix /nix
COPY --link=true --from=nix --chown=vscode:vscode /home/vscode /home/vscode
COPY --link=true --from=nix --chown=root:root --chmod=777 /usr/local/bin/devbox /usr/local/bin/devbox

ENV TERM=xterm-256color \
    PATH=${PATH}:/home/vscode/.nix-profile/bin \
    EDITOR=micro

USER vscode
WORKDIR /home/vscode
