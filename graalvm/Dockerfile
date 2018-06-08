# You should install graalvm tar.gz file.
#
# bundle exec ruby ./loop.rb
# bundle exec /usr/graalvm-1.0.0-rc1/bin/ruby ./loop.rb
FROM oraclelinux:7-slim

ENV GRAALVM_PKG=graalvm-ce-1.0.0-rc1-linux-amd64.tar.gz \
    JAVA_HOME=/usr/graalvm-1.0.0-rc1/jdk \
    PATH=/usr/graalvm-1.0.0-rc1/bin:$PATH \
    LANG=en_US.UTF-8
ADD $GRAALVM_PKG /usr/

RUN yum -y install gcc && rm -rf /var/cache/yum && \
    alternatives --install /usr/bin/java  java  $JAVA_HOME/bin/java  20000 && \
    alternatives --install /usr/bin/javac javac $JAVA_HOME/bin/javac 20000 && \
    alternatives --install /usr/bin/jar   jar   $JAVA_HOME/bin/jar   20000
RUN gu -c install org.graalvm.ruby
RUN yum install -y openssl-devel bzip2 make readline-devel vim git

RUN echo "gem: --no-ri --no-rdoc" > /root/.gemrc
RUN gem install bundler

# install Ruby 2.6.0-preview2 via rbenv
RUN git clone https://github.com/rbenv/rbenv.git ~/.rbenv
RUN git clone git://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build

ENV PATH /root/.rbenv/bin:$PATH
RUN echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc
RUN echo 'eval "$(rbenv init -)"' >> ~/.bashrc

ENV CONFIGURE_OPTS --disable-install-rdoc
ENV RUBY_CONFIGURE_OPTS --with-readline-dir=/usr/lib
RUN rbenv install 2.6.0-preview2
RUN rbenv global 2.6.0-preview2
RUN /bin/bash -l -c '/root/.rbenv/shims/gem install bundler'

COPY loop.rb ~/loop.rb
COPY Gemfile ~/Gemfile

WORKDIR ~/
RUN /root/.rbenv/shims/bundle install --path=vendor/bundle -j4
RUN /usr/graalvm-1.0.0-rc1/bin/bundle install --path=vendor/bundle -j4

CMD ["graalvm", "--version"]
