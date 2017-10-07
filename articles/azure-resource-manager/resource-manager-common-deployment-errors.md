---
title: aaaTroubleshoot vanliga fel i Azure-distribution | Microsoft Docs
description: "Beskriver hur tooresolve vanliga fel när du distribuerar resurser tooAzure med Azure Resource Manager."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "distributionsfel för azure-distribution distribuera tooazure"
ms.assetid: c002a9be-4de5-4963-bd14-b54aa3d8fa59
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: tomfitz
ms.openlocfilehash: 8571e9941879eb5586e4258a785b6a09247da771
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-common-azure-deployment-errors-with-azure-resource-manager"></a>Felsöka vanliga Azure-distribution med Azure Resource Manager
Det här avsnittet beskrivs hur du kan lösa några vanliga Azure-distributionsfel som kan uppstå.

följande felkoder hello beskrivs i det här avsnittet:

* [AccountNameInvalid](#accountnameinvalid)
* [Auktoriseringen misslyckades](#authorization-failed)
* [BadRequest](#badrequest)
* [DeploymentFailed](#deploymentfailed)
* [DisallowedOperation](#disallowedoperation)
* [InvalidContentLink](#invalidcontentlink)
* [InvalidTemplate](#invalidtemplate)
* [MissingSubscriptionRegistration](#noregisteredproviderfound)
* [NotFound](#notfound)
* [NoRegisteredProviderFound](#noregisteredproviderfound)
* [OperationNotAllowed](#quotaexceeded)
* [ParentResourceNotFound](#parentresourcenotfound)
* [QuotaExceeded](#quotaexceeded)
* [RequestDisallowedByPolicy](#requestdisallowedbypolicy)
* [ResourceNotFound](#notfound)
* [SkuNotAvailable](#skunotavailable)
* [StorageAccountAlreadyExists](#storagenamenotunique)
* [StorageAccountAlreadyTaken](#storagenamenotunique)

## <a name="deploymentfailed"></a>DeploymentFailed

Felet indikerar en allmän distributionsfel men det är inte hello felkoden måste toostart felsökning. hello felkoden som hjälper dig att lösa problemet hello faktiskt är vanligtvis en nivå under det här felet. Hello följande bild visar exempelvis hello **RequestDisallowedByPolicy** felkod som är under hello distributionsfel.

![Visa felkod](./media/resource-manager-common-deployment-errors/error-code.png)

## <a name="skunotavailable"></a>SkuNotAvailable

När du distribuerar en resurs (vanligtvis en virtuell dator), får hello följande kod och fel felmeddelande:

```
Code: SkuNotAvailable
Message: hello requested tier for resource '<resource>' is currently not available in location '<location>' 
for subscription '<subscriptionID>'. Please try another tier or deploy tooa different location.
```

Du får detta felmeddelande när hello resursen SKU som du har valt (till exempel VM-storlek) inte är tillgänglig för hello-plats som du har valt. tooresolve det här problemet behöver du toodetermine som SKU: er är tillgängliga i en region. Du kan använda PowerShell, hello-portalen eller en REST-åtgärden toofind tillgängliga SKU: er.

- PowerShell, Använd [Get-AzureRmComputeResourceSku](/powershell/module/azurerm.compute/get-azurermcomputeresourcesku) och filtrera efter plats. Du måste ha hello senaste versionen av PowerShell för det här kommandot.

  ```powershell
  Get-AzureRmComputeResourceSku | where {$_.Locations.Contains("southcentralus")}
  ```

  hello resultatet innehåller en lista över SKU: er för hello platsen och eventuella begränsningar för den SKU.

  ```powershell
  ResourceType                Name      Locations Restriction                      Capability Value
  ------------                ----      --------- -----------                      ---------- -----
  availabilitySets         Classic southcentralus             MaximumPlatformFaultDomainCount     3
  availabilitySets         Aligned southcentralus             MaximumPlatformFaultDomainCount     3
  virtualMachines      Standard_A0 southcentralus
  virtualMachines      Standard_A1 southcentralus
  virtualMachines      Standard_A2 southcentralus
  ```

- toouse hello [portal](https://portal.azure.com), logga in toohello portal och lägga till en resurs via hello-gränssnittet. När du ställer in hello värden du se hello tillgängliga SKU: er för den här resursen. Du behöver inte toocomplete hello distribution.

    ![tillgängliga SKU: er](./media/resource-manager-common-deployment-errors/view-sku.png)

- toouse hello REST API för virtuella datorer, skicka hello följande begäran:

  ```HTTP 
  GET
  https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.Compute/skus?api-version=2016-03-30
  ```

  Tillgängliga SKU: er och regioner returneras i hello följande format:

  ```json
  {
    "value": [
      {
        "resourceType": "virtualMachines",
        "name": "Standard_A0",
        "tier": "Standard",
        "size": "A0",
        "locations": [
          "eastus"
        ],
        "restrictions": []
      },
      {
        "resourceType": "virtualMachines",
        "name": "Standard_A1",
        "tier": "Standard",
        "size": "A1",
        "locations": [
          "eastus"
        ],
        "restrictions": []
      },
      ...
    ]
  }    
  ```

Om du toofind en lämplig SKU i den regionen eller en annan region som uppfyller behoven för din verksamhet kan skicka en [SKU begäran](https://aka.ms/skurestriction) tooAzure Support.

## <a name="disallowedoperation"></a>DisallowedOperation

```
Code: DisallowedOperation
Message: hello current subscription type is not permitted tooperform operations on any provider 
namespace. Please use a different subscription.
```

Om du får detta felmeddelande du använder en prenumeration som inte är tillåtet tooaccess alla Azure-tjänster än Azure Active Directory. Den här typen av prenumeration kan uppstå när du behöver tooaccess hello klassiska portalen men är inte tillåtna toodeploy resurser. tooresolve det här problemet måste du använda en prenumeration som har behörigheten toodeploy resurser.  

tooview tillgängliga prenumerationer med PowerShell, Använd:

```powershell
Get-AzureRmSubscription
```

Och tooset hello aktuell prenumeration, Använd:

```powershell
Set-AzureRmContext -SubscriptionName {subscription-name}
```

tooview tillgängliga prenumerationer med Azure CLI 2.0, Använd:

```azurecli
az account list
```

Och tooset hello aktuell prenumeration, Använd:

```azurecli
az account set --subscription {subscription-name}
```

## <a name="invalidtemplate"></a>InvalidTemplate
Det här felet kan bero på flera olika typer av fel.

- Syntaxfel

   Om du får ett felmeddelande som anger hello mallen misslyckades verifieringen kanske syntax problem i mallen.

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed
  ```

   Det här felet beror på enkelt toomake malluttryck kan vara komplicerade. Till exempel innehåller hello följande namntilldelning för ett lagringskonto en uppsättning av hakparenteser, tre funktioner, tre uppsättningar av parenteser, en uppsättning av enkla citattecken och en egenskap:

  ```json
  "name": "[concat('storage', uniqueString(resourceGroup().id))]",
  ```

   Om du inte anger hello matchande syntax genererar hello mallen ett värde som skiljer sig från din avsikt.

   Granska noggrant hello uttryckssyntax när du får den här typen av fel. Överväg att använda en JSON-redigerare som [Visual Studio](vs-azure-tools-resource-groups-deployment-projects-create-deploy.md) eller [Visual Studio Code](resource-manager-vs-code.md), vilket kan varna dig om syntaxfel.

- Felaktiga längder

   En annan ogiltig mall-fel uppstår när hello resursnamnet inte är i rätt format för hello.

  ```
  Code=InvalidTemplate
  Message=Deployment template validation failed: 'hello template resource {resource-name}'
  for type {resource-type} has incorrect segment lengths.
  ```

   En nivå rotresurs måste ha ett mindre segment i hello namn än i hello resurstypen. Varje segment differentierade med ett snedstreck. I följande exempel hello, hello typen har två segment och hello namn har ett segment så att den är en **giltigt namn**.

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "myHostingPlanName",
    ...
  }
  ```

   Men hello nästa exempel är **inte ett giltigt namn** eftersom den har hello samma antal segment som hello-typen.

  ```json
  {
    "type": "Microsoft.Web/serverfarms",
    "name": "appPlan/myHostingPlanName",
    ...
  }
  ```

   För underordnade resurser hello typ och namn har hello samma antal segment. Det här antalet segment är meningsfullt eftersom hello fullständigt namn och typ för hello underordnade innehåller hello överordnade namn och typen. Hello fullständiga namnet måste därför fortfarande ett mindre segment än hello fullständig typen.

  ```json
  "resources": [
      {
          "type": "Microsoft.KeyVault/vaults",
          "name": "contosokeyvault",
          ...
          "resources": [
              {
                  "type": "secrets",
                  "name": "appPassword",
                  ...
              }
          ]
      }
  ]
  ```

   Hämtning hello segment rätt kan vara svårt med Resource Manager-typer som tillämpas på resursleverantörer. Till exempel kräver använda en resurs Lås tooa webbplats en typ med fyra segment. Hello namn är därför tre segment:

  ```json
  {
      "type": "Microsoft.Web/sites/providers/locks",
      "name": "[concat(variables('siteName'),'/Microsoft.Authorization/MySiteLock')]",
      ...
  }
  ```

- Kopiera index är inte den förväntade

   Du får detta **InvalidTemplate** fel när du har tillämpat hello **kopiera** elementet tooa tillhör hello-mall som inte stöder det här elementet. Du kan bara använda hello kopiera elementet tooa resurstypen. Du kan inte använda egenskapen tooa inom en resurstyp. Till exempel du gäller kopiera tooa virtuell dator, men du kan inte använda den toohello OS-diskar för en virtuell dator. I vissa fall kan konvertera du en underordnad resurs tooa överordnade resurs toocreate en kopia skapas. Mer information om hur du använder kopia finns [skapa flera instanser av resurser i Azure Resource Manager](resource-group-create-multiple.md).

- Parametern är inte giltig

   Om du anger ett värde som inte är ett av dessa värden hello mall anger tillåtna värden för en parameter, visas ett meddelande liknande toohello följande fel:

  ```
  Code=InvalidTemplate;
  Message=Deployment template validation failed: 'hello provided value {parameter value}
  for hello template parameter {parameter name} is not valid. hello parameter value is not
  part of hello allowed values
  ``` 

   Kontrollera hello tillåtna värden i hello mallen och ange en under distributionen.

- Cirkulärt beroende har upptäckts

   Du får detta felmeddelande när resurser beroende av varandra på ett sätt som förhindrar hello distribution från att starta. En kombination av beroenden gör två eller flera resurs vänta tills andra resurser som väntar på också. Till exempel resource1 beror på resource3 resource2 beror på resource1 och resource3 beror på resource2. Vanligtvis kan du lösa problemet genom att ta bort onödiga beroenden. 

<a id="notfound" />
### <a name="notfound-and-resourcenotfound"></a>NotFound och ResourceNotFound
När mallen innehåller hello namnet på en resurs som inte kan matchas, visas ett felmeddelande:

```
Code=NotFound;
Message=Cannot find ServerFarm with name exampleplan.
```

Om du försöker toodeploy hello resursen i hello mall saknas kontrollerar du om du behöver tooadd ett beroende. Hanteraren för filserverresurser optimerar distribution genom att skapa resurser parallellt, när det är möjligt. Om en resurs måste distribueras efter en annan resurs, behöver du toouse hello **dependsOn** element i din mall toocreate en beroende hello andra resurser. Hello App Service-plan måste till exempel finnas när du distribuerar ett webbprogram. Om du inte har angett att hello webbprogrammet beror på hello programtjänstplanen Resource Manager skapar båda resurserna på hello samtidigt. Du får ett felmeddelande om att hello App Service-plan resursen inte kan hittas, eftersom det inte finns ännu vid försök tooset en egenskap på hello webbprogrammet. Du kan förhindra att det här felet genom att ange hello beroende i hello webbapp.

```json
{
  "apiVersion": "2015-08-01",
  "type": "Microsoft.Web/sites",
  "dependsOn": [
    "[variables('hostingPlanName')]"
  ],
  ...
}
```

Förslag på felsökning för beroende finns [Kontrollera distributionsordningen](#check-deployment-sequence).

Du kan också se felet när hello resursen finns i en annan resursgrupp än hello något som distribueras. I så fall använder hello [resourceId funktionen](resource-group-template-functions-resource.md#resourceid) tooget hello fullständigt kvalificerade namnet på hello resursen.

```json
"properties": {
    "name": "[parameters('siteName')]",
    "serverFarmId": "[resourceId('plangroup', 'Microsoft.Web/serverfarms', parameters('hostingPlanName'))]"
}
```

Om du försöker toouse hello [referens](resource-group-template-functions-resource.md#reference) eller [listKeys](resource-group-template-functions-resource.md#listkeys) funktioner med en resurs som inte kan lösas, du får hello följande fel:

```
Code=ResourceNotFound;
Message=hello Resource 'Microsoft.Storage/storageAccounts/{storage name}' under resource
group {resource group name} was not found.
```

Leta efter ett uttryck som innehåller hello **referens** funktion. Kontrollera att hello parametervärden är korrekta.

## <a name="parentresourcenotfound"></a>ParentResourceNotFound

När en resurs är en överordnad tooanother resurs, måste hello överordnade resursen finnas innan du skapar hello underordnade resursen. Om den inte ännu finns, visas hello följande fel:

```
Code=ParentResourceNotFound;
Message=Can not perform requested operation on nested resource. Parent resource 'exampleserver' not found."
```

hello ingår hello underordnade resursen hello överordnade namn. Till exempel kan en SQL-databas definieras som:

```json
{
  "type": "Microsoft.Sql/servers/databases",
  "name": "[concat(variables('databaseServerName'), '/', parameters('databaseName'))]",
  ...
```

Men om du inte anger ett beroende på hello överordnade resursen hello underordnade resursen kan distribueras innan hello överordnade. tooresolve det här felet är ett beroende.

```json
"dependsOn": [
    "[variables('databaseServerName')]"
]
```

<a id="storagenamenotunique" />

## <a name="storageaccountalreadyexists-and-storageaccountalreadytaken"></a>StorageAccountAlreadyExists och StorageAccountAlreadyTaken
Du måste ange ett namn för hello-resurs som är unik i Azure för storage-konton. Om du inte anger ett unikt namn får ett felmeddelande som:

```
Code=StorageAccountAlreadyTaken
Message=hello storage account named mystorage is already taken.
```

Du kan skapa ett unikt namn genom att sammanbinda en namngivningskonvention med hello resultatet av hello [uniqueString](resource-group-template-functions-string.md#uniquestring) funktion.

```json
"name": "[concat('storage', uniqueString(resourceGroup().id))]",
"type": "Microsoft.Storage/storageAccounts",
```

Om du distribuerar ett lagringskonto med hello med samma namn som ett befintligt lagringskonto i din prenumeration, men ger en annan plats, du får ett felmeddelande som anger hello storage-konto finns redan i en annan plats. Ta antingen bort hello befintligt lagringskonto eller ange hello samma plats som hello befintligt lagringskonto.

## <a name="accountnameinvalid"></a>AccountNameInvalid
Du ser hello **AccountNameInvalid** fel vid försök toogive en storage-konto ett namn som innehåller förbjudet tecken. Lagringskontonamn måste vara mellan 3 och 24 tecken långt och innehålla siffror och gemena bokstäver. Hej [uniqueString](resource-group-template-functions-string.md#uniquestring) funktionen returnerar 13 tecken. Om du sammanfoga ett prefix toohello **uniqueString** leda, ange ett prefix som är 11 tecken eller mindre.

## <a name="badrequest"></a>BadRequest

Status BadRequest kan uppstå när du anger ett ogiltigt värde för en egenskap. Du anger ett felaktigt värde för SKU för ett lagringskonto måste misslyckas hello distributionen. toodetermine giltiga värden för egenskapen titta på hello [REST API](/rest/api) för hello resurstyp som du distribuerar.

<a id="noregisteredproviderfound" />

## <a name="noregisteredproviderfound-and-missingsubscriptionregistration"></a>NoRegisteredProviderFound och MissingSubscriptionRegistration
När du distribuerar resurs, kan du får hello följande felkod och meddelande:

```
Code: NoRegisteredProviderFound
Message: No registered resource provider found for location {location}
and API version {api-version} for type {resource-type}.
```

Eller, du kan få en liknande meddelande om att:

```
Code: MissingSubscriptionRegistration
Message: hello subscription is not registered toouse namespace {resource-provider-namespace}
```

Felen visas i någon av tre orsaker:

1. Hej resursprovidern har inte registrerats för din prenumeration
2. API-versionen stöds inte för hello resurstyp
3. Plats stöds inte för hello resurstyp

hello felmeddelande bör du få förslag på hello stöds platser och API-versioner. Du kan ändra din mall tooone av hello föreslagna värden. De flesta leverantörer registreras automatiskt av hello Azure-portalen eller hello-kommandoradsgränssnittet du använder, men inte alla. Om du inte har använt en viss resurs-providern före eventuellt du tooregister providern. Du kan identifiera information om resursproviders via PowerShell eller Azure CLI.

**Portal**

Du kan se hello registreringsstatus och registrera en resursleverantörens namnrymd hello-portalen.

1. Din prenumeration, Välj **resursproviders**.

   ![Välj resursprovidrar](./media/resource-manager-common-deployment-errors/select-resource-provider.png)

2. Titta på hello lista över resursproviders och välj eventuellt hello **registrera** länk tooregister hello resursprovidern av typen hello försöker toodeploy.

   ![Visa en lista över resursprovidrar](./media/resource-manager-common-deployment-errors/list-resource-providers.png)

**PowerShell**

toosee din registreringsstatus, Använd **Get-AzureRmResourceProvider**.

```powershell
Get-AzureRmResourceProvider -ListAvailable
```

tooregister en provider, Använd **registrera AzureRmResourceProvider** och ange hello namn av hello resursprovidern gärna tooregister.

```powershell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Cdn
```

tooget hello stöds platser för en viss typ av resurs, Använd:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).Locations
```

tooget hello API-versioner som stöds för en viss typ av resurs, Använd:

```powershell
((Get-AzureRmResourceProvider -ProviderNamespace Microsoft.Web).ResourceTypes | Where-Object ResourceTypeName -eq sites).ApiVersions
```

**Azure CLI**

toosee om hello provider är registrerad, använda hello `azure provider list` kommando.

```azurecli
az provider list
```

tooregister en resursleverantör, använda hello `azure provider register` kommando och ange hello *namnområde* tooregister.

```azurecli
az provider register --namespace Microsoft.Cdn
```

Använd toosee hello stöds platser och API-versioner för en resurstyp:

```azurecli
az provider show -n Microsoft.Web --query "resourceTypes[?resourceType=='sites'].locations"
```

<a id="quotaexceeded" />

## <a name="quotaexceeded-and-operationnotallowed"></a>QuotaExceeded och OperationNotAllowed
Problem kan uppstå när distributionen överskrider kvoten som kan vara per resursgrupp, prenumerationer, konton och andra scope. Din prenumeration kan till exempel vara konfigurerade toolimit hello antalet kärnor för en region. Om du försöker toodeploy en virtuell dator med fler kärnor än hello tillåtna belopp, felmeddelande ett anger hello kvot har överskridits.
För fullständig kvotinformation finns i [Azure-prenumeration och tjänsten gränser, kvoter och begränsningar](../azure-subscription-service-limits.md).

tooexamine din prenumeration kvoter för kärnor, kan du använda hello `azure vm list-usage` i hello Azure CLI. hello som följande exempel visar att hello kärnkvot för ett kostnadsfritt utvärderingskonto är 4:

```azurecli
az vm list-usage --location "South Central US"
```

Som returnerar:

```azurecli
[
  {
    "currentValue": 0,
    "limit": 2000,
    "name": {
      "localizedValue": "Availability Sets",
      "value": "availabilitySets"
    }
  },
  ...
]
```

Om du distribuerar en mall som skapar fler än fyra kärnor i hello västra USA region, får du ett distribueringsfel som ser ut som:

```
Code=OperationNotAllowed
Message=Operation results in exceeding quota limits of Core.
Maximum allowed: 4, Current in use: 4, Additional requested: 2.
```

I PowerShell kan du använda hello **Get-AzureRmVMUsage** cmdlet.

```powershell
Get-AzureRmVMUsage
```

Som returnerar:

```powershell
...
CurrentValue : 0
Limit        : 4
Name         : {
                 "value": "cores",
                 "localizedValue": "Total Regional Cores"
               }
Unit         : null
...
```

I dessa fall bör du gå toohello portal och filen en stöd problemet tooraise din kvot för hello region som du vill toodeploy.

> [!NOTE]
> Kom ihåg att för resursgrupper hello kvot för varje enskild region, inte för hello hela prenumerationen. Om du behöver toodeploy 30 kärnor i västra USA kan ha du tooask för 30 Resource Manager kärnor i USA, västra. Om du behöver toodeploy 30 kärnor i någon av hello regioner toowhich har du åtkomst till, bör du be om 30 Resource Manager kärnor i alla regioner.
>
>

## <a name="invalidcontentlink"></a>InvalidContentLink
När du felmeddelande hello:

```
Code=InvalidContentLink
Message=Unable toodownload deployment content from ...
```

Du har troligen försökt toolink tooa kapslad mall som inte är tillgänglig. Kontrollera hello URI som du angav för hello kapslade mallen. Hello-mallen finns i ett lagringskonto, ifall hello URI är tillgänglig. Du kan behöva toopass en SAS-token. Mer information finns i [Använda länkade mallar med Azure Resource Manager](resource-group-linked-templates.md).

## <a name="requestdisallowedbypolicy"></a>RequestDisallowedByPolicy
Du får detta felmeddelande när prenumerationen innehåller en resursprincip som förhindrar att en åtgärd som du försöker tooperform under distributionen. Leta efter hello Principidentifierare i hello felmeddelande.

```
Policy identifier(s): '/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition'
```

I **PowerShell**, ange den Principidentifierare som hello **Id** parametern tooretrieve information om hello-princip som blockerats av din distribution.

```powershell
(Get-AzureRmPolicyDefinition -Id "/subscriptions/{guid}/providers/Microsoft.Authorization/policyDefinitions/regionPolicyDefinition").Properties.policyRule | ConvertTo-Json
```

I **Azure CLI**, ange hello namnet på definitionen av hello principen:

```azurecli
az policy definition show --name regionPolicyAssignment
```

Mer information finns i följande artiklar hello:

- [Fel med RequestDisallowedByPolicy](resource-manager-policy-requestdisallowedbypolicy-error.md)
- [Använda princip toomanage resurser och styr åtkomsten](resource-manager-policy.md).

## <a name="authorization-failed"></a>Auktoriseringen misslyckades
Du får ett fel under distributionen hello konto eller tjänstens huvudnamn försöker toodeploy hello resurser har inte åtkomst tooperform åtgärderna. Azure Active Directory kan du eller din administratör toocontrol vilka identiteter kan komma åt vilka resurser med en bra precision. Till exempel om ditt konto har tilldelats rollen för läsare av toohello, är du inte kan toocreate resurser. I så fall visas ett felmeddelande om att auktorisering misslyckades.

Mer information om rollbaserad åtkomstkontroll finns [rollbaserad åtkomstkontroll i](../active-directory/role-based-access-control-configure.md).


## <a name="next-steps"></a>Nästa steg
* toolearn om granskning åtgärder, se [granskningsåtgärder med Resource Manager](resource-group-audit.md).
* toolearn om åtgärder toodetermine hello fel under distributionen, se [visa distributionsåtgärder](resource-manager-deployment-operations.md).
