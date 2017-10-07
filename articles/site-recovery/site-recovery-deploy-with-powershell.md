---
title: aaaReplicate Hyper-V VMs tooAzure i hello klassiska portalen med PowerShell | Microsoft Docs
description: Automatisera hello replikering av Hyper-V virtuella datorer i VMM-moln med Site Recovery och PowerShell i hello klassiska portalen
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: tysonn
ms.assetid: 9011f567-e0b4-4306-951a-b30da19f5db6
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: d6847b46ac227209e6890de4ab603b23f827360f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-vms-tooazure-with-powershell-in-hello-classic-portal"></a>Replikera virtuella Hyper-V-datorer tooAzure med PowerShell i hello klassiska portalen
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [Klassisk portal](site-recovery-vmm-to-azure-classic.md)
> * [PowerShell – Klassisk](site-recovery-deploy-with-powershell.md)
>
>

## <a name="overview"></a>Översikt
Azure Site Recovery bidrar tooyour disaster recovery (BCDR) strategi för affärskontinuitet och genom att samordna replikering, redundans och återställning av virtuella datorer i ett antal distributionsscenarier. En fullständig lista över distributionen scenarier finns hello [översikt över Azure Site Recovery](site-recovery-overview.md).

Den här artikeln visar hur toouse PowerShell tooautomate vanliga uppgifter du behöver tooperform när du ställer in Azure Site Recovery tooreplicate Hyper-V virtuella datorer i System Center VMM-moln tooAzure lagring.

hello artikeln innehåller kraven för hello scenariot och visar dig hur tooset upp ett Site Recovery-valvet, installera hello Azure Site Recovery-providern på VMM-källservern hello, registrera hello servern i hello valv, lägga till ett Azure storage-konto, installera hello Azure Recovery Services-agenten på Hyper-V-värdservrar, konfigurera skyddsinställningar för VMM-moln som kommer att tillämpas tooall skyddade virtuella datorer och sedan aktivera skyddet för de virtuella datorerna. Avsluta med att testa hello redundans toomake att allt fungerar som förväntat.

Om du stöter på problem med att installera det här scenariot, publicera dina frågor på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen.
>
>

## <a name="before-you-start"></a>Innan du börjar
Kontrollera att du har dessa krav är uppfyllda:

### <a name="azure-prerequisites"></a>Krav för Azure
* Du behöver ett [Microsoft Azure](https://azure.microsoft.com/)-konto. Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
* Du behöver ett Azure storage-konto toostore replikerade data. hello-kontot måste geo-replikering aktiverat. Det bör vara i samma region som hello Azure Site Recovery-valvet hello och associeras med hello samma prenumeration. [Lär dig mer om Azure storage](../storage/common/storage-introduction.md).
* Du behöver toomake till virtuella datorer som du vill tooprotect följer [förutsättningar för Azure virtuell dator](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

### <a name="vmm-prerequisites"></a>VMM – krav
* Du måste VMM-servern som körs på System Center 2012 R2.
* Du behöver minst ett moln på hello VMM-servern som du vill tooprotect. hello moln bör innehålla:
  * En eller flera VMM-värdgrupper.
  * En eller flera Hyper-V-värdservrar eller kluster i varje värdgrupp.
  * En eller flera virtuella datorer på källservern för hello Hyper-V.

### <a name="hyper-v-prerequisites"></a>Hyper-V-krav
* hello Hyper-V-värdservrar måste köra minst **Windows Server 2012** med Hyper-V-rollen eller **Microsoft Hyper-V Server 2012** och har hello senaste uppdateringarna installerade.
* Om du kör Hyper-V i ett kluster bör du vara medveten om att klusterutjämning inte skapas automatiskt om du har ett statiskt IP-adressbaserat kluster. Du behöver tooconfigure hello klusterutjämning manuellt. toodo detta i Serverhanteraren > Klusterhanteraren för växling vid fel, Anslut toohello klustret, klicka på **konfigurera roll** och välj **Hyper-V Replica Broker** i hello **Välj roll**skärmen i guiden hög tillgänglighet för hello.
* Alla Hyper-V-värdserver eller kluster som du vill toomanage skydd måste ingå i ett VMM-moln.

### <a name="network-mapping-prerequisites"></a>Krav för nätverksmappning
När du skyddar virtuella datorer i Azure mappar nätverksmappningen mellan Virtuella datornätverk på VMM-källservern hello och rikta Azure nätverk tooenable hello följande:

* Alla datorer som redundansväxlas hello samma nätverk kan ansluta tooeach andra, oavsett vilken återställningsplan de finns i.
* Om en nätverksgateway har konfigurerats på hello Azure-målnätverket kan kan virtuella datorer ansluta tooother lokala virtuella datorer.
* Om du inte konfigurerar nätverk mappning endast virtuella datorer som växlar över i hello samma återställningsplan kommer att kunna tooconnect tooeach andra efter växling vid fel tooAzure.

Om du vill toodeploy nätverksmappning behöver hello följande:

* hello virtuella datorer du vill tooprotect på VMM-källservern hello ska vara anslutna tooa VM-nätverk. Nätverket bör vara länkade tooa logiska nätverk som är kopplad till hello moln.
* En Azure-nätverk toowhich replikerade virtuella datorer kan ansluta efter växling vid fel. Du väljer det här nätverket vid hello tiden för växling vid fel. hello nätverket bör finnas i hello samma region som din Azure Site Recovery-prenumeration.

### <a name="powershell-prerequisites"></a>PowerShell-krav
Kontrollera att du har Azure PowerShell redo toogo. Om du redan använder PowerShell, behöver du tooupgrade tooversion 0.8.10 eller senare. Information om hur du konfigurerar PowerShell finns i [hur tooinstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs). När du har skapat och konfigurerat PowerShell, kan du visa alla hello tillgängliga cmdlets för hello tjänsten [här](/powershell/azure/overview).

toolearn om tips som kan hjälpa dig att använda hello-cmdlets, till exempel hur parametervärden indata och utdata hanteras vanligtvis av Azure PowerShell finns [Kom igång med Azure-Cmdlets](/powershell/azure/get-started-azureps).

## <a name="step-1-set-hello-subscription"></a>Steg 1: Ange hello prenumeration
Kör följande cmdlets i PowerShell:

```
$UserName = "<user@live.com>"
$Password = "<password>"
$AzureSubscriptionName = "prod_sub1"

$SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
$Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $securePassword
Add-AzureAccount -Credential $Cred;
$AzureSubscription = Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

```

Ersätt hello element i hello ”< >” med din specifika information.

## <a name="step-2-create-a-site-recovery-vault"></a>Steg 2: Skapa ett Site Recovery-valv
Ersätt hello element i hello ”< >” med din specifika information i PowerShell och köra följande kommandon:

```

$VaultName = "<testvault123>"
$VaultGeo  = "<Southeast Asia>"
$OutputPathForSettingsFile = "<c:\>"

```


```
New-AzureSiteRecoveryVault -Location $VaultGeo -Name $VaultName;
$vault = Get-AzureSiteRecoveryVault -Name $VaultName;

```

## <a name="step-3-generate-a-vault-registration-key"></a>Steg 3: Skapa en valvregistreringsnyckel
Generera en registreringsnyckel i valvet hello. När du hämtar hello Azure Site Recovery-providern och installera den på hello VMM-servern, ska du använda denna nyckel tooregister hello VMM-server i hello-valvet.

1. Hämta filen hello valvet och ange hello kontext:

   ```

   $VaultName = "<testvault123>"
   $VaultGeo  = "<Southeast Asia>"
   $OutputPathForSettingsFile = "<c:\>"

   $VaultSetingsFile = Get-AzureSiteRecoveryVaultSettingsFile -Location $VaultGeo -Name $VaultName -Path $OutputPathForSettingsFile;

   ```
2. Ange hello valvet kontexten genom att köra följande kommandon hello:

   ```

   $VaultSettingFilePath = $vaultSetingsFile.FilePath
   $VaultContext = Import-AzureSiteRecoveryVaultSettingsFile -Path $VaultSettingFilePath -ErrorAction Stop

   ```

## <a name="step-4-install-hello-azure-site-recovery-provider"></a>Steg 4: Installera hello Azure Site Recovery Provider
1. Skapa en katalog på hello VMM-datorn genom att köra följande kommando hello:

   ```

   pushd C:\ASR\

   ```
2. Extrahera hello filer med hjälp av hello ned providern genom att köra följande kommando hello

   ```

   AzureSiteRecoveryProvider.exe /x:. /q

   ```
3. Installera hello-providern med hello följande kommandon:

   ```

   .\SetupDr.exe /i

   ```

   ```

   $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
   do
   {
     $isNotInstalled = $true;
     if(Test-Path $installationRegPath)
     {
         $isNotInstalled = $false;
     }
   }While($isNotInstalled)

   ```

   Vänta tills hello installation toofinish.
4. Registrera hello servern i hello valvet med hello följande kommando:

   ```

   $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
   pushd $BinPath
   $encryptionFilePath = "C:\temp\"
   .\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

   ```

## <a name="step-5-create-an-azure-storage-account"></a>Steg 5: Skapa ett Azure storage-konto
Om du inte har ett Azure storage-konto skapar du ett konto med geo-replikering aktiverat genom att köra följande kommando hello:

```

$StorageAccountName = "teststorageacc1"
$StorageAccountGeo  = "Southeast Asia"

New-AzureStorageAccount -StorageAccountName $StorageAccountName -Label $StorageAccountName -Location $StorageAccountGeo;

```

Observera att hello storage-konto måste vara i samma region som hello Azure Site Recovery-tjänsten hello och associeras med hello samma prenumeration.

## <a name="step-6-install-hello-azure-recovery-services-agent"></a>Steg 6: Installera hello Azure Recovery Services-agenten
Du vill tooprotect från hello Azure-portalen installera hello Azure Recovery Services-agenten på varje Hyper-V-värdserver i hello VMM-moln.

Kör följande kommando på alla värdar i VMM hello:

```

marsagentinstaller.exe /q /nu

```


## <a name="step-7-configure-cloud-protection-settings"></a>Steg 7: Konfigurera moln skyddsinställningarna
1. Skapa en moln skydd profil tooAzure genom att köra följande kommando hello:

   ```

   $ReplicationFrequencyInSeconds = "300";
   $ProfileResult = New-AzureSiteRecoveryProtectionProfileObject -ReplicationProvider HyperVReplica -RecoveryAzureSubscription $AzureSubscriptionName -RecoveryAzureStorageAccount $StorageAccountName -ReplicationFrequencyInSeconds     $ReplicationFrequencyInSeconds;

   ```
2. Hämta en skyddsbehållaren genom att köra följande kommandon hello:

   ```

   $PrimaryCloud = "testcloud"
   $protectionContainer = Get-AzureSiteRecoveryProtectionContainer -Name $PrimaryCloud;

   ```
3. Starta hello associering av hello skyddsbehållaren hello moln:

   ```

   $associationJob = Start-AzureSiteRecoveryProtectionProfileAssociationJob -ProtectionProfile $profileResult -PrimaryProtectionContainer $protectionContainer;        

   ```
4. När hello jobbet har slutförts, kör du hello följande kommando:

        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
        $isJobLeftForProcessing = $true;
        }


1. När hello jobbet har bearbetat köra hello följande kommando:

        Do
        {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

        if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
        }While($isJobLeftForProcessing)



toocheck hello slutföra hello åtgärden gör hello i [Övervakaraktiviteten](#monitor).

## <a name="step-8-configure-network-mapping"></a>Steg 8: Konfigurera nätverksmappning
Innan du börjar nätverksmappning Kontrollera att virtuella datorer på VMM-källservern hello anslutna tooa VM-nätverk. Dessutom kan skapa en eller flera virtuella Azure-nätverk. Observera att flera Virtuella datornätverk kan mappas tooa enda Azure-nätverk.

Observera att om hello målnätverket har flera undernät och ett av dessa undernät har hello samma namn som undernätet på vilka hello virtuella källdatorn finns så hello replikerade virtuella datorn kommer att vara anslutna toothat målundernätverket efter växling vid fel. Om det inte finns en målundernät med ett matchande namn att hello virtuella datorn anslutna toohello första undernätet i hello nätverk.

hello första kommandot hämtar servrar för hello aktuella Azure Site Recovery-valvet. hello kommandot lagrar hello Microsoft Azure Site Recovery-servrar i hello $Servers matrisvariabel.

    $Servers = Get-AzureSiteRecoveryServer


hello andra kommandot hämtar hello site recovery nätverk för hello första servern i hello $Servers matris. hello kommandot lagrar hello nätverk i hello $Networks variabeln.

    $Networks = Get-AzureSiteRecoveryNetwork -Server $Servers[0]

hello tredje kommandot hämtar dina Azure-prenumerationer genom att använda hello Get-AzureSubscription cmdlet och sedan lagrar värdet i hello $Subscriptions variabel.

    $Subscriptions = Get-AzureSubscription



hello fjärde kommandot hämtar virtuella Azure-nätverk med hjälp av hello Get-AzureVNetSite cmdlet och sedan värdet i hello $AzureVmNetworks variabel.

    $AzureVmNetworks = Get-AzureVNetSite



hello slutliga cmdlet skapar en mappning mellan hello primära nätverk och hello Azure virtuella nätverk. hello cmdlet anger hello primära nätverket som hello första elementet i $Networks. hello cmdlet anger ett virtuellt datornätverk som hello första elementet i $AzureVmNetworks med hjälp av ett ID. hello-kommandot innehåller din Azure prenumerations-ID.

    New-AzureSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureSubscriptionId $Subscriptions[0].SubscriptionId -AzureVMNetworkId $AzureVmNetworks[0].Id


## <a name="step-9-enable-protection-for-virtual-machines"></a>Steg 9: Aktivera skydd för virtuella datorer
Du kan aktivera skydd för virtuella datorer i molnet hello när servrar, moln och nätverk har konfigurerats korrekt. Observera följande hello:

Virtuella datorer måste uppfylla [förutsättningar för Azure virtuell dator](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

tooenable skydd hello-operativsystem och operativsystem diskegenskaper måste anges för hello virtuella datorn. När du skapar en virtuell dator i VMM med en mall för virtuell dator kan du ange hello-egenskapen. Du kan också ange egenskaperna för befintliga virtuella datorer på hello **allmänna** och **maskinvarukonfiguration** flikar hello egenskaperna för virtuella datorn. Om du inte anger dessa egenskaper i VMM kommer du att kunna tooconfigure dem i hello Azure Site Recovery-portalen.

1. tooenable skydd, kör följande kommando tooget hello skyddsbehållaren hello:

     $ProtectionContainer = get-AzureSiteRecoveryProtectionContainer-Name $CloudName
2. Hämta hello skyddad entitet (VM) genom att köra följande kommando hello:

        $protectionEntity = Get-AzureSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer



1. Aktivera hello DR för hello VM genom att köra följande kommando hello:

        $jobResult = Set-AzureSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity     -Protection Enable -Force



## <a name="test-your-deployment"></a>Testa distributionen
tootest planera distributionen av du kan köra ett redundanstest för en enskild virtuell dator eller skapa en återställningsplan som består av flera virtuella datorer och köra ett redundanstest för hello. Ett redundanstest simulerar redundans- och återställningsmekanismen i ett isolerat nätverk. Tänk på följande:

* Om du vill tooconnect toohello virtuell dator i Azure med hjälp av fjärrskrivbord efter hello växling vid fel, måste du aktivera anslutning till fjärrskrivbord på hello virtuell dator innan du kör hello testa redundans.
* Efter växling vid fel, ska du använda en offentlig IP-adress tooconnect toohello virtuell-dator i Azure med hjälp av fjärrskrivbord. Om du vill toodo detta, se till att du inte har några domänprinciper som hindrar dig från anslutande tooa virtuell dator med en offentlig adress.

toocheck hello slutföra hello åtgärden gör hello i [Övervakaraktiviteten](#monitor).

### <a name="create-a-recovery-plan"></a>Skapa en återställningsplan
1. Skapa en XML-fil som en mall för din återställningsplan med hello data nedan och spara den som ”C:\RPTemplatePath.xml”.
2. Ändra hello RecoveryPlan nod Id, namn, PrimaryServerId och SecondaryServerId.
3. Ändra hello ProtectionEntity nod PrimaryProtectionEntityId (vmid från VMM).
4. Du kan lägga till flera virtuella datorer genom att lägga till fler ProtectionEntity noder.

        <#
        <?xml version="1.0" encoding="utf-16"?>
        <RecoveryPlan Id="d0323b26-5be2-471b-addc-0a8742796610" Name="rp-test"     PrimaryServerId="9350a530-d5af-435b-9f2b-b941b5d9fcd5"     SecondaryServerId="21a9403c-6ec1-44f2-b744-b4e50b792387" Description=""     Version="V2014_07">
          <Actions />
          <ActionGroups>
            <ShutdownAllActionGroup Id="ShutdownAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </ShutdownAllActionGroup>
            <FailoverAllActionGroup Id="FailoverAllActionGroup">
              <PreActionSequence />
              <PostActionSequence />
            </FailoverAllActionGroup>
            <BootActionGroup Id="DefaultActionGroup">
              <PreActionSequence />
              <PostActionSequence />
              <ProtectionEntity PrimaryProtectionEntityId="d4c8ce92-a613-4c63-9b03-    cf163cc36ef8" />
            </BootActionGroup>
          </ActionGroups>
          <ActionGroupSequence>
            <ActionGroup Id="ShutdownAllActionGroup" ActionId="ShutdownAllActionGroup"     Before="FailoverAllActionGroup" />
            <ActionGroup Id="FailoverAllActionGroup" ActionId="FailoverAllActionGroup"     After="ShutdownAllActionGroup" Before="DefaultActionGroup" />
            <ActionGroup Id="DefaultActionGroup" ActionId="DefaultActionGroup" After="FailoverAllActionGroup"/>
          </ActionGroupSequence>
        </RecoveryPlan>
        #>



1. Fyll i hello data i hello mallen:

        $TemplatePath = "C:\RPTemplatePath.xml";



1. Skapa hello RecoveryPlan:

        $RPCreationJob = New-AzureSiteRecoveryRecoveryPlan -File $TemplatePath -WaitForCompletion;

### <a name="run-a-test-failover"></a>Köra ett redundanstest
1. Hämta hello RecoveryPlan objekt genom att köra följande kommando hello:

     $RPObject = get-AzureSiteRecoveryRecoveryPlan-Name $RPName;
2. Starta hello testa redundans genom att köra följande kommando hello:

        $jobIDResult = Start-AzureSiteRecoveryTestFailoverJob -RecoveryPlan $RPObject -Direction PrimaryToRecovery;


## <a name=monitor></a>Övervakaraktivitet
Använd följande kommandon toomonitor hello aktivitet hello. Observera att du har toowait mellan jobb för hello bearbetning toofinish.

    Do
    {
            $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
            Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
            if($job -eq $null -or $job.StateDescription -ne "Completed")
            {
                $isJobLeftForProcessing = $true;
            }

        if($isJobLeftForProcessing)
            {
                Start-Sleep -Seconds 60
            }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a>Nästa steg
[Läs mer](/powershell/azure/overview) om Azure Site Recovery PowerShell-cmdlets. </a>.
