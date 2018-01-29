---
title: "Distribuera Site Recovery-mobilitetstjänsten med Azure Automation DSC | Microsoft Docs"
description: "Beskriver hur du använder Azure Automation DSC för att automatiskt distribuera Azure Site Recovery-mobilitetstjänsten och Azure-agenten för VMware VM och fysisk serverreplikering till Azure"
services: site-recovery
documentationcenter: 
author: krnese
manager: lorenr
editor: 
ms.assetid: 1f8cd3ac-0522-48eb-a5f0-679ee9192ddb
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/22/2017
ms.author: krnese
ms.openlocfilehash: 118a2e775ae3d036f58989d9778104e372e8c701
ms.sourcegitcommit: 310748b6d66dc0445e682c8c904ae4c71352fef2
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/28/2017
---
# <a name="deploy-the-mobility-service-with-azure-automation-dsc-for-replication-of-vm"></a>Distribuera mobilitetstjänsten med Azure Automation DSC för replikering av virtuell dator
I Operations Management Suite ger vi dig en omfattande säkerhetskopiering och haveriberedskapslösning som du kan använda som en del av en kontinuitetsplan.

Vi har startats denna resa tillsammans med Hyper-V med hjälp av Hyper-V-replikering. Men vi har utökat stöd för en inställning för heterogena eftersom kunder har flera hypervisorer och plattformar i sina moln.

Om du kör VMware arbetsbelastningar och/eller fysiska servrar idag, körs alla Azure Site Recovery-komponenter i din miljö för att hantera replikering för kommunikation och data med Azure, när Azure är målet i en hanteringsserver.

## <a name="deploy-the-site-recovery-mobility-service-by-using-automation-dsc"></a>Distribuera Site Recovery-mobilitetstjänsten med hjälp av Automation DSC
Låt oss börja genom att göra en snabb uppdelning av den här hanteringsservern har.

Management-servern kör flera serverroller. En av dessa roller är *configuration*, som samordnar kommunikationen och hanterar processer för replikering och återställning av data.

Dessutom kan den *processen* roll som fungerar som en replikerings-gateway. Den här rollen tar emot replikeringsdata från skyddade källdatorer, optimerar dem med cachelagring, komprimering och kryptering och skickar det till ett Azure storage-konto. En av funktionerna för processen är också att push-installation av mobilitetstjänsten till skyddade datorer och utföra automatisk identifiering av virtuella VMware-datorer.

Om det finns en återställning från Azure, den *huvudmålservern* roll hanterar replikeringsdata som en del av den här åtgärden.

För de skydda datorerna vi förlitar sig på den *mobilitetstjänsten*. Den här komponenten distribueras till varje dator (VMware VM eller fysiska server) som du vill replikera till Azure. Den samlar in dataskrivningar på datorn och vidarebefordrar dem till hanteringsservern (Processroll).

När du hantera affärskontinuitet, är det viktigt att förstå dina arbetsbelastningar och din infrastruktur och komponenterna som ingår. Du kan sedan uppfyller kraven för återställning tid mål för Återställningstid och mål för återställningspunkt (RPO). I den här kontexten är mobilitetstjänsten nyckeln till att säkerställa att dina arbetsbelastningar skyddas som förväntat.

Hur kan du, på ett optimerat sätt se till att du har en tillförlitlig skyddade installationen med hjälp av vissa Operations Management Suite-komponenter?

Den här artikeln innehåller ett exempel på hur du kan använda Azure Automation önskad tillstånd Configuration (DSC), tillsammans med Site Recovery för att se till att:

* Mobilitetstjänstversionen och Virtuella Azure-agenten distribueras till de Windows-datorer som du vill skydda.
* Mobilitetstjänstversionen och Virtuella Azure-agenten körs alltid när Azure är replikeringsmålet.

## <a name="prerequisites"></a>Krav
* En databas för lagring av de nödvändiga inställningarna
* En databas för att lagra nödvändiga lösenfrasen registreras hos management server

  > [!NOTE]
  > En unik lösenfras genereras för varje hanteringsserver. Om du ska distribuera flera hanteringsservrar, måste du se till att rätt lösenfrasen lagras i filen passphrase.txt.
  >
  >
* Windows Management Framework (WMF) 5.0 installerat på de datorer som du vill aktivera skydd (ett krav för Automation DSC)

  > [!NOTE]
  > Om du vill använda DSC för Windows-datorer som har WMF 4.0 installerat finns i avsnittet [använder DSC i frånkopplade miljöer](## Use DSC in disconnected environments).
  

Mobilitetstjänsten kan installeras via kommandoraden och tar flera argument. Därför måste du ha binärfilerna (när du extraherar dem från din konfiguration) och lagra dem på en plats där du kan hämta dem med hjälp av DSC-konfigurationen.

## <a name="step-1-extract-binaries"></a>Steg 1: Extrahera binärfiler
1. Bläddra till följande katalog på hanteringsservern för att extrahera filerna som du behöver för den här installationen:

    **\Microsoft azure Site Recovery\home\svsystems\pushinstallsvc\repository**

    I den här mappen bör du se en MSI-fil med namnet:

    **Microsoft ASR_UA_version_Windows_GA_date_Release.exe**

    Använd följande kommando för att extrahera installationsprogrammet:

    **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**
2. Markera alla filer och skicka dem till en komprimerad mapp.

Nu har du de binära filerna som behövs för att automatisera installationen av mobilitetstjänsten med hjälp av Automation DSC.

### <a name="passphrase"></a>Lösenfrasen
Därefter måste du bestämma om du vill placera komprimerade mappen. Du kan använda ett Azure storage-konto som du ser senare för att lagra lösenfrasen som ska användas för installationen. Sedan registrerar agenten med hanteringsservern som en del av processen.

Lösenordet som du fick när du distribuerade hanteringsservern kan sparas till en textfil som passphrase.txt.

Placera både den komprimerade mappen och lösenfrasen i en dedikerad behållare i Azure storage-konto.

![Mapp](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Om du vill behålla dessa filer på en resurs i nätverket, kan du göra detta. Du behöver bara se till att DSC-resurs som du kommer att använda senare har åtkomst och kan få installationen och lösenfrasen.

## <a name="step-2-create-the-dsc-configuration"></a>Steg 2: Skapa DSC-konfigurationen
Inställningen beror på WMF 5.0. WMF 5.0 måste finnas för att kunna tillämpa konfigurationen med hjälp av Automation DSC datorn.

Miljön använder följande exempel DSC-konfigurationen:

```powershell
configuration ASRMobilityService {

    $RemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    $RemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    $RemoteAzureAgent = 'http://go.microsoft.com/fwlink/p/?LinkId=394789'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node localhost {

        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            Ensure = 'Present'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
```
Konfigurationen att göra följande:

* Variablerna talar om hur var du kan hämta de binära filerna för mobilitetstjänsten och Virtuella Azure-agenten var du kan hämta lösenfrasen och var du lagrar utdata.
* Konfigurationen importeras xPSDesiredStateConfiguration DSC-resurs så att du kan använda `xRemoteFile` ska hämta filerna från databasen.
* Konfigurationen skapar en katalog där du vill lagra de binära filerna.
* Arkivera resursen kommer att extrahera filerna från den komprimerade mappen.
* Paketet installera resursen kommer att installera mobilitetstjänsten från UNIFIEDAGENT. EXE-installationsprogram med specifika argument. (De variabler som skapar argumenten måste ändras för att avspegla din miljö.)
* Virtuella Azure-agenten, vilket rekommenderas på varje virtuell dator som körs i Azure installerar paketet AzureAgent resurs. Virtuella Azure-agenten gör det också möjligt att lägga till tillägg till den virtuella datorn efter redundans.
* Den tjänsten eller de resurser som säkerställer att körs alltid relaterade mobilitetstjänsterna och Azure-tjänster.

Spara konfigurationen som **ASRMobilityService**.

> [!NOTE]
> Kom ihåg att ersätta CSIP i konfigurationen för att återspegla faktiska management-servern så att agenten ska anslutas på rätt sätt och använder rätt lösenfras.
>
>

## <a name="step-3-upload-to-automation-dsc"></a>Steg 3: Överför till Automation DSC
Eftersom DSC-konfigurationen som du har gjort kommer att importera en modul för nödvändiga DSC-resurs (xPSDesiredStateConfiguration), måste du importera modulen i Automation innan du laddar upp DSC-konfigurationen.

Logga in på ditt Automation-konto, bläddra till **tillgångar** > **moduler**, och klicka på **Bläddra galleriet**.

Här kan du söka efter modulen och importera dem till ditt konto.

![Importera modulen](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

När du är klar detta går du till datorn där du har Azure Resource Manager-moduler som har installerats och fortsätta importera nyskapade DSC-konfigurationen.

### <a name="import-cmdlets"></a>Importera cmdlet: ar
Logga in på Azure-prenumerationen i PowerShell. Ändra cmdlet: ar för att avspegla din miljö och avbilda kontoinformationen Automation i en variabel:

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

Överför konfigurationen till Automation DSC genom att använda följande cmdlet:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-the-configuration-in-automation-dsc"></a>Kompilera konfigurationen i Automation DSC
Därefter måste kompilera konfigurationen i Automation DSC, så att du kan börja registrera noderna till den. Du kan åstadkomma som genom att köra följande cmdlet:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

Detta kan ta några minuter, eftersom du i princip distribuerar konfigurationen till den värdbaserade DSC pull-tjänsten.

När du sammanställer konfigurationen du kan hämta jobbinformation med hjälp av PowerShell (Get-AzureRmAutomationDscCompilationJob) eller genom att använda den [Azure-portalen](https://portal.azure.com/).

![Hämta jobb](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

Du har nu har publicerats och överföra DSC-konfigurationen till Automation DSC.

## <a name="step-4-onboard-machines-to-automation-dsc"></a>Steg 4: Publicera datorer till Automation DSC
> [!NOTE]
> En av förutsättningarna för att slutföra det här scenariot är att din Windows-datorer uppdateras med den senaste versionen av WMF. Du kan hämta och installera rätt version för din plattform från den [Download Center](https://www.microsoft.com/download/details.aspx?id=50395).
>
>

Nu skapar du en metaconfig för DSC som du ska gälla för noderna. Du måste hämta slutpunkts-URL och den primära nyckeln för det valda Automation-kontot i Azure för att lyckas med den här. Du hittar dessa värden under **nycklar** på den **alla inställningar** bladet för Automation-kontot.

![Nyckelvärden](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

I det här exemplet har du en Windows Server 2012 R2 fysisk server som du vill skydda med Site Recovery.

### <a name="check-for-any-pending-file-rename-operations-in-the-registry"></a>Kontrollera om det finns väntande filåtgärder Byt namn i registret
Innan du börjar koppla servern till slutpunkten för Automation DSC, rekommenderar vi att du söker efter eventuella väntande filåtgärder Byt namn i registret. De kan förhindra installationen slutförs på grund av en väntande omstart.

Kör följande cmdlet för att kontrollera att det finns ingen väntande omstart på servern:

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Om det visar tomt är OK för att fortsätta. Om inte, du kan åtgärda detta genom att servern startas om under en underhållsperiod.

Starta PowerShell Integrated Scripting Environment (ISE) och kör följande skript för att tillämpa konfigurationen på servern. Detta är i grunden en DSC lokala konfiguration som instruerar Local Configuration Manager-motorn att registrera med Automation DSC-tjänsten och hämta viss konfiguration (ASRMobilityService.localhost).

```powershell
[DSCLocalConfigurationManager()]
configuration metaconfig {
    param (
        $URL,
        $Key
    )
    node localhost {
        Settings {
            RefreshFrequencyMins = '30'
            RebootNodeIfNeeded = $true
            RefreshMode = 'PULL'
            ActionAfterReboot = 'ContinueConfiguration'
            ConfigurationMode = 'ApplyAndMonitor'
            AllowModuleOverwrite = $true
        }

        ResourceRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }

        ConfigurationRepositoryWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
            ConfigurationNames = 'ASRMobilityService.localhost'
        }

        ReportServerWeb AzureAutomationDSC {
            ServerURL = $URL
            RegistrationKey = $Key
        }
    }
}
metaconfig -URL 'https://we-agentservice-prod-1.azure-automation.net/accounts/<YOURAAAccountID>' -Key '<YOURAAAccountKey>'

Set-DscLocalConfigurationManager .\metaconfig -Force -Verbose
```

Den här konfigurationen gör att den lokala Configuration Manager-motorn att registrera sig med Automation DSC. Den avgör också hur motorn ska köras, vad den ska göra om det finns en konfigurationsavvikelser (ApplyAndAutoCorrect) och hur den ska fortsätta med konfigurationen av en omstart krävs.

När du har kört skriptet bör noden börja registrera med Automation DSC.

![Noden registrering pågår](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Om du går tillbaka till Azure-portalen kan se du att noden nyregistrerade nu har visats i portalen.

![Registrerade nod i portalen](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

Du kan köra följande PowerShell-cmdlet för att kontrollera att noden har registrerats korrekt på servern:

```powershell
Get-DscLocalConfigurationManager
```

Efter konfigurationen har hämtas och tillämpas på servern, kan du kontrollera detta genom att köra följande cmdlet:

```powershell
Get-DscConfigurationStatus
```

Utdata visar att servern har mottagit dess konfiguration:

![Resultat](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Dessutom tjänstinställning Mobility har sin egen logg som finns på *SystemDrive*\ProgramData\ASRSetupLogs.

Det stämmer. Du har nu distribuerats och registrerad mobilitetstjänsten på den dator som du vill skydda med Site Recovery. DSC ska kontrollera att de nödvändiga tjänsterna körs alltid.

![Slutförd distribution](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

När hanteringsservern identifierar lyckad distribution, kan du konfigurera skyddet och aktivera replikering på datorn med hjälp av Site Recovery.

## <a name="use-dsc-in-disconnected-environments"></a>Använd DSC i frånkopplade miljöer
Om dina datorer inte är ansluten till Internet, kan du fortfarande beroende DSC för att distribuera och konfigurera mobilitetstjänsten på arbetsbelastningar som du vill skydda.

Du kan initiera en egen hämtningsservern för DSC i din miljö för att ge i stort sett samma funktioner som du får från Automation DSC. Det vill säga hämtar klienter konfigurationen (när den är registrerad) till DSC-slutpunkten. Ett annat alternativ är dock manuellt push DSC-konfigurationen till dina datorer, lokalt eller fjärranslutet.

Observera att i det här exemplet finns det en extra parameter för namnet på datorn. Fjärråtkomst filerna finns nu på en fjärresurs som ska kunna nås av datorer som du vill skydda. I slutet av skriptet utfärdar konfigurationen och startar sedan tillämpa DSC-konfigurationen på måldatorn.

### <a name="prerequisites"></a>Krav
Se till att xPSDesiredStateConfiguration PowerShell-modulen är installerad. För Windows-datorer där WMF 5.0 är installerat, kan du installera modulen xPSDesiredStateConfiguration genom att köra följande cmdlet på måldatorer:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Du kan också hämta och spara modulen om du behöver distribuera den till Windows-datorer som har WMF 4.0. Kör denna cmdlet på en dator där PowerShellGet (WMF 5.0) förekommer:

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Även WMF 4.0, se till att den [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) installeras på datorer.

Följande konfiguration kan skickas till Windows-datorer som har WMF 5.0 och WMF 4.0:

```powershell
configuration ASRMobilityService {
    param (
        [Parameter(Mandatory=$true)]
        [ValidateNotNullOrEmpty()]
        [System.String] $ComputerName
    )

    $RemoteFile = '\\myfileserver\share\asr.zip'
    $RemotePassphrase = '\\myfileserver\share\passphrase.txt'
    $RemoteAzureAgent = '\\myfileserver\share\AzureVmAgent.msi'
    $LocalAzureAgent = 'C:\Temp\AzureVmAgent.msi'
    $TempDestination = 'C:\Temp\asr.zip'
    $LocalPassphrase = 'C:\Temp\Mobility_service\passphrase.txt'
    $Role = 'Agent'
    $Install = 'C:\Program Files (x86)\Microsoft Azure Site Recovery'
    $CSEndpoint = '10.0.0.115'
    $Arguments = '/Role "{0}" /InstallLocation "{1}" /CSEndpoint "{2}" /PassphraseFilePath "{3}"' -f $Role,$Install,$CSEndpoint,$LocalPassphrase

    Import-DscResource -ModuleName xPSDesiredStateConfiguration

    node $ComputerName {      
        File Directory {
            DestinationPath = 'C:\Temp\ASRSetup\'
            Type = 'Directory'            
        }

        xRemoteFile Setup {
            URI = $RemoteFile
            DestinationPath = $TempDestination
            DependsOn = '[File]Directory'
        }

        xRemoteFile Passphrase {
            URI = $RemotePassphrase
            DestinationPath = $LocalPassphrase
            DependsOn = '[File]Directory'
        }

        xRemoteFile AzureAgent {
            URI = $RemoteAzureAgent
            DestinationPath = $LocalAzureAgent
            DependsOn = '[File]Directory'
        }

        Archive ASRzip {
            Path = $TempDestination
            Destination = 'C:\Temp\ASRSetup'
            DependsOn = '[xRemotefile]Setup'
        }

        Package Install {
            Path = 'C:\temp\ASRSetup\ASR\UNIFIEDAGENT.EXE'
            Ensure = 'Present'
            Name = 'Microsoft Azure Site Recovery mobility Service/Master Target Server'
            ProductId = '275197FC-14FD-4560-A5EB-38217F80CBD1'
            Arguments = $Arguments
            DependsOn = '[Archive]ASRzip'
        }

        Package AzureAgent {
            Path = 'C:\Temp\AzureVmAgent.msi'
            Ensure = 'Present'
            Name = 'Windows Azure VM Agent - 2.7.1198.735'
            ProductId = '5CF4D04A-F16C-4892-9196-6025EA61F964'
            Arguments = '/q /l "c:\temp\agentlog.txt'
            DependsOn = '[Package]Install'
        }

        Service ASRvx {
            Name = 'svagents'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service ASR {
            Name = 'InMage Scout Application Service'
            State = 'Running'
            DependsOn = '[Package]Install'
        }

        Service AzureAgentService {
            Name = 'WindowsAzureGuestAgent'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }

        Service AzureTelemetry {
            Name = 'WindowsAzureTelemetryService'
            State = 'Running'
            DependsOn = '[Package]AzureAgent'
        }
    }
}
ASRMobilityService -ComputerName 'MyTargetComputerName'

Start-DscConfiguration .\ASRMobilityService -Wait -Force -Verbose
```

Om du vill skapa en instans av en egen DSC pull-server i företagsnätverket för att efterlikna de funktioner som du kan hämta från Automation DSC, se [ställer in en pull webbserver DSC](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Valfritt: Distribuera en DSC-konfigurationen med hjälp av en Azure Resource Manager-mall
Den här artikeln har fokuserar på hur du kan skapa egna DSC-konfiguration för att automatiskt distribuera mobilitetstjänsten och Azure VM-agenten-- och kontrollera som de körs på de datorer som du vill skydda. Vi har också en Azure Resource Manager-mall som ska distribuera den här DSC-konfigurationen till en ny eller befintlig Azure Automation-konto. Mallen använder indataparametrar för att skapa Automation tillgångar som innehåller variabler för din miljö.

När du distribuerar mallen kan du bara referera till steg 4 i den här guiden för att publicera dina datorer.

Mallen kommer att göra följande:

1. Använd ett befintligt Automation-konto eller skapa en ny
2. Ta indataparametrar:
   * ASRRemoteFile--den plats där du har sparat tjänstinställning Mobility
   * ASRPassphrase--den plats där du har sparat filen passphrase.txt
   * ASRCSEndpoint--IP-adressen för hanteringsservern
3. Importera xPSDesiredStateConfiguration PowerShell-modulen
4. Skapa och kompilera DSC-konfigurationen

De föregående stegen sker i rätt ordning, så att du kan starta onboarding dina datorer för skydd.

Mallen med instruktioner för distribution, finns på [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

Distribuera mallen med hjälp av PowerShell:

```powershell
$RGDeployArgs = @{
    Name = 'DSC3'
    ResourceGroupName = 'KNOMS'
    TemplateFile = 'https://raw.githubusercontent.com/krnese/AzureDeploy/master/OMS/MSOMS/DSC/azuredeploy.json'
    OMSAutomationAccountName = 'KNOMSAA'
    ASRRemoteFile = 'https://knrecstor01.blob.core.windows.net/asr/ASR.zip'
    ASRRemotePassphrase = 'https://knrecstor01.blob.core.windows.net/asr/passphrase.txt'
    ASRCSEndpoint = '10.0.0.115'
    DSCJobGuid = [System.Guid]::NewGuid().ToString()
}
New-AzureRmResourceGroupDeployment @RGDeployArgs -Verbose
```

## <a name="next-steps"></a>Nästa steg
När du har distribuerat agenterna Mobility service kan du [Aktivera replikering](site-recovery-vmware-to-azure.md) för virtuella datorer.
