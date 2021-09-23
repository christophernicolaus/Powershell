# Powershell
Various Powershell Scripts that I have written to provide services to a company

#This script will ping all IPs on a specified network, will take all IPs that did not reply and then will resolve the hostname of these IPs and output them to the results.txt

$iprange = 2..254
Foreach ($ip in $iprange)
{
    $computer = "<ip address>.$ip"
    $status = Test-Connection $computer -count 1 -Quiet
    if (!$status)
    {
       [System.Net.Dns]::GetHostEntry($computer).HostName + "- $computer - Ping Unavailable. NSLookup result. Most likely DNS Cache." | Add-Content -LiteralPath "C:\filepath\result.txt"
    }
}

#This script will ping all IPs and try to resolve DNS name. The end result shows only IP Addresses that are free to use on the network specified

$iprange = 2..254
Foreach ($ip in $iprange)
{
    $computer = "<ip address>.$ip"
    $status = Test-Connection $computer -count 1 -Quiet
    $dns = Resolve-DnsName($computer)
    if (!$dns -and !$status)
    {
       "- $computer - IP is available" | Add-Content -LiteralPath "C:\filepath\result.txt"
    }
}

#This script will move specified users to an AD Group of your choice

Get-ADUser -SearchBase '(Domain Path)'-Filter * | ForEach-Object {Add-ADGroupMember –Identity '(group name)’ –Members $_}
  
#This script will list all users in a specified OU
  
Get-ADUser -SearchBase '(Domain Path)'-Filter * | ft Name -AutoSize
  
