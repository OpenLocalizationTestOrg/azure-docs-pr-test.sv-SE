---
title: "aaaConfigure hello Always On-tillgänglighetsgruppen på en virtuell Azure-dator med hjälp av PowerShell | Microsoft Docs"
description: "Den här kursen använder resurser som har skapats med hello klassiska distributionsmodellen. Du kan använda PowerShell toocreate en Always On-tillgänglighetsgrupp i Azure."
services: virtual-machines-windows
documentationcenter: na
author: MikeRayMSFT
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: a4e2f175-fe56-4218-86c7-a43fb916cc64
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 03/17/2017
ms.author: mikeray
ms.openlocfilehash: d4a27e203b2ff299adebec2b010c03422459b3c3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-hello-always-on-availability-group-on-an-azure-vm-with-powershell"></a>Konfigurera hello Always On-tillgänglighetsgrupp på en virtuell Azure-dator med PowerShell
> [!div class="op_single_selector"]
> * [Klassiska: UI](../classic/portal-sql-alwayson-availability-groups.md)
> * [Klassiska: PowerShell](../classic/ps-sql-alwayson-availability-groups.md)
<br/>

Innan du börjar bör du överväga att du kan nu slutföra den här aktiviteten i Azure resource manager-modellen. Vi rekommenderar Azure resource manager-modellen för nya distributioner. Se [SQL Server Always On-Tillgänglighetsgrupper på Azure virtual machines](../sql/virtual-machines-windows-portal-sql-availability-group-overview.md).

> [!IMPORTANT]
> Vi rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen.

Virtuella Azure-datorer (VM) kan hjälpa administratörer toolower hello databaskostnader av ett SQL Server-system med hög tillgänglighet. Den här kursen visar hur tooimplement tillgänglighet gruppen med hjälp av SQL Server alltid aktiverad slutpunkt till slutpunkt i en Azure-miljö. Hello slutet av hello kursen består SQL Server alltid på lösningen i Azure av hello följande element:

* Ett virtuellt nätverk som innehåller flera undernät, inklusive en frontend- och ett backend-undernät.
* En domänkontrollant med en Active Directory-domän.
* Två SQL Server-datorer som är distribuerade toohello backend-undernät och kopplade toohello Active Directory-domän.
* Ett tre Windows redundanskluster med hello Nodmajoritet kvorummodellen.
* En tillgänglighetsgrupp med två replikerna med synkront genomförande av en tillgänglighetsdatabas.

Det här scenariot är ett bra alternativ för dess enkelhet på Azure, inte för kostnadseffektivitet eller andra faktorer. Exempelvis kan du minimera hello antal virtuella datorer för en två-replik tillgänglighet grupp toosave på beräkningstimmar i Azure med hjälp av hello domänkontrollant som filresursvittne i hello kvorum i ett kluster med två noder. Den här metoden minskar hello VM antal med ett från hello senare konfiguration.

Den här kursen är avsedd tooshow du hello steg som är nödvändiga tooset in hello beskrivs lösningen ovan, utan utarbeta hello detaljer för varje steg. Därför steg i stället för under förutsättning att hello GUI konfigurationssteg använder PowerShell scripting tootake du snabbt via varje. Den här självstudiekursen förutsätts hello följande:

* Du har redan ett Azure-konto med hello virtuella prenumeration.
* Du har installerat hello [Azure PowerShell-cmdlets](/powershell/azure/overview).
* Du har redan en god förståelse av Always On-Tillgänglighetsgrupper för lokala lösningar. Mer information finns i [Always On-Tillgänglighetsgrupper (SQL Server)](https://msdn.microsoft.com/library/hh510230.aspx).

## <a name="connect-tooyour-azure-subscription-and-create-hello-virtual-network"></a>Anslut tooyour Azure-prenumeration och skapa hello virtuellt nätverk
1. Importera hello Azure-modulen i PowerShell-fönstret på den lokala datorn, hämtar hello publicering inställningar filen tooyour datorn och ansluta din PowerShell-session tooyour Azure-prenumeration genom att importera hello hämtas publiceringsinställningarna.

        Import-Module "C:\Program Files (x86)\Microsoft SDKs\Azure\PowerShell\Azure\Azure.psd1"
        Get-AzurePublishSettingsFile
        Import-AzurePublishSettingsFile <publishsettingsfilepath>

    Hej **Get-AzurePublishSettingsFile** kommandot genererar ett certifikat med Azure och hämtar den tooyour datorn automatiskt. En webbläsare öppnas automatiskt och du kan ange tooenter hello Microsoft-kontouppgifter för din Azure-prenumeration. hello hämtas **.publishsettings** filen innehåller alla hello information du behöver toomanage din Azure-prenumeration. När du sparar filen tooa lokala katalogen kan du importera det med hello **importera AzurePublishSettingsFile** kommando.

   > [!NOTE]
   > Hej .publishsettings-fil innehåller dina autentiseringsuppgifter (ej kodade) som används tooadminister dina Azure-prenumerationer och tjänster. hello rekommenderade säkerhetsmetoder för den här filen är toostore den tillfälligt utanför katalogerna källa (till exempel i hello Libraries\Documents mappen), och tas bort när hello importen är klar. En obehörig användare som får åtkomst till toohello .publishsettings-fil kan du redigera, skapa och ta bort Azure-tjänster.

2. Definiera ett antal variabler när du ska använda toocreate ditt moln IT-infrastruktur.

        $location = "West US"
        $affinityGroupName = "ContosoAG"
        $affinityGroupDescription = "Contoso SQL HADR Affinity Group"
        $affinityGroupLabel = "IaaS BI Affinity Group"
        $networkConfigPath = "C:\scripts\Network.netcfg"
        $virtualNetworkName = "ContosoNET"
        $storageAccountName = "<uniquestorageaccountname>"
        $storageAccountLabel = "Contoso SQL HADR Storage Account"
        $storageAccountContainer = "https://" + $storageAccountName + ".blob.core.windows.net/vhds/"
        $winImageName = (Get-AzureVMImage | where {$_.Label -like "Windows Server 2008 R2 SP1*"} | sort PublishedDate -Descending)[0].ImageName
        $sqlImageName = (Get-AzureVMImage | where {$_.Label -like "SQL Server 2012 SP1 Enterprise*"} | sort PublishedDate -Descending)[0].ImageName
        $dcServerName = "ContosoDC"
        $dcServiceName = "<uniqueservicename>"
        $availabilitySetName = "SQLHADR"
        $vmAdminUser = "AzureAdmin"
        $vmAdminPassword = "Contoso!000"
        $workingDir = "c:\scripts\"

    Betala uppmärksamhet toohello följande tooensure dina kommandon lyckas senare:

   * Variabler **$storageAccountName** och **$dcServiceName** måste vara unikt eftersom de används tooidentify ditt lagringskonto för molnet och moln-servern, på hello Internet.
   * Hej namn som du anger för variabler **$affinityGroupName** och **$virtualNetworkName** konfigureras i hello virtuellt nätverk configuration dokument som du vill använda senare.
   * **$sqlImageName** anger hello uppdateras för hello VM-avbildning som innehåller SQL Server 2012 Service Pack 1 Enterprise Edition.
   * För enkelhetens skull **Contoso! 000** är hello samma lösenord som används i hela hello hela kursen.

3. Skapa en tillhörighetsgrupp.

        New-AzureAffinityGroup `
            -Name $affinityGroupName `
            -Location $location `
            -Description $affinityGroupDescription `
            -Label $affinityGroupLabel

4. Skapa ett virtuellt nätverk genom att importera en konfigurationsfil.

        Set-AzureVNetConfig `
            -ConfigurationPath $networkConfigPath

    hello konfigurationsfilen innehåller hello följande XML-dokument. I korthet det anger ett virtuellt nätverk kallas **ContosoNET** i hello tillhörighetsgrupp kallas **ContosoAG**. Den har hello adressutrymme **10.10.0.0/16** och har två undernät **10.10.1.0/24** och **10.10.2.0/24**, vilket är hello främre undernät och bakre undernät respektive. hello främre undernät är där du kan placera klientprogram, till exempel Microsoft SharePoint. hello tillbaka undernät är där du ska placera hello SQL Server-datorer. Om du ändrar hello **$affinityGroupName** och **$virtualNetworkName** variabler tidigare, måste du också ändra hello som motsvarar namnen nedan.

        <NetworkConfiguration xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
          <VirtualNetworkConfiguration>
            <Dns />
            <VirtualNetworkSites>
              <VirtualNetworkSite name="ContosoNET" AffinityGroup="ContosoAG">
                <AddressSpace>
                  <AddressPrefix>10.10.0.0/16</AddressPrefix>
                </AddressSpace>
                <Subnets>
                  <Subnet name="Front">
                    <AddressPrefix>10.10.1.0/24</AddressPrefix>
                  </Subnet>
                  <Subnet name="Back">
                    <AddressPrefix>10.10.2.0/24</AddressPrefix>
                  </Subnet>
                </Subnets>
              </VirtualNetworkSite>
            </VirtualNetworkSites>
          </VirtualNetworkConfiguration>
        </NetworkConfiguration>

5. Skapa ett lagringskonto som är kopplad till hello tillhörighetsgrupp som du skapat och ange den som hello aktuella storage-konto i din prenumeration.

        New-AzureStorageAccount `
            -StorageAccountName $storageAccountName `
            -Label $storageAccountLabel `
            -AffinityGroup $affinityGroupName
        Set-AzureSubscription `
            -SubscriptionName (Get-AzureSubscription).SubscriptionName `
            -CurrentStorageAccount $storageAccountName

6. Skapa hello Domänkontrollantservern i hello nya cloud service och tillgänglighet uppsättningen.

        New-AzureVMConfig `
            -Name $dcServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$dcServerName.vhd" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -Windows `
                -DisableAutomaticUpdates `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword |
                New-AzureVM `
                    -ServiceName $dcServiceName `
                    –AffinityGroup $affinityGroupName `
                    -VNetName $virtualNetworkName

    Kommandona via rörledningar hello följande saker:

   * **Nya AzureVMConfig** skapar en VM-konfiguration.
   * **Lägg till AzureProvisioningConfig** ger hello konfigurationsparametrar för en fristående Windows server.
   * **Lägg till AzureDataDisk** lägger till hello datadisk som du vill använda för att lagra Active Directory-data med hello cachelagring alternativet set tooNone.
   * **Nya AzureVM** skapar en ny molntjänst och skapar hello nya Azure VM i hello ny molntjänst.

7. Vänta tills hello nya VM toobe helt etablerad och hämta hello skrivbord fjärrfil tooyour arbetskatalogen. Eftersom hello nya Azure VM tar en lång tid tooprovision, hello `while` slingan fortsätter toopoll hello ny virtuell dator tills den är redo för användning.

        $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName

        While ($VMStatus.InstanceStatus -ne "ReadyRole")
        {
            write-host "Waiting for " $VMStatus.Name "... Current Status = " $VMStatus.InstanceStatus
            Start-Sleep -Seconds 15
            $VMStatus = Get-AzureVM -ServiceName $dcServiceName -Name $dcServerName
        }

        Get-AzureRemoteDesktopFile `
            -ServiceName $dcServiceName `
            -Name $dcServerName `
            -LocalPath "$workingDir$dcServerName.rdp"

hello Domänkontrollantservern nu etablerats. Du måste konfigurera hello Active Directory-domän på den här Domänkontrollantservern. Låt hello PowerShell-fönstret öppet på den lokala datorn. Du kommer att använda det igen senare toocreate hello två virtuella SQL Server-datorer.

## <a name="configure-hello-domain-controller"></a>Konfigurera hello-domänkontrollant
1. Ansluta toohello Domänkontrollantservern genom att starta hello remote desktop-fil. Använd hello datorn administratörens användarnamn AzureAdmin lösenord **Contoso! 000**, som du angav när du skapade hello ny virtuell dator.
2. Öppna ett PowerShell-fönster i administratörsläge.
3. Kör följande hello **DCPROMO. EXE** kommandot tooset in hello **corp.contoso.com** domän, med hello datakataloger på enhet M.

        dcpromo.exe `
            /unattend `
            /ReplicaOrNewDomain:Domain `
            /NewDomain:Forest `
            /NewDomainDNSName:corp.contoso.com `
            /ForestLevel:4 `
            /DomainNetbiosName:CORP `
            /DomainLevel:4 `
            /InstallDNS:Yes `
            /ConfirmGc:Yes `
            /CreateDNSDelegation:No `
            /DatabasePath:"C:\Windows\NTDS" `
            /LogPath:"C:\Windows\NTDS" `
            /SYSVOLPath:"C:\Windows\SYSVOL" `
            /SafeModeAdminPassword:"Contoso!000"

    Startar om automatiskt när hello-kommandot har slutförts hello VM.

4. Ansluta toohello Domänkontrollantservern igen genom att starta hello remote desktop-fil. Den här tiden kan logga in som **CORP\Administrator**.
5. Öppna ett PowerShell-fönster i administratörsläge och importera hello Active Directory PowerShell-modulen med hjälp av hello följande kommando:

        Import-Module ActiveDirectory

6. Kör följande kommandon tooadd tre användare toohello domän hello.

        $pwd = ConvertTo-SecureString "Contoso!000" -AsPlainText -Force
        New-ADUser `
            -Name 'Install' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc1' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true
        New-ADUser `
            -Name 'SQLSvc2' `
            -AccountPassword  $pwd `
            -PasswordNeverExpires $true `
            -ChangePasswordAtLogon $false `
            -Enabled $true

    **CORP\Install** är används tooconfigure allt relaterade toohello SQL Server-tjänstinstanser hello failover-kluster och hello-tillgänglighetsgruppen. **CORP\SQLSvc1** och **CORP\SQLSvc2** används som hello SQL Server-tjänstkontona för hello två SQL Server-datorer.
7. Hello nästa, kör följande kommandon toogive **CORP\Install** hello behörigheter toocreate datorobjekt i hello domän.

        Cd ad:
        $sid = new-object System.Security.Principal.SecurityIdentifier (Get-ADUser "Install").SID
        $guid = new-object Guid bf967a86-0de6-11d0-a285-00aa003049e2
        $ace1 = new-object System.DirectoryServices.ActiveDirectoryAccessRule $sid,"CreateChild","Allow",$guid,"All"
        $corp = Get-ADObject -Identity "DC=corp,DC=contoso,DC=com"
        $acl = Get-Acl $corp
        $acl.AddAccessRule($ace1)
        Set-Acl -Path "DC=corp,DC=contoso,DC=com" -AclObject $acl

    hello GUID som anges ovan är hello GUID för hello objekttypen för datorn. Hej **CORP\Install** kontot måste hello **läsa alla egenskaper** och **skapa datorobjekt** behörighet toocreate hello Active direkt objekt för hello redundans kluster. Hej **läsa alla egenskaper** behörigheter redan ges tooCORP\Install som standard, så du inte behöver toogrant det explicit. För mer information om behörigheter som är nödvändiga toocreate hello failover-kluster, se [stegvis Guide för Failover-kluster: Konfigurera konton i Active Directory](https://technet.microsoft.com/library/cc731002%28v=WS.10%29.aspx).

    Nu när du är klar med att konfigurera Active Directory och hello användarobjekt du skapa två virtuella SQL Server-datorer och Anslut dem toothis domän.

## <a name="create-hello-sql-server-vms"></a>Skapa hello SQL Server-datorer
1. Fortsätt toouse hello PowerShell-fönster som är öppna på den lokala datorn. Definiera hello följande ytterligare variabler:

        $domainName= "corp"
        $FQDN = "corp.contoso.com"
        $subnetName = "Back"
        $sqlServiceName = "<uniqueservicename>"
        $quorumServerName = "ContosoQuorum"
        $sql1ServerName = "ContosoSQL1"
        $sql2ServerName = "ContosoSQL2"
        $availabilitySetName = "SQLHADR"
        $dataDiskSize = 100
        $dnsSettings = New-AzureDns -Name "ContosoBackDNS" -IPAddress "10.10.0.4"

    Hej IP-adress **10.10.0.4** normalt tilldelas toohello första virtuella dator som du skapar i hello **10.10.0.0/16** undernätet för din virtuella Azure-nätverket. Du bör kontrollera att det här är din Domänkontrollantservern hello-adress genom att köra **IPCONFIG**.
2. Kör hello följande via rörledningar kommandon toocreate hello först VM i hello failover-kluster med namnet **ContosoQuorum**:

        New-AzureVMConfig `
            -Name $quorumServerName `
            -InstanceSize Medium `
            -ImageName $winImageName `
            -MediaLocation "$storageAccountContainer$quorumServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    New-AzureVM `
                        -ServiceName $sqlServiceName `
                        –AffinityGroup $affinityGroupName `
                        -VNetName $virtualNetworkName `
                        -DnsSettings $dnsSettings

    Observera följande hello om hello ovanstående kommando:

   * **Nya AzureVMConfig** skapar en VM-konfiguration med hello önskade tillgänglighetsuppsättningen. hello senare virtuella datorer kommer att skapas med hello samma namn på tillgänglighetsuppsättning så att de är anslutna toohello samma tillgänglighetsuppsättning.
   * **Lägg till AzureProvisioningConfig** kopplingar hello VM toohello Active Directory-domän som du skapade.
   * **Ange AzureSubnet** platser hello VM i hello tillbaka undernät.
   * **Nya AzureVM** skapar en ny molntjänst och skapar hello nya Azure VM i hello ny molntjänst. Hej **DnsSettings** parametern anger hello DNS-servern för hello servrar i hello ny molntjänst har hello IP-adress **10.10.0.4**. Detta är hello hello Domänkontrollantservern IP-adress. Den här parametern krävs tooenable hello nya virtuella datorer i hello cloud service toojoin toohello Active Directory-domän har. Utan den här parametern måste du manuellt ange hello IPv4-inställningar i din VM toouse hello Domänkontrollantservern som hello primära DNS-server när hello VM etableras och sedan ansluta hello VM toohello Active Directory-domän.
3. Kör hello följande via rörledningar kommandon toocreate hello SQL Server-datorer med namnet **ContosoSQL1** och **ContosoSQL2**.

        # Create ContosoSQL1...
        New-AzureVMConfig `
            -Name $sql1ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql1ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 1 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

        # Create ContosoSQL2...
        New-AzureVMConfig `
            -Name $sql2ServerName `
            -InstanceSize Large `
            -ImageName $sqlImageName `
            -MediaLocation "$storageAccountContainer$sql2ServerName.vhd" `
            -AvailabilitySetName $availabilitySetName `
            -HostCaching "ReadOnly" `
            -DiskLabel "OS" |
            Add-AzureProvisioningConfig `
                -WindowsDomain `
                -AdminUserName $vmAdminUser `
                -Password $vmAdminPassword `
                -DisableAutomaticUpdates `
                -Domain $domainName `
                -JoinDomain $FQDN `
                -DomainUserName $vmAdminUser `
                -DomainPassword $vmAdminPassword |
                Set-AzureSubnet `
                    -SubnetNames $subnetName |
                    Add-AzureEndpoint `
                        -Name "SQL" `
                        -Protocol "tcp" `
                        -PublicPort 2 `
                        -LocalPort 1433 |
                        New-AzureVM `
                            -ServiceName $sqlServiceName

    Observera följande hello om hello kommandona ovan:

   * **Nya AzureVMConfig** använder hello samma tillgänglighetsuppsättningen som hello Domänkontrollantservern och använder hello SQL Server 2012 Service Pack 1 Enterprise Edition bilden i hello galleriet för virtuella datorer. Den anger också hello fungerar system disk tooread cachelagring endast (ingen skrivcache). Vi rekommenderar att du migrerar hello databasen filer tooa separat datadisk att bifoga toohello VM, och konfigurera den med ingen läs eller skrivcache. Hello bästa alternativet är dock tooremove skrivcache på hello operativsystemdisk eftersom du inte ta bort läsa cachelagring på hello operativsystemdisken.
   * **Lägg till AzureProvisioningConfig** kopplingar hello VM toohello Active Directory-domän som du skapade.
   * **Ange AzureSubnet** platser hello VM i hello tillbaka undernät.
   * **Lägg till AzureEndpoint** lägger till slutpunkter för åtkomst så att klientprogram kan komma åt dessa tjänster SQL Server-instanser på hello Internet. Olika portar ges tooContosoSQL1 och ContosoSQL2.
   * **Nya AzureVM** skapar hello nya SQL Server-VM i hello samma molntjänst som ContosoQuorum. Du måste placera hello virtuella datorer i samma molntjänst om du vill att de toobe i hello hello samma tillgänglighetsuppsättning.
4. Vänta för varje VM-toobe helt etablerad och varje VM-toodownload dess skrivbord fjärrfil tooyour arbetskatalogen. Hej `for` loop går igenom hello tre nya virtuella datorer och utför hello kommandon i hello översta klammerparenteser för dem.

        Foreach ($VM in $VMs = Get-AzureVM -ServiceName $sqlServiceName)
        {
            write-host "Waiting for " $VM.Name "..."

            # Loop until hello VM status is "ReadyRole"
            While ($VM.InstanceStatus -ne "ReadyRole")
            {
                write-host "  Current Status = " $VM.InstanceStatus
                Start-Sleep -Seconds 15
                $VM = Get-AzureVM -ServiceName $VM.ServiceName -Name $VM.InstanceName
            }

            write-host "  Current Status = " $VM.InstanceStatus

            # Download remote desktop file
            Get-AzureRemoteDesktopFile -ServiceName $VM.ServiceName -Name $VM.InstanceName -LocalPath "$workingDir$($VM.InstanceName).rdp"
        }

    hello SQL Server-datorer har nu etablerats och körs, men de är installerade med SQL Server med standardalternativ.

## <a name="initialize-hello-failover-cluster-vms"></a>Initiera hello redundanskluster virtuella datorer
I det här avsnittet måste toomodify hello tre servrar som du vill använda i hello failover-kluster och hello SQL Server-installation. Närmare bestämt:

* Alla servrar: du behöver tooinstall hello **redundanskluster** funktion.
* Alla servrar: du behöver tooadd **CORP\Install** som hello dator **administratör**.
* ContosoSQL1 och ContosoSQL2 endast: du behöver tooadd **CORP\Install** som en **sysadmin** roll i hello standarddatabas.
* ContosoSQL1 och ContosoSQL2 endast: du behöver tooadd **NT AUTHORITY\System** som loggar in med hello följande behörigheter:

  * ALTER någon tillgänglighetsgrupp
  * Ansluta SQL
  * Visa-Servertillstånd
* ContosoSQL1 och ContosoSQL2 endast: hello **TCP** protokollet är redan aktiverat på hello SQL Server-VM. Behöver du dock fortfarande tooopen hello Brandvägg för fjärråtkomst av SQL Server.

Nu är du redo toostart. Från och med **ContosoQuorum**, hello följande sätt:

1. Ansluta för**ContosoQuorum** genom att starta hello remote desktop-filer. Använda hello datorn administratörens användarnamn **AzureAdmin** och lösenord **Contoso! 000**, som du angav när du skapade hello virtuella datorer.
2. Kontrollera att hello datorer har anslutits för**corp.contoso.com**.
3. Vänta tills hello SQL Server-installation toofinish kör hello automatisk initiering uppgifter innan du fortsätter.
4. Öppna ett PowerShell-fönster i administratörsläge.
5. Installera redundanskluster i Windows hello-funktionen.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. Lägg till **CORP\Install** som lokal administratör.

        net localgroup administrators "CORP\Install" /Add
7. Logga ut från ContosoQuorum. Du är klar med den här servern nu.

        logoff.exe

Därefter initiera **ContosoSQL1** och **ContosoSQL2**. Åtgärderna hello nedan, som är identiska för både SQL Server-datorer.

1. Ansluta toohello två SQL Server-datorer genom att starta hello remote desktop-filer. Använda hello datorn administratörens användarnamn **AzureAdmin** och lösenord **Contoso! 000**, som du angav när du skapade hello virtuella datorer.
2. Kontrollera att hello datorer har anslutits för**corp.contoso.com**.
3. Vänta tills hello SQL Server-installation toofinish kör hello automatisk initiering uppgifter innan du fortsätter.
4. Öppna ett PowerShell-fönster i administratörsläge.
5. Installera redundanskluster i Windows hello-funktionen.

        Import-Module ServerManager
        Add-WindowsFeature Failover-Clustering
6. Lägg till **CORP\Install** som lokal administratör.

        net localgroup administrators "CORP\Install" /Add
7. Importera hello PowerShell-providern för SQL Server.

        Set-ExecutionPolicy -Execution RemoteSigned -Force
        Import-Module -Name "sqlps" -DisableNameChecking
8. Lägg till **CORP\Install** som hello sysadmin-rollen för hello standardinstansen för SQL Server.

        net localgroup administrators "CORP\Install" /Add
        Invoke-SqlCmd -Query "EXEC sp_addsrvrolemember 'CORP\Install', 'sysadmin'" -ServerInstance "."
9. Lägg till **NT AUTHORITY\System** som loggar in med hello tre behörigheterna som beskrivs ovan.

        Invoke-SqlCmd -Query "CREATE LOGIN [NT AUTHORITY\SYSTEM] FROM WINDOWS" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT ALTER ANY AVAILABILITY GROUP too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT CONNECT SQL too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
        Invoke-SqlCmd -Query "GRANT VIEW SERVER STATE too[NT AUTHORITY\SYSTEM] AS SA" -ServerInstance "."
10. Öppna hello-brandväggen för fjärråtkomst av SQL Server.

         netsh advfirewall firewall add rule name='SQL Server (TCP-In)' program='C:\Program Files\Microsoft SQL Server\MSSQL11.MSSQLSERVER\MSSQL\Binn\sqlservr.exe' dir=in action=allow protocol=TCP
11. Logga ut från både virtuella datorer.

         logoff.exe

Slutligen är du redo tooconfigure hello-tillgänglighetsgruppen. Du kommer att använda hello PowerShell-providern för SQL Server tooperform all hello fungera i **ContosoSQL1**.

## <a name="configure-hello-availability-group"></a>Konfigurera hello-tillgänglighetsgrupp
1. Ansluta för**ContosoSQL1** igen genom att starta hello remote desktop-filer. I stället för att logga in med hjälp av hello datorkonto, logga in med hjälp av **CORP\Install**.
2. Öppna ett PowerShell-fönster i administratörsläge.
3. Definiera hello följande variabler:

        $server1 = "ContosoSQL1"
        $server2 = "ContosoSQL2"
        $serverQuorum = "ContosoQuorum"
        $acct1 = "CORP\SQLSvc1"
        $acct2 = "CORP\SQLSvc2"
        $password = "Contoso!000"
        $clusterName = "Cluster1"
        $timeout = New-Object System.TimeSpan -ArgumentList 0, 0, 30
        $db = "MyDB1"
        $backupShare = "\\$server1\backup"
        $quorumShare = "\\$server1\quorum"
        $ag = "AG1"
4. Importera hello PowerShell-providern för SQL Server.

        Set-ExecutionPolicy RemoteSigned -Force
        Import-Module "sqlps" -DisableNameChecking
5. Ändra hello SQL Server-tjänstkontot för ContosoSQL1 tooCORP\SQLSvc1.

        $wmi1 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server1
        $wmi1.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct1,$password)}
        $svc1 = Get-Service -ComputerName $server1 -Name 'MSSQLSERVER'
        $svc1.Stop()
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc1.Start();
        $svc1.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
6. Ändra hello SQL Server-tjänstkontot för ContosoSQL2 tooCORP\SQLSvc2.

        $wmi2 = new-object ("Microsoft.SqlServer.Management.Smo.Wmi.ManagedComputer") $server2
        $wmi2.services | where {$_.Type -eq 'SqlServer'} | foreach{$_.SetServiceAccount($acct2,$password)}
        $svc2 = Get-Service -ComputerName $server2 -Name 'MSSQLSERVER'
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
7. Hämta **CreateAzureFailoverCluster.ps1** från [Skapa kluster för växling vid fel för Always On-Tillgänglighetsgrupper i Azure VM](http://gallery.technet.microsoft.com/scriptcenter/Create-WSFC-Cluster-for-7c207d3a) toohello lokala arbetskatalogen. Du ska använda det här skriptet toohelp som du skapar ett fungerande kluster. Viktig information om hur Windows-redundanskluster samverkar med hello Azure nätverk, se [hög tillgänglighet och katastrofåterställning återställning för SQL Server i Azure Virtual Machines](../sql/virtual-machines-windows-sql-high-availability-dr.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).
8. Ändra tooyour arbetskatalog och skapa hello redundanskluster med hello hämtas skript.

        Set-ExecutionPolicy Unrestricted -Force
        .\CreateAzureFailoverCluster.ps1 -ClusterName "$clusterName" -ClusterNode "$server1","$server2","$serverQuorum"
9. Aktivera Always On-Tillgänglighetsgrupper för hello standard SQL Server-instanser på **ContosoSQL1** och **ContosoSQL2**.

        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server1\Default `
            -Force
        Enable-SqlAlwaysOn `
            -Path SQLSERVER:\SQL\$server2\Default `
            -NoServiceRestart
        $svc2.Stop()
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Stopped,$timeout)
        $svc2.Start();
        $svc2.WaitForStatus([System.ServiceProcess.ServiceControllerStatus]::Running,$timeout)
10. Skapa en katalog och bevilja hello SQL Server-tjänstkonton åtkomstbehörigheter. Du ska använda den här tillgänglighetsdatabasen directory tooprepare hello på hello sekundär replik.

         $backup = "C:\backup"
         New-Item $backup -ItemType directory
         net share backup=$backup "/grant:$acct1,FULL" "/grant:$acct2,FULL"
         icacls.exe "$backup" /grant:r ("$acct1" + ":(OI)(CI)F") ("$acct2" + ":(OI)(CI)F")
11. Skapa en databas på **ContosoSQL1** kallas **MyDB1**ta både en fullständig säkerhetskopia och en säkerhetskopia och återställa dem på **ContosoSQL2** med hello **WITH NORECOVERY** alternativet.

         Invoke-SqlCmd -Query "CREATE database $db"
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server1
         Backup-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server1 -BackupAction Log
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.bak" -ServerInstance $server2 -NoRecovery
         Restore-SqlDatabase -Database $db -BackupFile "$backupShare\db.log" -ServerInstance $server2 -RestoreAction Log -NoRecovery
12. Skapa hello tillgänglighet grupp slutpunkter på hello virtuella SQL Server-datorer och ange hello rätt behörigheter på hello slutpunkter.

         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server1\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"
         $endpoint =
             New-SqlHadrEndpoint MyMirroringEndpoint `
             -Port 5022 `
             -Path "SQLSERVER:\SQL\$server2\Default"
         Set-SqlHadrEndpoint `
             -InputObject $endpoint `
             -State "Started"

         Invoke-SqlCmd -Query "CREATE LOGIN [$acct2] FROM WINDOWS" -ServerInstance $server1
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] too[$acct2]" -ServerInstance $server1
         Invoke-SqlCmd -Query "CREATE LOGIN [$acct1] FROM WINDOWS" -ServerInstance $server2
         Invoke-SqlCmd -Query "GRANT CONNECT ON ENDPOINT::[MyMirroringEndpoint] too[$acct1]" -ServerInstance $server2
13. Skapa hello tillgänglighetsrepliker.

         $primaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server1 `
             -EndpointURL "TCP://$server1.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
         $secondaryReplica =
             New-SqlAvailabilityReplica `
             -Name $server2 `
             -EndpointURL "TCP://$server2.corp.contoso.com:5022" `
             -AvailabilityMode "SynchronousCommit" `
             -FailoverMode "Automatic" `
             -Version 11 `
             -AsTemplate
14. Skapa slutligen hello tillgänglighetsgruppen och koppling hello sekundär replik toohello tillgänglighetsgruppen.

         New-SqlAvailabilityGroup `
             -Name $ag `
             -Path "SQLSERVER:\SQL\$server1\Default" `
             -AvailabilityReplica @($primaryReplica,$secondaryReplica) `
             -Database $db
         Join-SqlAvailabilityGroup `
             -Path "SQLSERVER:\SQL\$server2\Default" `
             -Name $ag
         Add-SqlAvailabilityDatabase `
             -Path "SQLSERVER:\SQL\$server2\Default\AvailabilityGroups\$ag" `
             -Database $db

## <a name="next-steps"></a>Nästa steg
Du har nu har implementerat SQL Server alltid aktiverad genom att skapa en tillgänglighetsgrupp i Azure. tooconfigure en lyssnare för den här tillgänglighetsgruppen finns [konfigurera en ILB-lyssnare för Always On-Tillgänglighetsgrupper i Azure](../classic/ps-sql-int-listener.md).

Annan information om hur du använder SQL Server i Azure, se [SQL Server på Azure virtual machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).
