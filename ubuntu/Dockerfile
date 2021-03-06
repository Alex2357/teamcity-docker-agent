FROM teamcity-minimal-agent:latest

MAINTAINER Vladislav Rassokhin <vladislav.rassokhin@jetbrains.com>

LABEL dockerImage.teamcity.version="latest" \
      dockerImage.teamcity.buildNumber="latest"

ENV NUGET_XMLDOC_MODE=skip \
    DOTNET_CLI_TELEMETRY_OPTOUT=true \
    DOTNET_SKIP_FIRST_TIME_EXPERIENCE=true

RUN apt-get update && \
    apt-get install -y software-properties-common && add-apt-repository ppa:openjdk-r/ppa && apt-get update && \
    apt-get install -y git mercurial openjdk-8-jdk apt-transport-https ca-certificates && \
    \
    apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 9DC858229FC7DD38854AE2D88D81803C0EBFCD88 && \
    echo "deb [arch=amd64] https://download.docker.com/linux/ubuntu xenial stable" > /etc/apt/sources.list.d/docker.list && \
    \
    apt-cache policy docker-ce && \
    apt-get update && \
    apt-get install -y docker-ce=17.12.0~ce-0~ubuntu && \
    systemctl disable docker && \
    \
    apt-get install -y --no-install-recommends \
            libc6 \
            libcurl3 \
            libgcc1 \
            libgssapi-krb5-2 \
            libicu55 \
            liblttng-ust0 \
            libssl1.0.0 \
            libstdc++6 \
            libunwind8 \
            libuuid1 \
            zlib1g \
        && rm -rf /var/lib/apt/lists/* && \
    \
    curl -SL https://dotnetcli.blob.core.windows.net/dotnet/Sdk/2.1.4/dotnet-sdk-2.1.4-linux-x64.tar.gz --output dotnet.tar.gz \
        && mkdir -p /usr/share/dotnet \
        && tar -zxf dotnet.tar.gz -C /usr/share/dotnet \
        && rm dotnet.tar.gz \
        && ln -s /usr/share/dotnet/dotnet /usr/bin/dotnet && \
    \
    mkdir warmup \
        && cd warmup \
        && dotnet new \
        && cd .. \
        && rm -rf warmup \
        && rm -rf /tmp/NuGetScratch && \
    \
    apt-get clean all && \
    \
    usermod -aG docker buildagent

COPY run-docker.sh /services/run-docker.sh
