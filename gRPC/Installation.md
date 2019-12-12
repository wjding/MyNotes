# gRPC Installation
## protobuf installation

   Install protoc following
   https://github.com/protocolbuffers/protobuf/blob/master/src/README.md. 

   Notes:
   1. sudo apt-get install autoconf automake libtool curl make g++ unzip
   1. Download from https://github.com/protocolbuffers/protobuf/releases/latest
   1. ./configure
   1. make
   1. make check
   1. sudo make install
   1. sudo ldconfig

## gRPC installation

   Follow https://github.com/grpc/grpc/blob/master/BUILDING.md to install gRPC

   1. Require additionally: golang, libssl-dev
   1. Before running "cmake ../.." Need to set below variables to make sure it sues clang (rather than gcc) to compile:
      1. CC=clang-6.0
      1. CXX=clang++-6.0
   1. If non-pre-compiled dependencies are used, below command should be used to run cmake:

      ```cmake -DgRPC_INSTALL=ON -DgRPC_BUILD_TESTS=OFF -DgRPC_PROTOBUF_PROVIDER=package -DgRPC_ZLIB_PROVIDER=package -DgRPC_CARES_PROVIDER=package -DgRPC_SSL_PROVIDER=package -DCMAKE_BUILD_TYPE=Release ../..```

   For more informaiton, please see this [issue](https://github.com/grpc/grpc/issues/13841).

