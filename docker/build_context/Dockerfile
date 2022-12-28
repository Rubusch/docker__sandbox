FROM debian:11 AS sandbox
MAINTAINER Lothar Rubusch <l.rubusch@gmail.com>
ENV DEBIAN_FRONTEND=noninteractive


################################################################################
## the USER is needed to make it available as --build-arg
ARG USER="${USER}"
ENV MY_CONFIGS="/home/$USER/configs"


################################################################################
## set up apt fully functional
USER root
RUN apt-get update && apt-get upgrade -y && apt-get autoremove -y --purge

## container essentials - linux build, patch creation
RUN apt-get install -y locales \
	zlib1g-dev \
	g++ \
	build-essential \
	sudo \
	libssl-dev \
	bison \
	flex \
	bc \
	libncurses5-dev \
	libelf-dev \
	python3-all \
	rsync \
	binutils \
	unzip \
	bzip2 \
	make \
	gcc \
	cpio \
	libc6-dev \
	fakeroot \
	udev \
	hdparm \
	xmlto \
	perl \
	wget \
	curl \
	bin86

## kernel - generate documentation
RUN apt-get install -y doxygen \
	kmod \
	imagemagick \
	dvipng \
	fonts-noto-cjk \
	autoconf \
	libtool \
	libtool-bin \
	aptitude \
	bsdmainutils \
	i2c-tools \
	device-tree-compiler \
	graphviz \
	u-boot-tools \
	codespell \
	clang-format \
	clang-tools \
	clang-tidy \
	exuberant-ctags \
	tree \
	mc \
	colordiff \
	tig \
	dos2unix \
	emacs-nox \
	vim

## special develop
RUN apt-get install -y chrpath \
	diffstat \
	gawk \
	texinfo \
	libsdl1.2-dev \
	xterm \
	screen \
	x11-apps \
	emacs-goodies-el \
	emacs-goodies-extra-el \
	exuberant-ctags

## special - git
RUN apt-get install -y git-all


################################################################################
## locale sttings
USER root
RUN locale-gen
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

## locale-hack, needed for debian since locale-gen <arg> seems not to work
RUN sed -i '/^#.* en_US.UTF-8.* /s/^#//' /etc/locale.gen
RUN locale-gen


################################################################################
## user settings
USER root
RUN useradd -m -s /bin/bash "$USER"
RUN usermod -a -G sudo "$USER"
RUN sed -i '/%sudo/s/.*/%sudo   ALL=NOPASSWD: ALL/' /etc/sudoers
RUN mkdir -p /usr/local
RUN chown -R "${USER}":"${USER}" /usr/local
RUN mkdir -p /opt
RUN chown -R "${USER}":"${USER}" /opt
RUN adduser "$USER" dialout


################################################################################
## get sources
USER ${USER}
WORKDIR /home/${USER}

## bash config
RUN rm -f /home/$USER/.bashrc && ln -s ${MY_CONFIGS}/.bashrc /home/$USER/

## vim config
RUN rm -f /home/$USER/.vimrc && ln -s ${MY_CONFIGS}/.vimrc /home/$USER/

## emacs config
RUN rm -f /home/$USER/.emacs && ln -s ${MY_CONFIGS}/.emacs /home/$USER/

## user
RUN echo "export USER=${USER}" >> /home/$USER/.profile

## ssh preparations
RUN mkdir -p /home/$USER/.ssh


################################################################################
## command mode
USER ${USER}
WORKDIR /home/$USER
COPY *.sh /usr/local/bin/
CMD ["/bin/cat", "/etc/issue"]
