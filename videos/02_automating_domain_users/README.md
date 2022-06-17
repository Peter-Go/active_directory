# 02 Automating Domain Users

1. Add New-PSSession to variable

```shell
$creds = (Get-Credential)
$dc = New-pssession -ComputerName 172.16.4.250 -Credential $creds
```

```shell
Enter-PSSession $dc
```

2. Copy files to server

```shell
copy-item .\ad_schema.json -ToSession $dc C:\Windows\Tasks
```



