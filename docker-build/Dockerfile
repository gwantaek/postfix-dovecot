FROM ubuntu:20.04

ENV DEBIAN_FRONTEND 'noninteractive' 
ENV MAILNAME 'host.example.com'
ENV MAIN_MAILER_TYPE 'Internet Site'

RUN useradd -m -s /bin/bash incoming \
    && echo "incoming:incoming" | chpasswd \
    && echo "postfix postfix/mailname string $MAILNAME" | debconf-set-selections \
    && echo "postfix postfix/main_mailer_type string $MAIN_MAILER_TYPE" | debconf-set-selections \
    && apt-get update \
    && apt-get install tzdata postfix dovecot-imapd -y \
    && postconf -e "home_mailbox = Maildir/"

COPY docker-build/docker-entrypoint.sh /
COPY docker-build/postfix-conf/10-mail.conf /etc/dovecot/conf.d
COPY docker-build/postfix-conf/10-ssl.conf /etc/dovecot/conf.d
COPY docker-build/postfix-conf/10-auth.conf /etc/dovecot/conf.d

ENTRYPOINT ["/docker-entrypoint.sh"]

EXPOSE 25 143 465 993

STOPSIGNAL SIGTERM
