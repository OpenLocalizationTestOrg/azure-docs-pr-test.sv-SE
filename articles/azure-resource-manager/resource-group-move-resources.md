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
ms.date: 10/05/2017
ms.author: tomfitz
ms.openlocfilehash: 7d500d20dcce3e472e3e1e15b9ce307874caf22a
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/22/2018
---
# <a name="move-resources-to-new-resource-group-or-subscription"></a>Flytta resurser till en ny resursgrupp eller prenumeration

Den här artikeln visar hur du flyttar resurser till en ny prenumeration eller en ny resursgrupp med samma prenumeration. Du kan använda portalen, PowerShell, Azure CLI eller REST API för att flytta resursen. Flytta åtgärder i den här artikeln kan du utan hjälp från Azure-supporten.

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
  (Get-AzureRmSubscription -SubscriptionName <your-source-subscription>).TenantId
  (Get-AzureRmSubscription -SubscriptionName <your-destination-subscription>).TenantId
  ```

  Om du använder Azure CLI använder du:

  ```azurecli-interactive
  az account show --subscription <your-source-subscription> --query tenantId
  az account show --subscription <your-destination-subscription> --query tenantId
  ```

  Om klient-ID: N för käll- och -prenumerationer inte är samma, måste du kontakta [stöder](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) att flytta resurser till en ny klient.

2. Tjänsten måste göra det möjligt att flytta resurser. Den här artikeln innehåller tjänster som kan flytta resurser och tjänster som inte aktiverar flytta resurser.
3. Målprenumerationen måste vara registrerad för resursprovidern för den resurs som flyttas. Om inte, du får ett felmeddelande om att den **prenumerationen har inte registrerats för en resurstyp**. Du kan stöta på detta problem när en resurs flyttas till en ny prenumeration, men prenumerationen aldrig har använts med den resurstypen.

  Använd följande kommandon för att hämta registreringsstatus för PowerShell:

  ```powershell
  Set-AzureRmContext -Subscription <destination-subscription-name-or-id>
  Get-AzureRmResourceProvider -ListAvailable | Select-Object ProviderNamespace, RegistrationState
  ```

  Om du vill registrera en resursleverantör, använder du:

  ```powershell
  Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Batch
  ```

  För Azure CLI, använder du följande kommandon för att hämta registreringsstatus:

  ```azurecli-interactive
  az account set -s <destination-subscription-name-or-id>
  az provider list --query "[].{Provider:namespace, Status:registrationState}" --out table
  ```

  Om du vill registrera en resursleverantör, använder du:

  ```azurecli-interactive
  az provider register --namespace Microsoft.Batch
  ```

## <a name="when-to-call-support"></a>När anropet stöd

Du kan flytta de flesta resurser via självbetjäning åtgärder visas i den här artikeln. Använd åtgärderna självbetjäning till:

* Flytta resurshanterarens resurser.
* Flytta klassiska resurser enligt den [klassisk distribution begränsningar](#classic-deployment-limitations).

Kontakta [stöder](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) när du behöver:

* Flytta dina resurser till en ny Azure-konto (och Azure Active Directory-klient).
* Flytta klassiska resurser men har problem med begränsningar.

## <a name="services-that-enable-move"></a>Tjänster som gör att flytta

Tjänster som gör att du flyttar till en ny resursgrupp och en prenumeration är:

* API Management
* Apptjänst-appar (webbprogram) - finns [Apptjänst begränsningar](#app-service-limitations)
* Application Insights
* Automation
* Azure Cosmos DB
* Batch
* Bing-kartor
* CDN
* Molntjänster - Se [klassisk distribution begränsningar](#classic-deployment-limitations)
* Cognitive Services
* Content Moderator
* Data Catalog
* Data Factory
* Data Lake Analytics
* Data Lake Store
* DNS
* Händelsehubbar
* HDInsight-kluster - finns [HDInsight begränsningar](#hdinsight-limitations)
* IoT-hubbar
* Key Vault
* Belastningsutjämning
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
* Search
* Serverhantering
* Service Bus
* Service Fabric
* Lagring
* Storage (klassisk) - finns [klassisk distribution begränsningar](#classic-deployment-limitations)
* Stream Analytics - Stream Analytics-jobb inte kan flyttas när du kör i läget.
* SQL Database-server - databasen och servern måste finnas i samma resursgrupp. När du flyttar en SQLServer, flyttas även alla databaser.
* Traffic Manager
* Det går inte att flytta virtuella datorer – virtuella datorer med hanterade diskar. Se [begränsningar för virtuella datorer](#virtual-machines-limitations)
* Virtuella datorer (klassisk) - finns [klassisk distribution begränsningar](#classic-deployment-limitations)
* Skaluppsättningar för virtuell dator - finns [begränsningar för virtuella datorer](#virtual-machines-limitations)
* Virtuella nätverk - finns [begränsningar för virtuella nätverk](#virtual-networks-limitations)
* VPN-gateway

## <a name="services-that-do-not-enable-move"></a>Tjänster som inte gör flytta

De tjänster som för närvarande inte aktiverar flytta en resurs är:

* AD Domain Services
* AD-Hybrid-tjänsten för hälsotillstånd
* Application Gateway
* BizTalk Services
* Container Service
* Express Route
* DevTest Labs - flyttar till en ny resursgrupp i samma prenumeration har aktiverats men flytta mellan prenumeration har inte aktiverats.
* Dynamics LCS
* Hanterade program
* Hanterade diskar - Se [begränsningar för virtuella datorer](#virtual-machines-limitations)
* Recovery Services-ventilen - också vill inte flytta beräknings-, nätverks- och resurser som är associerade med Recovery Services-valvet finns [återställningstjänster begränsningar](#recovery-services-limitations).
* Säkerhet
* StorSimple Device Manager
* Virtuella nätverk (klassiskt) - finns [klassisk distribution begränsningar](#classic-deployment-limitations)

## <a name="virtual-machines-limitations"></a>Begränsningar för virtuella datorer

Hanterade diskar stöder inte flytta. Den här begränsningen innebär att flera relaterade resurser inte kan flyttas för. Du kan inte flytta:

* Hanterade diskar
* Virtuella datorer med hanterade diskar
* Bilder som har skapats från hanterade diskar
* Ögonblicksbilder som skapats från hanterade diskar
* Tillgänglighetsuppsättningar med virtuella datorer med hanterade diskar

Virtuella datorer som skapats från Marketplace resurser kan inte flyttas mellan prenumerationer. Ta bort etableringen av den virtuella datorn i den aktuella prenumerationen och distribuera igen i den nya prenumerationen.

Virtuella datorer med certifikat som lagras i Key Vault kan flyttas till en ny resursgrupp med samma prenumeration, men inte alla prenumerationer.

## <a name="virtual-networks-limitations"></a>Begränsningar för virtuella nätverk

Om du vill flytta peered virtuella nätverk måste du först inaktivera peering virtuellt nätverk. När inaktiverat, kan du flytta det virtuella nätverket. Återaktivera efter överflyttningen, virtuella nätverk peering.

Du kan inte flytta ett virtuellt nätverk till en annan prenumeration om det virtuella nätverket innehåller ett undernät med resursnavigeringslänkar. Om en resurs för Redis-Cache har distribuerats i ett undernät har som undernät en resurslänk för navigering.

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

## <a name="recovery-services-limitations"></a>Recovery Services-begränsningar

Flytta inte har aktiverats för lagring, nätverk, eller beräkningsresurser som används för att ställa in katastrofåterställning med Azure Site Recovery.

Anta att du har ställt in replikering av din lokala datorer till ett lagringskonto (Storage1) och vill att den skydda datorn att starta efter en redundansväxling till Azure som en virtuell dator (VM1) ansluten till ett virtuellt nätverk (Network1). Du kan inte flytta resurserna Azure - Storage1 VM1 och Network1 - över resursgrupper inom samma prenumeration eller alla prenumerationer.

Att flytta en virtuell dator har registrerats i **Azure backup** mellan resursgrupper:
 1. Tillfälligt stoppa säkerhetskopiering och behåller säkerhetskopierade data
 2. Flytta den virtuella datorn till målresursgruppen
 3. Skydda den på nytt under samma/nya valvet användare kan återställa från tillgängliga återställningspunkter som skapats före flyttningen.
Om användaren flyttar den virtuella datorn säkerhetskopierade alla prenumerationer, desamma steg 1 och 2. Användaren behöver skydda den virtuella datorn under ett nytt valv finns / i målprenumerationen i steg 3. Recovery Services valvet valvautentiseringsuppgifter support mellan säkerhetskopieringar för prenumerationen.

## <a name="hdinsight-limitations"></a>HDInsight-begränsningar

Du kan flytta HDInsight-kluster till en ny prenumeration eller resursgrupp. Men kan inte du flytta alla prenumerationer som nätverksresurser som är kopplad till HDInsight-klustret (till exempel virtuella nätverk, nätverkskort eller belastningsutjämning). Dessutom kan flytta du inte till en ny resursgrupp ett nätverkskort som är kopplad till en virtuell dator för klustret.

När du flyttar ett HDInsight-kluster till en ny prenumeration kan du först flytta andra resurser (till exempel storage-konto). Flytta sedan HDInsight-kluster av sig själv.

## <a name="search-limitations"></a>Sök-begränsningar

Du kan inte flytta flera Sök efter resurser placeras i olika regioner på samma gång.
I sådana fall behöver du flytta dem separat.

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

Flytta befintliga resurser till en annan resursgrupp eller prenumeration genom att använda den [flytta AzureRmResource](/powershell/module/azurerm.resources/move-azurermresource) kommando. I följande exempel visas hur du flyttar flera resurser till en ny resursgrupp.

```powershell
$webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
$plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId
```

Om du vill flytta till en ny prenumeration, innehåller ett värde för den `DestinationSubscriptionId` parameter.

## <a name="use-azure-cli"></a>Använda Azure CLI

Flytta befintliga resurser till en annan resursgrupp eller prenumeration genom att använda den [az resursflyttningen](/cli/azure/resource?view=azure-cli-latest#az_resource_move) kommando. Ange resurs-ID för resurserna för att flytta. I följande exempel visas hur du flyttar flera resurser till en ny resursgrupp. I den `--ids` parameter, ange en blankstegsavgränsad lista över resurs-ID att flytta.

```azurecli
webapp=$(az resource show -g OldRG -n ExampleSite --resource-type "Microsoft.Web/sites" --query id --output tsv)
plan=$(az resource show -g OldRG -n ExamplePlan --resource-type "Microsoft.Web/serverfarms" --query id --output tsv)
az resource move --destination-group newgroup --ids $webapp $plan
```

Om du vill flytta till en ny prenumeration, ange den `--destination-subscription-id` parameter.

## <a name="use-rest-api"></a>Använd REST-API

För att flytta befintliga resurser till en annan resursgrupp eller prenumeration, kör du:

```HTTP
POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version}
```

I begärandetexten anger du målresursgruppen och resurser för att flytta. Mer information om REST-flyttåtgärden finns [flyttar resurser](/rest/api/resources/Resources/MoveResources).

## <a name="next-steps"></a>Nästa steg

* Mer information om PowerShell-cmdletar för att hantera din prenumeration, se [med hjälp av Azure PowerShell med Resource Manager](powershell-azure-resource-manager.md).
* Mer information om Azure CLI-kommandon för att hantera din prenumeration, se [med hjälp av Azure CLI med Resource Manager](xplat-cli-azure-resource-manager.md).
* Mer information om Företagsportalen funktioner för att hantera din prenumeration, se [med Azure-portalen för att hantera resurser](resource-group-portal.md).
* Läs om hur du kopplar logiskt till resurser i [med taggar för att organisera dina resurser](resource-group-using-tags.md).
