---
title: aaaManage resurser med hello Azure CLI | Microsoft Docs
description: "Använd hello Azure-kommandoradsgränssnittet (CLI) toomanage Azure resurser och grupper"
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
ms.openlocfilehash: 3df70e123d14d3bbf2648c71970bac1db4afc025
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-azure-cli-toomanage-azure-resources-and-resource-groups"></a>Använd hello Azure CLI toomanage Azure resurser och resursgrupper
> [!div class="op_single_selector"]
> * [Portal](resource-group-portal.md) 
> * [Azure CLI](xplat-cli-azure-resource-manager.md)
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [REST-API](resource-manager-rest-api.md)
> 
> 

hello Azure-kommandoradsgränssnittet (Azure CLI) är en av flera verktyg du kan använda toodeploy och hantera resurser med Resource Manager. Den här artikeln innehåller vanliga sätt toomanage Azure resurser och resursgrupper med hjälp av hello Azure CLI i Resource Manager-läge. Information om hur du använder hello CLI toodeploy resurser finns [distribuera resurser med Resource Manager-mallar och Azure CLI](resource-group-template-deploy-cli.md). Information om Azure-resurser och Resource Manager finns hello [översikt över Azure Resource Manager](resource-group-overview.md).

> [!NOTE]
> toomanage Azure resurser med hello Azure CLI, du behöver för[installera hello Azure CLI](../cli-install-nodejs.md), och [logga in tooAzure](../xplat-cli-connect.md) med hjälp av hello `azure login` kommando. Se till att hello CLI finns i Resource Manager-läget (kör `azure config mode arm`). Om du har gjort detta kan är du klar toogo.
> 
> 

## <a name="get-resource-groups-and-resources"></a>Hämta resursgrupper och resurser
### <a name="resource-groups"></a>Resursgrupper
tooget en lista över alla resursgrupper i din prenumeration och deras platser kör det här kommandot.

    azure group list


### <a name="resources"></a>Resurser
 toolist alla resurser i en grupp, till exempel en med namnet *testRG*, använda hello följande kommando:

    azure resource list testRG

tooview en enskild resurs hello-gruppen, till exempel en virtuell dator med namnet *MyUbuntuVM*, använder du ett kommando som hello nedan:

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Meddelande hello **Microsoft.Compute/virtualMachines** parameter. Den här parametern anger hello hello-resurs som du begär information.

> [!NOTE]
> När du använder hello **azure-resurs** kommandon än hello **lista** kommandot måste du ange hello API-versionen av hello resursen med hello **-o** parameter. Om du är osäker på om hello API-version, se hello mallfilen och hitta hello apiVersion fältet för hello resurs. Mer information om API-versioner i Resource Manager finns i [resursproviders och typer](resource-manager-supported-services.md).
> 
> 

När du visar information på en resurs, är det ofta användbara toouse hello `--json` parameter. Denna parameter gör hello utdata mer lättläst eftersom vissa värden kapslade strukturer eller samlingar. hello det här exemplet returnerar hello resultaten av hello **visa** kommandot som JSON-dokument.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

> [!NOTE]
> Om du vill, spara hello JSON data toofile med hjälp av hello &gt; tooa för tecknet toodirect hello utdatafilen. Exempel:
> 
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`
> 
> 

### <a name="tags"></a>Taggar
[!INCLUDE [resource-manager-tag-resources-cli](../../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Hantera resurser
tooadd en resurs, till exempel en storage-konto tooa resursgrupp, köra ett kommando som liknar:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"

Dessutom toospecifying hello API-versionen av hello resurs med hello **-o** parametern, Använd hello **-p** parametern toopass en JSON-formaterad sträng med alla obligatoriska eller ytterligare egenskaper.

toodelete en befintlig resurs, till exempel en virtuell datorresurs kommandot hello följande:

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

toomove befintliga resurser tooanother resursgrupp eller prenumeration, använda hello **azure-resurs flytta** kommando. följande exempel visar hur hello toomove en Redis-Cache tooa ny resursgrupp. I hello **-i** parameter, ange en kommaavgränsad lista över hello resurs id toomove.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-tooresources"></a>Kontrollera åtkomst tooresources
Du kan använda hello Azure CLI toocreate och hantera principer toocontrol åtkomst tooAzure resurser. Information om principdefinitioner och tilldela principer tooresources finns [använda princip toomanage resurser och styr åtkomsten](resource-manager-policy.md).

Till exempel definiera hello följa principen toodeny alla begäranden om platsen inte är västra USA eller norra centrala USA och spara den toohello princip definition filen policy.json:

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

Kör sedan hello **principdefinitionen skapa** kommando:

    azure policy definition create MyPolicy -p c:\temp\policy.json

Det här kommandot visar utdata liknande toohello följande:

    + Skapar principdefinitionen MyPolicy data: Principnamn: MyPolicy data: PolicyDefinitionId: /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    data: PolicyType: anpassade data: DisplayName: Odefinierad data: beskrivning: Odefinierad data: PolicyRule: fältet = plats, i = [westus northcentralus], gälla = neka

 tooassign en princip definitionsområdet hello du vill använda hello **PolicyDefinitionId** returnerades från föregående hello-kommando. I följande exempel hello, detta scope hello prenumeration, men du kan definiera tooresource grupper eller enskilda resurser:

    Azure principtilldelningens skapa MyPolicyAssignment -p /subscriptions/###-###-###-###-###/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/###-###-###-###-### /

Du kan hämta, ändra eller ta bort principdefinitioner med hello **princip definition visa**, **definition principuppsättningen**, och **principdefinitionen ta bort** kommandon.

På samma sätt kan du kan hämta, ändra eller ta bort principtilldelningar med hello **princip tilldelningen visa**, **tilldelning principuppsättningen**, och **principtilldelning ta bort** kommandon .

## <a name="export-a-resource-group-as-a-template"></a>Exportera en resursgrupp som en mall
Du kan visa hello Resource Manager-mall för hello resursgrupp för en befintlig resursgrupp. Exporterar hello mallen har två fördelar:

1. Enkelt kan du automatisera framtida distributioner av hello lösning eftersom alla hello-infrastruktur är definierad i hello mallen.
2. Du kan bekanta dig med mallens syntax genom att titta på hello JSON som representerar din lösning.

Använder hello Azure CLI, kan du antingen exportera en mall som representerar hello aktuell status för din resursgrupp eller hämta hello-mallen som användes för en viss distribution.

* **Exportera hello mall för en resursgrupp** -det här är användbart när du har gjort ändringar tooa resursgrupp och måste tooretrieve hello JSON-representation av det aktuella tillståndet. Hello genererade mallen innehåller dock endast ett minimalt antal parametrar och inga variabler. De flesta hello värden i hello mallen är hårdkodat. Innan du distribuerar hello genereras mallen du tooconvert mer hello värden till parametrar så att du kan anpassa hello distribution för olika miljöer.
  
    tooexport hello mall för en resurs grupp tooa lokal katalog, kör hello `azure group export` som visas i följande exempel hello. (I stället använda en lokal katalog som är lämpliga för din operativsystemmiljö.)
  
        azure group export testRG ~/azure/templates/
* **Hämta hello mall för en viss distribution** – det här är användbart när du behöver tooview hello faktiska mall som har använt toodeploy resurser. hello mallen innehåller alla parametrar och variabler som definieras för hello ursprungliga distributionen. Om någon i din organisation har gjort ändringar toohello resursgruppen utanför hello definition i hello mallen representerar inte den här mallen hello hello resursgruppen aktuella tillstånd.
  
    toodownload hello mall som används för en viss distribution tooa lokal katalog, kör hello `azure group deployment template download` kommando. Exempel:
  
        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/

> [!NOTE]
> Exportera mallen är en förhandsversion och inte alla typer av resurser stöd för närvarande inte exportera en mall. Ett felmeddelande som anger att vissa resurser inte har exporterats kan visas vid försök tooexport en mall. Om det behövs, manuellt definiera resurserna i mallen när du har hämtat den.
> 
> 

## <a name="next-steps"></a>Nästa steg
* tooget information om distributionsåtgärder och felsöka distribution med hello Azure CLI, se [visa distributionsåtgärder](resource-manager-deployment-operations.md).
* Om du vill toouse hello CLI tooset upp ett program eller skript tooaccess resurser, se [Använd Azure CLI toocreate ett huvudnamn för tjänsten tooaccess resurser](resource-group-authenticate-service-principal-cli.md).
* Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).

