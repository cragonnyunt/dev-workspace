ARG NODE_VERSION=lts-slim

FROM node:${NODE_VERSION}

ARG YARN_VERSION=stable
ARG BUN_VERSION=1.4.2
ARG NPM_VERSION=12.2.0
ARG USER=node

USER root

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    ca-certificates \
    git \
    curl \
    unzip \
    && apt-get clean

ENV BUN_INSTALL=/home/${USER}/.bun
ENV PATH=/home/${USER}/.bun/bin:$PATH

# Install the toolchain as root, then hand the user's home back to $USER
# so the runtime (USER $USER) can write to the mounted caches.
RUN cd /home/$USER && \
    npm install -g npm@${NPM_VERSION} && \
    yarn set version $YARN_VERSION && \
    curl -fsSL https://bun.sh/install | bash -s "bun-v${BUN_VERSION}" && \
    node --version && \
    npm --version && \
    bun --version && \
    mkdir -p /home/$USER/.vscode-server \
             /home/$USER/.npm \
             /home/$USER/.yarn \
             /home/$USER/.bun/install/cache && \
    chown -R $USER:$USER /home/$USER

USER ${USER}

WORKDIR /var/www
