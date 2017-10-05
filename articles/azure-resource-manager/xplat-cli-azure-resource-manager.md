---
title: Hantera resurser med Azure CLI | Microsoft Docs
description: "Använda Azure-kommandoradsgränssnittet (CLI) för att hantera Azure-resurser och grupper"
editor: 
manager: timlt
documentationcenter: 
author: tfitzmac
services: azure-resource-manager
ms.assetid: bb0af466-4f65-4559-ac3a-43985fa096ff
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: tomfitz
ms.openlocfilehash: 3ad4e68b90979fd7f9d3ddf5278e65e19cb07152
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>Använda Azure CLI för att hantera Azure-resurser och resursgrupper
> [!div class="op_single_selector"]
> * [Portal](resource-group-portal.md) 
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [REST-API](resource-manager-rest-api.md)
> 
> 

Azure-kommandoradsgränssnittet (Azure CLI) är en av flera verktyg som du kan använda för att distribuera och hantera resurser med Resource Manager. Den här artikeln innehåller vanliga sätt att hantera Azure-resurser och resursgrupper med hjälp av Azure CLI i Resource Manager-läge. Information om hur du använder CLI för att distribuera resurser finns [distribuera resurser med Resource Manager-mallar och Azure CLI](resource-group-template-deploy-cli.md). Information om Azure-resurser och Resource Manager finns i [översikt över Azure Resource Manager](resource-group-overview.md).

> [!NOTE]
> Om du vill hantera Azure-resurser med Azure CLI, behöver du [installerar Azure CLI](../cli-install-nodejs.md), och [logga in på Azure](../xplat-cli-connect.md) med hjälp av den `azure login` kommando. Kontrollera att CLI finns i Resource Manager-läget (kör `azure config mode arm`). Om du har gjort detta kan är du redo att sätta igång.
> 
> 

## <a name="get-resource-groups-and-resources"></a>Hämta resursgrupper och resurser
### <a name="resource-groups"></a>Resursgrupper
Kör kommandot för att hämta en lista över alla resursgrupper i din prenumeration och deras platser.

    azure group list


### <a name="resources"></a>Resurser
 Att lista alla resurser i en grupp, t.ex ett med namnet *testRG*, använder du följande kommando:

    azure resource list testRG

Så här visar du en enskild resurs i gruppen, t.ex. en virtuell dator med namnet *MyUbuntuVM*, använder du ett kommando som följande:

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Observera den **Microsoft.Compute/virtualMachines** parameter. Den här parametern anger vilken typ av resurs som du begär information.

> [!NOTE]
> När du använder den **azure-resurs** kommandon än den **lista** kommando, måste du ange API-versionen av resursen med det **-o** parameter. Om du är osäker på om API-versionen, se mallfilen och hitta fältet apiVersion för resursen. Mer information om API-versioner i Resource Manager finns i [resursproviders och typer](resource-manager-supported-services.md).
> 
> 

När du visar information på en resurs kan det vara bra att använda den `--json` parameter. Denna parameter gör utdata mer lättläst eftersom vissa värden kapslade strukturer eller samlingar. Exemplet nedan visar returnerar resultatet av den **visa** kommandot som JSON-dokument.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

> [!NOTE]
> Om du vill spara JSON-data till filen med hjälp av den &gt; tecken för att dirigera utdata till en fil. Exempel:
> 
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`
> 
> 

### <a name="tags"></a>Taggar
[!INCLUDE [resource-manager-tag-resources-cli](../../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Hantera resurser
Lägg till en resurs, till exempel ett lagringskonto i en resursgrupp, kör du ett kommando som liknar:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"

Förutom att ange API-versionen av resursen med det **-o** parameter, Använd den **-p** parametern för att skicka en JSON-formaterad sträng med alla obligatoriska eller ytterligare egenskaper.

Om du vill ta bort en befintlig resurs, till exempel en virtuell datorresurs, använder du ett kommando som liknar följande:

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Flytta befintliga resurser till en annan resursgrupp eller prenumeration genom att använda den **azure-resurs flytta** kommando. I följande exempel visas hur du flyttar ett Redis-Cache till en ny resursgrupp. I den **-i** parameter, ange en kommaavgränsad lista med resurs-id är för att flytta.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-to-resources"></a>Styr åtkomsten till resurser
Du kan använda Azure CLI för att skapa och hantera principer för att styra åtkomsten till Azure-resurser. Bakgrund om principdefinitioner och tilldelning av principer till resurser, se [använda för att hantera resurser och åtkomstkontroll](resource-manager-policy.md).

Till exempel definiera följande princip för att neka alla begäranden om platsen inte är västra USA eller norra centrala USA och spara den på principen definition filen policy.json:

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

Kör sedan den **principdefinitionen skapa** kommando:

    azure policy definition create MyPolicy -p c:\temp\policy.json

Det här kommandot visar utdata som liknar följande:

    + Skapar principdefinitionen MyPolicy data: Principnamn: MyPolicy data: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    data: PolicyType: anpassade data: DisplayName: Odefinierad data: beskrivning: Odefinierad data: PolicyRule: fältet = plats, i = [westus northcentralus], gälla = neka

 Om du vill tilldela en princip för område som du vill använda den **PolicyDefinitionId** returneras från det föregående kommandot. I följande exempel detta scope prenumerationen, men du kan ange omfång för resursgrupper eller enskilda resurser:

    Azure principtilldelningens skapa MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /

Du kan hämta, ändra eller ta bort principdefinitioner genom att använda den **princip definition visa**, **definition principuppsättningen**, och **principdefinitionen ta bort** kommandon.

På samma sätt kan du kan hämta, ändra eller ta bort principtilldelningar med den **princip tilldelningen visa**, **tilldelning principuppsättningen**, och **principtilldelning ta bort** kommandon.

## <a name="export-a-resource-group-as-a-template"></a>Exportera en resursgrupp som en mall
Du kan visa Resource Manager-mallen för resursgruppen för en befintlig resursgrupp. Exportera mallen har två fördelar:

1. Du kan enkelt automatisera framtida distributioner av lösningen eftersom infrastrukturen som definieras i mallen.
2. Du kan bekanta dig med mallens syntax genom att titta på JSON som representerar din lösning.

Med hjälp av Azure CLI, kan du antingen exportera en mall som representerar det aktuella tillståndet för resursgruppen eller hämta mallen som användes för en viss distribution.

* **Exportera mallen för en resursgrupp** -det här är användbart när du har gjort ändringar i en resursgrupp och behöver hämta JSON-representation av det aktuella tillståndet. Genererade mallen innehåller dock endast ett minimalt antal parametrar och inga variabler. De flesta av värdena i mallen är hårdkodat. Innan du distribuerar mallen genererade, kanske du vill konvertera flera värden till parametrar, så du kan anpassa distributionen för olika miljöer.
  
    Om du vill exportera mallen för en resursgrupp till en lokal katalog, kör den `azure group export` som visas i följande exempel. (I stället använda en lokal katalog som är lämpliga för din operativsystemmiljö.)
  
        azure group export testRG ~/azure/templates/
* **Hämta mall för en viss distribution** – det här är användbart när du vill visa den faktiska mall som användes för att distribuera resurser. Mallen innehåller alla parametrar och variabler som definieras för den ursprungliga distributionen. Om någon i organisationen har gjort ändringar i resursgruppen utanför definition i mallen representerar inte den här mallen det aktuella tillståndet för resursgruppen.
  
    Om du vill hämta en mall för en viss distribution till en lokal katalog, kör den `azure group deployment template download` kommando. Exempel:
  
        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/

> [!NOTE]
> Exportera mallen är en förhandsversion och inte alla typer av resurser stöd för närvarande inte exportera en mall. När du försöker exportera en mall kan du se ett felmeddelande om att vissa resurser inte exporterades. Om det behövs, manuellt definiera resurserna i mallen när du har hämtat den.
> 
> 

## <a name="next-steps"></a>Nästa steg
* För att få information om distributionsåtgärder och felsöka distribution med Azure CLI, se [visa distributionsåtgärder](resource-manager-deployment-operations.md).
* Om du vill använda CLI för att ställa in ett program eller skript för att komma åt resurser, se [använder Azure CLI för att skapa ett huvudnamn för tjänsten att komma åt resurser](resource-group-authenticate-service-principal-cli.md).
* Vägledning för hur företag kan använda resurshanteraren för att effektivt hantera prenumerationer finns i [Azure enterprise scaffold - förebyggande prenumerationsåtgärder](resource-manager-subscription-governance.md).

