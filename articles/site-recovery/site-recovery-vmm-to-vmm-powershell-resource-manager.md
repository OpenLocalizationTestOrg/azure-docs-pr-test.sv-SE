---
title: "aaaReplicate Hyper-V virtuella datorer i VMM tooa sekundär plats med PowerShell (Azure Resource Manager) | Microsoft Docs"
description: "Beskriver hur toodeploy Azure Site Recovery tooorchestrate replikering, redundans och återställning av virtuella Hyper-V-datorer i VMM-moln tooa sekundär VMM-plats med hjälp av PowerShell (Resource Manager)"
services: site-recovery
documentationcenter: 
author: sujaytalasila
manager: rochakm
editor: raynew
ms.assetid: 9d38e9c3-217c-4e44-830c-575e9a4141f2
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: sutalasi
ms.openlocfilehash: a769dcc68d66c18b9dc47539071f4d0e0f1db70f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site-using-powershell-resource-manager"></a>Replikera virtuella Hyper-V-datorer i VMM-moln tooa sekundär VMM-plats med hjälp av PowerShell (Resource Manager)
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-vmm-to-vmm.md)
> * [Klassisk portal](site-recovery-vmm-to-vmm-classic.md)
> * [PowerShell – Resource Manager](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

Välkommen tooAzure Site Recovery! Använd den här artikeln om du vill tooreplicate lokala Hyper-V virtuella datorer som hanteras i System Center Virtual Machine Manager (VMM) moln tooa sekundär plats.

Den här artikeln visar hur toouse PowerShell tooautomate vanliga uppgifter du behöver tooperform när du ställer in Azure Site Recovery tooreplicate Hyper-V-datorer i System Center VMM-moln tooSystem Center VMM-moln på sekundär plats.

hello artikeln handlar om förutsättningar för hello scenariot och visar

* Hur tooset upp ett Recovery Services-valv
* Installera hello Azure Site Recovery-providern på VMM-källservern hello och hello VMM-målservern
* Registrera hello VMM-servrarna i hello-valv
* Konfigurera replikeringsprincip för hello VMM-moln. hello replikeringsinställningarna i hello princip kommer att tillämpas tooall skyddade virtuella datorer
* Aktivera skydd för hello virtuella datorer.
* Testa hello redundans för virtuella datorer separat eller som en del av en återställning planera toomake att allt fungerar som förväntat.
* Utför en planerad eller oplanerad växling för virtuella datorer separat eller som en del av en återställning plan toomake att allt fungerar som förväntat.

Om du stöter på problem med att installera det här scenariot, publicera dina frågor på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

> [!NOTE]
> Azure har två olika [distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md) för att skapa och arbeta med resurser: Azure Resource Manager och den klassiska distributionsmodellen. Azure har också två portaler – hello klassiska Azure-portalen som stöder hello klassiska distributionsmodellen och hello Azure-portalen med stöd för båda distributionsmodellerna. Den här artikeln beskriver hello Resource Manager-modellen.
>
>

## <a name="on-premises-prerequisites"></a>Krav för det lokala systemet
Här är vad du behöver i hello primära och sekundära lokala platser toodeploy det här scenariot:

| **Förutsättningar** | **Detaljer** |
| --- | --- |
| **VMM** |Vi rekommenderar att du distribuerar en VMM-servern i hello primär plats och en VMM-servern i hello sekundär plats.<br/><br/> Du kan också [replikera mellan moln på en VMM-server](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment). toodo detta behöver du minst två moln som konfigurerats på hello VMM-servern.<br/><br/> VMM-servrar måste köra minst System Center 2012 SP1 med hello senaste uppdateringarna.<br/><br/> Varje VMM-servern måste ha en eller flera moln har konfigurerats och alla moln måste ha hello Hyper-V kapacitetsprofilen anges. <br/><br/>Molnen måste innehålla en eller flera VMM-värdgrupper.<br/><br/>Lär dig mer om hur du konfigurerar VMM-moln i [konfigurera hello VMM molnet fabric](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), och [genomgång: skapa privata moln med System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).<br/><br/> VMM-servrar ska ha tillgång till internet. |
| **Hyper-V** |Hyper-V-servrar måste köra minst Windows Server 2012 med hello Hyper-V-rollen och har hello senaste uppdateringarna installerade.<br/><br/> En Hyper-V-server måste innehålla en eller flera virtuella datorer.<br/><br/>  Hyper-V-värdservrar ska finnas i värdgrupper i hello primära och sekundära VMM-moln.<br/><br/> Om du kör Hyper-V i ett kluster i Windows Server 2012 R2 bör du installera [uppdatera 2961977](https://support.microsoft.com/kb/2961977)<br/><br/> Om du kör Hyper-V i ett kluster på Windows Server 2012 Observera att klusterutjämning skapas inte automatiskt om du har en statisk IP-adress-kluster. Du behöver tooconfigure hello klusterutjämning manuellt. [Läs mer](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx). |
| **Leverantör** |Under distributionen av Site Recovery installerar du hello Azure Site Recovery-providern på VMM-servrar. hello providern kommunicerar med Site Recovery via HTTPS 443 tooorchestrate replikering. Replikering sker mellan hello primära och sekundära Hyper-V-servrar över hello LAN eller VPN-anslutning.<br/><br/> hello providern som körs på hello VMM-servern måste komma åt toothese URL: er: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.windows.net; *. backup.windowsazure.com; *. blob.core.windows.net; *. store.core.windows.net.<br/><br/> Dessutom tillåter brandväggen kommunikation från hello VMM-servrar toohello [IP-adressintervall för Azure-datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) och tillåta hello HTTPS (443)-protokollet. |

### <a name="network-mapping-prerequisites"></a>Krav för nätverksmappning
Mappar nätverksmappningen mellan VMM VM-nätverk på hello primära och sekundära VMM-servrar till:

* Placera optimalt Replikdatorerna på sekundära Hyper-V-värdar efter växling vid fel.
* Ansluta replikering VMs tooappropriate Virtuella datornätverk.
* Om du inte konfigurerar nätverket mappning replik virtuella datorer inte anslutna tooany nätverket efter redundans.
* Om du vill tooset nätverk är mappning under Site Recovery distribution här vad du behöver:

  * Se till att virtuella datorer på hello källan Hyper-V-värdservern är anslutna tooa VMM VM-nätverk. Nätverket bör vara länkade tooa logiska nätverk som är kopplad till hello moln.
  * Kontrollera att hello sekundära moln som du vill använda för återställning har ett motsvarande Virtuellt datornätverk. Det Virtuella datornätverket ska vara länkade tooa logiskt nätverk som är kopplad till hello sekundära molnet.

Lär dig mer om hur du konfigurerar nätverk i VMM i hello nedan artiklar

* [Hur tooconfigure logiska nätverk i VMM](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [Hur tooconfigure Virtuella nätverk och gateways i VMM](http://go.microsoft.com/fwlink/p/?LinkId=386308)

[Lär dig mer](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) om hur nätverksmappning fungerar.

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
1. Om du har ett valv som redan har skapats, kör du hello nedan kommandot tooget hello valvet.

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. Ange hello valvet kontexten genom att köra hello nedan kommando.

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

## <a name="step-5-create-and-associate-a-replication-policy"></a>Steg 5: Skapa och koppla en replikeringsprincip
1. Skapa en replikeringsprincip för Hyper-V 2012 R2 genom att köra följande kommando hello:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify hello number of hours tooretain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify hello frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify hello port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod

    > [!NOTE]
    > hello VMM-moln kan innehålla Hyper-V-värdar som kör olika versioner av Windows Server (som anges i hello Hyper-V-krav), men hello replikeringsprincip är OS-version som är specifika. Om du har olika värdar som körs på olika operativsystemversioner skapa separata replikeringsprinciper för varje typ av OS-version. För t.ex: Om du har fem värdar som kör Windows 2012 för servrar och tre på Windows Server 2012 R2 kan du skapa två principer för replikering – en för varje typ av versioner av operativsystemet.

1. Hämta hello primära skyddsbehållaren (primära VMM-moln) och återställning skyddsbehållaren (återställning VMM-moln) genom att köra följande kommandon hello:

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. Hämta hello-principen som du skapade i steg 1 med hjälp av hello eget namn för hello-princip

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. Starta hello associering av hello skyddsbehållaren (VMM-moln) med hello replikeringsprincip:

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. Vänta tills hello princip association jobbet toocomplete. Du kan kontrollera om hello jobbet har slutförts med hello följande PowerShell-fragment.

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

   När hello jobbet har bearbetat köra hello följande kommando:

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

toocheck hello slutföra hello åtgärden gör hello i [Övervakaraktiviteten](#monitor).

## <a name="step-6-configure-network-mapping"></a>Steg 6: Konfigurera nätverksmappning
1. hello första kommandot hämtar servrar för hello aktuella Azure Site Recovery-valvet. hello kommandot lagrar hello Microsoft Azure Site Recovery-servrar i hello $Servers matrisvariabel.

        $Servers = Get-AzureRmSiteRecoveryServer
2. hello nedan kommandon hämta hello site recovery nätverk för VMM-källservern hello och hello VMM-målservern.

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > hello VMM-källservern kan vara hello första eller hello andra en i hello servrar matris. Kontrollera hello namnen på hello VMM-servrar och hämta hello nätverk på rätt sätt


1. hello slutliga cmdlet skapar en mappning mellan hello primära nätverk och hello recovery nätverk. hello cmdlet anger hello primära nätverket som hello första elementet i $PrimaryNetworks och hello recovery nätverk som hello första elementet i $RecoveryNetworks.

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-7-configure-storage-mapping"></a>Steg 7: Konfigurera lagring-mappning
1. hello nedan kommando hämtar hello lista med lagringsklassificeringar i $storageclassifications variabel.

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification
2. hello nedan kommandon hämta hello källa klassificeringen i $SourceClassificaion variabeln och mål klassificering i $TargetClassification variabel.

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    > [!NOTE]
    > hello klassificeringar för källa och mål kan vara ett element i matrisen hello. Läs toohello utdata från hello nedan kommandot toofigure hello index för käll- och klassificeringar på $storageclassifications matris.

    > Get-AzureRmSiteRecoveryStorageClassification | Select-Object - egenskapen FriendlyName, Id | Formatera tabell


1. hello nedan cmdlet skapar en mappning mellan hello källa klassificering och hello mål klassificering.

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-8-enable-protection-for-virtual-machines"></a>Steg 8: Aktivera skydd för virtuella datorer
Du kan aktivera skydd för virtuella datorer i molnet hello när hello servrar, moln och nätverk har konfigurerats korrekt.

1. tooenable skydd, kör följande kommando tooget hello skyddsbehållaren hello:

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. Hämta hello skyddad entitet (VM) genom att köra följande kommando hello:

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. Aktivera replikering för hello VM genom att köra följande kommando hello:

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="test-your-deployment"></a>Testa distributionen
tootest planera distributionen av du kan köra ett redundanstest för en enskild virtuell dator eller skapa en återställningsplan som består av flera virtuella datorer och köra ett redundanstest för hello. Ett redundanstest simulerar redundans- och återställningsmekanismen i ett isolerat nätverk.

> [!NOTE]
> Du kan skapa en återställningsplan för ditt program i Azure-portalen.
>
>

toocheck hello slutföra hello åtgärden gör hello i [Övervakaraktiviteten](#monitor).

### <a name="run-a-test-failover"></a>Köra ett redundanstest
1. Kör hello nedan cmdlets tooget hello Virtuella nätverk toowhich du tootest redundans dina virtuella datorer till.

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. Utför ett redundanstest av en virtuell dator genom att göra hello följande:

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
3. Utför ett redundanstest av en återställningsplan hello följande:

       $recoveryplanname = "test-recovery-plan"

       $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

### <a name="run-a-planned-failover"></a>Kör en planerad redundans
1. Utför en planerad redundansväxling av en virtuell dator genom att göra hello följande:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
2. Utför en planerad redundansväxling av en återställningsplan hello följande:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a>Kör en oplanerad redundans
1. Utföra en oplanerad redundans för en virtuell dator genom att göra hello följande:

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

2. utföra en oplanerad redundans för en återställningsplan hello följande:

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

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
