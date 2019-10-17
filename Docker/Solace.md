# Note of Solace / MQ

- [消息队列与JMS][https://blog.csdn.net/u012723607/article/details/77449764]

- [基于硬件的消息队列中间件 Solace 简介之二][https://blog.csdn.net/aqudgv83/article/details/80745222]

- [RabbitMQ（AMQP）与ActiveMQ（JMS）的比较](https://blog.csdn.net/u013905744/article/details/80538937)

- [ActiveMq和RabbitMq区别及其解析](https://blog.csdn.net/CLG_CSDN/article/details/90021459)

## 1. Install Solace in Docker

- [Docker Images][https://docs.solace.com/Solace-SW-Broker-Set-Up/Docker-Containers/Set-Up-Docker-Container-Image.htm]

  ### 1.1 Pull image

  ```linux
  docker pull solace/solace-pubsub-standard
  ```

  ### 1.2 Deploy to Docker

  ```linux
  docker run -d -p 8080:8080 -p 55555:55555 --shm-size=2g --env username_admin_globalaccesslevel=admin --env username_admin_password=admin --name=solace solace/solace-pubsub-standard:latest
  ```

  