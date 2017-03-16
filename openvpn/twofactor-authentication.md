
yum install -y pam-devel gcc
wget https://google-authenticator.googlecode.com/files/libpam-google-authenticator-1.0-source.tar.bz2
tar xvf libpam-google-authenticator-1.0-source.tar.bz2
cd libpam-google-authenticator-1.0
make
make install
ls -al /lib64/security/pam_google_authenticator.so



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


[root@vpn-sec.mgmt.dev ()~]# tail /etc/openvpn/server.conf
log-append  openvpn.log

verb 3

#plugin /usr/lib64/openvpn/plugins/openvpn-plugin-auth-pam.so login
#plugin /usr/share/openvpn/plugin/lib/openvpn-auth-pam.so login
plugin /usr/share/openvpn/plugin/lib/openvpn-auth-pam.so openvpn
client-cert-not-required
username-as-common-name
reneg-sec 0
