FROM python:3.10-slim

RUN apt-get update && apt-get install -y \
    openjdk-17-jdk \
    wget \
    curl \
    git \
    procps \
    lsb-release \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

ENV JAVA_HOME=/usr/lib/jvm/java-1.17.0-openjdk-amd64
ENV PATH=$JAVA_HOME/bin:$PATH

RUN mkdir /workspace
WORKDIR /workspace

# Copy requirements.txt to the container
COPY requirements.txt .

# Update pip to the latest version
RUN pip install --upgrade pip

# Install Python dependencies via pip
RUN pip install --no-cache-dir -r requirements.txt

CMD ["bash"]
