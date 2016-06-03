通过 OpenSSL 命令来生成 CA 证书、服务器私钥、客户端证书、签名，openssl 命令比较复杂。
执行脚本后，当前目录下会生成certs目录，里面存放生成的所有证书文件。

- docker daemon的配置
其中，CA和服务器私钥有`ca.pem`,`server-cert.pem`,`server-key.pem`。
将这三个文件放到`docker`存放证书的目录下。
```
mkdir -p /etc/docker/certs
cd certs/
cp ca.pem server-cert.pem server-key.pem /etc/docker/certs/
```
配置docker使用证书
```
vim /etc/default/docker
DOCKER_OPTS='--tlsverify --tlscacert=/etc/docker/certs/ca.pem --tlscert=/etc/docker/certs/server-cert.pem --tlskey=/etc/docker/certs/server-key.pem'
```

- docker客户端的配置
将`certs`目录下的`ca.pem`,`cert.pem`,`key.pem`放到`~/.docker`目录下
```
cd certs/
cp ca.pem cert.pem key.pem ~/.docker
```

测试
```
docker --tlsverify images
```


