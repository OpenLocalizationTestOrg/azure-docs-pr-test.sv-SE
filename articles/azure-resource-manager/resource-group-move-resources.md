---
title: Flytta Azure-resurser till nya prenumerationen eller resursen grupp | Microsoft Docs
description: "Använd Azure Resource Manager för att flytta resurser till en ny resursgrupp eller prenumeration."
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
ms.openlocfilehash: e138f80e808968ab4bf5c11cfd5fd46fe4a1bcce
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="move-resources-to-new-resource-group-or-subscription"></a>Flytta resurser till en ny resursgrupp eller prenumeration
Det här avsnittet visar hur du flyttar resurser till en ny prenumeration eller en ny resursgrupp med samma prenumeration. Du kan använda portalen, PowerShell, Azure CLI eller REST API för att flytta resursen. Flytta åtgärder i det här avsnittet kan du utan hjälp från Azure-supporten.

När du flyttar resurser, låst både gruppen och målgruppen under åtgärden. Skriva och ta bort blockeras på resursgrupper tills flyttningen är klar. Låset innebär det går inte att lägga till, uppdatera eller ta bort resurser i resursgrupper, men innebär inte resurserna som är låsta. Om du flyttar en SQL Server och dess databas till en ny resursgrupp påträffar ett program som använder databasen utan avbrott. Det kan fortfarande läsa och skriva till databasen.

Du kan inte ändra platsen för resursen. Flytta en resurs bara flyttas till en ny resursgrupp. Den nya resursgruppen kan ha en annan plats, men som ändra inte platsen för resursen.

> [!NOTE]
> Den här artikeln beskrivs hur du flyttar resurser i en befintlig Azure-konto erbjudanden. Om du vill ändra ditt Azure-konto erbjudande (till exempel uppgraderar från betala per användning före betala) när fortsätter att fungera med din befintliga resurser, se [växla din Azure-prenumeration till ett annat erbjudande](../billing/billing-how-to-switch-azure-offer.md).
>
>

## <a name="checklist-before-moving-resources"></a>Checklista innan du flyttar resurser
Några viktiga steg måste utföras innan en resurs flyttas. Du kan undvika fel genom att verifiera dessa villkor.

1. Käll- och -prenumerationer måste finnas inom samma [Azure Active Directory-klient](../active-directory/active-directory-howto-tenant.md). Använd Azure PowerShell eller Azure CLI för att kontrollera att båda prenumerationer har samma klient-ID.

  Använd för Azure PowerShell:

  ```powershell
  (Get-AzureRmSubscription -SubscriptionName "Example Subscription").TenantId
  ```

  Använd för Azure CLI 2.0:

  ```azurecli
  az account show --subscription "Example Subscription" --query tenantId
  ```

  Om klient-ID: N för käll- och -prenumerationer inte är samma, kan du försöka ändra katalogen för prenumerationen. Det här alternativet är endast tillgänglig för administratörer som har loggat in med ett Microsoft-konto (inte ett organisationskonto). Logga in för att försöka ändra katalogen till den [klassiska portalen](https://manage.windowsazure.com/), och välj **inställningar**, och välj prenumerationen. Om den **redigera katalog** ikonen är tillgänglig kan du välja att ändra de associerade Azure Active Directory.

  ![Redigera katalog](./media/resource-group-move-resources/edit-directory.png)

  Om ikonen inte är tillgängligt måste du kontakta supporten om du vill flytta resurserna till en ny klient.

2. Tjänsten måste göra det möjligt att flytta resurser. Det här avsnittet visas vilka tjänster kan flytta resurser och tjänster som gör inte flytta resurser.
3. Målprenumerationen måste vara registrerad för resursprovidern för den resurs som flyttas. Om inte, du får ett felmeddelande om att den **prenumerationen har inte registrerats för en resurstyp**. Du kan stöta på detta problem när en resurs flyttas till en ny prenumeration, men prenumerationen aldrig har använts med den resurstypen. Information om hur du kontrollerar registreringsstatus och registrerar resursprovidrar finns i [Resursprovidrar och typer](resource-manager-supported-services.md).

## <a name="when-to-call-support"></a>När anropet stöd
Du kan flytta de flesta resurser via självbetjäning åtgärder visas i det här avsnittet. Använd åtgärderna självbetjäning till:

* Flytta resurshanterarens resurser.
* Flytta klassiska resurser enligt den [klassisk distribution begränsningar](#classic-deployment-limitations).

Ring supporten när du behöver:

* Flytta dina resurser till en ny Azure-konto (och Azure Active Directory-klient).
* Flytta klassiska resurser men har problem med begränsningar.

## <a name="services-that-enable-move"></a>Tjänster som gör att flytta
För tillfället är de tjänster som gör att du flyttar till en ny resursgrupp och en prenumeration

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
* SQL Database-server - databasen och servern måste finnas i samma resursgrupp. När du flyttar en SQLServer, flyttas även alla databaser.
* Traffic Manager
* Virtuella datorer
* Virtuella datorer med certifikat som lagras i Key Vault - flytta till ny resurs gruppen i samma prenumeration är aktiverad men flytta mellan prenumeration har inte aktiverats.
* Virtuella datorer (klassisk) - finns [klassisk distribution begränsningar](#classic-deployment-limitations)
* Skalningsuppsättningar för Virtual Machines
* Virtuella nätverk - just nu, ett peered virtuellt nätverk går inte att flytta tills VNet-peering har inaktiverats. När inaktiverat, det virtuella nätverket kan flyttas utan problem och VNet-peering kan aktiveras. Ett virtuellt nätverk kan dessutom inte flyttas till en annan prenumeration om det virtuella nätverket innehåller inget undernät med resursnavigeringslänkar. Ett undernät för virtuellt nätverk har till exempel en resurslänk för navigering när en resurs för Microsoft.Cache redis har distribuerats i det här undernätet.
* VPN-gateway


## <a name="services-that-do-not-enable-move"></a>Tjänster som inte gör flytta
De tjänster som för närvarande inte aktiverar flytta en resurs är:

* AD DS
* AD-Hybrid-tjänsten för hälsotillstånd
* Application Gateway
* Tillgänglighetsuppsättningar med virtuella datorer med hanterade diskar
* BizTalk Services
* Container Service
* Express Route
* DevTest Labs - flyttar till en ny resursgrupp i samma prenumeration har aktiverats men flytta mellan prenumeration har inte aktiverats.
* Dynamics LCS
* Bilder som har skapats från hanterade diskar
* Managed Disks
* Hanterade program
* Recovery Services-ventilen - också vill inte flytta beräknings-, nätverks- och resurser som är associerade med Recovery Services-valvet finns [återställningstjänster begränsningar](#recovery-services-limitations).
* Säkerhet
* Ögonblicksbilder som skapats från hanterade diskar
* StorSimple Enhetshanteraren
* Virtuella datorer med hanterade diskar
* Virtuella nätverk (klassiskt) - finns [klassisk distribution begränsningar](#classic-deployment-limitations)
* Virtuella datorer som skapats från Marketplace resurser - kan inte flyttas mellan prenumerationer. Resursen måste avetableras i den aktuella prenumerationen och distribuerade igen på den nya prenumerationen

## <a name="app-service-limitations"></a>Begränsningar för App Service
När du arbetar med Apptjänst-appar kan flytta du inte endast en App Service-plan. Om du vill flytta Apptjänst-appar är alternativen:

* Flytta App Service-plan och alla andra resurser i Apptjänst i resursgruppen till en ny resursgrupp som inte redan har App Service-resurser. Det här kravet innebär måste du flytta även Apptjänst resurser som inte är associerad med App Service-plan.
* Flytta apparna till en annan resursgrupp men behålla alla programtjänstplaner i den ursprungliga resursgruppen.

Programtjänstplanen behöver inte finnas i samma resursgrupp som appen för appen ska fungera korrekt.

Om till exempel din resursgrupp innehåller:

* **Web-a** som är associerad med **planera en**
* **Web-b** som är associerad med **plan b**

Alternativen är:

* Flytta **web-a**, **planera en**, **web-b**, och **plan b**
* Flytta **web-a** och **web-b**
* Flytta **web-a**
* Flytta **web-b**

Alla andra kombinationer involverar lämnar bakom en resurstyp som kan finnas kvar när du flyttar en apptjänstplan (någon typ av App Service-resurs).

Om ditt webbprogram finns i en annan resursgrupp än dess App Service-plan, men du vill flytta både en ny resursgrupp, måste du flytta i två steg. Exempel:

* **Web-a** finns i **web-grupp**
* **Planera en** finns i **plan-grupp**
* Du vill **web-a** och **planera en** finns i **kombineras grupp**

För att åstadkomma detta steg, utför du två separata flytta åtgärderna i följande ordning:

1. Flytta den **web-a** till **plan-grupp**
2. Flytta **web-a** och **planera en** till **kombineras grupp**.

Du kan flytta ett certifikat för App Service till en ny resursgrupp eller prenumeration utan problem. Om webbappen innehåller ett SSL-certifikat som du köpt externt och överförs till appen, måste du radera certifikatet innan du flyttar webbprogrammet. Exempelvis kan du utföra följande steg:

1. Ta bort det överförda certifikatet från webbappen
2. Flytta webbappen
3. Överför certifikatet till webbappen

## <a name="recovery-services-limitations"></a>Recovery Services-begränsningar
Flytta inte har aktiverats för lagring, nätverk, eller beräkningsresurser som används för att ställa in katastrofåterställning med Azure Site Recovery.

Anta att du har ställt in replikering av din lokala datorer till ett lagringskonto (Storage1) och vill att den skydda datorn att starta efter en redundansväxling till Azure som en virtuell dator (VM1) ansluten till ett virtuellt nätverk (Network1). Du kan inte flytta resurserna Azure - Storage1 VM1 och Network1 - över resursgrupper inom samma prenumeration eller alla prenumerationer.

## <a name="hdinsight-limitations"></a>HDInsight-begränsningar

Du kan flytta HDInsight-kluster till en ny prenumeration eller resursgrupp. Men kan inte du flytta alla prenumerationer som nätverksresurser som är kopplad till HDInsight-klustret (till exempel virtuella nätverk, nätverkskort eller belastningsutjämning). Dessutom kan flytta du inte till en ny resursgrupp ett nätverkskort som är kopplad till en virtuell dator för klustret.

När du flyttar ett HDInsight-kluster till en ny prenumeration kan du först flytta andra resurser (till exempel storage-konto). Flytta sedan HDInsight-kluster av sig själv.

## <a name="classic-deployment-limitations"></a>Klassisk distribution begränsningar
Alternativ för att flytta resurser har distribuerats via den klassiska modellen variera beroende på om du flyttar resurser inom en prenumeration eller till en ny prenumeration.

### <a name="same-subscription"></a>Samma prenumeration
När du flyttar resurser från en resursgrupp till en annan resursgrupp inom samma prenumeration, gäller följande begränsningar:

* Virtuella nätverk (klassiskt) kan inte flyttas.
* Virtuella datorer (klassisk) måste flyttas med Molntjänsten.
* Molntjänsten kan bara flyttas när flytten omfattar alla virtuella datorer.
* Endast en molnbaserad tjänst kan flyttas åt gången.
* Endast en storage-konto (klassiskt) kan flyttas åt gången.
* Storage-konto (klassiskt) kan inte flyttas på samma gång med en virtuell dator eller en tjänst i molnet.

Om du vill flytta klassiska resurser till en ny resursgrupp inom samma prenumeration, använder du standard move-åtgärder via den [portal](#use-portal), [Azure PowerShell](#use-powershell), [Azure CLI](#use-azure-cli), eller [REST API](#use-rest-api). Du kan använda samma åtgärder som du använder för att flytta Resource Manager-resurser.

### <a name="new-subscription"></a>Ny prenumeration
När resurserna flyttas till en ny prenumeration, gäller följande begränsningar:

* Alla klassiska resurser i prenumerationen måste flyttas samtidigt.
* Målprenumerationen får inte innehålla andra klassiska resurser.
* Flyttningen kan endast begäras via en separat REST-API för klassiska flyttar. Kommandona flytta standard Resource Manager fungerar inte när du flyttar klassiska resurser till en ny prenumeration.

Flytta klassiska resurser till en ny prenumeration genom att använda REST-åtgärder som är specifika för klassiska resurser. Utför följande steg om du vill använda REST:

1. Kontrollera om källprenumerationen kan delta i en flytt mellan prenumerationer. Använd följande åtgärd:

  ```HTTP   
  POST https://management.azure.com/subscriptions/{sourceSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     I frågans brödtext är:

  ```json
  {
    "role": "source"
  }
  ```

     Svaret för valideringen har följande format:

  ```json
  {
    "status": "{status}",
    "reasons": [
      "reason1",
      "reason2"
    ]
  }
  ```

2. Kontrollera om målprenumerationen kan delta i en flytt mellan prenumerationer. Använd följande åtgärd:

  ```HTTP
  POST https://management.azure.com/subscriptions/{destinationSubscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
  ```

     I frågans brödtext är:

  ```json
  {
    "role": "target"
  }
  ```

     Svaret är i samma format som källa prenumeration verifieringen.
3. Om båda prenumerationer har klarat valideringen, flytta alla klassiska resurser från en prenumeration till en annan prenumeration med följande åtgärd:

  ```HTTP
  POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01
  ```

    I frågans brödtext är:

  ```json
  {
    "target": "/subscriptions/{target-subscription-id}"
  }
  ```

Åtgärden kan ta flera minuter.

## <a name="use-portal"></a>Använda portalen
Välj den resursgrupp som innehåller resurserna för att flytta resurser och välj sedan den **flytta** knappen.

![Flytta resurser](./media/resource-group-move-resources/select-move.png)

Välj om du flyttar resurser till en ny resursgrupp eller en ny prenumeration.

Välj att flytta resurserna och resursgruppen mål. Bekräfta att du behöver uppdatera skript för dessa resurser och välj **OK**. Om du valde ikonen Redigera prenumeration i föregående steg, måste du också välja målprenumerationen.

![Välj mål](./media/resource-group-move-resources/select-destination.png)

I **meddelanden**, du ser att flyttåtgärden körs.

![Visa flytta status](./media/resource-group-move-resources/show-status.png)

När det har slutförts, meddelas du om resultatet.

![Visa flytta resultat](./media/resource-group-move-resources/show-result.png)

## <a name="use-powershell"></a>Använd PowerShell
Flytta befintliga resurser till en annan resursgrupp eller prenumeration genom att använda den `Move-AzureRmResource` kommando.

Det första exemplet visar hur du flyttar en resurs till en ny resursgrupp.

```powershell
$resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId
```

Det andra exemplet visar hur du flyttar flera resurser till en ny resursgrupp.

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Om du vill flytta till en ny prenumeration, innehåller ett värde för den `DestinationSubscriptionId` parameter.

Du uppmanas att bekräfta att du vill flytta de angivna resurserna.

```powershell
Confirm
Are you sure you want to move these resources to the resource group
'/subscriptions/{guid}/resourceGroups/newRG' the resources:

/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
/subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y
```

## <a name="use-azure-cli-20"></a>Använda Azure CLI 2.0
Flytta befintliga resurser till en annan resursgrupp eller prenumeration genom att använda den `az resource move` kommando. Ange resurs-ID för resurserna för att flytta. Du kan få resurs-ID med följande kommando:

```azurecli
az resource show -g sourceGroup -n storagedemo --resource-type "Microsoft.Storage/storageAccounts" --query id
```

I följande exempel visas hur du flyttar ett storage-konto till en ny resursgrupp. I den `--ids` parameter, ange en blankstegsavgränsad lista över resurs-ID att flytta.

```azurecli
az resource move --destination-group newgroup --ids "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo"
```

Om du vill flytta till en ny prenumeration, ange den `--destination-subscription-id` parameter.

## <a name="use-azure-cli-10"></a>Använda Azure CLI 1.0
Flytta befintliga resurser till en annan resursgrupp eller prenumeration genom att använda den `azure resource move` kommando. Ange resurs-ID för resurserna för att flytta. Du kan få resurs-ID med följande kommando:

```azurecli
azure resource list -g sourceGroup --json
```

Som returnerar följande format:

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

I följande exempel visas hur du flyttar ett storage-konto till en ny resursgrupp. I den `-i` parameter, ange en kommaavgränsad lista över resurs-ID att flytta.

```azurecli
azure resource move -i "/subscriptions/{guid}/resourceGroups/sourceGroup/providers/Microsoft.Storage/storageAccounts/storagedemo" -d "destinationGroup"
```

Du uppmanas att bekräfta att du vill flytta den angivna resursen.

## <a name="use-rest-api"></a>Använd REST-API
För att flytta befintliga resurser till en annan resursgrupp eller prenumeration, kör du:

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

I begärandetexten anger du målresursgruppen och resurser för att flytta. Mer information om REST-flyttåtgärden finns [flyttar resurser](https://msdn.microsoft.com/library/azure/mt218710.aspx).

## <a name="next-steps"></a>Nästa steg
* Mer information om PowerShell-cmdletar för att hantera din prenumeration, se [med hjälp av Azure PowerShell med Resource Manager](powershell-azure-resource-manager.md).
* Mer information om Azure CLI-kommandon för att hantera din prenumeration, se [med hjälp av Azure CLI med Resource Manager](xplat-cli-azure-resource-manager.md).
* Mer information om Företagsportalen funktioner för att hantera din prenumeration, se [med Azure-portalen för att hantera resurser](resource-group-portal.md).
* Läs om hur du kopplar logiskt till resurser i [med taggar för att organisera dina resurser](resource-group-using-tags.md).
