FROM ubuntu:xenial

RUN apt-get update \
     && apt-get install -y wget software-properties-common \
     && rm -rf /var/lib/apt/lists/*

WORKDIR "/root"

RUN add-apt-repository -y ppa:ethereum/ethereum

ARG binary
RUN apt-get update \
     && apt-get install -y ethereum

ARG password
ARG privatekey
RUN echo $password > ~/.accountpassword
RUN echo $privatekey > ~/.privatekey
ADD ./genesis.json ./genesis.json
RUN geth init genesis.json
RUN geth account import --password ~/.accountpassword  ~/.privatekey

ENV address=""
ENV bootnodeId=""
ENV bootnodeIp=""
ENV rpcport=""
CMD exec geth --shh --bootnodes "enode://$bootnodeId@$bootnodeIp:30301" --networkid "6660001" --verbosity=4 --rpc --rpcport "$rpcport" --rpcaddr "0.0.0.0" --rpcapi "eth,web3,shh,net,admin,debug,db" --rpccorsdomain "*" --syncmode="full"

ARG port
EXPOSE $port
EXPOSE 30303
