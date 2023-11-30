FROM r-base

RUN apt-get update && apt-get install \
    curl \
    autoconf \
    automake \
    make \ 
    wget \
    libtool \
    pkg-config \
    build-essential \
    checkinstall \
    zlib1g-dev \
    git -y

# install libpostal
RUN git clone https://github.com/openvenues/libpostal

WORKDIR /libpostal

RUN ./bootstrap.sh &&\
./configure &&\
make -j8 &&\
make install &&\
ldconfig

# install openssl from source
WORKDIR /usr/local/src

RUN wget https://www.openssl.org/source/openssl-1.1.1l.tar.gz &&\
tar -xf openssl-1.1.1l.tar.gz &&\
./openssl-1.1.1l/config \
    --prefix=/usr/local/ssl \
    --openssldir=/usr/local/ssl shared zlib &&\
make && \ 
make install

RUN mkdir /temp

WORKDIR /temp

# install libcurl from source
RUN wget https://curl.se/download/curl-7.79.1.tar.gz &&\
tar -xvf curl-7.79.1.tar.gz

ENV PKG_CONFIG_PATH=/usr/lib/pkgconfig:/usr/local/ssl/lib/pkgconfig 

WORKDIR /temp/curl-7.79.1

RUN ./configure --with-ssl &&\
make &&\ 
make install

WORKDIR /temp

# install libxml from source
RUN git clone https://gitlab.gnome.org/GNOME/libxml2.git

WORKDIR /temp/libxml2

RUN ./autogen.sh &&\
make &&\ 
make install

WORKDIR /

# openssl.pc , libcurl.pc missing -> install from sources when apt-get
RUN Rscript -e 'Sys.setenv(PKG_CONFIG_PATH="/usr/lib/pkgconfig:/usr/local/ssl/lib/pkgconfig");Sys.setenv(LD_LIBRARY_PATH=paste0(Sys.getenv("LD_LIBRARY_PATH"), ":/usr/local/lib"));install.packages("devtools");devtools::install_github("ironholds/poster")'
CMD ["R"]
