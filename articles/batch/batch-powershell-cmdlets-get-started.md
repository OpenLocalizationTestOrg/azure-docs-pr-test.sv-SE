---
title: "aaaGet igång med PowerShell för Azure Batch | Microsoft Docs"
description: "En snabb introduktion toohello Azure PowerShell-cmdlets som du kan använda toomanage Batch resurser."
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: f9ad62c5-27bf-4e6b-a5bf-c5f5914e6199
ms.service: batch
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: powershell
ms.workload: big-compute
ms.date: 02/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3e4d12e9c1e52a5b2db2dd44346edda93b7ef92b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-batch-resources-with-powershell-cmdlets"></a>Hantera Batch-resurser med PowerShell-cmdletar

Med hello Azure Batch-PowerShell-cmdlets, du kan utföra och skript många hello samma uppgifter som du vill utföra med hello Batch-API: er, hello Azure-portalen och hello Azure-kommandoradsgränssnittet (CLI). Det här är en snabb introduktion toohello cmdlets som du kan använda toomanage Batch-konton och arbeta med resurserna Batch som pooler, jobb och uppgifter.

En fullständig lista över Batch-cmdlets och detaljerade cmdlet syntax finns hello [cmdlet-referens för Azure Batch](/powershell/module/azurerm.batch/#batch).

Den här artikeln baseras på cmdlet:ar i Azure PowerShell, version 3.0.0. Vi rekommenderar att du uppdaterar din Azure PowerShell ofta tootake nytta av tjänsteuppdateringar och förbättringar.

## <a name="prerequisites"></a>Krav
Utför följande åtgärder toouse Azure PowerShell toomanage hello Batch-resurser.

* Installera och konfigurera [Azure PowerShell](/powershell/azure/overview)
* Kör hello **Login-AzureRmAccount** cmdlet tooconnect tooyour prenumeration (hello Azure Batch-cmdlets leverera i hello Azure Resource Manager-modulen):
  
    `Login-AzureRmAccount`
* **Registrera med hello Batch leverantörens namnrymd**. Den här åtgärden behöver bara utföras toobe **en gång per prenumeration**.
  
    `Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch`

## <a name="manage-batch-accounts-and-keys"></a>Hantera Batch-konton och nycklar
### <a name="create-a-batch-account"></a>Skapa ett Batch-konto
**New-AzureRmBatchAccount** skapar ett Batch-konto i en angiven resursgrupp. Om du inte redan har en resursgrupp, skapa en genom att köra hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) cmdlet. Ange en hello Azure regioner i hello **plats** parameter, till exempel ”centrala USA”. Exempel:

    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"

Skapa sedan en Batch-kontot i hello resursgruppen att ange ett namn för hello-konto <*kontonamn*> och hello plats och namn för resursgruppen. Skapa hello Batch-kontot kan ta viss tid toocomplete. Exempel:

    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName <res_group_name>

> [!NOTE]
> hello Batch-kontot måste vara unika toohello Azure-region för hello resursgruppen innehålla mellan 3 och 24 tecken och använda gemena bokstäver och siffror.
> 
> 

### <a name="get-account-access-keys"></a>Hämta kontoåtkomstnycklar
**Get-AzureRmBatchAccountKeys** visar hello snabbtangenter som är associerad med ett Azure Batch-konto. Till exempel hello följande tooget hello primära och sekundära nycklarna för hello-konto som du skapade.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <account_name>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey

### <a name="generate-a-new-access-key"></a>Generera en ny åtkomstnyckel
**New-AzureRmBatchAccountKey** genererar en ny primär eller sekundär kontonyckel för ett Azure Batch-konto. Till exempel toogenerate en ny primär nyckel för Batch-kontot, skriver du:

    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary

> [!NOTE]
> ”Sekundär” anger du toogenerate en ny sekundär nyckel för hello **KeyType** parameter. Du måste separat tooregenerate hello primära och sekundära nycklarna.
> 
> 

### <a name="delete-a-batch-account"></a>Ta bort ett Batch-konto
**Remove-AzureRmBatchAccount** tar bort ett Batch-konto. Exempel:

    Remove-AzureRmBatchAccount -AccountName <account_name>

När du uppmanas bekräfta tooremove hello-konto. Borttagning av konto kan ta viss tid toocomplete.

## <a name="create-a-batchaccountcontext-object"></a>Skapa ett BatchAccountContext-objekt
Hej tooauthenticate med hjälp av Batch-PowerShell-cmdlets när du skapar och hanterar Batch pooler, projekt, uppgifter, och andra resurser, först skapa en BatchAccountContext objektet toostore ditt kontonamn och nycklar:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

Du skickar hello BatchAccountContext objekt till cmdlets för att använda hello **BatchContext** parameter.

> [!NOTE]
> Som standard hello databaskontots primärnyckel används för autentisering, men du kan uttryckligen väljer hello viktiga toouse genom att ändra BatchAccountContext objektets **KeyInUse** egenskap: `$context.KeyInUse = "Secondary"`.
> 
> 

## <a name="create-and-modify-batch-resources"></a>Skapa och ändra Batch-resurser
Använd cmdlet: ar som **ny AzureBatchPool**, **ny AzureBatchJob**, och **ny AzureBatchTask** toocreate resurser under ett Batch-konto. Det motsvarande **Get -** och **Set -** cmdlets tooupdate hello egenskaperna för befintliga resurser och **Remove -** cmdlets tooremove resurser under ett Batch-konto.

När du använder många av dessa cmdlets i tillägg toopassing ett BatchContext-objekt, måste toocreate eller skicka objekt som innehåller detaljerade resursinställningar som visas i följande exempel hello. Se hello utförlig information för varje cmdlet ytterligare exempel.

### <a name="create-a-batch-pool"></a>Skapa en Batch-pool
När du skapar eller uppdaterar en Batch-pool du väljer hello molnet tjänstkonfiguration eller hello konfiguration av virtuell dator för hello operativsystemet på hello compute-noder (se [Batch funktionsöversikt](batch-api-basics.md#pool)). Om du anger hello cloud service configuration compute-noder utrustas med en hello [Azure Gästoperativsystem släpper](../cloud-services/cloud-services-guestos-update-matrix.md#releases). Om du anger hello konfiguration av virtuell dator kan du antingen ange en hello stöds Linux eller Windows VM bilder i listan hello [Azure virtuella datorer Marketplace][vm_marketplace], eller ange ett eget bilden som du har förberett.

När du kör **ny AzureBatchPool**, skickar hello operativsysteminställningar i ett PSCloudServiceConfiguration eller PSVirtualMachineConfiguration objekt. Till exempel hello följande cmdlet skapar en ny Batch-pool med storleken små beräkningsnoder i hello cloud service configuration har avbildats med hello senaste operativsystemversion familj 3 (Windows Server 2012). Här hello **CloudServiceConfiguration** parametern anger hello *$configuration* variabeln som hello PSCloudServiceConfiguration objekt. Hej **BatchContext** parametern anger en tidigare definierad variabel *$context* som hello BatchAccountContext objekt.

    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(4,"*")

    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

hello mål antalet beräkningsnoder i hello ny pool bestäms av en autoskalning formel. I det här fallet hello formeln är helt enkelt **$TargetDedicated = 4**, som visar hello antalet beräkningsnoder i poolen hello är 4.

## <a name="query-for-pools-jobs-tasks-and-other-details"></a>Fråga avseende pooler, jobb, uppgifter och annan information
Använd cmdlet: ar som **Get-AzureBatchPool**, **Get-AzureBatchJob**, och **Get-AzureBatchTask** tooquery för entiteter som skapats under ett Batch-konto.

### <a name="query-for-data"></a>Fråga efter data
Exempelvis använda **Get-AzureBatchPools** toofind din pooler. Som standard detta frågor för alla pooler under kontot, under förutsättning att du redan lagras hello BatchAccountContext objekt i *$context*:

    Get-AzureBatchPool -BatchContext $context

### <a name="use-an-odata-filter"></a>Använda ett OData-filter
Du kan ange en OData-filter som använder hello **Filter** parametern toofind hello endast objekt som du är intresserad av. Du kan till exempel hämta alla pooler med ID:n som börjar med ”myPool”:

    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context

Den här metoden är inte lika flexibel som ”Where-Object”-metoden i en lokal pipeline. Dock skickas hello fråga toohello Batch-tjänsten direkt så att alla filtrering uppstår på serversidan för hello, vilket sparar bandbredd för Internet.

### <a name="use-hello-id-parameter"></a>Använd hello Id-parametern
Ett alternativt tooan OData-filtret är toouse hello **Id** parameter. tooquery för en specifik grupp med id ”myPool”:

    Get-AzureBatchPool -Id "myPool" -BatchContext $context

Hej **Id** stöder endast fullständig-ID-sökning, inte jokertecken eller filter för OData-format.

### <a name="use-hello-maxcount-parameter"></a>Använd hello MaxCount-parameter
Som standard returnerar varje cmdlet högst 1 000 objekt. Om du når denna gräns och förfina din filter toobring tillbaka färre objekt, eller uttryckligen har angett maximalt med hello **MaxCount** parameter. Exempel:

    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

tooremove hello övre gränsen, ange **MaxCount** too0 eller mindre.

### <a name="use-hello-powershell-pipeline"></a>Använd hello PowerShell-pipeline
Batch-cmdlets kan utnyttja hello PowerShell pipeline toosend data mellan cmdlets. Detta har samma effekt som anger en parameter, men gör att arbeta med flera enheter enklare hello.

Exempelvis söka efter och visa alla aktiviteter på ditt konto:

    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context

Starta om varje beräkningsnod i en pool:

    Get-AzureBatchComputeNode -PoolId "myPool" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

## <a name="application-package-management"></a>Hantera programpaket
Programpaket erbjuder ett förenklat toodeploy program toohello compute-noder i din pooler. Du kan använda hello Batch PowerShell-cmdlets för att ladda upp och hantera programpaket i Batch-kontot och distribuera paketet versioner toocompute noder.

**Skapa** ett program:

    New-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

**Lägg till** ett programpaket:

    New-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0" -Format zip -FilePath package001.zip

Ange hello **standardversionen** för programmet hello:

    Set-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -DefaultVersion "1.0"

**Lista** ett programs paket

    $application = Get-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

    $application.ApplicationPackages

**Ta bort** ett programpaket

    Remove-AzureRmBatchApplicationPackage -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication" -ApplicationVersion "1.0"

**Ta bort** ett program

    Remove-AzureRmBatchApplication -AccountName <account_name> -ResourceGroupName <res_group_name> -ApplicationId "MyBatchApplication"

> [!NOTE]
> Du måste ta bort alla programpaketversioner för ett program innan du tar bort hello program. Kommer felmeddelandet ”konflikt' om du försöker toodelete ett program som för närvarande har programpaket.
> 
> 

### <a name="deploy-an-application-package"></a>Distribuera ett programpaket
Du kan ange ett eller flera programpaket för distribution när du skapar en pool. När du anger ett paket vid tidpunkten för skapandet av poolen är distribuerade tooeach nod som hello nod ansluter till poolen. Paket distribueras också när en nod startas om eller när en avbildning återställs.

Ange hello `-ApplicationPackageReference` alternativ när du skapar en pool toodeploy paketet toohello programpoolens noder som de gå hello poolen. Skapa först en **PSApplicationPackageReference** objekt och konfigurera det med hello program-Id och paketet version du vill använda toodeploy toohello pool compute-noder:

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "1.0"

Nu skapar hello pool, och ange hello paketet referensobjekt som hello argumentet toohello `ApplicationPackageReferences` alternativ:

    New-AzureBatchPool -Id "PoolWithAppPackage" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -BatchContext $context -ApplicationPackageReferences $appPackageReference

Du hittar mer information om programpaket i [distribuera program toocompute noder med Batch-programpaket](batch-application-packages.md).

> [!IMPORTANT]
> Du måste [länka ett Azure Storage-konto](#linked-storage-account-autostorage) tooyour Batch-kontot toouse programpaket.
> 
> 

### <a name="update-a-pools-application-packages"></a>Uppdatera programpaket för en pool
tooupdate hello program tilldelade tooan befintlig pool, först skapa ett PSApplicationPackageReference-objekt med hello önskade egenskaper (program-Id och paketet version):

    $appPackageReference = New-Object Microsoft.Azure.Commands.Batch.Models.PSApplicationPackageReference

    $appPackageReference.ApplicationId = "MyBatchApplication"

    $appPackageReference.Version = "2.0"

Därefter hämta hello pool från Batch, rensa eventuella befintliga paket, Lägg till vår nya paket referens och uppdatera hello Batch-tjänsten med hello nya inställningar för programpool:

    $pool = Get-AzureBatchPool -BatchContext $context -Id "PoolWithAppPackage"

    $pool.ApplicationPackageReferences.Clear()

    $pool.ApplicationPackageReferences.Add($appPackageReference)

    Set-AzureBatchPool -BatchContext $context -Pool $pool

Hello programpoolens egenskaper i hello Batch-tjänsten har uppdaterats. tooactually distribuera hello nya program paketet toocompute noder i hello pool, men du måste starta om eller återavbilda noderna. Du kan starta om varje nod i en pool med det här kommandot:

    Get-AzureBatchComputeNode -PoolId "PoolWithAppPackage" -BatchContext $context | Restart-AzureBatchComputeNode -BatchContext $context

> [!TIP]
> Du kan distribuera flera program paket toohello compute-noder i en pool. Om du vill ha för*lägga till* ett programpaket i stället för att ersätta hello som för närvarande har distribuerats paket utelämna hello `$pool.ApplicationPackageReferences.Clear()` raden ovan.
> 
> 

## <a name="next-steps"></a>Nästa steg
* Detaljerad cmdlet-syntax och exempel finns i [Cmdlet-referens för Azure Batch](/powershell/module/azurerm.batch/#batch).
* Mer information om program och programpaket i Batch finns [distribuera program toocompute noder med Batch-programpaket](batch-application-packages.md).

[vm_marketplace]: https://azure.microsoft.com/marketplace/virtual-machines/