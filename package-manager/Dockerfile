ARG R_VERSION=3.6.2
FROM rstudio/r-base:${R_VERSION}-bionic
LABEL maintainer="RStudio Docker <docker@rstudio.com>"

# Locale configuration --------------------------------------------------------#
RUN localedef -i en_US -f UTF-8 en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update --fix-missing && apt-get install -y gdebi-core libssl1.0.0 libssl-dev

# Install RStudio Package Manager ---------------------------------------------#
ARG RSPM_VERSION=1.1.2-10
RUN curl -O curl -O https://s3.amazonaws.com/rstudio-package-manager/ubuntu/amd64/rstudio-pm_${RSPM_VERSION}_amd64.deb && \
    gdebi -n rstudio-pm_${RSPM_VERSION}_amd64.deb && \
    rm rstudio-pm_${RSPM_VERSION}_amd64.deb
RUN mkdir -p /var/run/rstudio-pm
ENV PATH /opt/rstudio-pm/bin:$PATH

# Runtime settings ------------------------------------------------------------#
ARG TINI_VERSION=0.18.0
RUN curl -L -o /usr/local/bin/tini https://github.com/krallin/tini/releases/download/v${TINI_VERSION}/tini && \
    chmod +x /usr/local/bin/tini

COPY startup.sh /usr/local/bin/startup.sh
RUN chmod +x /usr/local/bin/startup.sh

EXPOSE 4242/tcp
ENV RSPM_LICENSE ""
ENV RSPM_LICENSE_SERVER ""
COPY rstudio-pm.gcfg /etc/rstudio-pm/rstudio-pm.gcfg
VOLUME ["/data"]

ENTRYPOINT ["tini", "--"]
CMD ["/usr/local/bin/startup.sh"]
