---
title: "aaaAzure översikt över Resource Manager | Microsoft Docs"
description: "Beskriver hur toouse Azure Resource Manager för distribution, hantering och åtkomstkontroll av resurser i Azure."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 76df7de1-1d3b-436e-9b44-e1b3766b3961
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/19/2017
ms.author: tomfitz
ms.openlocfilehash: a44fccd96d722c006224145d71cc44292255debf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-resource-manager-overview"></a>Översikt över Azure Resource Manager
hello infrastrukturen för ditt program består normalt av många komponenter – kanske en virtuell dator, storage-konto och virtuella nätverk eller en webbapp, databas, databasserver och 3 tjänster. Du ser inte de här komponenterna som separata entiteter, utan som relaterade delar av samma enhet som är beroende av varandra. Du vill toodeploy, hantera och övervaka dem som en grupp. Azure Resource Manager aktiverar toowork med hello resurser i din lösning som en grupp. Du kan distribuera, uppdatera eller ta bort alla hello resurser i lösningen i en enda, samordnad åtgärd. Du använder en mall för distributionen. Mallen kan användas i olika miljöer, till exempel för testning, mellanlagring och produktion. Resource Manager tillhandahåller säkerhets-, granskning och märkning funktioner toohelp hantera dina resurser efter distributionen. 

## <a name="terminology"></a>Terminologi
Om du är ny tooAzure Resource Manager, är det vissa termer som du inte kanske känner till.

* **resurs** – Ett hanterbart objekt som är tillgängligt via Azure. Exempel på vanliga resurser är virtuella datorer, lagringskonton, webbappar, databaser och virtuella nätverk, men det finns många fler.
* **resursgrupp** – En behållare som innehåller relaterade resurser för en Azure-lösning. hello resursgrupp kan innehålla alla hello resurser för hello lösning eller bara de resurser som du vill toomanage som en grupp. Du bestämma hur du vill att tooallocate resurser tooresource grupper baserat på vad som är hello bäst för din organisation. Mer information finns i [Resursgrupper](#resource-groups).
* **resursprovidern** -en tjänst som tillhandahåller hello resurser som du kan distribuera och hantera via Resource Manager. Varje resursprovider ger åtgärder för att arbeta med hello resurser som distribueras. Några vanliga resursproviders är Microsoft.Compute som tillhandahåller hello virtuell datorresurs, Microsoft.Storage som ger hello lagringsresurs konto och Microsoft.Web som tillhandahåller resurser relaterade tooweb appar. Mer information finns i [Resursproviders](#resource-providers).
* **Resource Manager-mall** -A JavaScript Object Notation (JSON)-fil som definierar en eller flera resurser toodeploy tooa resursgruppens namn. Den definierar även hello beroenden mellan hello distribuerade resurser. hello mallen kan vara används toodeploy hello resurser konsekvent och flera gånger. Mer information finns i [Malldistribution](#template-deployment).
* **deklarativa syntaxen** -Syntax som låter dig tillstånd ”här är vad jag avser toocreate” utan toowrite hello sekvens av programmering kommandon toocreate den. hello Resource Manager-mall är ett exempel på deklarativa syntaxen. I hello-filen kan du definiera hello egenskaper för hello infrastruktur toodeploy tooAzure. 

## <a name="hello-benefits-of-using-resource-manager"></a>hello fördelarna med att använda Resource Manager
Resource Manager har flera fördelar:

* Du kan distribuera, hantera och övervaka alla hello resurser för din lösning som en grupp i stället för att hantera resurserna separat.
* Du kan flera gånger distribuerar din lösning i hela hello utvecklingslivscykeln och vara säker på att dina resurser distribueras i ett konsekvent tillstånd.
* Du kan hantera infrastrukturen med hjälp av deklarativa mallar i stället för skript.
* Du kan definiera hello beroenden mellan resurser så att de distribueras i rätt ordning för hello.
* Du kan använda tjänster för åtkomstkontroll tooall i resursgruppen eftersom rollbaserad åtkomstkontroll (RBAC) är inbyggt i hello plattform.
* Du kan lägga till taggar tooresources toologically ordna alla hello resurser i din prenumeration.
* Du kan tydliggöra faktureringen för din organisation genom att visa kostnaderna för en grupp med resurser dela hello samma tagg.  

Resource Manager tillhandahåller ett nytt sätt toodeploy och hantera dina lösningar. Om du använde hello tidigare distributionsmodellen och vill toolearn om hello ändringarna, se [förstå Resource Manager distribution och klassisk distribution](resource-manager-deployment-model.md).

## <a name="consistent-management-layer"></a>Enhetligt hanteringslager
Resource Manager tillhandahåller ett enhetligt hanteringslager för hello uppgifter som du gör med hjälp av Azure PowerShell, Azure CLI, Azure-portalen, REST-API och utvecklingsverktyg. Alla hello verktyg använder en gemensam uppsättning åtgärder. Du kan använda hello verktyg som passar dig bäst och kan använda dem synonymt utan förvirring. 

hello hello följande bild visar hur alla hello verktyg interagera med samma Azure Resource Manager API. hello API skickar begäranden toohello Resource Manager-tjänsten, som autentiserar och auktoriserar hello begäranden. Hanteraren för filserverresurser dirigerar sedan hello begäranden toohello lämplig resursleverantörer.

![Modell för Resource Manager-begäranden](./media/resource-group-overview/consistent-management-layer.png)

## <a name="guidance"></a>Riktlinjer
hello hjälper följande rekommendationer dig att dra full nytta av Resource Manager när du arbetar med dina lösningar.

1. Definiera och distribuera din infrastruktur via hello deklarativa syntaxen i Resource Manager-mallar i stället för med tvingande kommandon.
2. Definiera alla distributions- och konfigurationssteg i hello mallen. Inga manuella steg ska behövas för att konfigurera lösningen.
3. Kör tvingande kommandon toomanage resurser, till exempel toostart eller stoppa en app eller dator.
4. Ordna resurser med hello samma livscykel i en resursgrupp. Använd taggar för all annan resursorganisation.

Rekommendationer om mallar finns i [Metodtips för att skapa Azure Resource Manager-mallar](resource-manager-template-best-practices.md).

Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).

## <a name="resource-groups"></a>Resursgrupper
Det finns några viktiga faktorer tooconsider när du definierar en resursgrupp:

1. Alla hello resurser i gruppen dela hello samma livscykel. Du distribuerar, uppdaterar och tar bort dem tillsammans. Om en resurs, till exempel en databasserver måste tooexist på en annan distributionscykel ska den vara i en annan resursgrupp.
2. En enskild resurs kan bara finnas i en resursgrupp.
3. Du kan lägga till eller ta bort en resurs tooa resursgrupp när som helst.
4. Du kan flytta en resurs från en grupp tooanother resursgrupp. Mer information finns i [flytta resurser toonew resursgrupp eller prenumeration](resource-group-move-resources.md).
5. En resursgrupp kan innehålla resurser som finns i olika regioner.
6. En resursgrupp kan vara används tooscope åtkomstkontroll för administrativa åtgärder.
7. En resurs kan interagera med resurser i andra resursgrupper. Interaktionen är vanliga när hello två resurser som är relaterade men inte delar hello samma livscykel (till exempel webbappar som ansluter tooa databas).

När du skapar en resursgrupp måste tooprovide en plats för resursgruppen. Du kanske undrar, "varför behöver en resursgrupp en plats? Och om hello resurser kan ha olika platser än hello resursgrupp, varför spelar hello resursgruppens plats roll alls ”? hello resursgruppen lagras metadata om hello resurser. När du anger en plats för hello resursgrupp kan anger du därför där den metadata lagras. Av kompatibilitetsskäl, kanske du måste tooensure som dina data lagras i ett visst område.

## <a name="resource-providers"></a>Resursproviders
Varje resursprovider tillhandahåller en uppsättning resurser och åtgärder för arbete i en Azure-tjänst. Till exempel om du vill toostore nycklar och hemligheter arbetar du med hello **Microsoft.KeyVault** resursprovidern. Den här resursprovidern erbjuder resurstypen **valv** för att skapa hello nyckelvalvet. 

hello-namnet för en resurstyp har formatet hello: **{resursprovidern} / {resurstypen}**. Hej nyckelvalv typ är till exempel **Microsoft.KeyVault/vaults**.

Innan du börjar med att distribuera dina resurser bör du få insikt i hello tillgängliga resursproviders. Att veta hello namnen på resursen providers och resurser kan du definiera resurser vill du toodeploy tooAzure. Du måste också tooknow hello giltiga platser och API-versioner för varje resurstyp av. Mer information finns i [Resursproviders och typer](resource-manager-supported-services.md).

## <a name="template-deployment"></a>Malldistribution
Med Resource Manager kan du skapa en mall (i JSON-format) som definierar hello-infrastrukturen och konfigurationen av din Azure-lösning. Genom att använda en mall kan du distribuera lösningen flera gånger under dess livscykel och vara säker på att dina resurser distribueras konsekvent. När du skapar en lösning från hello portal innehåller hello den automatiskt en Distributionsmall. Du har inte toocreate mallen från grunden eftersom du kan börja med hello mall för din lösning och anpassa den toomeet dina specifika behov. Du kan hämta en mall för en befintlig resursgrupp genom att exportera hello aktuell status för hello resursgrupp eller visa hello-mall som används för en viss distribution. Visa hello [exporterad mall](resource-manager-export-template.md) är ett bra sätt toolearn om hello mallens syntax.

toolearn om hello format hello mall och hur du skapar det, se [skapa din första Azure Resource Manager-mallen](resource-manager-create-first-template.md). tooview hello JSON-syntax för typer av resurser, se [definiera resurser i Azure Resource Manager-mallar](/azure/templates/).

Hanteraren för filserverresurser bearbetar hello mall som alla andra förfrågningar (se hello bild för [enhetligt hanteringslager](#consistent-management-layer)). Den Parsar hello mall och konverterar dess syntax till REST API-åtgärder för hello lämpliga resursprovidrar. Till exempel när hanteraren för filserverresurser tar emot en mall med hello följande resursdefinitionen:

```json
"resources": [
  {
    "apiVersion": "2016-01-01",
    "type": "Microsoft.Storage/storageAccounts",
    "name": "mystorageaccount",
    "location": "westus",
    "sku": {
      "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {
    }
  }
]
```

Den konverterar hello definition toohello följande REST API-åtgärd som skickas toohello Microsoft.Storage-resursleverantören:

```HTTP
PUT
https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/mystorageaccount?api-version=2016-01-01
REQUEST BODY
{
  "location": "westus",
  "properties": {
  }
  "sku": {
    "name": "Standard_LRS"
  },   
  "kind": "Storage"
}
```

Hur du definierar mallar och resursgrupper är helt upp tooyou och hur du vill att toomanage din lösning. Du kan till exempel distribuera programmet tre nivåer genom en enda mallen tooa enskild resursgrupp.

![mall med tre nivåer](./media/resource-group-overview/3-tier-template.png)

Men du har inte toodefine hela infrastrukturen i samma mall. Ofta gör det klokt toodivide dina distributionskrav i en uppsättning riktade mallar för specifika ändamål. Du kan enkelt återanvända dessa mallar för olika lösningar. toodeploy en lösning, skapa en Huvudmall som länkar alla mallar för hello krävs. hello följande bild visar hur toodeploy en tre nivå-lösning via en överordnad mall som innehåller tre kapslade mallar.

![mall med kapslad nivå](./media/resource-group-overview/nested-tiers-template.png)

Om du förutser din nivåer med separata livscykler kan du distribuera tre nivåerna tooseparate resursgrupperna. Meddelande hello resurser kan fortfarande vara länkade tooresources i andra resursgrupper.

![nivåmall](./media/resource-group-overview/tier-templates.png)

Fler förslag på hur du kan utforma dina mallar finns i [Metodtips för att utforma Azure Resource Manager-mallar](best-practices-resource-manager-design-templates.md). Mer information om kapslade mallar finns i [Använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).

Azure Resource Manager analyserar alla beroenden tooensure resurserna skapas i rätt ordning för hello. Om en resurs bygger på ett värde från en annan resurs (till exempel en virtuell dator som behöver ett lagringskonto för diskar) anger du ett beroende. Mer information finns i [Definiera beroenden i Azure Resource Manager-mallar](resource-group-define-dependencies.md).

Du kan också använda hello mallen för uppdateringar toohello infrastruktur. Du kan till exempel lägga till en resurs tooyour lösning och lägga till konfigurationsregler för hello resurser som redan har distribuerats. Om hello mallen anger att skapa en resurs, men att resursen finns redan, utför Azure Resource Manager en uppdatering i stället för att skapa en ny tillgång. Azure Resource Manager uppdateringar hello befintliga tillgången toohello samma tillstånd som den skulle ha som ny.  

Resource Manager tillhandahåller tillägg för scenarier när du behöver ytterligare åtgärder som att installera viss programvara som inte ingår i hello installationsprogrammet. Om du redan använder en konfigurationshanteringstjänst, t.ex. DSC, Chef eller Puppet, kan du fortsätta arbeta med den tjänsten genom att använda tillägg. Information om tillägg för virtuella datorer finns i [Om tillägg och funktioner för virtuella datorer](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Slutligen blir hello mallen en del av hello källkoden för din app. Du kan kontrollera i tooyour källdatabasen koden och uppdatera den som appen utvecklas. Du kan redigera hello mallen från Visual Studio.

När du definierar mallen, är du redo toodeploy hello resurser tooAzure. För hello kommandon toodeploy hello resurser, se:

* [Distribuera resurser med Resource Manager-mallar och Azure PowerShell](resource-group-template-deploy.md)
* [Distribuera resurser med Resource Manager-mallar och Azure CLI](resource-group-template-deploy-cli.md)
* [Distribuera resurser med Resource Manager-mallar och Azure Portal](resource-group-template-deploy-portal.md)
* [Distribuera resurser med Resource Manager-mallar och Resource Manager REST API](resource-group-template-deploy-rest.md)

## <a name="tags"></a>Taggar
Resource Manager innehåller en taggningsfunktion som du toocategorize resurser enligt tooyour krav för att hantera och fakturering. Använd taggar när du har en komplex samling resursgrupper och resurser och behöver toovisualize tillgångarna hello sätt som gör hello de flesta meningsfullt tooyou. Du kan till exempel markera resurser som har en liknande roll i organisationen eller toohello tillhör samma avdelning. Utan taggar, användare i din organisation kan skapa flera resurser som kan vara svårt toolater identifiera och hantera. Du kan till exempel vill toodelete alla hello resurser för ett visst projekt. Om resurserna inte är märkta för hello projekt, har du toomanually hittar dem. Taggning kan vara ett bra sätt tooreduce onödiga kostnader i din prenumeration. 

Resurser behöver inte tooreside i hello samma resurs grupp tooshare en tagg. Du kan skapa egna taggen taxonomi tooensure att alla användare i din organisation använder vanliga taggar i stället för användare av misstag använder varianter (till exempel ”Avd” i stället för ”avdelning”).

hello som följande exempel visar en tagg tillämpas tooa virtuell dator.

```json
"resources": [    
  {
    "type": "Microsoft.Compute/virtualMachines",
    "apiVersion": "2015-06-15",
    "name": "SimpleWindowsVM",
    "location": "[resourceGroup().location]",
    "tags": {
        "costCenter": "Finance"
    },
    ...
  }
]
```

tooretrieve alla hello resurser med ett Taggvärde använder hello följande PowerShell-cmdlet:

```powershell
Find-AzureRmResource -TagName costCenter -TagValue Finance
```

Eller hello följande Azure CLI 2.0-kommando:

```azurecli
az resource list --tag costCenter=Finance
```

Du kan också visa taggade resurser via hello Azure-portalen.

Hej [användningsrapporten](../billing/billing-understand-your-bill.md) för din prenumeration omfattar taggnamn och värden, vilket gör att du toobreak ut kostnader efter taggar. Mer information om taggar finns [med hjälp av taggar tooorganize resurserna i Azure](resource-group-using-tags.md).

## <a name="access-control"></a>Åtkomstkontroll
Resource Manager kan toocontrol som har åtkomst toospecific åtgärder för din organisation. Internt integrerar rollbaserad åtkomstkontroll (RBAC) i hello-plattform och tillämpar de tooall tjänsterna för åtkomstkontroll i resursgruppen. 

Det finns två huvudsakliga koncepten toounderstand när du arbetar med rollbaserad åtkomstkontroll:

* Rolldefinitioner – beskriver en uppsättning behörigheter och kan användas i många tilldelningar.
* Rolltilldelningar – associera en definition med en identitet (användare eller grupp) för en viss omfattning (prenumeration, resursgrupp eller resurs). hello tilldelning ärvs av lägre scope.

Du kan lägga till användare toopre definierats plattform och resursspecifika roller. Du kan dra nytta av hello fördefinierade rollen läsare som tillåter att användare tooview resurser men inte ändra dem.. Du lägger till användare i organisationen som behöver den här typen av rollen som läsare toohello och tillämpa hello rollen toohello prenumerationen, resursgruppen eller resursen.

Azure tillhandahåller hello följande fyra plattform roller:

1. Ägare – kan hantera allt, inklusive åtkomst
2. Deltagare – kan hantera allt förutom åtkomst
3. Läsare – kan visa allt, men inte göra några ändringar
4. Administratör för användaråtkomst - kan hantera användare komma åt tooAzure resurser

Azure tillhandahåller också flera resursspecifika roller. Några vanliga är:

1. Virtual Machine-deltagare - kan hantera virtuella datorer men inte att bevilja åtkomst till toothem och kan inte hantera hello virtuella nätverks- eller lagringsdrivrutiner konto toowhich de är anslutna
2. Network-deltagare - kan hantera alla nätverksresurser, men inte att bevilja åtkomst toothem
3. Storage-konto deltagare - hantera storage-konton, men inte att bevilja åtkomst toothem
4. SQL Server-deltagare – kan hantera SQL-servrar och databaser, men inte deras säkerhetsrelaterade principer
5. Webbplatsen deltagare - hantera webbplatser, men inte hello web planer toowhich de är anslutna

Hello fullständig lista över roller och tillåtna åtgärder finns i [RBAC: inbyggda roller](../active-directory/role-based-access-built-in-roles.md). Mer information om rollbaserad åtkomstkontroll finns i [Rollbaserad åtkomst i Azure](../active-directory/role-based-access-control-configure.md). 

I vissa fall kan du toorun kod eller skript som har åtkomst till resurser, men du inte vill toorun den under en användares autentiseringsuppgifter. I stället vill toocreate en identitet som kallas en tjänst huvudnamn för programmet hello och tilldela hello rätt roll för hello tjänstens huvudnamn. Resource Manager kan du toocreate autentiseringsuppgifter för programmet hello och autentisera programmässigt hello program. toolearn om att skapa tjänstens huvudnamn finns i följande avsnitt:

* [Använda Azure PowerShell toocreate en service principal tooaccess resurser](resource-group-authenticate-service-principal.md)
* [Använda Azure CLI toocreate en service principal tooaccess resurser](resource-group-authenticate-service-principal-cli.md)
* [Använda portalen toocreate Azure Active Directory-program och tjänstens huvudnamn som har åtkomst till resurser](resource-group-create-service-principal-portal.md)

Du kan också explicit låsa viktiga resurser tooprevent användare tar bort eller ändra dem. Mer information finns i [Låsa resurser med Azure Resource Manager](resource-group-lock-resources.md).

## <a name="activity-logs"></a>Aktivitetsloggar
Resource Manager loggar alla åtgärder som skapar, ändrar eller tar bort en resurs. Du kan använda hello aktivitet loggar toofind ett fel när du felsöker eller toomonitor hur en användare i din organisation ändrades en resurs. Välj toosee hello loggar **aktivitetsloggar** i hello **inställningar** bladet för en resursgrupp. Du kan filtrera hello loggar av många olika värden, inklusive vilka användarinitierad hello-åtgärden. Information om hur du arbetar med hello aktivitetsloggar finns [visa aktivitet loggar toomanage Azure resurser](resource-group-audit.md).

## <a name="customized-policies"></a>Anpassade principer
Resource Manager kan toocreate anpassade principer för att hantera dina resurser. hello typer av principer som du skapar kan innehålla olika scenarier. Du kan tillämpa en namngivningskonvention på resurser, begränsa vilka typer och instanser av resurser som kan distribueras eller begränsa vilka regioner som kan vara värd för en viss typ av resurs. Du kan kräva ett taggvärde för resurser tooorganize faktureringen av avdelningar. Du skapar principer toohelp minska kostnaderna och upprätthålla konsekvensen i din prenumeration. 

Du definierar principer med JSON och sedan använder du dessa principer antingen i hela prenumerationen eller i en resursgrupp. Principer är annat än rollbaserad åtkomstkontroll eftersom de används tooresource typer.

hello som följande exempel visar en princip som garanterar taggen konsekvens genom att ange att alla resurser inkluderar en costCenter-tagg.

```json
{
  "if": {
    "not" : {
      "field" : "tags",
      "containsKey" : "costCenter"
    }
  },
  "then" : {
    "effect" : "deny"
  }
}
```

Det finns många fler typer av principer som du kan skapa. Mer information finns i [använda princip toomanage resurser och kontrollera åtkomst](resource-manager-policy.md).

## <a name="sdks"></a>SDK:er
Azure-SDK:er är tillgängliga för flera språk och plattformar. Var och en av dessa språkimplementeringar är tillgängliga via pakethanteraren i respektive implementerings ekosystem och på GitHub.

Här är våra databaser för SDK med öppen källkod. Vi tar gärna emot feedback, information om problem och pull-förfrågningar.

* [Azure SDK för .NET](https://github.com/Azure/azure-sdk-for-net)
* [Azures hanteringsbibliotek för Java](https://github.com/Azure/azure-sdk-for-java)
* [Azure SDK för Node.js](https://github.com/Azure/azure-sdk-for-node)
* [Azure SDK för PHP](https://github.com/Azure/azure-sdk-for-php)
* [Azure SDK för Python](https://github.com/Azure/azure-sdk-for-python)
* [Azure SDK för Ruby](https://github.com/Azure/azure-sdk-for-ruby)

Information om hur du använder dessa språk med dina resurser finns i:

* [Azure för .NET-utvecklare](/dotnet/azure/?view=azure-dotnet)
* [Azure för Java-utvecklare](/java/azure/)
* [Azure för Node.js-utvecklare](/nodejs/azure/)
* [Azure för Python-utvecklare](/python/azure/)

> [!NOTE]
> Om hello SDK inte tillhandahåller hello nödvändiga funktioner, kan du också anropa toohello [Azure REST API](https://docs.microsoft.com/rest/api/resources/) direkt.
> 
> 

## <a name="next-steps"></a>Nästa steg
* En enkel introduktion tooworking med mallar finns i [exportera en Azure Resource Manager-mall från befintliga resurser](resource-manager-export-template.md).
* Mer omfattande anvisningar om hur du skapar en mall finns i [Skapa din första Azure Resource Manager-mall](resource-manager-create-first-template.md).
* toounderstand hello funktioner som kan användas i en mall finns [Mallfunktioner](resource-group-template-functions.md)
* Information om hur du använder Visual Studio med Resource Manager finns i [Skapa och distribuera Azure-resursgrupper via Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md).

Här är en videodemonstration av den här översikten:

>[!VIDEO https://channel9.msdn.com/Blogs/Azure-Documentation-Shorts/Azure-Resource-Manager-Overview/player]


[powershellref]: https://docs.microsoft.com/powershell/resourcemanager/azurerm.resources/v3.2.0/azurerm.resources
