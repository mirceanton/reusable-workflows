## ================================================================================================
# Utility versions
## ================================================================================================
ARG ACTIONLINT_VERSION=1.7.2@sha256:89d3f90f82781dee3c8724651129634b08cf2241bbd67fcd02a1c5198119fc5e
ARG SHELLCHECK_VERSION=v0.10.0@sha256:2097951f02e735b613f4a34de20c40f937a6c8f18ecb170612c88c34517221fb


## ================================================================================================
# "Build" stage for utilities with docker images already present
## ================================================================================================
FROM rhysd/actionlint:${ACTIONLINT_VERSION} AS actionlint
FROM koalaman/shellcheck:${SHELLCHECK_VERSION} AS shellcheck

## ================================================================================================
## Main image
## ================================================================================================
FROM mcr.microsoft.com/devcontainers/base:bookworm@sha256:37eec4b19c24313f2bf77f031b9045bc0275b697d8daaed399811f9c496df24e
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
