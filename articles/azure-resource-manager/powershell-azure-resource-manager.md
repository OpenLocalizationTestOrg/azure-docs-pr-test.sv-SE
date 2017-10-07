---
title: "aaaManage Azure lösningar med PowerShell | Microsoft Docs"
description: "Använda Azure PowerShell och Resource Manager toomanage dina resurser."
services: azure-resource-manager
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: b33b7303-3330-4af8-8329-c80ac7e9bc7f
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: tomfitz
ms.openlocfilehash: 47a91af8d7eb59585bcfd43571ce76b90a0d7971
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-resources-with-azure-powershell-and-resource-manager"></a>Hantera resurser med Azure PowerShell och Resource Manager
> [!div class="op_single_selector"]
> * [Portal](resource-group-portal.md)
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [REST-API](resource-manager-rest-api.md)
>
>

I den här artikeln får du lära dig hur toomanage dina lösningar med Azure PowerShell och Azure Resource Manager. Om du inte är bekant med Resource Manager finns [översikt över Resource Manager](resource-group-overview.md). Det här avsnittet fokuserar på hanteringsuppgifter. Du kommer att:

1. Skapa en resursgrupp
2. Lägg till en resurs toohello resursgrupp
3. Lägg till en tagg toohello resurs
4. Fråga resurser baserat på namn eller värden
5. Använda och ta bort ett lås på hello resurs
6. Ta bort en resursgrupp

Den här artikeln visar inte hur toodeploy en Resource Manager mallen tooyour prenumeration. Den här informationen finns [distribuera resurser med Resource Manager-mallar och Azure PowerShell](resource-group-template-deploy.md).

## <a name="get-started-with-azure-powershell"></a>Kom igång med Azure PowerShell

Om du inte har installerat Azure PowerShell, se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

Överväg att installera hello senaste versionen om du har installerat Azure PowerShell i hello tidigare men inte har uppdaterat den nyligen. Du kan uppdatera hello version via hello samma metod som du använde tooinstall den. Om du använde hello installationsprogram för webbplattform, starta den igen och leta efter en uppdatering.

toocheck din version av hello Azure-resurser modulen, använder hello följande cmdlet:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Det här avsnittet har uppdaterats för version 3.3.0. Om du har en tidigare version överensstämmer hello steg som visas i det här avsnittet inte med din upplevelse. Dokumentation om hello-cmdlets i den här versionen finns [AzureRM.Resources modulen](/powershell/module/azurerm.resources).

## <a name="log-in-tooyour-azure-account"></a>Logga in tooyour Azure-konto
Du måste logga in tooyour konto innan du arbetar med din lösning.

toolog i tooyour Azure-konto, använder hello **Login-AzureRmAccount** cmdlet.

```powershell
Login-AzureRmAccount
```

hello cmdlet efterfrågar hello inloggningsuppgifterna för ditt Azure-konto. När du loggar in hämtar den inställningarna för ditt konto, så att de är tillgängliga tooAzure PowerShell.

hello cmdlet returnerar information om ditt konto och hello prenumeration toouse för hello uppgifter.

```powershell
Environment           : AzureCloud
Account               : example@contoso.com
TenantId              : {guid}
SubscriptionId        : {guid}
SubscriptionName      : Example Subscription One
CurrentStorageAccount :

```

Om du har mer än en prenumeration kan växla du tooa annan prenumeration. Först ska vi se alla hello prenumerationer för ditt konto.

```powershell
Get-AzureRmSubscription
```

Den returnerar aktiverade och inaktiverade prenumerationer.

```powershell
SubscriptionName : Example Subscription One
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Two
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Enabled

SubscriptionName : Example Subscription Three
SubscriptionId   : {guid}
TenantId         : {guid}
State            : Disabled
```

tooswitch tooa annan prenumeration, ge hello prenumerationsnamn hello **Set-AzureRmContext** cmdlet.

```powershell
Set-AzureRmContext -SubscriptionName "Example Subscription Two"
```

## <a name="create-a-resource-group"></a>Skapa en resursgrupp
Innan du distribuerar någon resurser tooyour prenumeration, måste du skapa en resursgrupp som innehåller hello resurser.

toocreate en resursgrupp, använda hello **New-AzureRmResourceGroup** cmdlet. hello kommando använder hello **namn** parametern toospecify ett namn för resursgruppen hello och hello **plats** parametern toospecify dess plats.

```powershell
New-AzureRmResourceGroup -Name TestRG1 -Location "South Central US"
```

hello utdata har hello följande format:

```powershell
ResourceGroupName : TestRG1
Location          : southcentralus
ProvisioningState : Succeeded
Tags              :
ResourceId        : /subscriptions/{guid}/resourceGroups/TestRG1
```

Om du behöver tooretrieve hello resursgruppen senare, Använd hello följande cmdlet:

```powershell
Get-AzureRmResourceGroup -ResourceGroupName TestRG1
```

tooget alla Hej resursgrupper i din prenumeration, ange inte ett namn:

```powershell
Get-AzureRmResourceGroup
```

## <a name="add-resources-tooa-resource-group"></a>Lägg till resurser tooa resursgrupp
tooadd en resurs toohello resursgrupp som du kan använda hello **ny AzureRmResource** cmdlet eller en cmdlet som är specifika toohello typ av resurs som du skapar (t.ex. **New-AzureRmStorageAccount**). Det kan vara enklare toouse en cmdlet som är specifika tooa resurstyp eftersom den innehåller parametrar för hello egenskaper som krävs för hello ny resurs. toouse **ny AzureRmResource**, måste du känna till alla hello egenskaper tooset utan som efterfrågas.

Lägga till en resurs via cmdlets kan vara förvirrande för framtida hello ny resurs finns inte i en Resource Manager-mall. Microsoft rekommenderar att definiera hello infrastrukturen för Azure lösningen i en Resource Manager-mall. Mallar kan du tooreliably och upprepade gånger distribuera din lösning. Du skapar ett lagringskonto med en PowerShell-cmdlet för det här avsnittet, men senare du generera en mall från resursgruppen.

hello följande cmdlet skapar ett lagringskonto. Ange ett unikt namn för hello storage-konto istället för att använda hello-namnet som visas i hello exempel. hello namn måste vara mellan 3 och 24 tecken långt och innehålla endast siffror och gemener. Om du använder hello-namnet som visas i hello exempel får ett felmeddelande eftersom namnet redan används.

```powershell
New-AzureRmStorageAccount -ResourceGroupName TestRG1 -AccountName mystoragename -Type "Standard_LRS" -Location "South Central US"
```

Om du behöver tooretrieve resursen senare, Använd hello följande cmdlet:

```powershell
Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1
```

## <a name="add-a-tag"></a>Lägga till en tagg

Taggar kan du tooorganize dina resurser enligt toodifferent egenskaper. Du kan till exempel har flera resurser i olika resursgrupper som tillhör toohello samma avdelning. Du kan tillämpa en avdelning taggen och värdet toothose resurser toomark dem som tillhör toohello samma kategori. Eller så kan du markera om en resurs används i en produktionsmiljö eller testmiljö. Du använder taggar tooonly en resurs i det här avsnittet, men i din miljö är det mest sannolika klokt tooapply taggar tooall dina resurser.

följande cmdlet hello gäller två taggar tooyour storage-konto:

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
 ```

Taggar uppdateras som ett enskilt objekt. tooadd en tagg tooa resurs som redan omfattar taggar, först hämta hello befintliga taggar. Lägg till hello ny tagg toohello objekt som innehåller hello befintliga taggar och tillämpa alla hello taggar toohello resurs.

```powershell
$tags = (Get-AzureRmResource -ResourceName mystoragename -ResourceGroupName TestRG1).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName mystoragename -ResourceGroupName TestRG1 -ResourceType Microsoft.Storage/storageAccounts
```

## <a name="search-for-resources"></a>Sök efter resurser

Använd hello **hitta AzureRmResource** cmdlet tooretrieve resurser för olika sökvillkor.

* tooget en resurs med namnet, ange hello **ResourceNameContains** parameter:

  ```powershell
  Find-AzureRmResource -ResourceNameContains mystoragename
  ```

* tooget alla hello resurser i en resursgrupp, ange hello **ResourceGroupNameContains** parameter:

  ```powershell
  Find-AzureRmResource -ResourceGroupNameContains TestRG1
  ```

* tooget alla hello resurser med taggnamn och värde, ange hello **TagName** och **TagValue** parametrar:

  ```powershell
  Find-AzureRmResource -TagName Dept -TagValue IT
  ```

* tooall hello resurser med en viss resurstyp innehåller hello **ResourceType** parameter:

  ```powershell
  Find-AzureRmResource -ResourceType Microsoft.Storage/storageAccounts
  ```

## <a name="lock-a-resource"></a>Låsa en resurs

När du behöver toomake att en kritisk resurs tas inte bort av misstag eller ändras kan tillämpa en toohello lock-resurs. Du kan ange antingen en **CanNotDelete** eller **ReadOnly**.

toocreate eller ta bort lås för hantering, måste du ha tillgång för`Microsoft.Authorization/*` eller `Microsoft.Authorization/locks/*` åtgärder. I hello inbyggda roller beviljas endast ägare och administratör för användaråtkomst dessa åtgärder.

tooapply ett lås Använd hello följande cmdlet:

```powershell
New-AzureRmResourceLock -LockLevel CanNotDelete -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

hello kan inte låst resurs i föregående exempel hello tas bort förrän hello låset tas bort. Använd tooremove ett lås:

```powershell
Remove-AzureRmResourceLock -LockName LockStorage -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
```

Mer information om inställningen Lås finns [låsa resurser med Azure Resource Manager](resource-group-lock-resources.md).

## <a name="remove-resources-or-resource-group"></a>Ta bort resurser eller resursgrupp
Du kan ta bort en resurs eller en resursgrupp. När du tar bort en resursgrupp kan du också ta bort alla hello resurser i resursgruppen.

* toodelete en resurs från hello resursgrupp, Använd hello **ta bort AzureRmResource** cmdlet. Denna cmdlet tar bort hello resurs, men tar inte bort hello resursgruppen.

  ```powershell
  Remove-AzureRmResource -ResourceName mystoragename -ResourceType Microsoft.Storage/storageAccounts -ResourceGroupName TestRG1
  ```

* toodelete en resursgrupp och alla dess resurser använder hello **Remove-AzureRmResourceGroup** cmdlet.

  ```powershell
  Remove-AzureRmResourceGroup -Name TestRG1
  ```

För båda cmdlets uppmanas tooconfirm du vill tooremove hello resurs eller resursgrupp. Om hello har tas bort hello resurs eller resursgrupp, returnerar **SANT**.

## <a name="run-resource-manager-scripts-with-azure-automation"></a>Kör skript för Resource Manager med Azure Automation

Det här avsnittet beskrivs hur du tooperform grundläggande åtgärder på resurser med Azure PowerShell. Mer avancerade scenarier med hantering av du vanligtvis vill toocreate ett skript och återanvända skriptet efter behov eller enligt ett schema. [Azure Automation](../automation/automation-intro.md) gör det möjligt för dig tooautomate vanliga skript som hanterar dina Azure-lösningar.

hello visar följande avsnitt hur toouse Azure Automation Resource Manager och PowerShell tooeffectively utföra administrativa uppgifter:

- Information om hur du skapar en runbook finns [min första PowerShell-runbook](../automation/automation-first-runbook-textual-powershell.md).
- Information om hur du arbetar med gallerier av skript finns [Azure Automation Runbook- och stänga](../automation/automation-runbook-gallery.md).
- Runbooks som kan starta och stoppa virtuella datorer, se [Azure Automation-scenario: använda JSON-formaterad taggar toocreate ett schema för Virtuella Azure-start och stopp](../automation/automation-scenario-start-stop-vm-wjson-tags.md).
- Runbooks som kan starta och stoppa virtuella datorer låg belastning på nätverket, se [Starta/stoppa virtuella datorer vid låg belastning på nätverket lösning i Automation](../automation/automation-solution-vm-management.md).

## <a name="next-steps"></a>Nästa steg
* toolearn om hur du skapar Resource Manager-mallar finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).
* toolearn om hur du distribuerar mallar, se [distribuera ett program med Azure Resource Manager-mall](resource-group-template-deploy.md).
* Du kan flytta befintliga resurser tooa ny resursgrupp. Exempel finns i [flytta resurser tooNew resursgrupp eller prenumeration](resource-group-move-resources.md).
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).

