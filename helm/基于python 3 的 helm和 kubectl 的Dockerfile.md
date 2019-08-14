```dockerfile
FROM python:rc-alpine3.10

ARG VCS_REF
ARG BUILD_DATE

ENV KUBE_LATEST_VERSION="v1.15.0"
# https://github.com/kubernetes/helm/releases
ENV HELM_VERSION="v3.0.0-alpha.2"

RUN set -eux && apk add --no-cache ca-certificates bash git openssh curl \
    && wget -q https://storage.googleapis.com/kubernetes-release/release/${KUBE_LATEST_VERSION}/bin/linux/amd64/kubectl -O /usr/local/bin/kubectl \
    && chmod +x /usr/local/bin/kubectl \
    && wget -q https://get.helm.sh/helm-${HELM_VERSION}-linux-amd64.tar.gz -O - | tar -xzO linux-amd64/helm > /usr/local/bin/helm \
    && chmod +x /usr/local/bin/helm

WORKDIR /config

CMD bash
```



