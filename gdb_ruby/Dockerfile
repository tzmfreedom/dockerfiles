FROM ubuntu

RUN apt-get update
RUN apt-get install -y binutils build-essential \
  sysstat strace vim curl wget git \
  autoconf automake libtool ruby bison gdb

WORKDIR /root
RUN git clone https://github.com/ruby/ruby

WORKDIR /root/ruby
RUN autoreconf
RUN ./configure optflags="-O0" debugflags="-g3"
RUN make
RUN make install
