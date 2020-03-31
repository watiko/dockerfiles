FROM debian:buster

ARG AWS_REGION="ap-northeast-1"

RUN apt-get update \
  && apt-get install --no-install-recommends -y \
    wget \
    curl \
    less \
    vim-nox \
  && rm -rf /var/lib/apt/lists/*

ARG SSM_AGENT_DEB="https://s3.${AWS_REGION}.amazonaws.com/amazon-ssm-${AWS_REGION}/latest/debian_amd64/amazon-ssm-agent.deb"

RUN mkdir /tmp/ssm \
  && cd /tmp/ssm \
  && wget "$SSM_AGENT_DEB" \
    -O amazon-ssm-agent.deb \
  && dpkg -i amazon-ssm-agent.deb \
  && rm amazon-ssm-agent.deb \
  && cp /etc/amazon/ssm/seelog.xml.template /etc/amazon/ssm/seelog.xml

ENTRYPOINT bash -c "\
  set -u;\
  amazon-ssm-agent -register -code "\$SSM_ACTIVATION_CODE" -id "\$SSM_ACTIVATION_ID" -region "\$AWS_REGION";\
  exec amazon-ssm-agent start\
"

