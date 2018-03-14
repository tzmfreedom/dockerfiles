FROM ubuntu

RUN apt-get update
RUN apt-get install -y binutils build-essential \
  sysstat strace vim curl wget git \
  autoconf automake libtool ruby bison gdb \
  openssl libreadline6-dev git-core \
  zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev \
  sqlite3 libxml2-dev libxslt-dev libc6-dev ncurses-dev

WORKDIR /root
RUN git clone https://github.com/ruby/ruby

WORKDIR /root/ruby
RUN autoreconf
RUN ./configure optflags="-O0" debugflags="-g3"
RUN make
RUN make install

RUN gem install bundler
WORKDIR /root
