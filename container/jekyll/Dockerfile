FROM ubuntu:jammy
ARG TARGETPLATFORM

# ARCH reference
COPY expand_env_with_platform /expand_env_with_platform

RUN apt-get update && apt-get install --no-install-recommends -y \
    ruby-full build-essential zlib1g-dev python3 nodejs curl \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

RUN gem install bundler
COPY ../Gemfile /Gemfile
RUN bundle config set --local path vendor/bundle && \
    bundle install --system


# Add Tini
ENV TINI_VERSION v0.19.0
RUN . /expand_env_with_platform && \
    curl -sSL https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini-${PLATFORM_ARCH_SHORT} -o /tini
RUN ls -la /tini
RUN chmod +x /tini
ENV ACTION "serve"
ENTRYPOINT ["/tini", "--"]
CMD ["jekyll", "$ACTION", "--watch"]