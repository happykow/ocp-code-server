FROM custom-code-server:debian-base

USER root

# install more tools
RUN apt-get update && \
    apt-get -y install \
    pass screen && \
    rm -rf /var/lib/apt/lists/*

## install azure cli tools
#RUN curl -sL https://packages.microsoft.com/keys/microsoft.asc | \
#    gpg --dearmor | tee /etc/apt/trusted.gpg.d/microsoft.asc.gpg > /dev/null
##    echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/azure-cli.list && \
##    apt-get update && \
##    apt-get -y install azure-cli && \
##    rm -rf /var/lib/apt/lists/*
#
## see https://docs.microsoft.com/en-us/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server
#ENV DOTNET_CLI_TELEMETRY_OPTOUT=true
## hadolint ignore=DL3008
#RUN curl -sL https://packages.microsoft.com/config/debian/10/prod.list > /etc/apt/sources.list.d/microsoft-prod.list && \
#    apt-get update && \
#    ACCEPT_EULA=Y apt-get -y install --no-install-recommends dotnet-sdk-3.1 msodbcsql17 mssql-tools unixodbc-dev && \
#    rm -rf /var/lib/apt/lists/*
#
## install: powershell
## hadolint ignore=DL3008 
#RUN apt-get update && \
#    apt-get -y install --no-install-recommends powershell && \
#    rm -rf /var/lib/apt/lists/*
#
## TODO: patch code-server with webdav support / work upstream
## /usr/local/lib/code-server/
## create diff -u
## run patch command
#
## TODO - add some magic
## RUN curl -Ls -o /tmp/kustomize.tar.gz https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize/v3.8.1/kustomize_v3.8.1_linux_amd64.tar.gz && \
##    tar -C /opt/app-root/bin -zxf /tmp/kustomize.tar.gz kustomize && \
##    rm /tmp/kustomize.tar.gz

# install: skopeo
# hadolint ignore=DL3008
RUN echo 'deb http://download.opensuse.org/repositories/devel:/kubic:/libcontainers:/stable/Debian_10/ /' > /etc/apt/sources.list.d/devel:kubic:libcontainers:stable.list && \
    curl -sL https://download.opensuse.org/repositories/devel:kubic:libcontainers:stable/Debian_10/Release.key | apt-key add - && \
    apt-get update && \
    apt-get -y --no-install-recommends install skopeo && \
    rm -rf /var/lib/apt/lists/*

## install: gurobi
#ENV GUROBI_HOME=/opt/gurobi/linux64
#ENV PATH=${PATH}:${GUROBI_HOME}/bin
#ENV LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:${GUROBI_HOME}/lib
#
## TODO - get gurobi into pypi
#ARG GUROBI_URL=https://packages.gurobi.com/9.1/gurobi9.1.1_linux64.tar.gz
#
## Should update this install step to not use cd (without breaking it)
## hadolint ignore=DL3003
#RUN curl -s "${GUROBI_URL}" | tar zxf - -C /opt && \
#    mv /opt/gurobi911 /opt/gurobi && \
#    cd "${GUROBI_HOME}" && \
#    python3 setup.py install && \
#    rm /opt/gurobi/linux64/bin/python3.7 && \
#    ln -s /usr/bin/python3.7 /opt/gurobi/linux64/bin/python3.7
#
## install: node v12
#ARG NODE_HASH="b018082c06206162bb03b97cf7e9463b7e63e7d4fb1024ec9591f071a0ca7a56"
## hadolint ignore=SC2002,DL3008 
#RUN curl -sL https://deb.nodesource.com/setup_12.x -o setup_12.x.sh && \
#    sha256sum < setup_12.x.sh && \
#    [ "$NODE_HASH  -" = "$(sha256sum < setup_12.x.sh)" ] && \
#    echo "node: checksum PASSED" &&  \
#    bash setup_12.x.sh && \
#    apt-get -y install --no-install-recommends nodejs && \
#    rm -rf /var/lib/apt/lists
#
## install: angular cli
## setup npm registry / bash completion
#RUN npm install -g @angular/cli@11.2.5 && \
#    npm completion > /etc/bash_completion.d/npm

USER coder
