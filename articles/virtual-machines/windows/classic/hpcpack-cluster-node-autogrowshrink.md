---
title: aaaAutoscale HPC Pack klusternoder | Microsoft Docs
description: "Automatiskt växa eller krympa hello antalet noder i HPC Pack beräkning i Azure"
services: virtual-machines-windows
documentationcenter: 
author: dlepow
manager: 
editor: tysonn
ms.assetid: 38762cd1-f917-464c-ae5d-b02b1eb21e3f
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: big-compute
ms.date: 12/08/2016
ms.author: danlep
ms.openlocfilehash: 0bdf55625d337a2bbfe05677682d645a584798d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automatically-grow-and-shrink-hello-hpc-pack-cluster-resources-in-azure-according-toohello-cluster-workload"></a>Automatiskt växa eller krympa hello HPC Pack klusterresurser i Azure enligt toohello klusterarbetsbelastning
Om du distribuerar Azure ”burst” noder i klustret HPC Pack, eller skapar du ett HPC Pack-kluster i Azure Virtual Machines, du kan öka eller minska hello klusterresurser, till exempel noder eller kärnor enligt hello arbetsbelastningen på hello klustret automatiskt. Skalning hello klusterresurser på så sätt kan du toouse Azure-resurser mer effektivt och kontrollera deras kostnader.

Den här artikeln visar HPC Pack innehåller beräkningsresurser tooautoscale på två sätt:

* hello HPC Pack klustret egenskapen **AutoGrowShrink**

* Hej **AzureAutoGrowShrink.ps1** HPC PowerShell-skript

[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-both-include.md)]

För närvarande kan endast automatiskt växa och krympa HPC Pack compute-noder som kör ett Windows Server-operativsystem.


## <a name="set-hello-autogrowshrink-cluster-property"></a>Egenskapen hello AutoGrowShrink kluster
### <a name="prerequisites"></a>Krav

* **HPC Pack 2012 R2 uppdatering 2 eller ett senare kluster** -hello klustrets huvudnod kan distribueras antingen lokalt eller i en Azure VM. Se [ställer in en hybrid-kluster med HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget igång med en lokal huvudnod och Azure ”burst” noder. Se hello [HPC Pack IaaS distributionsskriptet](hpcpack-cluster-powershell-script.md) tooquickly distribuera ett kluster som HPC Pack i virtuella Azure-datorer.

* **För ett kluster med en huvudnod i Azure (Resource Manager-modellen)** – med början i HPC Pack 2016 certifikatautentisering i ett Azure Active Directory-program används för att automatiskt växer och krympande klustrets virtuella datorer distribueras via Azure Resource Manager. Konfigurera ett certifikat på följande sätt:

  1. Anslut med fjärrskrivbord tooone huvudnod efter klusterdistribution.

  2. Ladda upp certifikatet (PFX-format med privat nyckel) hello tooeach huvudnod och installera tooCert:\LocalMachine\My och Cert: \LocalMachine\Root.

  3. Starta Azure PowerShell som administratör och kör följande kommandon i en huvudnod hello:

    ```powershell
        cd $env:CCP_HOME\bin

        Login-AzureRmAccount
    ```
        
    Om ditt konto finns i fler än en Azure Active Directory-klient eller Azure-prenumeration, kan du köra följande hello kommandot tooselect hello rätt klient och prenumeration:
  
    ```powershell
        Login-AzureRMAccount -TenantId <TenantId> -SubscriptionId <subscriptionId>
    ```     
       
    Kör följande kommando tooview hello markerade hello klient och prenumeration:
    
    ```powershell
        Get-AzureRMContext
    ```

  4. Kör följande skript hello

    ```powershell
        .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -CertificateThumbprint "XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX" -TenantId xxxxxxxx-xxxxx-xxxxx-xxxxx-xxxxxxxxxxxx
    ```

    där

    **DisplayName** -Azure Active programmets visningsnamn. Om programmet hello inte finns, skapas den i Azure Active Directory.

    **Webbsida** -hello programmet hello startsida. Du kan konfigurera en dummy URL, som i föregående exempel hello.

    **IdentifierUri** -identifierare för programmet hello. Du kan konfigurera en dummy URL, som i föregående exempel hello.

    **CertificateThumbprint** -tumavtryck hello certifikat du installerade på hello huvudnod i steg 1.

    **TenantId** -klient-ID för din Azure Active Directory. Du kan hämta hello klient-ID från hello Azure Active Directory-portalen **egenskaper** sidan.

    Mer information om **ConfigARMAutoGrowShrinkCert.ps1**kör `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`.


* **För ett kluster med en huvudnod i Azure (klassiska distributionsmodellen)** - om du använder hello HPC Pack IaaS distribution skriptet toocreate hello kluster i hello klassiska distributionsmodellen, aktivera hello **AutoGrowShrink** kluster Egenskapen genom att ange hello AutoGrowShrink alternativet i konfigurationsfilen för hello-klustret. Mer information finns i hello-dokumentationen som medföljer hello [hämtning av](https://www.microsoft.com/download/details.aspx?id=44949).

    Du kan också aktivera hello **AutoGrowShrink** klustret egenskapen när du har distribuerat hello kluster med hjälp av HPC PowerShell-kommandon beskrivs i följande avsnitt hello. tooprepare för den här första fullständig hello följande steg:

  1. Konfigurera ett Azure-hanteringscertifikat i hello huvudnod och i hello Azure-prenumeration. En test-distribution kan du använda hello standard Microsoft HPC Azure självsignerade certifikat som HPC Pack installeras på hello huvudnod och ladda upp det certifikatet tooyour Azure-prenumeration. Alternativ och steg finns hello [TechNet-biblioteket vägledning](https://technet.microsoft.com/library/gg481759.aspx).

  2. Kör **regedit** på hello huvudnod, gå tooHKLM\SOFTWARE\Micorsoft\HPC\IaasInfo och Lägg till ett strängvärde. Ange namnet på hello värdet för ”tumavtryck” och värdet data toohello hello certifikatets tumavtryck i steg 1.

### <a name="hpc-powershell-commands-tooset-hello-autogrowshrink-property"></a>HPC PowerShell-kommandon tooset hello AutoGrowShrink-egenskap
Följande är exempel HPC PowerShell-kommandon tooset **AutoGrowShrink** och tootune sitt beteende med ytterligare parametrar. Se [AutoGrowShrink parametrar](#AutoGrowShrink-parameters) senare i den här artikeln hello fullständig lista över inställningar.

toorun dessa kommandon starta HPC PowerShell på hello klustrets huvudnod som administratör.

**tooenable hello AutoGrowShrink egenskapen**

```powershell
Set-HpcClusterProperty –EnableGrowShrink 1
```

**toodisable hello AutoGrowShrink egenskapen**

```powershell
Set-HpcClusterProperty –EnableGrowShrink 0
```

**toochange hello växer intervall i minuter**

```powershell
Set-HpcClusterProperty –GrowInterval <interval>
```

**toochange hello krympa intervall i minuter**

```powershell
Set-HpcClusterProperty –ShrinkInterval <interval>
```

**tooview hello aktuella konfigureringen av AutoGrowShrink**

```powershell
Get-HpcClusterProperty –AutoGrowShrink
```

**tooexclude noden grupper från AutoGrowShrink**

```powershell
Set-HpcClusterProperty –ExcludeNodeGroups <group1,group2,group3>
```

>[!NOTE] 
> Den här parametern stöds från och med HPC Pack 2016
>

### <a name="autogrowshrink-parameters"></a>AutoGrowShrink parametrar
hello följande är AutoGrowShrink parametrar som du kan ändra med hjälp av hello **Set HpcClusterProperty** kommando.

* **EnableGrowShrink** – växla tooenable eller inaktivera hello **AutoGrowShrink** egenskapen.
* **ParamSweepTasksPerCore** -antal parametrisk rensning uppgifter toogrow en kärna. hello standardvärdet är en kärna med toogrow per aktivitet.

  > [!NOTE]
  > HPC Pack QFE KB3134307 ändringar **ParamSweepTasksPerCore** för**TasksPerResourceUnit**. Den baseras på resurstypen för hello jobb och kan vara nod, socket eller kärnor.
  >
  >
* **GrowThreshold** -tröskelvärdet köade aktiviteter tootrigger automatisk ökning. hello standardvärdet är 1, vilket innebär att om det finns 1 eller flera aktiviteter i hello köade tillstånd, utöka noder automatiskt.
* **GrowInterval** -intervallet i minuter tootrigger automatisk ökning. hello Standardintervallet är 5 minuter.
* **ShrinkInterval** -intervallet i minuter tootrigger automatisk krymper. hello Standardintervallet är 5 minuter. |
* **ShrinkIdleTimes** -antalet kontinuerlig kontrollerar tooshrink tooindicate hello noder är inaktiva. hello standardinställningen är 3 gånger. Till exempel, om hello **ShrinkInterval** är 5 minuter, HPC Pack kontrollerar om hello nod är inaktiv var femte minut. Om hello noder är i viloläge hello efter 3 kontinuerlig kontrolleras (15 minuter), krymper HPC Pack noden.
* **ExtraNodesGrowRatio** -ytterligare procentandelen noder toogrow för Message Passing Interface (MPI) jobb. hello standardvärdet är 1, vilket innebär att HPC Pack växer noder 1% för MPI-jobb.
* **GrowByMin** -växla tooindicate om hello autogrow princip baseras på hello minsta resurser som krävs för hello jobbet. hello standard är FALSKT, vilket innebär att HPC Pack växer noder för jobb baserat på hello maximala resurser som krävs för hello jobb.
* **SoaJobGrowThreshold** -tröskelvärdet för inkommande SOA-begäranden tootrigger hello automatisk växa processen. hello standardvärdet är 50000.

  > [!NOTE]
  > Den här parametern stöds från och med HPC Pack 2012 R2 uppdatering 3.
  >
  >
* **SoaRequestsPerCore** -antal inkommande SOA-begäranden toogrow en kärna. hello standardvärdet är 20000.

  > [!NOTE]
  > Den här parametern stöds från och med HPC Pack 2012 R2 uppdatering 3.
  >
* **ExcludeNodeGroups** – noder i hello angetts noden grupper inte automatiskt växa eller krympa.
  
  > [!NOTE]
  > Den här parametern stöds från och med HPC Pack 2016.
  >

### <a name="mpi-example"></a>MPI-exempel
Som standard HPC Pack växer 1% extra noder för MPI-jobb (**ExtraNodesGrowRatio** anges too1). hello orsaken är att MPI kan kräva flera noder och hello jobb kan endast köra när alla noder är klar. När Azure startar noder behöva ibland en nod mer tid toostart än andra orsakar andra noder toobe inaktiv under väntan på att noden tooget redo. HPC Pack av växande extra noder minskar väntetiden för den här resursen och kostnaderna potentiellt. tooincrease hello procentandelen extra noder för MPI-jobb (till exempel too10%), köra ett kommando som liknar

    Set-HpcClusterProperty -ExtraNodesGrowRatio 10

### <a name="soa-example"></a>SOA-exempel
Som standard **SoaJobGrowThreshold** anges too50000 och **SoaRequestsPerCore** too200000 anges. Om du skickar ett SOA-jobb med 70000 begäranden, det finns en köad aktivitet och inkommande begäranden 70000. I det här fallet HPC Pack växer 1 kärna för hello i kö aktivitet och växer (70000 50000) efter inkommande begäranden / 20000 = 1 core i totalt växer 2 kärnor för denna SOA-jobb.

## <a name="run-hello-azureautogrowshrinkps1-script"></a>Kör hello AzureAutoGrowShrink.ps1 skript
### <a name="prerequisites"></a>Krav

* **HPC Pack 2012 R2 Update 1 eller ett senare kluster** - hello **AzureAutoGrowShrink.ps1** skript är installerat i hello % CCP_HOME % bin-mappen. hello klustrets huvudnod kan distribueras antingen lokalt eller i en Azure VM. Se [ställer in en hybrid-kluster med HPC Pack](../../../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md) tooget igång med en lokal huvudnod och Azure ”burst” noder. Se hello [HPC Pack IaaS distributionsskriptet](hpcpack-cluster-powershell-script.md) tooquickly distribuera ett kluster som HPC Pack Azure-dator eller Använd en [Azure quickstart mallen](https://azure.microsoft.com/documentation/templates/create-hpc-cluster/).
* **Azure PowerShell 1.4.0** -hello skript för närvarande är beroende av den här specifika versionen av Azure PowerShell.
* **För ett kluster med Azure burst-noder** -kör hello skriptet på en klientdator där HPC Pack är installerad eller på hello huvudnod. Om körs på en klientdator, kontrollerar du att du angett hello variabeln $env: CCP_SCHEDULER toopoint toohello huvudnod. hello Azure ”burst” noder måste läggas till toohello kluster, men de kan vara i hello inte distribuerats tillstånd.
* **För ett kluster som distribueras på virtuella Azure-datorer (Resource Manager-distributionsmodellen)** -för ett kluster med Azure virtuella datorer som distribueras i hello Resource Manager-distributionsmodellen hello skript stöder två metoder för Azure-autentisering: Logga in tooyour Azure-konto toorun hello skript varje gång (genom att köra `Login-AzureRmAccount`, eller konfigurera en service principal tooauthenticate med ett certifikat. HPC Pack innehåller hello skriptet **ConfigARMAutoGrowShrinkCert.ps** toocreate ett huvudnamn för tjänsten med certifikat. hello skriptet skapar ett program för Azure Active Directory (Azure AD) och ett huvudnamn för tjänsten och tilldelar hello deltagare rollen toohello tjänstens huvudnamn. toorun hello skript, starta Azure PowerShell som administratör och kör följande kommandon hello:

    ```powershell
    cd $env:CCP_HOME\bin

    Login-AzureRmAccount

    .\ConfigARMAutoGrowShrinkCert.ps1 -DisplayName “YourHpcPackAppName” -HomePage "https://YourHpcPackAppHomePage" -IdentifierUri "https://YourHpcPackAppUri" -PfxFile "d:\yourcertificate.pfx"
    ```

    Mer information om **ConfigARMAutoGrowShrinkCert.ps1**kör `Get-Help .\ConfigARMAutoGrowShrinkCert.ps1 -Detailed`,

* **För ett kluster som distribueras på virtuella Azure-datorer (klassiska distributionsmodellen)** -körning hello skript på hello huvudnod VM, eftersom den är beroende av hello **Start HpcIaaSNode.ps1** och **Stop-HpcIaaSNode.ps1**skript som är installerade det. Dessa skript Dessutom kräver ett Azure-hanteringscertifikat eller inställningsfilen för publicering (se [hantera compute-noder i ett HPC Pack kluster i Azure](hpcpack-cluster-node-manage.md)). Kontrollera att alla hello compute-nod virtuella datorer som du redan har lagts till toohello klustret. De kan vara i hello stoppat tillstånd.



### <a name="syntax"></a>Syntax
```powershell
AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfActiveQueuedTasksPerNodeToGrow <Single> [-NumOfActiveQueuedTasksToGrowThreshold <Int32>]
    [-NumOfInitialNodesToGrow <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>]
    [-ShrinkCheckIdleTimes <Int32>] [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>]
    [<CommonParameters>]

AzureAutoGrowShrink.ps1 [-NodeTemplates <String[]>] [-JobTemplates <String[]>] [-NodeType <String>]
    -NumOfQueuedJobsPerNodeToGrow <Single> [-NumOfQueuedJobsToGrowThreshold <Int32>] [-NumOfInitialNodesToGrow
    <Int32>] [-GrowCheckIntervalMins <Int32>] [-ShrinkCheckIntervalMins <Int32>] [-ShrinkCheckIdleTimes <Int32>]
    [-ExtraNodesGrowRatio <Int32>] [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]

AzureAutoGrowShrink.ps1 -UseLastConfigurations [-ArgFile <String>] [-LogFilePrefix <String>] [<CommonParameters>]
```
### <a name="parameters"></a>Parametrar
* **NodeTemplates** -namnen på hello noden mallar toodefine hello omfång för hello noder toogrow och minska. Om inget annat anges (hello standardvärdet är @()), alla noder i hello **AzureNodes** nodgrupp är i omfattning när **NodeType** har värdet AzureNodes och alla noder i hello **ComputeNodes** nodgrupp är i omfattning när **NodeType** har värdet ComputeNodes.
* **JobTemplates** -namnen på hello jobbet mallar toodefine hello omfång för hello noder toogrow.
* **NodeType** – hello typ av noden toogrow och minska. Värden som stöds är:

  * **AzureNodes** – för Azure PaaS (burst) noder i ett lokalt eller Azure IaaS-kluster.
  * **ComputeNodes** - för beräkningsnod virtuella datorer i ett Azure IaaS-kluster.

* **NumOfQueuedJobsPerNodeToGrow** -antalet köade jobb krävs toogrow en nod.
* **NumOfQueuedJobsToGrowThreshold** -hello tröskelvärdet för antal köade jobb toostart hello växa processen.
* **NumOfActiveQueuedTasksPerNodeToGrow** -hello antalet aktiva köade aktiviteter krävs toogrow en nod. Om **NumOfQueuedJobsPerNodeToGrow** anges med ett värde som är större än 0, ignoreras den här parametern.
* **NumOfActiveQueuedTasksToGrowThreshold** -hello tröskelvärdet för antal aktiva köade aktiviteter toostart hello växa processen.
* **NumOfInitialNodesToGrow** - hello inledande minsta antal noder toogrow om alla hello-noder i omfånget är **inte distribuerade** eller **Stoppad (Deallocated)**.
* **GrowCheckIntervalMins** -hello-intervall i minuter mellan kontrollerar toogrow.
* **ShrinkCheckIntervalMins** -hello-intervall i minuter mellan kontrollerar tooshrink.
* **ShrinkCheckIdleTimes** -hello antal kontinuerlig förminskas kontroller (avgränsade med **ShrinkCheckIntervalMins**) tooindicate hello noder är inaktiv.
* **UseLastConfigurations** -hello tidigare konfigurationer i hello argumentet-filen.
* **ArgFile**- hello namnet på hello argumentet filen används toosave och uppdatera hello toorun hello-konfigurationsskript.
* **LogFilePrefix** -hello-prefixet i hello loggfilen. Du kan ange en sökväg. Som standard är hello loggen skriftliga toohello aktuella arbetskatalogen.

### <a name="example-1"></a>Exempel 1
hello konfigureras följande exempel hello Azure burst-noder som distribueras med standardmall AzureNode toogrow och minska automatiskt. Om alla noder är från början hello **inte distribuerade** tillstånd, minst 3 noder har startats. Om hello antalet köade jobb överskrider 8, hello skriptet börjar noder tills deras nummer överskrider hello förhållandet mellan jobb i kön till **NumOfQueuedJobsPerNodeToGrow**. Om en nod har hittats toobe inaktiv i 3 på varandra följande ledig tid, stoppas.

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates @('Default AzureNode
 Template') -NodeType AzureNodes -NumOfQueuedJobsPerNodeToGrow 5
 -NumOfQueuedJobsToGrowThreshold 8 -NumOfInitialNodesToGrow 3
 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 3
```

### <a name="example-2"></a>Exempel 2
hello konfigureras följande exempel hello Azure compute-nod virtuella datorer som distribueras med hello standardmall ComputeNode toogrow och minska automatiskt.
hello jobb som konfigurerats av hello standardmallen för jobbet definiera hello omfattning arbetsbelastningen på hello klustret. Om alla noder i hello ursprungligen har stoppats, startas minst 5 noder. Om hello antalet aktiva köade aktiviteter överskrider 15, hello skriptet börjar noder tills deras nummer överskrider hello förhållandet mellan aktiva köade aktiviteter för**NumOfActiveQueuedTasksPerNodeToGrow**. Om en nod har hittats toobe inaktiv i 10 på varandra följande ledig tid, stoppas.

```powershell
.\AzureAutoGrowShrink.ps1 -NodeTemplates 'Default ComputeNode Template' -JobTemplates 'Default' -NodeType ComputeNodes -NumOfActiveQueuedTasksPerNodeToGrow 10 -NumOfActiveQueuedTasksToGrowThreshold 15 -NumOfInitialNodesToGrow 5 -GrowCheckIntervalMins 1 -ShrinkCheckIntervalMins 1 -ShrinkCheckIdleTimes 10 -ArgFile 'IaaSVMComputeNodes_Arg.xml' -LogFilePrefix 'IaaSVMComputeNodes_log'
```
