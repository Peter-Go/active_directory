# 01 Installing the Domain Controller

1. Use `sconfig` to:
    - Change the hostname
    - Change the IP address to static
    - Change the DNS server to our own IP address

    sconfig in server 2022 has a bug where the IP address wont change so for now use powershell:

    ```shell
    Rename-Computer -NewName dc01
    Get-NetAdapter
    $ip = "172.16.0.1"
    $gw="172.16.0.254"
    $dns = "172.16.0.1"
    ```

2. Add server to trusted hosts and enable remoting

    - On DC01 run:
    ```shell
    start-service winrm
    ```

    - On management client run:
    ```shell
    set-item wsman:\localhost\Client\TrustedHosts -value 172.16.4.250
    ```

    ```shell
    New-pssession -ComputerName 172.16.4.250 -Credential (Get-Credential)
    ```

    ```shell
    Enter-PSSession -computername dc01
    ```

3. Install the Active Directory Windows Feature

```shell
Install-WindowsFeature AD-Domain-Services –IncludeManagementTools -Verbose
```

Confirm that AD is installed
```shell
Get-WindowsFeature -Name *AD*
```
4. Setup a new forest

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

Check IP Addresses
```shell
Get-NetIPAddress
Get-DnsClientServerAddress
```

4. Joining the Workstation to the domain
```shell
Add-Computer -Domainname xyz.com -Credential xyz\Administrator -Force -Restart
```

5. Install chocolatey
```shell
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

```shell
choco install git
choco install vscode
choco install microsoft-windows-terminal
```