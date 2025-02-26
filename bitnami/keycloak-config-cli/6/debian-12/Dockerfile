# Copyright Broadcom, Inc. All Rights Reserved.
# SPDX-License-Identifier: APACHE-2.0

FROM ubuntu:noble

ARG DOWNLOADS_URL="downloads.bitnami.com/files/stacksmith"
ARG TARGETARCH

ENV HOME="/" \
    OS_ARCH="${TARGETARCH:-amd64}" \
    OS_FLAVOUR="debian-12" \
    OS_NAME="linux"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "errexit", "-o", "nounset", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN apt-get update && apt-get install -y ca-certificates curl procps zlib1g
RUN mkdir -p /tmp/bitnami/pkg/cache/ ; cd /tmp/bitnami/pkg/cache/ ; \
    COMPONENTS=( \
      "jre-21.0.6-10-1-linux-${OS_ARCH}-debian-12" \
      "keycloak-config-cli-6.4.0-0-linux-${OS_ARCH}-debian-12" \
    ) ; \
    for COMPONENT in "${COMPONENTS[@]}"; do \
      if [ ! -f "${COMPONENT}.tar.gz" ]; then \
        curl -SsLf "https://${DOWNLOADS_URL}/${COMPONENT}.tar.gz" -O ; \
        curl -SsLf "https://${DOWNLOADS_URL}/${COMPONENT}.tar.gz.sha256" -O ; \
      fi ; \
      sha256sum -c "${COMPONENT}.tar.gz.sha256" ; \
      tar -zxf "${COMPONENT}.tar.gz" -C /opt/bitnami --strip-components=2 --no-same-owner --wildcards '*/files' ; \
      rm -rf "${COMPONENT}".tar.gz{,.sha256} ; \
    done
RUN apt-get autoremove --purge -y curl && \
    apt-get update && apt-get upgrade -y && \
    apt-get clean && rm -rf /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami
RUN find / -perm /6000 -type f -exec chmod a-s {} \; || true

# Replace with latest version of the keycloak-config-cli
RUN rm /opt/bitnami/keycloak-config-cli/keycloak-config-cli.jar
COPY assets/keycloak-config-cli.jar /opt/bitnami/keycloak-config-cli/keycloak-config-cli.jar

# Remove old versions of keycloak-config-cli to address vulns
RUN rm /opt/bitnami/keycloak-config-cli/keycloak-config-cli-24.0.5.jar
RUN rm /opt/bitnami/keycloak-config-cli/keycloak-config-cli-25.0.1.jar

RUN ln -sf /opt/bitnami/keycloak-config-cli/keycloak-config-cli.jar /opt/keycloak-config-cli.jar

ENV APP_VERSION="6.4.0" \
    BITNAMI_APP_NAME="keycloak-config-cli" \
    PATH="/opt/bitnami/java/bin:$PATH"

WORKDIR /opt/bitnami/keycloak-config-cli
# ubuntu user is 1000
USER 1000
ENTRYPOINT [ "java", "-jar", "./keycloak-config-cli.jar" ]
