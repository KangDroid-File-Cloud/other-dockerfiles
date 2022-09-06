FROM jenkins/inbound-agent

USER root

# Make sure we do not have interactive console while building.
ENV DEBIAN_FRONTEND=noninteractive

# Use Docker CLI(CLI Only)
COPY --from=docker:20.10.17-cli /usr/local/bin/docker /usr/local/bin/docker
COPY --from=docker:20.10.17-cli /usr/local/bin/docker-compose /usr/local/bin/docker-compose

# Install Dependencies (Does not support System.Drawing.Common)
RUN apt update && apt install -y libc6 libgcc-s1 libgssapi-krb5-2 libicu67 libssl1.1 libstdc++6 libstdc++6 curl

# Install .NET SDK
WORKDIR /dotnet
RUN curl -L https://dot.net/v1/dotnet-install.sh > install-dotnet.sh && chmod a+x install-dotnet.sh
RUN ./install-dotnet.sh -c Current

# Install Dagger.io
WORKDIR /dagger
RUN curl -L https://dl.dagger.io/dagger/install.sh > install.sh && chmod a+x install.sh
RUN ./install.sh

# Install Mongosh
WORKDIR /mongo
RUN curl -L 'https://downloads.mongodb.com/compass/mongosh-1.5.4-linux-x64.tgz' -o ./mongosh.tgz
RUN tar -xzvf ./mongosh.tgz
RUN mv mongosh-1.5.4-linux-x64/bin/* /usr/bin/
RUN chmod a+x /usr/bin/mongosh

# Cleanup APT Cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Setup Environment
ENV PATH="${PATH}:/dagger/bin:/root/.dotnet:/root/.dotnet/tools"
