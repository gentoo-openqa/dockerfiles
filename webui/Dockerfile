FROM opensuse:42.3
LABEL maintainer Jan Sedlak <jsedlak@redhat.com>, Josef Skladanka <jskladan@redhat.com>, wnereiz <wnereiz@eienteiland.org>, Sergio Lindo Mansilla <slindomansilla@suse.com>
LABEL version="0.2"

RUN zypper ar -f obs://devel:openQA/openSUSE_Leap_42.3 openQA && \
    zypper ar -f obs://devel:openQA:Leap:42.3/openSUSE_Leap_42.3 openQA-perl-modules && \
    zypper --gpg-auto-import-keys ref && \
    zypper --non-interactive in ca-certificates-mozilla curl && \
    zypper --non-interactive in --force-resolution openQA apache2 hostname which w3m


# setup apache
RUN gensslcert && \
    a2enmod headers && \
    a2enmod proxy && \
    a2enmod proxy_http && \
    a2enmod proxy_wstunnel && \
    a2enmod ssl && \
    a2enflag SSL
ADD openqa-ssl.conf /etc/apache2/vhosts.d/openqa-ssl.conf
ADD openqa.conf /etc/apache2/vhosts.d/openqa.conf
ADD run_openqa.sh /root/

# set-up shared data and configuration
RUN chmod +x /root/run_openqa.sh && \
    mkdir -p /var/lib/openqa/pool && \
    mkdir -p /run/dbus && \
    chown -R geekotest /usr/share/openqa /var/lib/openqa /var/log/openqa && \
    chmod ug+rw /usr/share/openqa /var/lib/openqa /var/log/openqa && \
    find /usr/share/openqa /var/lib/openqa /var/log/openqa -type d -exec chmod ug+x {} \;

VOLUME ["/etc/openqa/","/var/lib/openqa/"]
EXPOSE 80 443
CMD ["/root/run_openqa.sh"]
