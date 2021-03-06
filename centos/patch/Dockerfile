FROM custom-code-server:centos

USER root

# install more tools
RUN dnf update && \
    dnf -y install epel-release && \
    dnf -y install \
    pass screen && \
    rm -rf /var/cache /var/log/dnf* /var/log/yum.*

# see https://docs.microsoft.com/en-us/sql/connect/odbc/linux-mac/installing-the-microsoft-odbc-driver-for-sql-server
ENV DOTNET_CLI_TELEMETRY_OPTOUT=true
# hadolint ignore=DL3008
RUN rpm --import https://packages.microsoft.com/keys/microsoft.asc && \
    curl -sL curl https://packages.microsoft.com/config/rhel/8/prod.repo > /etc/yum.repos.d/mssql-release.repo && \
    yum remove unixODBC-utf16 unixODBC-utf16-devel && \
    echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> /etc/profile.d/mssql.sh && \
    ACCEPT_EULA=Y yum -y install dotnet-sdk-3.1 msodbcsql17 mssql-tools unixODBC-devel && \
    rm -rf /var/cache /var/log/dnf* /var/log/yum.*

# install: powershell
# hadolint ignore=DL3008 
# RUN yum -y install powershell && \
#    rm -rf /var/cache /var/log/dnf* /var/log/yum.*

# TODO: patch code-server with webdav support / work upstream
# /usr/local/lib/code-server/
# create diff -u
# run patch command

# TODO - add some magic
# RUN curl -Ls -o /tmp/kustomize.tar.gz https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize/v3.8.1/kustomize_v3.8.1_linux_amd64.tar.gz && \
#    tar -C /opt/app-root/bin -zxf /tmp/kustomize.tar.gz kustomize && \
#    rm /tmp/kustomize.tar.gz

# install: skopeo
# hadolint ignore=DL3008
RUN dnf -y install skopeo fuse-overlayfs && \
    rm -rf /var/cache /var/log/dnf* /var/log/yum.*

# install: gurobi
ENV GUROBI_HOME=/opt/gurobi/linux64
ENV PATH=${PATH}:${GUROBI_HOME}/bin
ENV LD_LIBRARY_PATH=${LD_LIBRARY_PATH}:${GUROBI_HOME}/lib

# TODO - get gurobi into pypi
ARG GUROBI_URL=https://packages.gurobi.com/9.1/gurobi9.1.1_linux64.tar.gz

# Should update this install step to not use cd (without breaking it)
# hadolint ignore=DL3003
RUN curl -s "${GUROBI_URL}" | tar zxf - -C /opt && \
    mv /opt/gurobi911 /opt/gurobi && \
    cd "${GUROBI_HOME}" && \
    python3 setup.py install && \
    ln -s /usr/bin/python3.6 /opt/gurobi/linux64/bin/python3.6

# install: node v12
# hadolint ignore=SC2002,DL3008 
RUN dnf -y module install nodejs:12 && \
    rm -rf /var/cache /var/log/dnf* /var/log/yum.*

# install: angular cli
# setup npm registry / bash completion
RUN npm install -g @angular/cli@11.2.5 && \
    npm completion > /etc/bash_completion.d/npm

USER 1001
