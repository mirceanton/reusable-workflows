## ================================================================================================
# Utility versions
## ================================================================================================
ARG ACTIONLINT_VERSION=1.7.3@sha256:7617f05bd698cd2f1c3aedc05bc733ccec92cca0738f3e8722c32c5b42c70ae6
ARG SHELLCHECK_VERSION=v0.10.0@sha256:2097951f02e735b613f4a34de20c40f937a6c8f18ecb170612c88c34517221fb


## ================================================================================================
# "Build" stage for utilities with docker images already present
## ================================================================================================
FROM rhysd/actionlint:${ACTIONLINT_VERSION} AS actionlint
FROM koalaman/shellcheck:${SHELLCHECK_VERSION} AS shellcheck

## ================================================================================================
## Main image
## ================================================================================================
FROM mcr.microsoft.com/devcontainers/base:bookworm@sha256:b3fd61ae07a1ebd211cac645c758f4018688a86e2c9b596a6c862ca89b0a668d
ENV EDITOR=vim

# Install tools
COPY --from=actionlint /usr/local/bin/actionlint /usr/local/bin/actionlint
COPY --from=shellcheck /bin/shellcheck /usr/local/bin/shellcheck

# Install additional OS packages
RUN DEBIAN_FRONTEND=noninteractive \
	apt-get update && apt-get upgrade -y && \
	apt-get install -y \
	sudo \
	git \
	bash-completion \
	vim \
	curl \
	wget \
	unzip \
	htop \
	net-tools \
	iputils-ping \
	docker-compose \
	dnsutils && \
	apt-get clean && \
	rm -rf /var/lib/apt/lists/*

RUN usermod -aG docker vscode
USER vscode

ENTRYPOINT [ "/bin/bash", "-l", "-c" ]
