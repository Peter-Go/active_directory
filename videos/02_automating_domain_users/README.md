# 02 Automating Domain Users

1. Add New-PSSession to variable

```shell
$dc = New-pssession -ComputerName 172.16.4.250 -Credential (Get-Credential)
```

```shell
Enter-PSSession $dc
```

2. Copy files to server

```shell
copy-item .\ad_schema.json -ToSession $dc C:\Windows\Tasks
```




