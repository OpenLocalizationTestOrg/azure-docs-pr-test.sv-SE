---
title: aaaReplicate Hyper-V virtuella datorer i VMM-moln med Azure Site Recovery och PowerShell (Resource Manager) | Microsoft Docs
description: Replikera virtuella Hyper-V-datorer i VMM-moln med Azure Site Recovery och PowerShell
services: site-recovery
documentationcenter: 
author: Rajani-Janaki-Ram
manager: rochakm
editor: raynew
ms.assetid: 6ac509ad-5024-43d8-b621-d8fec019b9a9
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: rajanaki
ms.openlocfilehash: b0f641de4e10a600ead415ceb9bd488fb4d1659d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure-using-powershell-and-azure-resource-manager"></a>Replikera virtuella Hyper-V-datorer i VMM-moln tooAzure med PowerShell och Azure Resource Manager
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

hello artikeln handlar om förutsättningar för hello scenariot och visar

* Hur tooset upp ett Recovery Services-valv
* Installera hello Azure Site Recovery-providern på VMM-källservern hello
* Registrera hello servern i hello valv, lägga till ett Azure storage-konto
* Installera hello Azure Recovery Services-agenten på Hyper-V-värdservrar
* Konfigurera skyddsinställningar för VMM-moln som ska tillämpas tooall skyddade virtuella datorer
* Aktivera skyddet för de virtuella datorerna.
* Testa hello failover toomake att allt fungerar som förväntat.

Om du stöter på problem med att installera det här scenariot, publicera dina frågor på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [!NOTE]
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md). Den här artikeln täcker hello Resource Manager-modellen.
>
>

## <a name="before-you-start"></a>Innan du börjar
Kontrollera att du har dessa krav är uppfyllda:

### <a name="azure-prerequisites"></a>Krav för Azure
* Du behöver ett [Microsoft Azure](https://azure.microsoft.com/)-konto. Om du inte har någon, börja med en [kostnadsfritt konto](https://azure.microsoft.com/free). Dessutom kan du läsa om hello [priser för Azure Site Recovery Manager](https://azure.microsoft.com/pricing/details/site-recovery/).
* Du behöver en CSP-prenumeration om du provar ut hello replikering tooa CSP prenumeration scenario. Mer information om hello CSP-programmet i [hur tooenroll i hello CSP program](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).
* Du behöver ett Azure v2 lagring (Resource Manager) konto toostore data som replikeras tooAzure. hello-kontot måste geo-replikering aktiverat. Det bör vara i samma region som hello Azure Site Recovery-tjänsten hello och associeras med hello samma prenumeration eller hello CSP-prenumeration. toolearn mer om hur du konfigurerar Azure storage finns hello [introduktion tooMicrosoft Azure Storage](../storage/common/storage-introduction.md) referens.
* Du behöver toomake till att virtuella datorer som du vill tooprotect följer hello [förutsättningar för Azure virtuell dator](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

> [!NOTE]
> För närvarande är endast VM nivån åtgärder möjlig via Powershell. Stöd för nivån återställningsåtgärder för planen kommer att göras snart.  Nu är du begränsad tooperforming misslyckas-FELNIVÅ endast på en ”skyddad VM' kornighet och inte på en återställning planera nivå.
>
>

### <a name="vmm-prerequisites"></a>VMM – krav
* Du måste VMM-servern som körs på System Center 2012 R2.
* Någon VMM-server som innehåller virtuella datorer vill du tooprotect måste köra hello Azure Site Recovery-providern. Detta installeras under hello Azure Site Recovery-distribution.
* Du behöver minst ett moln på hello VMM-servern som du vill tooprotect. hello moln bör innehålla:
  * En eller flera VMM-värdgrupper.
  * En eller flera Hyper-V-värdservrar eller Hyper-V-kluster i varje värdgrupp.
  * En eller flera virtuella datorer på källservern för hello Hyper-V.
* Lär dig mer om hur du konfigurerar VMM-moln:
  * Läs mer om privata VMM-moln i [vad är nytt i privata moln med VMM för System Center 2012 R2](http://go.microsoft.com/fwlink/?LinkId=324952) och i [VMM 2012 och hello moln](http://go.microsoft.com/fwlink/?LinkId=324956).
  * Lär dig mer om [konfigurera hello VMM molnet fabric](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)
  * När ditt moln fabric-element är på plats Lär dig mer om att skapa privata moln i [skapa ett privat moln i VMM](http://go.microsoft.com/fwlink/p/?LinkId=324953) och i [genomgång: skapa privata moln med System Center 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).

### <a name="hyper-v-prerequisites"></a>Hyper-V-krav
* hello Hyper-V-värdservrar måste köra minst **Windows Server 2012** med Hyper-V-rollen eller **Microsoft Hyper-V Server 2012** och har hello senaste uppdateringarna installerade.
* Om du kör Hyper-V i ett kluster bör du vara medveten om att klusterutjämning inte skapas automatiskt om du har ett statiskt IP-adressbaserat kluster. Du behöver tooconfigure hello klusterutjämning manuellt. för
* Instruktioner finns i [hur tooConfigure Hyper-V Replica Broker](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).
* Alla Hyper-V-värdserver eller kluster som du vill toomanage skydd måste ingå i ett VMM-moln.

### <a name="network-mapping-prerequisites"></a>Krav för nätverksmappning
När du skyddar virtuella datorer i Azure mappar nätverksmappningen hello hello virtuella datornätverk på VMM-källservern hello och mål Azure nätverk tooenable hello följande:

* Alla datorer som redundansväxlas hello samma nätverk kan ansluta tooeach andra, oavsett vilken återställningsplan de finns i.
* Om en nätverksgateway har konfigurerats på hello Azure-målnätverket kan kan virtuella datorer ansluta tooother lokala virtuella datorer.
* Om du inte konfigurerar nätverksmappning kan endast hello virtuella datorer som växlar över i hello samma återställningsplan kommer att kunna tooconnect tooeach andra efter failover tooAzure.

Om du vill toodeploy nätverksmappning behöver hello följande:

* hello virtuella datorer du vill tooprotect på VMM-källservern hello ska vara anslutna tooa VM-nätverk. Nätverket bör vara länkade tooa logiska nätverk som är kopplad till hello moln.
* En Azure-nätverk toowhich replikerade virtuella datorer kan ansluta efter redundansväxling. Du väljer det här nätverket när hello failover. hello nätverket bör finnas i hello samma region som din Azure Site Recovery-prenumeration.

Mer information om nätverksmappning i

* [Hur tooconfigure logiska nätverk i VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [Hur tooconfigure Virtuella nätverk och gateways i VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)
* [Hur tooconfigure och övervaka virtuella nätverk i Azure](https://azure.microsoft.com/documentation/services/virtual-network/)

### <a name="powershell-prerequisites"></a>PowerShell-krav
Kontrollera att du har Azure PowerShell redo toogo. Om du redan använder PowerShell, behöver du tooupgrade tooversion 0.8.10 eller senare. Information om hur du konfigurerar PowerShell finns i hello [Guide tooinstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs). När du har skapat och konfigurerat PowerShell, kan du visa alla hello tillgängliga cmdlets för hello tjänsten [här](/powershell/azure/overview).

toolearn om tips som kan hjälpa dig att använda hello-cmdlets, till exempel hur parametervärden indata och utdata hanteras vanligtvis av Azure PowerShell finns hello [Guide tooget igång med Azure-Cmdlets](/powershell/azure/get-started-azureps).

## <a name="step-1-set-hello-subscription"></a>Steg 1: Ange hello prenumeration
1. Från Azure powershell, inloggning tooyour Azure-konto: med hello följande cmdlet: ar

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. Hämta en lista över dina prenumerationer. Detta visar också en lista över hello subscriptionIDs för varje hello prenumerationer. Notera hello prenumerations-ID för hello prenumeration där du vill toocreate hello recovery services-valvet

        Get-AzureRmSubscription
3. Ange hello prenumeration i vilka hello recovery services-ventilen är toobe som skapats av nämna hello prenumerations-ID

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="step-2-create-a-recovery-services-vault"></a>Steg 2: Skapa ett Recovery Services-valv
1. Skapa en resursgrupp i Azure Resource Manager om du inte redan har en

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. Skapa ett nytt Recovery Services-valv och spara hello skapade ASR valvet objekt i en variabel (kommer att användas senare). Du kan också hämta hello ASR valvet post skapa objektet med hello Get-AzureRMRecoveryServicesVault cmdlet:-

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-hello-recovery-services-vault-context"></a>Steg 3: Ange hello Recovery Services-ventilen kontext

Ange hello valvet kontexten genom att köra hello nedan kommando.

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-hello-azure-site-recovery-provider"></a>Steg 4: Installera hello Azure Site Recovery Provider
1. Skapa en katalog på hello VMM-datorn genom att köra följande kommando hello:

       New-Item c:\ASR -type directory
2. Extrahera hello filer med hjälp av hello ned providern genom att köra följande kommando hello

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. Installera hello-providern med hello följande kommandon:

       .\SetupDr.exe /i
       $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
       do
       {
         $isNotInstalled = $true;
         if(Test-Path $installationRegPath)
         {
           $isNotInstalled = $false;
         }
       }While($isNotInstalled)

   Vänta tills hello installation toofinish.
4. Registrera hello servern i hello valvet med hello följande kommando:

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="step-5-create-an-azure-storage-account"></a>Steg 5: Skapa ett Azure storage-konto

Om du inte har ett Azure storage-konto, skapa ett konto med geo-replikering är aktiverat i hello samma geo som hello valvet genom att köra följande kommando hello:

        $StorageAccountName = "teststorageacc1"    #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"     
        $ResourceGroupName =  “myRG”             #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

Observera att hello storage-konto måste vara i samma region som hello Azure Site Recovery-tjänsten hello och associeras med hello samma prenumeration.

## <a name="step-6-install-hello-azure-recovery-services-agent"></a>Steg 6: Installera hello Azure Recovery Services-agenten
1. Hämta hello Azure Recovery Services-agenten på [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) och installera det på varje Hyper-V-värdserver i hello VMM moln du vill tooprotect.
2. Kör följande kommando på alla värdar i VMM hello:

       marsagentinstaller.exe /q /nu

## <a name="step-7-configure-cloud-protection-settings"></a>Steg 7: Konfigurera moln skyddsinställningarna
1. Skapa en replikering princip tooAzure genom att köra följande kommando hello:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify hello number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

1. Hämta en skyddsbehållaren genom att köra följande kommandon hello:

       $PrimaryCloud = "testcloud"
       $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  
2. Hämta hello princip information tooa variabeln med hello jobb som har skapats och nämna hello eget namn:

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. Starta hello associering av hello skyddsbehållaren med hello replikeringsprincip:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  
4. När hello jobbet har slutförts, kör du hello följande kommando:

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }
5. När hello jobbet har bearbetat köra hello följande kommando:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

toocheck hello slutföra hello åtgärden gör hello i [Övervakaraktiviteten](#monitor).

## <a name="step-8-configure-network-mapping"></a>Steg 8: Konfigurera nätverksmappning
Innan du börjar nätverksmappning Kontrollera att virtuella datorer på VMM-källservern hello anslutna tooa VM-nätverk. Dessutom kan skapa en eller flera virtuella Azure-nätverk.

Mer information om hur toocreate ett virtuellt nätverk med Azure Resource Manager och PowerShell, i [skapa ett virtuellt nätverk med en plats-till-plats VPN-anslutning med Azure Resource Manager och PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)

Observera att flera virtuella nätverk kan vara mappade tooa enda Azure-nätverk. Om hello målnätverket har flera undernät och ett av dessa undernät har hello samma namn som undernätet på vilka hello virtuella källdatorn finns så hello replikerade virtuella datorn kommer att vara anslutna toothat målundernätverket efter redundansväxling. Om det inte finns något målundernät med ett matchande namn, att hello virtuella datorn anslutna toohello första undernätet i hello nätverk.

1. hello första kommandot hämtar servrar för hello aktuella Azure Site Recovery-valvet. hello kommandot lagrar hello Microsoft Azure Site Recovery-servrar i hello $Servers matrisvariabel.

        $Servers = Get-AzureRmSiteRecoveryServer
2. hello andra kommandot hämtar hello site recovery nätverk för hello första servern i hello $Servers matris. hello kommandot lagrar hello nätverk i hello $Networks variabeln.

        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

1. hello tredje kommandot hämtar virtuella Azure-nätverk och värdet i hello $AzureVmNetworks variabel.

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork
2. hello slutliga cmdlet skapar en mappning mellan hello primära nätverk och hello Azure virtuella nätverk. hello cmdlet anger hello primära nätverket som hello första elementet i $Networks. hello cmdlet anger ett virtuellt datornätverk som hello första elementet i $AzureVmNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]

## <a name="step-9-enable-protection-for-virtual-machines"></a>Steg 9: Aktivera skydd för virtuella datorer
Du kan aktivera skydd för virtuella datorer i molnet hello när hello servrar, moln och nätverk har konfigurerats korrekt.

 Observera följande hello:

* Virtuella datorer måste uppfylla kraven för Azure. Kontrollera dessa i [krav och Support](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) i Planeringsguiden för hello.
* tooenable skydd, hello operativsystem och operativsystem diskegenskaper måste anges för hello virtuella datorn. När du skapar en virtuell dator i VMM med en mall för virtuell dator kan du ange hello-egenskapen. Du kan också ange egenskaperna för befintliga virtuella datorer på hello **allmänna** och **maskinvarukonfiguration** flikar hello egenskaperna för virtuella datorn. Om du inte anger dessa egenskaper i VMM kommer du att kunna tooconfigure dem i hello Azure Site Recovery-portalen.

1. tooenable skydd, kör följande kommando tooget hello skyddsbehållaren hello:

          $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName
2. Hämta hello skyddad entitet (VM) genom att köra följande kommando hello:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer
3. Aktivera hello DR för hello VM genom att köra följande kommando hello:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1

## <a name="test-your-deployment"></a>Testa distributionen
tootest din distribution som du kan köra ett test failover för en enskild virtuell dator eller skapa en återställningsplan som består av flera virtuella datorer och kör ett test failover för hello-plan. Test av redundansväxling simulerar din failover- och återställningsmekanismen i ett isolerat nätverk. Tänk på följande:

* Om du vill tooconnect toohello virtuell dator i Azure med hjälp av fjärrskrivbord efter hello failover, aktivera anslutning till fjärrskrivbord på hello virtuell dator innan du kör hello testa redundans.
* Efter redundansväxling, ska du använda en offentlig IP-adress tooconnect toohello virtuell-dator i Azure med hjälp av fjärrskrivbord. Om du vill toodo detta, se till att du inte har några domänprinciper som hindrar dig från anslutande tooa virtuell dator med en offentlig adress.

toocheck hello slutföra hello åtgärden gör hello i [Övervakaraktiviteten](#monitor).

### <a name="run-a-test-failover"></a>Köra ett redundanstest
- Starta hello testa redundans genom att köra följande kommando hello:

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a>Kör en planerad redundans
- Starta hello planerad redundans genom att köra följande kommando hello:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a>Kör en oplanerad redundans
- Starta hello oplanerad redundans genom att köra följande kommando hello:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

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
[Läs mer](/powershell/module/azurerm.recoveryservices.backup/#recovery) om Azure Site Recovery med Azure Resource Manager PowerShell-cmdlets.
