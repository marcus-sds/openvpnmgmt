# CentOS

## centos install

    yum install -y pam-devel gcc<br>
    wget https://github.com/google/google-authenticator-libpam/archive/1.03.tar.gz<br>
    tar xvfz 1.03.tar.gz<br>
    cd google-authenticator-libpam-*<br>
    yum install -y libtool automake autoconf<br>
    ./configure<br>
    make<br>
    make install<br>
    cp google-authenticator /usr/bin/
    ls -al /lib64/security/pam_google_authenticator.so<br>


## systemctl has some issue when using two factor authentication.  you can start with command with vpn
#/usr/sbin/openvpn --cd /etc/openvpn/ --config server.conf

<pre>
[root@vpn-sec.mgmt.dev ()~]# cat /etc/pam.d/openvpn
# google auth
#auth        required    /usr/local/lib/security/pam_google_authenticator.so
#auth        required    /lib64/security/pam_google_authenticator.so
auth [success=2 default=ignore] pam_succeed_if.so user ingroup sec
auth       required      /lib64/security/pam_google_authenticator.so nullok forward_pass
#auth required pam_google_authenticator.so nullok forward_pass
#auth required pam_unix.so use_first_pass
auth required pam_unix.so try_first_pass

auth       include      system-auth
account    required     pam_nologin.so
account    include      system-auth
password   include      system-auth
# pam_selinux.so close should be the first session rule
session    required     pam_selinux.so close
session    required     pam_loginuid.so
session    optional     pam_console.so
# pam_selinux.so open should only be followed by sessions to be executed in the user context
session    required     pam_selinux.so open
session    required     pam_namespace.so
session    optional     pam_keyinit.so force revoke
session    include      system-auth
-session   optional     pam_ck_connector.so
</pre>
<pre>
[root@vpn-sec.mgmt.dev ()~]# tail /etc/openvpn/server.conf
log-append  openvpn.log

verb 3

#plugin /usr/lib64/openvpn/plugins/openvpn-plugin-auth-pam.so login
#plugin /usr/share/openvpn/plugin/lib/openvpn-auth-pam.so login
plugin /usr/share/openvpn/plugin/lib/openvpn-auth-pam.so openvpn
client-cert-not-required
username-as-common-name
reneg-sec 0
</pre>


# Ubuntu


## ubuntu install
  
    wget https://github.com/google/google-authenticator-libpam/archive/1.03.tar.gz<br>
    tar xvfz 1.03.tar.gz<br>
    cd google-authenticator-libpam-*<br>
    apt-get install libpam0g-dev libtool automake autoconf
    make
    make install
    
## Config

    ;/usr/lib64/openvpn/plugin/lib/openvpn-auth-pam.so
    ;plugin /usr/lib/openvpn/openvpn-plugin-auth-pam.so login
    plugin /usr/lib/openvpn/openvpn-plugin-auth-pam.so openvpn
    client-cert-not-required
    username-as-common-name
    reneg-sec 0


    root@VM-0-17-ubuntu:/etc/pam.d# cat openvpn
    #
    # /etc/pam.d/common-account - authorization settings common to all services
    #
    # This file is included from other service-specific PAM config files,
    # and should contain a list of the authorization modules that define
    # the central access policy for use on the system.  The default is to
    # only deny service to users whose accounts are expired in /etc/shadow.
    #
    # As of pam 1.0.1-6, this file is managed by pam-auth-update by default.
    # To take advantage of this, it is recommended that you configure any
    # local modules either before or after the default block, and use
    # pam-auth-update to manage selection of other modules.  See
    # pam-auth-update(8) for details.
    #

    # here are the per-package modules (the "Primary" block)
    auth    required                        /usr/local/lib/security/pam_google_authenticator.so nullok forward_pass
    account [success=1 new_authtok_reqd=done default=ignore]        pam_unix.so try_first_pass
    # here's the fallback if no module succeeds
    account requisite                       pam_deny.so
    # prime the stack with a positive return value if there isn't one already;
    # this avoids us returning an error just because nothing sets a success code
    # since the modules above will each just jump around
    account required                        pam_permit.so
    # and here are more per-package modules (the "Additional" block)
    # end of pam-auth-update config

