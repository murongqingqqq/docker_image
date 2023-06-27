FROM ubuntu:latest

RUN apt-get update && \
    apt-get install -y build-essential git python3 python3-dev python3-setuptools libcap-dev libssl-dev libldap2-dev libgpgme11-dev libavahi-client-dev libacl1-dev libarchive-dev libattr1-dev libblkid-dev libgnutls28-dev libreadline-dev pkg-config wget

RUN wget https://download.samba.org/pub/samba/stable/samba-4.14.6.tar.gz && \
    tar -xvf samba-4.14.6.tar.gz && \
    cd samba-4.14.6 && \
    ./configure --enable-debug --enable-selftest --with-modulesdir=/usr/lib/x86_64-linux-gnu/samba --with-lockdir=/var/run/samba --with-piddir=/var/run/samba --with-privatedir=/etc/samba --with-systemd --with-cluster-support --with-acl-support --with-shared-modules=idmap_ad,idmap_rid,idmap_adex,idmap_hash,idmap_tdb2,vfs_default_quota,vfs_fruit,vfs_glusterfs,vfs_shadow_copy2,vfs_streams_depot --with-ldap --with-ads --with-gpgme --with-avahi --with-pam --with-quotas --with-syslog --with-utmp --with-dnsupdate --with-json-audit --with-socket-wrapper --with-libsmbclient --with-winbind --with-automount --with-pie --with-tdb --with-ldb --with-system-mitkrb5 --with-system-mitkrb5crypto --with-system-gssapi-h --with-system-gcrypt --with-system-unwind --with-system-mitkrb5 --with-system-mitkrb5crypto --with-system-gssapi --with-system-libtirpc --with-system-pie --with-system-tdb --with-system-ldb --with-system-libtasn1 --with-system-heimdal-kcm --with-system-heimdal-gssapi --with-system-heimdal-hcrypto --with-system-heimdal-roken --with-system-heimdal-compat --with-system-heimdal-asn1 --with-system-heimdal-krb5 --with-system-heimdal-krb5support --with-system-heimdal-krb5types --with-system-heimdal-krb5asn1 --with-system-heimdal-krb5crypto --with-system-heimdal-krb5rcache --with-system-heimdal-krb5plugin --with-system-heimdal-krb5kdc --with-system-heimdal-krb5admin --with-system-heimdal-krb5rpc --with-system-heimdal-kdc --with-system-heimdal-rt --with-cluster-support && \
    make -j4 && \
    make install && \
    cd .. && \
    rm -rf samba-4.14.6.tar.gz samba-4.14.6

RUN git clone https://github.com/zfsonlinux/zfs.git && \
    cd zfs && \
    git checkout zfs-2.1.1 && \
    ./autogen.sh && \
    ./configure --with-linux=/lib/modules/`uname -r`/build && \
    make -j4 && \
    make install && \
    cd .. && \
    rm -rf zfs

RUN git clone https://github.com/zfsonlinux/spl.git && \
    cd spl && \
    git checkout spl-2.1.0 && \
    ./autogen.sh && \
    ./configure --with-linux=/lib/modules/`uname -r`/build && \
    make -j4 && \
    make install && \
    cd .. && \
    rm -rf spl

RUN mkdir /var/run/samba && \
    mkdir /var/log/samba && \
    mkdir /etc/samba && \
    touch /etc/samba/smb.conf && \
    chmod 777 /var/run/samba && \
    chmod 777 /var/log/samba && \
    chmod 777 /etc/samba && \
    chmod 777 /etc/samba/smb.conf

COPY smb.conf /etc/samba/smb.conf

EXPOSE 139/tcp 445/tcp

CMD ["/usr/local/samba/sbin/smbd", "-FS"] 