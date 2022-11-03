FROM python:3.10-alpine
LABEL maintainer='Codeship Inc., <maintainers@codeship.com>'

# which version of the AWS CLI to install.
# https://pypi.python.org/pypi/awscli
ARG AWS_CLI_VERSION="1.25.95"

ENV PIP_DISABLE_PIP_VERSION_CHECK=true

RUN \
  pip install awscli==${AWS_CLI_VERSION} && \
  mkdir -p "${HOME}/.aws" && \
  apk --no-cache add \
    bash \
    curl \
    groff \
    jq \
    less \
    zip \
    python3 \
    python3-dev \
    py3-pip \
    && pip install virtualenv

# install kubectl from AWS
# https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html
ARG KUBECTL_VERSION="1.19.6"
ARG KUBECTL_BUILD_DATE="2021-01-05"

RUN curl -L https://amazon-eks.s3-us-west-2.amazonaws.com/${KUBECTL_VERSION}/${KUBECTL_BUILD_DATE}/bin/linux/amd64/kubectl > /usr/local/bin/kubectl \
    && chmod +x /usr/local/bin/kubectl

# install aws-iam-authenticator
RUN curl -L "https://amazon-eks.s3-us-west-2.amazonaws.com/${KUBECTL_VERSION}/${KUBECTL_BUILD_DATE}/bin/linux/amd64/aws-iam-authenticator" > /usr/local/bin/aws-iam-authenticator \
    && chmod +x /usr/local/bin/aws-iam-authenticator

RUN virtualenv root/.codeship-venv

ENV CODESHIP_VIRTUALENV="/root/.codeship-venv"

COPY scripts/ /usr/bin/
