FROM mcr.microsoft.com/devcontainers/base:jammy

ARG TARGETPLATFORM

# ARCH reference
COPY expand_env_with_platform /expand_env_with_platform

# base deps
RUN . /expand_env_with_platform && \
    # docker cli package-repo
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg && \
    echo \
    "deb [arch=${PLATFORM_ARCH_SHORT} signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null && \
    # install libs
    apt-get update && apt install --no-install-recommends -y docker-ce-cli \
    dnsutils iputils-ping \
    bash-completion \
    && \
    # clean up
    apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# Task

ENV DEV_EXTENSIONS_DIR=/.devcontainer
RUN mkdir -p $DEV_EXTENSIONS_DIR && chmod 660 $DEV_EXTENSIONS_DIR && \
    touch $DEV_EXTENSIONS_DIR/.bash_profile

ENV TASK_VERSION=v3.18.0
RUN . /expand_env_with_platform && \
    # install task
    mkdir /tmp/task && curl -sSL https://github.com/go-task/task/releases/download/$TASK_VERSION/task_linux_${PLATFORM_ARCH_SHORT}.tar.gz -o /tmp/task/task.tar.gz && tar -xf /tmp/task/task.tar.gz -C /tmp/task/ && mv /tmp/task/task /usr/local/bin/task && rm -rf /tmp/task && chmod +x /usr/local/bin/task && \
    curl -sSL https://raw.githubusercontent.com/go-task/task/master/completion/bash/task.bash -o $DEV_EXTENSIONS_DIR/task.bash  && chmod +x $DEV_EXTENSIONS_DIR/task.bash && \
    echo "source $DEV_EXTENSIONS_DIR/task.bash" >> $DEV_EXTENSIONS_DIR/.bash_profile

# jekyll stuff
RUN apt-get update && apt-get install --no-install-recommends -y \
    ruby-full build-essential zlib1g-dev python3 nodejs curl npm \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

RUN gem install bundler