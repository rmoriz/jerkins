FROM debian:jessie-backports

RUN apt-get update \
    && apt-get install -y \
      apt-transport-https \
      ca-certificates \
      curl \
      gnupg2 \
      software-properties-common \
      lsb-release \
      sudo \
      libffi-dev \
      build-essential \
      git \
      zlib1g-dev \
      libssl-dev \
      redis-server \
    && curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add - \
    && add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian jessie stable" \
    && apt-get update \
    && apt-get install -y docker-ce \
    && useradd -ms /bin/bash builder \
    && usermod -a -G staff builder \
    && usermod -a -G sudo builder \
    && sed -i.bkp -e \
       's/%sudo\s\+ALL=(ALL\(:ALL\)\?)\s\+ALL/%sudo ALL=NOPASSWD:ALL/g' \
       /etc/sudoers

USER builder
ENV PATH="/home/builder/.asdf/bin:/home/builder/.asdf/shims:${PATH}" \
    BUNDLE_JOBS=6

# Ruby / Rake
RUN git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.4.0 \
    && echo '. $HOME/.asdf/asdf.sh' >> ~/.bashrc \
    && echo '. $HOME/.asdf/completions/asdf.bash' >> ~/.bashrc \
    && asdf plugin-add ruby https://github.com/asdf-vm/asdf-ruby.git \
    && asdf install ruby 2.4.2 \
    && asdf global ruby 2.4.2 \
    && echo 'gem: --no-rdoc --no-ri' >> ~/.gemrc \
    && gem install rake bundler \
    && rm -rf /tmp/ruby_build*

ENTRYPOINT ["/bin/bash", "-c"]

