---
title: aaaReplicate Hyper-V virtuella datorer med PowerShell och Azure Resource Manager | Microsoft Docs
description: Automatisera hello replikering av Hyper-V VMs tooAzure med Azure Site Recovery med PowerShell och Azure Resource Manager.
services: site-recovery
documentationcenter: 
author: bsiva
manager: abhiag
editor: 
ms.assetid: 05e0d76e-c3f5-4845-8052-094019b6d102
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 05/31/2017
ms.author: bsiva
ms.openlocfilehash: 4fb15ce2e9ad54f1dd6f54ff769eb912aa4b0272
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-between-on-premises-hyper-v-virtual-machines-and-azure-by-using-powershell-and-azure-resource-manager"></a>Replikera mellan lokala Hyper-V virtuella datorer och Azure med hjälp av PowerShell och Azure Resource Manager
> [!div class="op_single_selector"]
> * [Azure Portal](site-recovery-hyper-v-site-to-azure.md)
> * [PowerShell – Resource Manager](site-recovery-deploy-with-powershell-resource-manager.md)
> * [Klassisk portal](site-recovery-hyper-v-site-to-azure-classic.md)
>
>

## <a name="overview"></a>Översikt
Azure Site Recovery bidrar tooyour disaster recovery strategi för affärskontinuitet och genom att samordna replikering, redundans och återställning av virtuella datorer i ett antal distributionsscenarier. En fullständig lista över scenarier för distribution finns hello [översikt över Azure Site Recovery](site-recovery-overview.md).

Azure PowerShell är en modul som tillhandahåller cmdlets toomanage Azure via Windows PowerShell. De fungerar med två typer av moduler: hello Azure profil modul eller hello Azure Resource Manager-modul.

Site Recovery PowerShell-cmdletar tillgängliga med Azure PowerShell för Azure Resource Manager hjälpa dig att skydda och återställa dina servrar i Azure.

Den här artikeln beskriver hur toouse Windows PowerShell, tillsammans med Azure Resource Manager, toodeploy Site Recovery tooconfigure och samordna server protection tooAzure. hello-exempel som används i den här artikeln visar hur tooprotect, växla över och återställa virtuella datorer på en värd tooAzure Hyper-V med hjälp av Azure PowerShell med Azure Resource Manager.

> [!NOTE]
> hello Site Recovery PowerShell-cmdlets för närvarande kan du tooconfigure hello följande: en tooanother för Virtual Machine Manager-plats, tooAzure en Virtual Machine Manager-plats och tooAzure en Hyper-V-platsen.
>
>

Du behöver inte toobe en expert toouse PowerShell den här artikeln, men du behöver toounderstand hello grundläggande begrepp som moduler, cmdletar och sessioner. Mer information om Windows PowerShell finns i [Komma igång med Windows PowerShell](http://technet.microsoft.com/library/hh857337.aspx).

Du kan också läsa mer om [med hjälp av Azure PowerShell med Azure Resource Manager](../powershell-azure-resource-manager.md).

> [!NOTE]
> Microsoft-partners som är en del av hello Cloud Solution Providers (CSP) program kan konfigurera och hantera skydd av sina kunders servrar tootheir kunder respektive CSP prenumerationer (klient subscriptions).
>
>

## <a name="before-you-start"></a>Innan du börjar
Kontrollera att du har dessa krav är uppfyllda:

* En [Microsoft Azure](https://azure.microsoft.com/) konto. Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/). Dessutom kan du läsa om [priser för Azure Site Recovery Manager](https://azure.microsoft.com/pricing/details/site-recovery/).
* Azure PowerShell 1.0. Information om den här versionen och hur tooinstall, se [Azure PowerShell 1.0.](https://azure.microsoft.com/)
* Hej [AzureRM.SiteRecovery](https://www.powershellgallery.com/packages/AzureRM.SiteRecovery/) och [AzureRM.RecoveryServices](https://www.powershellgallery.com/packages/AzureRM.RecoveryServices/) moduler. Du kan hämta hello senaste versionerna av dessa moduler från hello [PowerShell-galleriet](https://www.powershellgallery.com/)

Den här artikeln beskrivs hur toouse Azure Powershell med Azure Resource Manager tooconfigure och hantera skydd för dina servrar. hello-exempel som används i den här artikeln beskrivs hur du tooprotect en virtuell dator som körs på en Hyper-V-värd, tooAzure. hello följande förutsättningar är specifika toothis exempel. En mer omfattande uppsättning krav för hello finns olika Site Recovery-scenarier toohello dokumentation som rör toothat scenario.

* En Hyper-V-värd som kör Windows Server 2012 R2 eller Microsoft Hyper-V Server 2012 R2 som innehåller en eller flera virtuella datorer.
* Hyper-V-servrar ansluten toohello Internet, antingen direkt eller via en proxyserver.
* hello virtuella datorer som du vill tooprotect måste uppfylla [förutsättningar för virtuell dator](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).

## <a name="step-1-sign-in-tooyour-azure-account"></a>Steg 1: Logga in tooyour Azure-konto
1. Öppna ett PowerShell-konsolen och kör det här kommandot toosign tooyour Azure-konto. hello cmdlet öppnar en webbsida som uppmanas du att autentiseringsuppgifterna för ditt konto.

        Login-AzureRmAccount

    Du kan alternativt kan också inkludera autentiseringsuppgifterna för ditt konto som en parameter toohello `Login-AzureRmAccount` cmdlet, med hjälp av hello `-Credential` parameter.

    Om du är CSP-partner som arbetar för en klient måste du ange hello kund som en klient med hjälp av deras primära domännamn tenantID eller klienten.

        Login-AzureRmAccount -Tenant "fabrikam.com"
2. Ett konto kan ha flera prenumerationer, så du bör koppla hello-prenumeration som du vill toouse med hello-konto.

        Select-AzureRmSubscription -SubscriptionName $SubscriptionName
3. Kontrollera att prenumerationen är registrerad toouse hello Azure providers för återställningstjänster och Site Recovery med hjälp av hello följande kommandon:

   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices`
   * `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`

   I hello utdata från de här kommandona om hello **RegistrationState** har angetts för**registrerade**, kan du fortsätta tooStep 2. Om så inte är fallet, bör du registrera hello saknas provider i din prenumeration.

   tooregister hello Azure provider för Site Recovery och Recovery Services genom att köra följande kommandon hello:

       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.SiteRecovery
       Register-AzureRmResourceProvider -ProviderNamespace Microsoft.RecoveryServices

   Kontrollera att hello providers har registrerats med hjälp av följande kommandon hello: `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.RecoveryServices` och `Get-AzureRmResourceProvider -ProviderNamespace  Microsoft.SiteRecovery`.

## <a name="step-2-set-up-hello-recovery-services-vault"></a>Steg 2: Konfigurera hello Recovery Services-valvet
1. Skapa en resursgrupp i Azure Resource Manager där du ska skapa hello valvet, eller Använd en befintlig resursgrupp. Du kan skapa en ny resursgrupp med hjälp av hello följande kommando:

        New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Geo  

    hello $ResourceGroupName variabeln innehåller hello namnet på resursgruppen hello önskade toocreate och hello $Geo variabeln innehåller hello Azure region i vilken toocreate hello resursgrupp (till exempel ”södra”).

    Du kan hämta en lista över resursgrupper i din prenumeration med hjälp av hello `Get-AzureRmResourceGroup` cmdlet.
2. Skapa ett nytt Azure Recovery Services-valv enligt följande:

        $vault = New-AzureRmRecoveryServicesVault -Name <string> -ResourceGroupName <string> -Location <string>

    Du kan hämta en lista över befintliga valv med hello `Get-AzureRmRecoveryServicesVault` cmdlet.

> [!NOTE]
> Om du vill tooperform åtgärder på Site Recovery-valv som skapats med hjälp av hello klassiska portalen eller hello Azure Service Management PowerShell-modulen kan du hämta en lista över sådana valv med hello `Get-AzureRmSiteRecoveryVault` cmdlet. Du bör skapa ett nytt Recovery Services-valv för alla nya åtgärder. hello Site Recovery-valv du har skapat tidigare stöds, men har inte hello senaste funktionerna.
>
>

## <a name="step-3-set-hello-recovery-services-vault-context"></a>Steg 3: Ange hello Recovery Services-valvet kontext
1. Ange hello valvet kontexten genom att köra följande kommando hello:

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-create-a-hyper-v-site-and-generate-a-new-vault-registration-key-for-hello-site"></a>Steg 4: Skapa ett Hyper-V-platsen och generera en ny registreringsnyckel i valvet för hello plats.
1. Skapa en ny Hyper-V-plats på följande sätt:

        $sitename = "MySite"                #Specify site friendly name
        New-AzureRmSiteRecoverySite -Name $sitename

    Denna cmdlet startar en Platsåterställning jobbet toocreate hello plats och returnerar ett jobbobjekt för Site Recovery. Vänta tills hello jobbet toocomplete och kontrollera hello jobbet har slutförts.

    Du kan hämta hello jobbobjektet och därmed kontrollera hello hello jobbets aktuella status genom att använda hello Get-AzureRmSiteRecoveryJob cmdlet.
2. Generera och ladda ned en registreringsnyckel för hello plats på följande sätt:

        $SiteIdentifier = Get-AzureRmSiteRecoverySite -Name $sitename | Select -ExpandProperty SiteIdentifier
        Get-AzureRmRecoveryServicesVaultSettingsFile -Vault $vault -SiteIdentifier $SiteIdentifier -SiteFriendlyName $sitename -Path $Path

    Kopiera hello ned viktiga toohello Hyper-V-värden. Du behöver hello viktiga tooregister hello Hyper-V-värd toohello plats.

## <a name="step-5-install-hello-azure-site-recovery-provider-and-azure-recovery-services-agent-on-your-hyper-v-host"></a>Steg 5: Installera hello Azure Site Recovery-providern och Azure Recovery Services-agenten på Hyper-V-värd
1. Hämta hello installationsprogrammet för hello senaste versionen av hello providern från [Microsoft](https://aka.ms/downloaddra).
2. Kör hello installer på Hyper-V-värd och hello slutet av hello installationen fortsätta toohello registreringssteget.
3. När du uppmanas ange hello hämtat plats registreringsnyckel och slutföra registreringen av hello Hyper-V-värd toohello plats.
4. Kontrollera att hello Hyper-V-värd är registrerade toohello plats med hjälp av hello följande kommando:

        $server =  Get-AzureRmSiteRecoveryServer -FriendlyName $server-friendlyname

## <a name="step-6-create-a-replication-policy-and-associate-it-with-hello-protection-container"></a>Steg 6: Skapa en replikeringsprincip och koppla den till hello skyddsbehållaren
1. Skapa en replikeringsprincip enligt följande:

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify hello number of recovery points
        $storageaccountID = Get-AzureRmStorageAccount -Name "mystorea" -ResourceGroupName "MyRG" | Select -ExpandProperty Id

        $PolicyResult = New-AzureRmSiteRecoveryPolicy -Name $PolicyName -ReplicationProvider “HyperVReplicaAzure” -ReplicationFrequencyInSeconds $ReplicationFrequencyInSeconds  -RecoveryPoints $Recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId $storageaccountID

    Kontrollera hello returnerade jobbet tooensure som hello replikering princip skapandet lyckas.

   > [!IMPORTANT]
   > hello storage-konto som angetts ska vara i hello samma Azure-region som Recovery Services-valvet och ska ha geo-replikering aktiverat.
   >
   > * Om hello anges Recovery storage-konto är av typen Azure Storage (klassisk) skyddade redundans för hello datorer Återställ hello datorn tooAzure IaaS (klassisk).
   > * Om hello anges Recovery storage-konto är av typen Azure Storage (Azure Resource Manager) skyddade redundans för hello datorer Återställ hello datorn tooAzure IaaS (Azure Resource Manager).
   >
   >
2. Hämta hello skydd behållaren motsvarande toohello plats, på följande sätt:

        $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer
3. Starta hello associering av hello skyddsbehållaren med hello replikeringsprincip på följande sätt:

     $Policy = get-AzureRmSiteRecoveryPolicy - FriendlyName $PolicyName $associationJob = Start AzureRmSiteRecoveryPolicyAssociationJob-princip $Policy - PrimaryProtectionContainer $protectionContainer

   Vänta tills hello association jobbet toocomplete och se till att den har slutförts.

## <a name="step-7-enable-protection-for-virtual-machines"></a>Steg 7: Aktivera skydd för virtuella datorer
1. Hämta hello skydd entitet motsvarande toohello VM som du vill tooprotect, enligt följande:

        $VMFriendlyName = "Fabrikam-app"                    #Name of hello VM
        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName
2. Börja skydda hello virtuell dator på följande sätt:

        $Ostype = "Windows"                                 # "Windows" or "Linux"
        $DRjob = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionEntity -Policy $Policy -Protection Enable -RecoveryAzureStorageAccountId $storageaccountID  -OS $OStype -OSDiskName $protectionEntity.Disks[0].Name

   > [!IMPORTANT]
   > hello storage-konto som angetts ska vara i hello samma Azure-region som Recovery Services-valvet och ska ha geo-replikering aktiverat.
   >
   > * Om hello anges Recovery storage-konto är av typen Azure Storage (klassisk) skyddade redundans för hello datorer Återställ hello datorn tooAzure IaaS (klassisk).
   > * Om hello anges Recovery storage-konto är av typen Azure Storage (Azure Resource Manager) skyddade redundans för hello datorer Återställ hello datorn tooAzure IaaS (Azure Resource Manager).
   >
   > Om hello VM som du skyddar har fler än en disk ansluten tooit, ange hello operativsystemdisk med hello *OSDiskName* parameter.
   >
   >
3. Vänta tills hello virtuella datorer tooreach ett skyddat läge efter hello inledande replikering. Detta kan ta en stund, beroende på faktorer som hello mängden data toobe replikeras och hello överordnade bandbredd tooAzure. hello jobbstatus och StateDescription uppdateras enligt följande vid hello VM når ett skyddat läge.

        PS C:\> $DRjob = Get-AzureRmSiteRecoveryJob -Job $DRjob

        PS C:\> $DRjob | Select-Object -ExpandProperty State
        Succeeded

        PS C:\> $DRjob | Select-Object -ExpandProperty StateDescription
        Completed
4. Uppdatera egenskaperna för återställning, till exempel hello Virtuella rollstorlek och hello Azure-nätverk tooattach hello virtuella datorns nätverk gränssnittet kort tooupon redundans.

        PS C:\> $nw1 = Get-AzureRmVirtualNetwork -Name "FailoverNw" -ResourceGroupName "MyRG"

        PS C:\> $VMFriendlyName = "Fabrikam-App"

        PS C:\> $VM = Get-AzureRmSiteRecoveryVM -ProtectionContainer $protectionContainer -FriendlyName $VMFriendlyName

        PS C:\> $UpdateJob = Set-AzureRmSiteRecoveryVM -VirtualMachine $VM -PrimaryNic $VM.NicDetailsList[0].NicId -RecoveryNetworkId $nw1.Id -RecoveryNicSubnetName $nw1.Subnets[0].Name

        PS C:\> $UpdateJob = Get-AzureRmSiteRecoveryJob -Job $UpdateJob

        PS C:\> $UpdateJob

        Name             : b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        ID               : /Subscriptions/a731825f-4bf2-4f81-a611-c331b272206e/resourceGroups/MyRG/providers/Microsoft.RecoveryServices/vault
                           s/MyVault/replicationJobs/b8a647e0-2cb9-40d1-84c4-d0169919e2c5
        Type             : Microsoft.RecoveryServices/vaults/replicationJobs
        JobType          : UpdateVmProperties
        DisplayName      : Update hello virtual machine
        ClientRequestId  : 805a22a3-be86-441c-9da8-f32685673112-2015-12-10 17:55:51Z-P
        State            : Succeeded
        StateDescription : Completed
        StartTime        : 10-12-2015 17:55:53 +00:00
        EndTime          : 10-12-2015 17:55:54 +00:00
        TargetObjectId   : 289682c6-c5e6-42dc-a1d2-5f9621f78ae6
        TargetObjectType : ProtectionEntity
        TargetObjectName : Fabrikam-App
        AllowedActions   : {Restart}
        Tasks            : {UpdateVmPropertiesTask}
        Errors           : {}



## <a name="step-8-run-a-test-failover"></a>Steg 8: Köra ett redundanstest
1. Kör ett testjobb växling vid fel, enligt följande:

        $nw = Get-AzureRmVirtualNetwork -Name "TestFailoverNw" -ResourceGroupName "MyRG" #Specify Azure vnet name and resource group

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMFriendlyName -ProtectionContainer $protectionContainer

        $TFjob = Start-AzureRmSiteRecoveryTestFailoverJob -ProtectionEntity $protectionEntity -Direction PrimaryToRecovery -AzureVMNetworkId $nw.Id
2. Kontrollera att hello testa virtuell dator skapas i Azure. (hello testa redundans jobb pausas, när du har skapat hello test VM i Azure. hello jobbet har slutförts genom att rensa hello som skapats av då du återupptar hello jobb, enligt beskrivningen i hello nästa steg.)
3. Fullständig hello testa redundans, enligt följande:

        $TFjob = Resume-AzureRmSiteRecoveryJob -Job $TFjob

## <a name="next-steps"></a>Nästa steg
[Läs mer](https://msdn.microsoft.com/library/azure/mt637930.aspx) om Azure Site Recovery med Azure Resource Manager PowerShell-cmdlets.
