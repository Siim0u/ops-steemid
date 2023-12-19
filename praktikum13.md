# 13. praktikum
13. praktikumis õppisin, kuidas Windowsi Powershellis skriptida. Sain teada täpsemalt, kuidas muutujad, funktsioonid ja faili sisse kirjutamine töötab. Teadmiste abil kirjutasin koodi, kasutades antud algfaili, mis otsib infot arvuti, selles toimuva ning võrguühenduste kohta. <br>
Kood:
```powershell
#$nr:	küsimuse number
#$param: mis parameetriga tegemist (võimalikult lühidalt)
#$sisu:	väljastatav sisu
function valjasta{
	param ($nr, $param, $sisu)
	$fail = ".\valmis.txt"
	$aeg = Get-Date -Format "HH:mm:ss.fff"
	if($sisu -eq $null){
		$rida = "$nr.	$aeg	${param}:	NULL"
		Write-Output $rida
		$rida | Out-File -FilePath $fail -Append
	}elseif($sisu.GetType().Name -eq "Object[]"){
		$rida = "$nr.	$aeg	${param}:"
		Write-Output $rida $sisu
		$rida | Out-File -FilePath $fail -Append
		$sisu | Out-File -FilePath $fail -Append
	}else{
		$rida = "$nr.	$aeg	${param}:	$sisu"
		Write-Output $rida
		$rida | Out-File -FilePath $fail -Append
	}
}

Valjasta 1.1 "Masin" (hostname)
Valjasta 1.2 "PS versioon" ($PSVersionTable.PSVersion)
Valjasta 1.3 "Windows versioon" ((Get-CimInstance Win32_OperatingSystem).Version)

$net = Get-WmiObject Win32_NetworkAdapterConfiguration -Filter "IPEnabled='True'" | Select-Object PSComputername, Description, IPAddress, IPSubnet, DefaultIPGateway | Sort-Object DHCPEnabled | Format-List
Valjasta 2 "Network info" ($net)

$computerinfo = Get-WmiObject Win32_ComputerSystem |Select-Object TotalPhysicalMemory, NumberOfProcessors, NumberOfLogicalProcessors | Format-List
Valjasta 3 "Protsessor ja RAM" $computerinfo

$graphics = Get-WmiObject Win32_VideoController |
    Select-Object Caption, DriverVersion, DriverDate, VideoModeDescription | Format-List

Valjasta 4 "Graafika" $graphics

$ketas = Get-WmiObject Win32_DiskDrive | Select-Object MediaType, DeviceID, Size | Format-List
Valjasta 5.1 "Ketas" ($ketas)

$partition = Get-WmiObject Win32_DiskPartition
Valjasta 5.2 "Patition" ($partition)

$c = Get-WmiObject Win32_LogicalDisk -Filter "DeviceID='C:'"
Valjasta 5.3 "C:-ketta vaba ruum" "$($c.FreeSpace) baiti"

$pci = Get-WmiObject –Class Win32_PnpSignedDriver | Where-Object {$_.DeviceID –like „PCI*“} | Select-Object Description, DriverVersion, Manufacturer | Sort-Object Description | Format-Table –AutoSize
Valjasta 6 "PCI drivers" ($pci)

$localUsers = Get-WmiObject Win32_UserAccount -Filter "LocalAccount='$true'" | Select-Object Name, Description, LocalAccount, Disabled
Valjasta 7 "Kasutajad" ($localUsers)

$protsess_arv = (Get-Process).Count
Valjasta 8 "Käimasolevate protsesside arv" ($protsess_arv)

$10protsess = Get-Process | Sort-Object { $_.StartTime } -Descending | Select-Object -First 10 | Select-Object Id, StartTime, Name | Format-List
Valjasta 9 "Viimased 10 protsessi" ($10protsess)

[int]$aegL = (Get-Date).Millisecond
$ajakulu = ($aegL - $aegA)

[int]$aegA = (Get-Date).Millisecond
Valjasta 10 "ALGUS" (Get-Date -Format "dddd MM/dd/yyyy HH:mm K")

Valjasta "*" "TEHTUD" "$ajakulu`n`n"
```
[valmis.txt](https://github.com/Siim0u/ops-steemid/files/13718271/valmis.txt)
