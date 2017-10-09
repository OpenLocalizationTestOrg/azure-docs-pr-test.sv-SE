---
title: "aaaDeploy hello Site Recovery mobilitetstjänsten med Azure Automation DSC | Microsoft Docs"
description: "Beskriver hur toouse Azure Automation DSC tooautomatically distribuera hello Azure Site Recovery mobilitetstjänsten och Azure-agenten för VMware VM och fysisk server replication tooAzure"
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
ms.date: 08/01/2017
ms.author: krnese
ms.openlocfilehash: 52cdd13ceb61718a21137180c55db86919af5929
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-mobility-service-with-azure-automation-dsc-for-replication-of-vm"></a>Distribuera hello mobilitetstjänsten med Azure Automation DSC för replikering av virtuell dator
I Operations Management Suite ger vi dig en omfattande säkerhetskopiering och haveriberedskapslösning som du kan använda som en del av en kontinuitetsplan.

Vi har startats denna resa tillsammans med Hyper-V med hjälp av Hyper-V-replikering. Men vi har utökade toosupport heterogena installationen eftersom kunder har flera hypervisorer och plattformar i sina moln.

Om du kör VMware arbetsbelastningar och/eller fysiska servrar idag, kör en hanteringsserver alla hello Azure Site Recovery-komponenter i din miljö toohandle hello kommunikations- och replikering med Azure, när Azure är målet.

## <a name="deploy-hello-site-recovery-mobility-service-by-using-automation-dsc"></a>Distribuera hello på Site Recovery-mobilitetstjänsten med hjälp av Automation DSC
Låt oss börja genom att göra en snabb uppdelning av den här hanteringsservern har.

hello management-servern kör flera serverroller. En av dessa roller är *configuration*, som samordnar kommunikationen och hanterar processer för replikering och återställning av data.

Dessutom hello *processen* roll som fungerar som en replikerings-gateway. Den här rollen tar emot replikeringsdata från skyddade källdatorer, optimerar dem med cachelagring, komprimering och kryptering och skickar sedan tooan Azure storage-konto. En av hello funktioner för hello Processroll är också toopush installation av hello Mobility service tooprotected datorer och utför automatisk identifiering av virtuella VMware-datorer.

Om det finns en återställning från Azure, hello *huvudmålservern* roll hanterar replikeringsdata hello som en del av den här åtgärden.

För hello skyddade datorer, vi förlitar sig på hello *mobilitetstjänsten*. Den här komponenten är distribuerade tooevery dator (VMware VM eller fysiska server) som du vill tooreplicate tooAzure. Den samlar in dataskrivningar på hello datorn och vidarebefordrar dem toohello hanteringsservern (Processroll).

När du behandlar kontinuitet för företag är viktigt toounderstand dina arbetsbelastningar, din infrastruktur och hello komponenter inblandade. Du kan sedan uppfyller hello krav för recovery tid mål för Återställningstid och mål för återställningspunkt (RPO). I den här kontexten hello mobilitetstjänsten är viktiga tooensuring som dina arbetsbelastningar skyddas som förväntat.

Hur kan du, på ett optimerat sätt se till att du har en tillförlitlig skyddade installationen med hjälp av vissa Operations Management Suite-komponenter?

Den här artikeln innehåller ett exempel på hur du kan använda Azure Automation önskad tillstånd Configuration (DSC), tillsammans med Site Recovery tooensure som:

* Hej mobilitetstjänsten och Virtuella Azure-agenten är distribuerade toohello Windows-datorer som du vill tooprotect.
* Hej mobilitetstjänsten och Virtuella Azure-agenten körs alltid när Azure är hello replikeringsmål.

## <a name="prerequisites"></a>Krav
* En databas toostore hello som krävs för installationen
* En databas toostore hello krävs lösenfras tooregister hello management server

  > [!NOTE]
  > En unik lösenfras genereras för varje hanteringsserver. Om du vill toodeploy flera hanteringsservrar blir har tooensure som hello rätt lösenfras lagras i hello passphrase.txt filen.
  >
  >
* Windows Management Framework (WMF) 5.0 installerat på hello datorer som du vill tooenable för skydd (ett krav för Automation DSC)

  > [!NOTE]
  > Om du vill toouse DSC för Windows-datorer som har WMF 4.0 installerat avsnittet hello [Använd DSC i frånkopplade miljöer](## Use DSC in disconnected environments).
  

Hej mobilitetstjänsten kan installeras via hello kommandoraden och tar flera argument. Det är därför du behöver toohave hello-binärfiler (när du extraherar dem från din konfiguration) och lagra dem på en plats där du kan hämta dem med hjälp av DSC-konfigurationen.

## <a name="step-1-extract-binaries"></a>Steg 1: Extrahera binärfiler
1. tooextract hello filer som behövs för den här installationen Bläddra toohello följande katalog på hanteringsservern:

    **\Microsoft azure Site Recovery\home\svsystems\pushinstallsvc\repository**

    I den här mappen bör du se en MSI-fil med namnet:

    **Microsoft ASR_UA_version_Windows_GA_date_Release.exe**

    Använd följande kommando tooextract hello installer hello:

    **.\Microsoft-ASR_UA_9.1.0.0_Windows_GA_02May2016_release.exe /q /x:C:\Users\Administrator\Desktop\Mobility_Service\Extract**
2. Markera alla filer och skicka dem tooa komprimerad mapp.

Nu har du hello-binärfiler som du behöver tooautomate hello installationen av hello mobilitetstjänsten med hjälp av Automation DSC.

### <a name="passphrase"></a>Lösenfrasen
Sedan måste toodetermine där du vill att tooplace komprimerade mappen. Du kan använda ett Azure storage-konto som visas senare toostore hello lösenfras som du behöver för hello-installationen. hello agenten registreras sedan med hello hanteringsservern som en del av hello-processen.

hello lösenfras som du fick när du distribuerade hanteringsservern hello kan sparas tooa textfil som passphrase.txt.

Placera både hello komprimerade mappen och lösenfrasen hello i en dedikerad behållare i hello Azure storage-konto.

![Mapp](./media/site-recovery-automate-mobilitysevice-install/folder-and-passphrase-location.png)

Om du vill tookeep dessa filer på en resurs i nätverket kan göra du detta. Du behöver bara tooensure att hello DSC-resurs som du kommer att använda senare har åtkomst och kan få hello installationen och lösenfrasen.

## <a name="step-2-create-hello-dsc-configuration"></a>Steg 2: Skapa hello DSC-konfiguration
hello inställningen beror på WMF 5.0. Hello datorn toosuccessfully tillämpas i hello konfigurationen med hjälp av Automation DSC, WMF 5.0 måste toobe finns.

hello miljö använder hello följande exempel DSC-konfigurationen:

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
hello konfiguration gör hello följande:

* hello variabler talar hello konfiguration där tooget hello binärfiler för hello mobilitetstjänsten och hello Azure VM-agent, där tooget hello lösenfras och toostore hello utdata.
* hello konfigurationen importeras hello xPSDesiredStateConfiguration DSC-resurs, så att du kan använda `xRemoteFile` toodownload hello filer från hello-databas.
* hello konfigurationen skapar en katalog där du vill toostore hello binärfiler.
* hello Arkiv resursen kommer att extrahera hello filer från hello komprimerade mappen.
* hello paketet installera resursen kommer att installera mobilitetstjänsten hello från hello UNIFIEDAGENT. EXE-installationsprogram med specifika hello-argument. (hello variabler som skapar hello argument måste toobe ändras tooreflect din miljö.)
* hello paketet AzureAgent resurs installeras hello Azure VM, vilket rekommenderas på varje virtuell dator som körs i Azure. hello Azure VM-agenten är det också möjligt tooadd tillägg toohello VM efter växling vid fel.
* hello säkerställer tjänsten eller de resurser som att hello relaterade mobilitetstjänsterna och hello Azure-tjänster alltid körs.

Spara hello konfiguration som **ASRMobilityService**.

> [!NOTE]
> Kom ihåg tooreplace hello CSIP i din konfiguration tooreflect hello faktiska management-servern så att hello agent ansluts korrekt och använder hello rätt lösenfras.
>
>

## <a name="step-3-upload-tooautomation-dsc"></a>Steg 3: Överför tooAutomation DSC
Eftersom hello DSC-konfiguration som du gjort importerar en modul för nödvändiga DSC-resurs (xPSDesiredStateConfiguration), måste tooimport att modulen i Automation innan du laddar upp hello DSC-konfigurationen.

Logga in tooyour Automation-konto för Bläddra**tillgångar** > **moduler**, och klicka på **Bläddra galleriet**.

Här kan du söka efter hello modulen och importera den tooyour konto.

![Importera modulen](./media/site-recovery-automate-mobilitysevice-install/search-and-import-module.png)

När du är klar detta går tooyour datorn där du har moduler som hello Azure Resource Manager har installerats och fortsätta tooimport hello nyskapad DSC-konfigurationen.

### <a name="import-cmdlets"></a>Importera cmdlet: ar
Logga in tooyour Azure-prenumeration i PowerShell. Ändra hello cmdlets tooreflect din miljö och avbilda kontoinformationen Automation i en variabel:

```powershell
$AAAccount = Get-AzureRmAutomationAccount -ResourceGroupName 'KNOMS' -Name 'KNOMSAA'
```

Överför hello configuration tooAutomation DSC med hjälp av hello följande cmdlet:

```powershell
$ImportArgs = @{
    SourcePath = 'C:\ASR\ASRMobilityService.ps1'
    Published = $true
    Description = 'DSC Config for Mobility Service'
}
$AAAccount | Import-AzureRmAutomationDscConfiguration @ImportArgs
```

### <a name="compile-hello-configuration-in-automation-dsc"></a>Kompilera hello konfigurationen i Automation DSC
Sedan måste toocompile hello konfigurationen i Automation DSC, så att du kan starta tooregister noder tooit. Du kan uppnå som genom att köra följande cmdlet hello:

```powershell
$AAAccount | Start-AzureRmAutomationDscCompilationJob -ConfigurationName ASRMobilityService
```

Detta kan ta några minuter, eftersom du i princip distribuerar hello toohello finns DSC pull-konfigurationstjänsten.

När du kompilerar hello konfiguration du kan hämta hello jobbinformation med hjälp av PowerShell (Get-AzureRmAutomationDscCompilationJob) eller genom att använda hello [Azure-portalen](https://portal.azure.com/).

![Hämta jobb](./media/site-recovery-automate-mobilitysevice-install/retrieve-job.png)

Du har nu publicerade och överföra din DSC-konfiguration tooAutomation DSC.

## <a name="step-4-onboard-machines-tooautomation-dsc"></a>Steg 4: Publicera datorer tooAutomation DSC
> [!NOTE]
> En av hello förutsättningar för att slutföra det här scenariot är att din Windows-datorer uppdateras med hello senaste versionen av WMF. Du kan hämta och installera rätt version av hello för din plattform från hello [Download Center](https://www.microsoft.com/download/details.aspx?id=50395).
>
>

Nu skapar du en metaconfig för DSC att du använder tooyour noder. toosucceed med den här behöver du tooretrieve hello endpoint URL och hello primär nyckel för det valda Automation-kontot i Azure. Du hittar dessa värden under **nycklar** på hello **alla inställningar** bladet för hello Automation-konto.

![Nyckelvärden](./media/site-recovery-automate-mobilitysevice-install/key-values.png)

I det här exemplet har du en fysisk server för Windows Server 2012 R2 som du vill tooprotect genom att använda Site Recovery.

### <a name="check-for-any-pending-file-rename-operations-in-hello-registry"></a>Kontrollera om det finns väntande filåtgärder rename i hello registret
Vi rekommenderar att du söker efter eventuella väntande filåtgärder rename i hello registret innan du börjar tooassociate hello server med hello Automation DSC-slutpunkten. De kan förhindra hello installationen att slutföra på grund av tooa väntande omstart.

Kör följande cmdlet tooverify att det finns ingen väntande omstart på hello server hello:

```powershell
Get-ItemProperty 'HKLM:\SYSTEM\CurrentControlSet\Control\Session Manager\' | Select-Object -Property PendingFileRenameOperations
```
Om det visar tomt är OK tooproceed. Om inte, du kan åtgärda detta genom hello servern startas om under en underhållsperiod.

tooapply hello konfigurationen på hello-servern, starta hello PowerShell Integrated Scripting Environment (ISE) och kör följande skript hello. Detta är i grunden en DSC lokala konfiguration som instruerar hello Local Configuration Manager-motorn tooregister med hello Automation DSC-tjänsten och hämta hello specifik konfiguration (ASRMobilityService.localhost).

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

Den här konfigurationen medför hello Local Configuration Manager motorn tooregister sig själv med Automation DSC. Den avgör också hur hello motorn ska köras, vad den ska göra om det finns en konfigurationsavvikelser (ApplyAndAutoCorrect) och hur den ska fortsätta med hello konfiguration om en omstart krävs.

När du har kört skriptet bör hello nod börja tooregister med Automation DSC.

![Noden registrering pågår](./media/site-recovery-automate-mobilitysevice-install/register-node.png)

Om du går tillbaka toohello Azure-portalen kan se du hello nyregistrerade noden nu har visats i hello-portalen.

![Registrerade nod i hello-portalen](./media/site-recovery-automate-mobilitysevice-install/registered-node.png)

Du kan köra hello följande PowerShell-cmdlet tooverify som hello nod har registrerats korrekt på hello-servern:

```powershell
Get-DscLocalConfigurationManager
```

När hello konfiguration har ägardatabasen och tillämpade toohello server kan kan du kontrollera detta genom att köra följande cmdlet hello:

```powershell
Get-DscConfigurationStatus
```

hello utdata visar hello servern har har hämtas dess konfiguration:

![Resultat](./media/site-recovery-automate-mobilitysevice-install/successful-config.png)

Dessutom hello Mobility tjänstinställning har sin egen logg som finns på *SystemDrive*\ProgramData\ASRSetupLogs.

Det stämmer. Du har nu distribuerats och registrerad hello mobilitetstjänsten på hello datorn som du vill tooprotect genom att använda Site Recovery. DSC ska kontrollera att tjänsterna för hello krävs alltid är igång.

![Slutförd distribution](./media/site-recovery-automate-mobilitysevice-install/successful-install.png)

När hello hanteringsservern identifierar hello lyckad distribution, kan du konfigurera skyddet och aktivera replikering på hello dator genom att använda Site Recovery.

## <a name="use-dsc-in-disconnected-environments"></a>Använd DSC i frånkopplade miljöer
Om dina datorer inte är anslutna toohello Internet kan du fortfarande är beroende av DSC-toodeploy och konfigurerar hello mobilitetstjänsten på hello arbetsbelastningar som du vill tooprotect.

Du kan initiera en egen hämtningsservern för DSC i din miljö tooessentially innehåller hello samma funktioner som du får från Automation DSC. Det vill säga hello klienter hämtar hello-konfiguration (när den är registrerad) toohello DSC-slutpunkten. Ett annat alternativ är dock toomanually push hello DSC-konfiguration tooyour datorer, lokalt eller fjärranslutet.

Observera att i det här exemplet finns det en parameter som lagts till för hello datornamn. hello fjärrfiler finns nu på en fjärresurs som ska vara tillgängliga av hello datorer som du vill tooprotect. hello slutet av hello skript utfärdar hello-konfigurationen och startar sedan tooapply hello DSC-konfiguration toohello måldatorn.

### <a name="prerequisites"></a>Krav
Se till att hello xPSDesiredStateConfiguration PowerShell-modulen är installerad. Du kan installera hello xPSDesiredStateConfiguration modul för Windows-datorer där WMF 5.0 är installerat, genom att köra följande cmdlet för hello måldatorerna hello:

```powershell
Find-Module -Name xPSDesiredStateConfiguration | Install-Module
```

Du kan också hämta och spara hello modulen om du behöver toodistribute den tooWindows datorer som har WMF 4.0. Kör denna cmdlet på en dator där PowerShellGet (WMF 5.0) förekommer:

```powershell
Save-Module -Name xPSDesiredStateConfiguration -Path <location>
```

Kontrollera också att hello för WMF 4.0, [Windows 8.1 update KB2883200](https://www.microsoft.com/download/details.aspx?id=40749) är installerat på hello datorer.

hello flyttas följande konfiguration tooWindows datorer som har WMF 5.0 och WMF 4.0:

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

Om du vill tooinstantiate hämtningsservern egna DSC på ditt företagsnätverk toomimic hello-funktioner som du kan hämta från Automation DSC, se [ställer in en pull webbserver DSC](https://msdn.microsoft.com/powershell/dsc/pullserver?f=255&MSPPError=-2147217396).

## <a name="optional-deploy-a-dsc-configuration-by-using-an-azure-resource-manager-template"></a>Valfritt: Distribuera en DSC-konfigurationen med hjälp av en Azure Resource Manager-mall
Den här artikeln har fokuserar på hur du kan skapa egna DSC-konfigurationen tooautomatically distribuera hello mobilitetstjänsten och hello Azure VM-agenten och se till att de körs på hello datorer som du vill tooprotect. Vi har också en Azure Resource Manager-mall som ska distribuera den här DSC-konfiguration tooa ny eller befintlig Azure Automation-konto. hello-mallen använder indataparametrar toocreate Automation tillgångar som innehåller hello variabler för din miljö.

När du har distribuerat hello mallen kan du bara referera toostep 4 i den här guiden tooonboard dina datorer.

hello mallen gör hello följande:

1. Använd ett befintligt Automation-konto eller skapa en ny
2. Ta indataparametrar:
   * ASRRemoteFile--hello plats där du har sparat hello Mobility tjänstinställning
   * ASRPassphrase--hello plats där du har sparat hello passphrase.txt fil
   * ASRCSEndpoint--hello IP-adressen för hanteringsservern
3. Importera hello xPSDesiredStateConfiguration PowerShell-modulen
4. Skapa och kompilera hello DSC-konfiguration

Alla hello föregående steg sker i hello rätt ordning, så att du kan starta onboarding dina datorer för skydd.

hello-mallen med instruktioner för distribution, finns på [GitHub](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/DSC).

Distribuera hello-mallen med hjälp av PowerShell:

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
När du distribuerar hello Mobility service agenter, kan du [Aktivera replikering](site-recovery-vmware-to-azure.md) för hello virtuella datorer.
