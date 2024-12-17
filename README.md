# 0net_hls_ace_arm

Repository with Zeronet, HLSProxy, Acestream for ARM machines

## 0net ARM

````
cd 0net
docker build -t 0net:arm64v8 .
docker compose up -d
````

## Acestream ARM

````
cd acestream-arm
docker build -t acestream-arm .
docker compose up -d
````

## HLSProxy ARM

````
cd hlsproxy
docker build -t hlsproxy:arm .
docker compose up -d
````
