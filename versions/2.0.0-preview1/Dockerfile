#
# NOTE: THIS DOCKERFILE IS GENERATED VIA "./hack/run update"
#
# DO NOT EDIT IT DIRECTLY.
#
FROM microsoft/aspnetcore-build:2.0.0-preview1-jessie
# <pre>

MAINTAINER AusNimbus <support@ausnimbus.com.au>

LABEL summary="Platform for running .NET 2.0.0-preview1 applications" \
      maintainer="AusNimbus <support@ausnimbus.com.au>" \
      io.k8s.description="Platform for running .NET 2.0.0-preview1 applications" \
      io.k8s.display-name=".NET 2.0.0-preview1"

ENV HOME=/opt/app-root/src

USER 0

RUN apt-get update && apt-get install -y \
      rsync nano vim && \
    mkdir -p $HOME && \
    useradd -u 1001 -g 0 -d ${HOME} -s /sbin/nologin \
      -c "Default Application User" default && \
    chown -R 1001:0 /opt/app-root

# Copy common executable utilities.
COPY ./common/bin/ /usr/bin/

# Each container image can have 'contrib' a directory with extra files needed to
# run and build the application.
COPY ./contrib/ /opt/app-root

# </pre>
# <s2i>

RUN apt-get install -y git

ENV STI_SCRIPTS_PATH=/usr/libexec/s2i

LABEL io.openshift.expose-services="8080:http" \
    io.openshift.tags="builder,dotnet" \
    io.openshift.s2i.scripts-url=image:///usr/libexec/s2i

# Copy the S2I scripts from the specific language image to $STI_SCRIPTS_PATH
COPY ./s2i/bin/ $STI_SCRIPTS_PATH

# Directory with the sources is set as the working directory so all S2I scripts
# can execute relative to this path.
WORKDIR ${HOME}

EXPOSE 8080
CMD /usr/bin/base-usage

# </s2i>
# <template>

ENV ASPNETCORE_URLS http://+:8080

# </template>
# <post>

# Standardize on UID 999
RUN usermod -o -u 999 default

# Drop the root user and make the content of /opt/app-root owned by user default
RUN chown -R 999:0 /opt/app-root && chmod -R ug+rwx /opt/app-root
USER 999

ENTRYPOINT ["container-entrypoint"]

# </post>
