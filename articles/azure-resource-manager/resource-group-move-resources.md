---
title: aaaMove Azure-resurser toonew prenumerationen eller resursen grupp | Microsoft Docs
description: "Använd Azure Resource Manager toomove resurser tooa ny resursgrupp eller prenumeration."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: ab7d42bd-8434-4026-a892-df4a97b60a9b
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: tomfitz
ms.openlocfilehash: 09d35f0afbbcdc0c66779f98a982d878f0807497
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-resources-toonew-resource-group-or-subscription"></a>Flytta resurser toonew resursgrupp eller prenumeration
Det här avsnittet visar hur toomove resurser tooeither en ny prenumeration eller en ny resurs gruppera i hello samma prenumeration. Du kan använda hello portal, PowerShell, Azure CLI eller hello REST API toomove resurs. hello flytta åtgärder i det här avsnittet är tillgängliga tooyou utan hjälp från Azure-supporten.

När du flyttar resurser, låst både hello källgrupp och hello målgruppen under hello igen. Skriva och ta bort blockeras på hello resursgrupper tills hello flytta har slutförts. Låset innebär det går inte att lägga till, uppdatera eller ta bort hello resursgrupper, men innebär inte hello resurser är låsta. Om du flyttar en SQL Server och dess databas tooa ny resursgrupp, påträffar ett program som använder hello databas utan avbrott. Det kan fortfarande läsa och skriva toohello databas.

Du kan inte ändra hello platsen för hello resurs. Om du flyttar en resurs flyttas det endast tooa ny resursgrupp. hello ny resursgrupp kan ha en annan plats, men som ändras inte hello platsen för hello resurs.

> [!NOTE]
> Den här artikeln beskriver hur toomove resurser i en befintlig Azure-konto erbjudanden. Om du verkligen vill toochange ditt Azure-konto erbjudande (till exempel uppgraderar från betalning per användning toopre-pay) medan toowork med din befintliga resurser, se [växla Azure-prenumeration tooanother erbjudandet](../billing/billing-how-to-switch-azure-offer.md).
>
>

## <a name="checklist-before-moving-resources"></a>Checklista innan du flyttar resurser
Det finns vissa viktiga steg tooperform innan du flyttar en resurs. Du kan undvika fel genom att verifiera dessa villkor.

1. hello käll- och prenumerationer måste finnas inom hello samma [Azure Active Directory-klient](../active-directory/active-directory-howto-tenant.md). toocheck att båda prenumerationer har hello samma klient-ID, använda Azure PowerShell eller Azure CLI.

  Använd för Azure PowerShell:

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName "Example Subscription").TenantId
  ```

  Använd för Azure CLI 2.0:

  ```azurecli
  az account show --subscription "Example Subscription" --query tenantId
  ```

  Om hello klient-ID för är hello käll- och prenumerationer inte hello samma försök toochange hello katalogen för hello-prenumeration. Det här alternativet är dock endast tillgängliga tooService administratörer som har loggat in med ett Microsoft-konto (inte ett organisationskonto). Ändra hello directory, logga in toohello tooattempt [klassiska portalen](https://manage.windowsazure.com/), och välj **inställningar**, och välj hello-prenumeration. Om hello **redigera katalog** ikonen är tillgänglig, markerar du den toochange hello som är associerade med Azure Active Directory.

  ![Redigera katalog](./media/resource-group-move-resources/edit-directory.png)

  Om ikonen inte är tillgängligt måste du kontakta supporten toomove hello resurser tooa nya innehavaren.

2. hello-tjänsten måste aktivera hello möjlighet toomove resurser. Det här avsnittet visas vilka tjänster kan flytta resurser och tjänster som gör inte flytta resurser.
3. Hej målprenumerationen måste registreras för hello resursprovidern hello resurs som flyttas. Om inte, du får ett felmeddelande om att hello **prenumerationen har inte registrerats för en resurstyp**. Det här problemet kan uppstå när du flyttar en resurs tooa ny prenumeration, men den prenumerationen aldrig har använts med den resurstypen. toolearn hur toocheck hello registreringsstatus och registrera resursprovidrar finns [resursproviders och typer](resource-manager-supported-services.md).

## <a name="when-toocall-support"></a>När toocall stöd
Du kan flytta de flesta resurser via hello självbetjäning åtgärder visas i det här avsnittet. Använd hello självbetjäning åtgärder:

* Flytta resurshanterarens resurser.
* Flytta klassiska resurser enligt toohello [klassisk distribution begränsningar](#classic-deployment-limitations).

Ring supporten när du behöver:

* Flytta resurser tooa nya Azure-konto (och Azure Active Directory-klient).
* Flytta klassiska resurser men har problem med hello begränsningar.

## <a name="services-that-enable-move"></a>Tjänster som gör att flytta
Hello-tjänster som möjliggör glidande tooboth en ny resursgrupp och prenumerationen är för tillfället:

* API Management
* Apptjänst-appar (webbprogram) - finns [Apptjänst begränsningar](#app-service-limitations)
* Application Insights
* Automation
* Batch
* Bing Maps
* CDN
* Molntjänster - Se [klassisk distribution begränsningar](#classic-deployment-limitations)
* Cognitive Services
* Content Moderator
* Data Catalog
* Data Factory
* Data Lake Analytics
* Data Lake Store
* DNS
* Azure Cosmos DB
* Händelsehubbar
* HDInsight-kluster - finns [HDInsight begränsningar](#hdinsight-limitations)
* IoT-hubbar
* Key Vault
* Belastningsutjämnare
* Logic Apps
* Machine Learning
* Media Services
* Mobile Engagement
* Notification Hubs
* Åtgärdsinformation
* Operations Management
* Power BI
* Redis Cache
* Scheduler
* Söka
* Serverhantering
* Service Bus
* Service Fabric
* Lagring
* Storage (klassisk) - finns [klassisk distribution begränsningar](#classic-deployment-limitations)
* Stream Analytics - Stream Analytics-jobb inte kan flyttas när du kör i läget.
* SQL-databasservern - hello databasen och servern måste finnas i hello samma resursgrupp. När du flyttar en SQLServer, flyttas även alla databaser.
* Traffic Manager
* Virtuella datorer
* Virtuella datorer med certifikat som lagras i Key Vault - flytta toonew resursgrupp i samma prenumeration har aktiverats men flytta mellan prenumeration har inte aktiverats.
* Virtuella datorer (klassisk) - finns [klassisk distribution begränsningar](#classic-deployment-limitations)
* Skalningsuppsättningar för Virtual Machines
* Virtuella nätverk - just nu, ett peered virtuellt nätverk går inte att flytta tills VNet-peering har inaktiverats. När inaktiverad hello virtuellt nätverk kan flyttas utan problem och hello VNet-peering kan aktiveras. Dessutom får inte ett virtuellt nätverk vara flyttade tooa annan prenumeration om hello virtuellt nätverk innehåller inget undernät med resursnavigeringslänkar. Ett undernät för virtuellt nätverk har till exempel en resurslänk för navigering när en resurs för Microsoft.Cache redis har distribuerats i det här undernätet.
* VPN-gateway


## <a name="services-that-do-not-enable-move"></a>Tjänster som inte gör flytta
hello-tjänster som för närvarande inte aktiverar flytta en resurs är:

* AD DS
* AD-Hybrid-tjänsten för hälsotillstånd
* Application Gateway
* Tillgänglighetsuppsättningar med virtuella datorer med hanterade diskar
* BizTalk Services
* Container Service
* Express Route
* DevTest Labs - flytta toonew resursgrupp i samma prenumeration har aktiverats men mellan flytta för prenumerationen har inte aktiverats.
* Dynamics LCS
* Bilder som har skapats från hanterade diskar
* Managed Disks
* Hanterade program
* Recovery Services-valvet – även hello återställningstjänster av inte flytta hello beräknings-, nätverks- och resurser som är associerade med valvet, se [återställningstjänster begränsningar](#recovery-services-limitations).
* Säkerhet
* Ögonblicksbilder som skapats från hanterade diskar
* StorSimple Enhetshanteraren
* Virtuella datorer med hanterade diskar
* Virtuella nätverk (klassiskt) - finns [klassisk distribution begränsningar](#classic-deployment-limitations)
* Virtuella datorer som skapats från Marketplace resurser - kan inte flyttas mellan prenumerationer. Resursen behöver toobe avetableras i hello aktuell prenumeration och distribueras igen i hello ny prenumeration

## <a name="app-service-limitations"></a>Begränsningar för App Service
När du arbetar med Apptjänst-appar kan flytta du inte endast en App Service-plan. toomove Apptjänst-appar, alternativen är:

* Flytta hello App Service-plan och alla andra resurser i Apptjänst i som gruppen tooa ny resurs resursgruppen som inte redan har App Service-resurser. Det här kravet innebär måste du flytta även hello Apptjänst resurser som inte är associerad med hello App Service-plan.
* Flytta hello appar tooa annan resursgrupp, men behålla alla programtjänstplaner i hello ursprungliga resursgruppen.

hello hello App Service-plan inte behöver tooreside i samma resursgrupp som hello app för hello app toofunction korrekt.

Om till exempel din resursgrupp innehåller:

* **Web-a** som är associerad med **planera en**
* **Web-b** som är associerad med **plan b**

Alternativen är:

* Flytta **web-a**, **planera en**, **web-b**, och **plan b**
* Flytta **web-a** och **web-b**
* Flytta **web-a**
* Flytta **web-b**

Alla andra kombinationer involverar lämnar bakom en resurstyp som kan finnas kvar när du flyttar en apptjänstplan (någon typ av App Service-resurs).

Om ditt webbprogram finns i en annan resursgrupp än dess App Service-plan, men toomove båda tooa ny resursgrupp, måste du flytta hello i två steg. Exempel:

* **Web-a** finns i **web-grupp**
* **Planera en** finns i **plan-grupp**
* Du vill **web-a** och **planera en** tooreside i **kombineras grupp**

tooaccomplish detta flytta utföra åtgärder för två separata flytta i hello följande ordning:

1. Flytta hello **web-a** för**plan-grupp**
2. Flytta **web-a** och **planera en** för**kombineras grupp**.

Du kan flytta en App tjänstcertifikat tooa ny resursgrupp eller prenumeration utan problem. Om webbappen innehåller ett SSL-certifikat som du köpt externt och överföra toohello app, måste du radera hello certifikat innan glidande hello-webbprogram. Du kan exempelvis utföra hello följande steg:

1. Ta bort hello upp certifikatet från hello webbapp
2. Flytta hello-webbprogram
3. Överföra hello certifikat toohello webbapp

## <a name="recovery-services-limitations"></a>Recovery Services-begränsningar
Flytta har inte aktiverats för lagring, nätverk, eller beräkningsresurser används tooset in katastrofåterställning med Azure Site Recovery.

Till exempel anta att du har ställt in replikering av ditt lokala datorer tooa storage-konto (Storage1) och vill att hello skyddad dator toocome efter redundans tooAzure som en virtuell dator (VM1) kopplad tooa virtuella nätverk (Network1). Du kan inte flytta någon av dessa Azure-resurser - Storage1, VM1, och Network1 - över resurs grupper inom hello samma prenumeration eller mellan prenumerationer.

## <a name="hdinsight-limitations"></a>HDInsight-begränsningar

Du kan flytta HDInsight-kluster tooa ny prenumeration eller resursgrupp. Men kan inte du flytta mellan prenumerationer hello nätverk resurser länkade toohello HDInsight-kluster (till exempel hello virtuellt nätverk, nätverkskort eller belastningsutjämnare). Dessutom kan du flytta tooa ny resursgrupp ett nätverkskort som är anslutna tooa virtuell dator för hello-kluster.

När du flyttar en ny prenumeration för HDInsight-kluster tooa först flytta andra resurser (till exempel hello storage-konto). Flytta sedan hello HDInsight-kluster av sig själv.

## <a name="classic-deployment-limitations"></a>Klassisk distribution begränsningar
hello variera alternativ för att flytta resurser har distribuerats via hello klassiska modellen beroende på om du flyttar hello resurser inom en prenumeration eller tooa ny prenumeration.

### <a name="same-subscription"></a>Samma prenumeration
När du flyttar resurser från en grupp tooanother resurs resursgrupp inom samma prenumeration, hello följande begränsningar gäller hello:

* Virtuella nätverk (klassiskt) kan inte flyttas.
* Virtuella datorer (klassisk) måste flyttas med hello-Molntjänsten.
* Molntjänsten kan bara flyttas när hello flytta innehåller alla virtuella datorer.
* Endast en molnbaserad tjänst kan flyttas åt gången.
* Endast en storage-konto (klassiskt) kan flyttas åt gången.
* Storage-konto (klassisk) inte kan flyttas i hello samma igen med en virtuell dator eller en tjänst i molnet.

toomove klassiska resurser tooa ny resursgrupp i Hej samma prenumeration, använda hello standard flytta åtgärder med hjälp av hello [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), eller [REST API](#use-rest-api). Du använder hello samma åtgärder som du använder för att flytta Resource Manager-resurser.

### <a name="new-subscription"></a>Ny prenumeration
När du flyttar resurser tooa ny prenumeration gäller hello följande begränsningar:

* Alla klassiska resurser i hello prenumerationen måste flyttas i hello samma åtgärd.
* Hej målprenumerationen får inte innehålla andra klassiska resurser.
* hello flytta kan endast begäras via en separat REST-API för klassiska flyttar. hello Resource Manager flytta standardkommandon fungerar inte när du flyttar klassiska resurser tooa ny prenumeration.

toomove klassiska resurser tooa ny prenumeration, Använd hello REST-åtgärder som är specifika tooclassic resurser. toouse vila, utför följande steg hello:

1. Kontrollera om hello källprenumerationen kan delta i över prenumerationer flytta. Använd hello följande åtgärd:

  ```HTTP   
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     I hello frågans brödtext är:

  ```json
  {
    "role": "source"
  }
  ```

     hello-svar för hello valideringen har hello följande format:

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. Kontrollera om hello målprenumerationen kan delta i över prenumerationer flytta. Använd hello följande åtgärd:

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     I hello frågans brödtext är:

  ```json
  {
    "role": "target"
  }
  ```

     hello svaret är i hello samma format som hello källa prenumeration validering.
3. Om båda prenumerationer har klarat valideringen, flytta alla klassiska resurser från en prenumeration tooanother prenumeration med hello följande åtgärd:

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    I hello frågans brödtext är:

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

hello-åtgärden kan ta flera minuter.

## <a name="use-portal"></a>Använda portalen
toomove resurser, Välj hello resursgruppen som innehåller de resurserna och välj sedan hello **flytta** knappen.

![Flytta resurser](./media/resource-group-move-resources/select-move.png)

Välj om du flyttar hello resurser tooa ny resursgrupp eller en ny prenumeration.

Välj hello resurser toomove och hello mål resursgruppen. Bekräfta att du behöver tooupdate skript för dessa resurser och välj **OK**. Om du har valt hello redigeringsikonen prenumeration i hello föregående steg, måste du också välja hello målprenumerationen.

![Välj mål](./media/resource-group-move-resources/select-destination.png)

I **meddelanden**, du ser att hello flytta åtgärd körs.

![Visa flytta status](./media/resource-group-move-resources/show-status.png)

När det har slutförts, meddelas du om hello resultat.

![Visa flytta resultat](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>Använd PowerShell
toomove befintliga resurser tooanother resursgrupp eller prenumeration, använda hello `Move-AzureRmResource` kommando.

Hej det första exemplet visas hur toomove en resurs tooa ny resursgrupp.

```powershell
$resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId
```

hello andra exempel visar hur toomove flera resurser tooa ny resursgrupp.

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

toomove tooa ny prenumeration, innehåller ett värde för hello `DestinationSubscriptionId` parameter.

Du uppmanas tooconfirm som du vill toomove hello anges resurser.

```powershell
Confirm
Are you sure you want toomove these resources toohello resource group
'/subscriptions/{guid}/resourceGroups/newRG' hello resources:

/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
```

## <a name="use-azure-cli-20"></a>Använda Azure CLI 2.0
toomove befintliga resurser tooanother resursgrupp eller prenumeration, använda hello `az resource move` kommando. Ange hello resurs-ID för hello resurser toomove. Du kan få resurs-ID med hello följande kommando:

```azurecli
az resource show -g sourceGroup -n storagedemo --resource-type "Microsoft.Storage/storageAccounts" --query id
```

hello som följande exempel visar hur toomove en lagringsplats för kontot tooa ny resursgrupp. I hello `--ids` parameter, ange en blankstegsavgränsad lista över hello resurs-ID: N toomove.

```azurecli
az resource move --destination-group newgroup --ids "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo"
```

toomove tooa ny prenumeration, ange hello `--destination-subscription-id` parameter.

## <a name="use-azure-cli-10"></a>Använda Azure CLI 1.0
toomove befintliga resurser tooanother resursgrupp eller prenumeration, använda hello `azure resource move` kommando. Ange hello resurs-ID för hello resurser toomove. Du kan få resurs-ID med hello följande kommando:

```azurecli
azure resource list -g sourceGroup --json
```

Som returnerar hello följande format:

```azurecli
[
  {
    "id": "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo",
    "name": "storagedemo",
    "type": "Microsoft.Storage/storageAccounts",
    "location": "southcentralus",
    "tags": {},
    "kind": "Storage",
    "sku": {
      "name": "Standard_RAGRS",
      "tier": "Standard"
    }
  }
]
```

hello som följande exempel visar hur toomove en lagringsplats för kontot tooa ny resursgrupp. I hello `-i` parameter, ange en kommaavgränsad lista över hello resurs-ID: N toomove.

```azurecli
azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
```

Du uppmanas tooconfirm som du vill toomove hello angivna resursen.

## <a name="use-rest-api"></a>Använd REST-API
toomove befintliga resurser tooanother resursgrupp eller prenumeration, kör:

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

I hello begärantext anger du hello målresursgruppen och hello resurser toomove. Läs mer om hello flytta REST-åtgärden [flyttar resurser](https://msdn.microsoft.com/library/azure/mt218710.aspx).

## <a name="next-steps"></a>Nästa steg
* toolearn om PowerShell-cmdletar för att hantera din prenumeration, se [med hjälp av Azure PowerShell med Resource Manager](powershell-azure-resource-manager.md).
* toolearn om Azure CLI-kommandon för att hantera din prenumeration, se [Using hello Azure CLI med Resource Manager](xplat-cli-azure-resource-manager.md).
* toolearn om portalen funktioner för att hantera din prenumeration, se [använder hello Azure portal toomanage resurser](resource-group-portal.md).
* toolearn om hur du kopplar ett logiskt tooyour resurser, se [med hjälp av taggar tooorganize dina resurser](resource-group-using-tags.md).
