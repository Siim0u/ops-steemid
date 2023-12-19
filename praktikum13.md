# 13. praktikum
13. praktikumis õppisin, kuidas Windowsi Powershellis skriptida. Sain teada täpsemalt, kuidas muutujad, funktsioonid ja faili sisse kirjutamine töötab. Teadmiste abil kirjutasin koodi, kasutades antud algfaili, mis otsib infot arvuti, selles toimuva ning võrguühenduste kohta. 
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
[valmis.txt](https://github.com/Siim0u/ops-steemid/files/13718271/valmis.txt)1.1.	18:38:01.597	Masin:	DESKTOP-STVJN20
1.2.	18:38:01.600	PS versioon:	5.1.19041.3803
1.3.	18:38:01.651	Windows versioon:	10.0.19045
2.	18:38:01.728	Network info:


PSComputerName   : DESKTOP-STVJN20
Description      : LogMeIn Hamachi Virtual Ethernet Adapter
IPAddress        : {fe80::72dc:cd24:2320:c8ec, 2620:9b::194d:2f4b}
IPSubnet         : {64, 64}
DefaultIPGateway : {2620:9b::1900:1}

PSComputerName   : DESKTOP-STVJN20
Description      : Intel(R) Ethernet Connection (2) I218-V
IPAddress        : {192.168.1.146, fe80::9cbd:3864:c30a:2ca2, 2001:7d0:8885:1d80:f124:77ed:7ae6:9910, 2001:7d0:8885:1d80:12ac:e7df:67df:19e9}
IPSubnet         : {255.255.255.0, 64, 128, 64}
DefaultIPGateway : {192.168.1.1, fe80::222:7ff:fe9e:11b0}

PSComputerName   : DESKTOP-STVJN20
Description      : VirtualBox Host-Only Ethernet Adapter
IPAddress        : {192.168.56.1, fe80::47b4:13fb:b0ab:2989}
IPSubnet         : {255.255.255.0, 64}
DefaultIPGateway : 

PSComputerName   : DESKTOP-STVJN20
Description      : Famatech Radmin VPN Ethernet Adapter
IPAddress        : {26.249.78.203, fe80::4220:77bb:f0b3:9b7f, fdfd::1af9:4ecb}
IPSubnet         : {255.0.0.0, 64, 64}
DefaultIPGateway : {26.0.0.1}



3.	18:38:01.765	Protsessor ja RAM:


TotalPhysicalMemory       : 17068490752
NumberOfProcessors        : 1
NumberOfLogicalProcessors : 12



4.	18:38:01.790	Graafika:


Caption              : NVIDIA GeForce GTX 1660 Ti
DriverVersion        : 31.0.15.4592
DriverDate           : 20231019000000.000000-000
VideoModeDescription : 1920 x 1080 x 4294967296 colors



5.1.	18:38:01.870	Ketas:


MediaType : Fixed hard disk media
DeviceID  : \\.\PHYSICALDRIVE0
Size      : 250056737280

MediaType : Fixed hard disk media
DeviceID  : \\.\PHYSICALDRIVE1
Size      : 3000590369280



5.2.	18:38:01.904	Patition:


NumberOfBlocks   : 921600
BootPartition    : False
Name             : Disk #0, Partition #0
PrimaryPartition : False
Size             : 471859200
Index            : 0

NumberOfBlocks   : 202752
BootPartition    : True
Name             : Disk #0, Partition #1
PrimaryPartition : True
Size             : 103809024
Index            : 1

NumberOfBlocks   : 486095732
BootPartition    : False
Name             : Disk #0, Partition #2
PrimaryPartition : True
Size             : 248881014784
Index            : 2

NumberOfBlocks   : 1138688
BootPartition    : False
Name             : Disk #0, Partition #3
PrimaryPartition : False
Size             : 583008256
Index            : 3

NumberOfBlocks   : 5860268032
BootPartition    : False
Name             : Disk #1, Partition #0
PrimaryPartition : True
Size             : 3000457232384
Index            : 0



5.3.	18:38:01.991	C:-ketta vaba ruum:	44464472064 baiti
6.	18:38:04.506	PCI drivers:

Description                                                                                                                 DriverVersion   Manufacturer                 
-----------                                                                                                                 -------------   ------------                 
High Definition Audio Controller                                                                                            10.0.19041.3636 Microsoft                    
High Definition Audio Controller                                                                                            10.0.19041.3636 Microsoft                    
Intel Device                                                                                                                10.0.20.0       Intel                        
Intel(R) C610 series/X99 chipset LPC Controller - 8D47                                                                      10.1.1.38       INTEL                        
Intel(R) C610 series/X99 chipset PCI Express Root Port #1 - 8D10                                                            10.1.1.38       INTEL                        
Intel(R) C610 series/X99 chipset PCI Express Root Port #4 - 8D16                                                            10.1.1.38       INTEL                        
Intel(R) C610 series/X99 chipset PCI Express Root Port #5 - 8D18                                                            10.1.1.38       INTEL                        
Intel(R) C610 series/X99 chipset SMBus Controller - 8D22                                                                    10.1.1.38       INTEL                        
Intel(R) C610 series/X99 chipset SPSR - 8D7C                                                                                10.1.1.38       INTEL                        
Intel(R) Ethernet Connection (2) I218-V                                                                                     12.19.0.16      Intel                        
Intel(R) Management Engine Interface                                                                                        11.7.0.1045     Intel                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Address Map VTd_Misc System Management - 2F28                                  10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Buffered Ring Agent - 2FF8                                                     10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Buffered Ring Agent - 2FF9                                                     10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 DDRIO (VMSE) 0 & 1 - 2FBC                                                      10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 DDRIO (VMSE) 0 & 1 - 2FBD                                                      10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 DDRIO (VMSE) 0 & 1 - 2FBE                                                      10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 DDRIO (VMSE) 0 & 1 - 2FBF                                                      10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 DDRIO (VMSE) 2 & 3 - 2FB8                                                      10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 DDRIO (VMSE) 2 & 3 - 2FB9                                                      10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 DDRIO (VMSE) 2 & 3 - 2FBA                                                      10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 DDRIO (VMSE) 2 & 3 - 2FBB                                                      10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 DDRIO Channel 0/1 Broadcast - 2FAE                                             10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 DDRIO Channel 2/3 Broadcast - 2F6E                                             10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 DDRIO Global Broadcast - 2F6F                                                  10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 DDRIO Global Broadcast - 2FAF                                                  10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 DMI2 - 2F00                                                                    10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Home Agent 0 - 2F30                                                            10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Home Agent 0 - 2FA0                                                            10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Hot Plug - 2F29                                                                10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 I/O APIC - 2F2C                                                                10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Integrated Memory Controller 0 Channel 0 ERROR Registers - 2FB2                10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Integrated Memory Controller 0 Channel 0 Thermal Control - 2FB0                10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Integrated Memory Controller 0 Channel 1 ERROR Registers - 2FB3                10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Integrated Memory Controller 0 Channel 1 Thermal Control - 2FB1                10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Integrated Memory Controller 0 Channel 2 ERROR Registers - 2FB6                10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Integrated Memory Controller 0 Channel 2 Thermal Control - 2FB4                10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Integrated Memory Controller 0 Channel 3 ERROR Registers - 2FB7                10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Integrated Memory Controller 0 Channel 3 Thermal Control - 2FB5                10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Integrated Memory Controller 0 Channel Target Address Decoder - 2FAA           10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Integrated Memory Controller 0 Channel Target Address Decoder - 2FAB           10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Integrated Memory Controller 0 Channel Target Address Decoder - 2FAC           10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Integrated Memory Controller 0 Channel Target Address Decoder - 2FAD           10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Integrated Memory Controller 0 Target Address / Thermal / RAS Registers - 2F71 10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Integrated Memory Controller 0 Target Address / Thermal / RAS Registers - 2FA8 10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Integrated Memory Controller 1 Channel 0 Thermal Control - 2FD0                10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Integrated Memory Controller 1 Target Address / Thermal / RAS Registers - 2F68 10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 PCI Express Root Port 1 - 2F02                                                 10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 PCI Express Root Port 1 - 2F03                                                 10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 PCI Express Root Port 2 - 2F04                                                 10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 PCI Express Root Port 2 - 2F06                                                 10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 PCI Express Root Port 2 - 2F07                                                 10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 PCI Express Root Port 3 - 2F08                                                 10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 PCIe Ring Interface - 2F1D                                                     10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 PCIe Ring Interface - 2F34                                                     10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Power Control Unit - 2F98                                                      10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Power Control Unit - 2F99                                                      10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Power Control Unit - 2F9A                                                      10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Power Control Unit - 2FC0                                                      10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 R3 QPI Link 0 & 1 Monitoring - 2F36                                            10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 R3 QPI Link 0 & 1 Monitoring - 2F37                                            10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 R3 QPI Link 0 & 1 Monitoring - 2F81                                            10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 RAS Control Status and Global Errors - 2F2A                                    10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Scratchpad & Semaphore Registers - 2F1E                                        10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Scratchpad & Semaphore Registers - 2F1F                                        10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Scratchpad & Semaphore Registers - 2F7D                                        10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 System Address Decoder & Broadcast Registers - 2FFC                            10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 System Address Decoder & Broadcast Registers - 2FFD                            10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 System Address Decoder & Broadcast Registers - 2FFE                            10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Unicast Registers - 2FE0                                                       10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Unicast Registers - 2FE1                                                       10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Unicast Registers - 2FE2                                                       10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Unicast Registers - 2FE3                                                       10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Unicast Registers - 2FE4                                                       10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 Unicast Registers - 2FE5                                                       10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 VCU - 2F88                                                                     10.1.2.19       INTEL                        
Intel(R) Xeon(R) E7 v3/Xeon(R) E5 v3/Core i7 VCU - 2F8A                                                                     10.1.2.19       INTEL                        
NVIDIA GeForce GTX 1660 Ti                                                                                                  31.0.15.4592    NVIDIA                       
NVIDIA USB Type-C Port Policy Controller                                                                                    1.50.831.832    NVIDIA                       
Standard Enhanced PCI to USB Host Controller                                                                                10.0.19041.3636 (Standard USB Host Control...
Standard Enhanced PCI to USB Host Controller                                                                                10.0.19041.3636 (Standard USB Host Control...
Standard SATA AHCI Controller                                                                                               10.0.19041.3636 Standard SATA AHCI Controller
USB xHCI Compliant Host Controller                                                                                          10.0.19041.3636 Generic USB xHCI Host Cont...
USB xHCI Compliant Host Controller                                                                                          10.0.19041.3636 Generic USB xHCI Host Cont...
USB xHCI Compliant Host Controller                                                                                          10.0.19041.3636 Generic USB xHCI Host Cont...


7.	18:38:04.556	Kasutajad:

Name               Description                                                                                     LocalAccount Disabled
----               -----------                                                                                     ------------ --------
Administrator      Built-in account for administering the computer/domain                                                  True     True
DefaultAccount     A user account managed by the system.                                                                   True     True
Guest              Built-in account for guest access to the computer/domain                                                True     True
Siim                                                                                                                       True    False
WDAGUtilityAccount A user account managed and used by the system for Windows Defender Application Guard scenarios.         True     True


8.	18:38:04.565	Käimasolevate protsesside arv:	198
9.	18:38:04.579	Viimased 10 protsessi:


Id        : 14724
StartTime : 19/12/2023 18:38:00
Name      : SearchProtocolHost

Id        : 13156
StartTime : 19/12/2023 18:37:37
Name      : firefox

Id        : 3488
StartTime : 19/12/2023 18:37:34
Name      : SearchProtocolHost

Id        : 3364
StartTime : 19/12/2023 18:37:34
Name      : smartscreen

Id        : 1604
StartTime : 19/12/2023 18:36:52
Name      : SearchFilterHost

Id        : 5124
StartTime : 19/12/2023 18:36:35
Name      : firefox

Id        : 5224
StartTime : 19/12/2023 18:36:04
Name      : firefox

Id        : 14404
StartTime : 19/12/2023 18:35:33
Name      : firefox

Id        : 9108
StartTime : 19/12/2023 18:28:19
Name      : firefox

Id        : 4836
StartTime : 19/12/2023 18:27:58
Name      : svchost



10.	18:38:04.612	ALGUS:	Tuesday 12/19/2023 18:38 +02:00
*.	18:38:04.615	TEHTUD:	612




