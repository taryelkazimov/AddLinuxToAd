# AddLinuxToAd




1. Configure DNS
2. Configure hostname as FQDN
3. Configure NTP

# ONELINER

yum install realmd sssd oddjob oddjob-mkhomedir adcli samba-common samba-common-tools && realm join -U administrator contoso.com && sed -i '/use_fully_qualified_names = True/c\use_fully_qualified_names = False' /etc/sssd/sssd.conf && realm permit -g centos01@contoso.com && sed -i '/^## Same thing without a password/a %centos01@contoso.com  ALL=(ALL)       NOPASSWD: ALL' /etc/sudoers >> /etc/sudoers && systemctl enable sssd.service && systemctl restart sssd



# Use Ansible PlayBook 

ansible-playbook join-redhat-domain.yml --limit workers  --become

_______________________________________________________
## Join Centos to Active Directory

yum install realmd sssd oddjob oddjob-mkhomedir adcli samba-common samba-common-tools

## Start and enable sssd

systemctl enable sssd && systemctl start sssd

## DISCOVER DOMAIN

realm discover contoso.com

## jOINT CENTOS TO DOMAIN

realm --verbose join -U 'administrator' dc01.contoso.com

## TO Leave domain if you need.

#realm leave ad.example.com
#realm leave ad.example.com -U 'AD.EXAMPLE.COM\user'

## Edit SSSD conf

/etc/sssd/sssd.conf

## change like this

use_fully_qualified_names = False

## How to permit only one Active Directory group to logon

realm permit -g centos01@contoso.com

## How to give sudo permissions to an Active Directory group

visudo

%centos01@contoso.com ALL=(ALL) ALL

Or

%contoso\\centos01 ALL=(ALL) ALL

## Restart SSSD

systemctl enable sssd.service && systemctl restart sssd
