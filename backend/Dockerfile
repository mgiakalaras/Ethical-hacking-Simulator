
FROM debian:bookworm-slim

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    dos2unix \
    netcat-openbsd \
    bash \
    vim \
    openssh-server \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /app

RUN mkdir -p /var/run/sshd

RUN echo 'root:anwar' | chpasswd \
    && sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config \
    && sed -i 's/#PasswordAuthentication yes/PasswordAuthentication yes/' /etc/ssh/sshd_config \
    && sed -i 's/#Port 22/Port 2222/' /etc/ssh/sshd_config

COPY requirements.txt .

RUN pip3 install --no-cache-dir --break-system-packages -r requirements.txt

COPY . .

RUN dos2unix /app/entrypoint.sh \
    && chmod +x /app/entrypoint.sh

EXPOSE 2222
EXPOSE 5000

RUN mkdir -p /var/run/docker

ENTRYPOINT ["/app/entrypoint.sh"]
