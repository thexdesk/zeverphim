FROM debian:buster-slim

ARG GITHUB_RUNNER_VERSION="2.280.2"

ENV RUNNER_NAME=$RUNNER_NAME 
ENV GITHUB_PAT=$GITHUB_PAT 
ENV GITHUB_OWNER=$GITHUB_OWNER 
ENV GITHUB_REPOSITORY=$GITHUB_REPOSITORY
ENV RUNNER_WORKDIR=$RUNNER_WORKDIR

RUN apt-get update \
    && apt-get install -y \
    curl \
    sudo \
    ruby \
    git \
    jq \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && useradd -m github \       
    && usermod -aG sudo github \
    && echo "%sudo ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

USER github
WORKDIR /home/github

RUN curl -Ls https://github.com/actions/runner/releases/download/v${GITHUB_RUNNER_VERSION}/actions-runner-linux-x64-${GITHUB_RUNNER_VERSION}.tar.gz | tar xz \
    && sudo ./bin/installdependencies.sh

COPY --chown=github:github entrypoint.sh ./entrypoint.sh
RUN sudo chmod u+x ./entrypoint.sh

ENTRYPOINT ["/home/github/entrypoint.sh"] 