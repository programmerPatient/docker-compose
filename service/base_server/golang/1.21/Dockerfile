FROM golang:1.21

LABEL maintainer="Ving <ving@nivin.cn>"

ENV GOPROXY https://goproxy.cn,direct

# 安装必要的软件包和依赖包
USER root

# 设置阿里云镜像源（Debian 10 Buster）
# RUN echo "deb http://mirrors.aliyun.com/debian/ buster main contrib non-free" > /etc/apt/sources.list
# RUN echo "deb http://mirrors.aliyun.com/debian/ buster-updates main contrib non-free" >> /etc/apt/sources.list
# RUN echo "deb http://mirrors.aliyun.com/debian/ buster-backports main contrib non-free" >> /etc/apt/sources.list
# RUN echo "deb http://mirrors.aliyun.com/debian-security/ buster/updates main contrib non-free" >> /etc/apt/sources.list


# # 使用网易源镜像（Debian 10 Buster）
# RUN echo "deb http://mirrors.163.com/debian/ buster main contrib non-free" > /etc/apt/sources.list
# RUN echo "deb http://mirrors.163.com/debian/ buster-updates main contrib non-free" >> /etc/apt/sources.list
# RUN echo "deb http://mirrors.163.com/debian/ buster-backports main contrib non-free" >> /etc/apt/sources.list
# RUN echo "deb http://mirrors.163.com/debian-security/ buster/updates main contrib non-free" >> /etc/apt/sources.list

# 使用中国科技大学源镜像（Debian 10 Buster）
# RUN echo "deb http://mirrors.ustc.edu.cn/debian/ buster main contrib non-free" > /etc/apt/sources.list
# RUN echo "deb http://mirrors.ustc.edu.cn/debian/ buster-updates main contrib non-free" >> /etc/apt/sources.list
# RUN echo "deb http://mirrors.ustc.edu.cn/debian/ buster-backports main contrib non-free" >> /etc/apt/sources.list
# RUN echo "deb http://mirrors.ustc.edu.cn/debian-security/ buster/updates main contrib non-free" >> /etc/apt/sources.list

# 使用清华大学源镜像（Debian 10 Buster）
RUN echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster main contrib non-free" > /etc/apt/sources.list
RUN echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster-updates main contrib non-free" >> /etc/apt/sources.list
RUN echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian/ buster-backports main contrib non-free" >> /etc/apt/sources.list
RUN echo "deb http://mirrors.tuna.tsinghua.edu.cn/debian-security/ buster/updates main contrib non-free" >> /etc/apt/sources.list

RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends \
    curl \
    zip \
    unzip \
    git \
    vim 

# 安装 goctl
USER root
RUN GOPROXY=https://goproxy.cn/,direct go install github.com/zeromicro/go-zero/tools/goctl@latest

# 安装 protoc
USER root
RUN curl -L -o /tmp/protoc.zip https://github.com/protocolbuffers/protobuf/releases/download/v3.19.1/protoc-3.19.1-linux-x86_64.zip && \
    unzip -d /tmp/protoc /tmp/protoc.zip && \
    mv /tmp/protoc/bin/protoc $GOPATH/bin

# 安装 protoc-gen-go
USER root
RUN go install google.golang.org/protobuf/cmd/protoc-gen-go@latest

# 安装 protoc-gen-go-grpc
USER root
RUN go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@latest


# 安装 air 工具
USER root
RUN go install github.com/cosmtrek/air@v1.42.0
RUN go install github.com/go-delve/delve/cmd/dlv@latest
ENTRYPOINT ["air"]

# 安装Swag
RUN go install github.com/swaggo/swag/cmd/swag@latest

# $GOPATH/bin添加到环境变量中
ENV PATH $GOPATH/bin:$PATH


# 清理垃圾
USER root
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    rm /var/log/lastlog /var/log/faillog
    

# 设置工作目录
WORKDIR /var/www