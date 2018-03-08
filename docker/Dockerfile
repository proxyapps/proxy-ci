from ubuntu:latest

ARG APP
ARG DEPS
ARG INSECURE

RUN apt-get -q update -y
RUN apt-get -qq install -y make cmake git g++ gfortran mercurial graphviz gnupg2 r-base r-base-core r-base-dev perl perl-base libopenmpi-dev 

RUN useradd -m ci

COPY proxy-ci/ /home/ci/proxy-ci
COPY spack /home/ci/spack
RUN rm -rf /home/ci/.ccache
COPY ccache/ /home/ci/.ccache
USER root
RUN chown -R ci:ci /home/ci/proxy-ci /home/ci/.ccache /home/ci/spack
USER ci

WORKDIR /home/ci
RUN cp proxy-ci/packages.yaml spack/etc/spack/packages.yaml
ENV SPACK=/home/ci/spack/bin/spack

RUN if [ -n "${DEPS}" ]; then ${SPACK} install -j4 -v ${DEPS}; else ${SPACK} install -j4 -v --only dependencies ${APP}; fi
RUN ${SPACK} ${INSECURE} install --source -j4 --only package -v ${APP}
