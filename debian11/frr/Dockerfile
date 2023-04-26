ARG BASE=debian11
FROM kathara/base:${BASE}
ARG DEBIAN_FRONTEND="noninteractive"

# FRR is integrated in official debian packet sources
#RUN curl -s https://deb.frrouting.org/frr/keys.asc | apt-key add -
#RUN echo deb https://deb.frrouting.org/frr $(lsb_release -s -c) "frr-7" | tee -a /etc/apt/sources.list.d/frr.list

RUN apt update && \
    apt install -y frr frr-pythontools frr-rpki-rtrlib && \
    apt clean && \
    rm -rf /tmp/* /var/lib/apt/lists/* /var/tmp/*

RUN sed -i -e 's/service/no service/' /etc/frr/vtysh.conf \
    && rm /etc/frr/frr.conf
