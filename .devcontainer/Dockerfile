# See here for image contents: https://github.com/microsoft/vscode-dev-containers/tree/main/containers/python-3
# [Choice] Python version (use -bullseye variants on local arm64/Apple Silicon): 3, 3.10, 3.9, 3.8, 3.7, 3.6, 3-bullseye, 3.10-bullseye, 3.9-bullseye, 3.8-bullseye, 3.7-bullseye, 3.6-bullseye, 3-buster, 3.10-buster, 3.9-buster, 3.8-buster, 3.7-buster, 3.6-buster
ARG VARIANT=3-slim-bookworm
FROM python:${VARIANT} AS base

LABEL maintainer="toomore_such <info(at)webgent.com>"

# Install needed packages and setup non-root user. Use a separate RUN statement to add your own dependencies.

ENV LANG C.UTF-8
ENV DEBCONF_NOWARNINGS yes
ENV DEBIAN_FRONTEND noninteractive
# https://docs.python.org/3/using/cmdline.html#environment-variables
ENV PYTHONIOENCODING UTF-8
ENV PYTHONUTF8 1
# https://github.com/astral-sh/uv
ENV UV_NO_CACHE 1

RUN apt-get update -qq && \
    apt-get upgrade -y && \
    apt-get install -y --no-install-recommends ca-certificates curl && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*


# ====================
#  Development Stage
# ====================
FROM base AS develop

# Excute as root
ARG USER=vscode
ARG GROUP=${USER}
ARG UID=1000
ARG GID=${UID}
RUN groupadd -g $GID $GROUP && \
    useradd -m -s /bin/bash -u $UID -g $GID $USER

# Excute as user
WORKDIR /tmp
USER ${USER}
RUN mkdir -p apps
# https://github.com/astral-sh/uv
RUN curl -LsSf https://astral.sh/uv/install.sh | sh

# https://docs.python.org/3/using/cmdline.html#environment-variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1


# ====================
#  Production Stage
# ====================
FROM base AS production
