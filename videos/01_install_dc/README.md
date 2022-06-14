# 01 Installing the Domain Controller

1. Use `sconfig` to:
    - Change the hostname
    - Change the IP address to static
    - Cahnge the DNS server to our own IP address

2. Install the Active Directory Windows Feature

```shell
Install-WindowsFeature AD-Domain-Services –IncludeManagementTools -Verbose
```

Confirm that AD is installed
```shell
Get-WindowsFeature -Name *AD*
```

Setup a new forest
```shell
Install-ADDSForest -DomainName gorley.internal -ForestMode Win2012r2 -DomainMode Win2012r2 -DomainNetbiosName GORLEY -InstallDns:$true
```

Check that Active Directory services are running:
```shell
Get-Service adws,kdc,netlogon,dns
```

Check AD folders (SYSVOL and NETLOGON) are shared:
```shell
Get-SMBShare
```

Perform a test using the dcdiag command (all stages must be Passed), and check replication between the DCs using the following commands:
```shell
repadmin /replsummary
```

Check where the FSMO roles are located
```shell
Netdom /query FSMO
```

Check IP Address
```shell
Get-NetIPAddress
```

# Joining the Workstation to the domain
```shell
Add-Computer -Domainname xyz.com -Credential xyz\Administrator -Force -Restart
```