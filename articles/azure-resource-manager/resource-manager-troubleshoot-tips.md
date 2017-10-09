---
title: aaaUnderstand Azure distributionsfel | Microsoft Docs
description: "Beskriver hur toolearn om fel för Azure-distribution."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "distributionsfel för azure-distribution distribuera tooazure"
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: tomfitz
ms.openlocfilehash: a335e121e9b908a763374907e34b1f6e823d6e96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-azure-deployment-errors"></a>Förstå Azure distributionsfel
Det här avsnittet beskriver distributionsfel och hur du kan hitta mer information om ett fel. Distributionsfel för lösningar toocommon finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).

## <a name="two-types-of-errors"></a>Två typer av fel
Det finns två typer av fel som du kan ta emot:

* Verifieringsfel
* Distributionsfel

hello följande bild visar hello aktivitetsloggen för en prenumeration. Den representerar två distributioner. I en distribution hello mallen gick inte att validera (**verifiera**) och fortsatte inte. I Hej annan distribution, hello mallen valideras men det gick inte att skapa hello resurser (**skriva distributioner**). 

![Visa felkod](./media/resource-manager-troubleshoot-tips/show-activity-log.png)

Verifieringsfel uppstår scenarier som kan fastställas innan distribution. De omfattar syntaxfel i mallen, eller försök toodeploy resurser som skulle överskrida kvoter för din prenumeration. Distributionsfel uppkommer villkor som kan inträffa under hello distribueringen. De omfattar försök tooaccess en resurs som distribueras parallellt.

Båda typerna av fel returnerar en felkod som du använder tootroubleshoot hello distribution. Båda typerna av fel visas i hello [aktivitetsloggen](resource-group-audit.md). Dock visas inte valideringsfel i distributionshistoriken eftersom hello distributionen aldrig har startats.

## <a name="determine-error-code"></a>Fastställa felkod:

Du kan lära dig om ett fel genom att titta på hello felmeddelande och hello-felkod. Hej [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md) artikeln visar lösningar av felkod. Det här avsnittet visar hur toouse hello Azure portal toodiscover hello-felkod.

### <a name="validation-errors"></a>Verifieringsfel

När du distribuerar via hello portal finns ett verifieringsfel efter att ha skickat värdena.

![Visa portal verifieringsfel](./media/resource-manager-troubleshoot-tips/validation-error.png)

Välj hello-meddelande för mer information. I följande bild hello, visas en **InvalidTemplateDeployment** fel och ett meddelande som anger en princip blockeras distributionen.

![Visa valideringsinformation](./media/resource-manager-troubleshoot-tips/validation-details.png)

### <a name="deployment-errors"></a>Distributionsfel

När hello åtgärden klarar valideringen, men inte fungerar under distributionen, ser du hello fel i hello meddelanden. Välj hello-meddelande.

![meddelandet-fel](./media/resource-manager-troubleshoot-tips/notification.png)

Du kan visa mer information om hello-distribution. Välj hello alternativet toofind mer information om hello felet.

![distributionen misslyckades](./media/resource-manager-troubleshoot-tips/deployment-failed.png)

Du ser hello meddelandet och fel felkoder. Observera att det finns två felkoder. Hej första felkoden (**DeploymentFailed**) är ett allmänt fel som inte innehåller hello information du behöver toosolve hello fel. Hej andra felkoden (**StorageAccountNotFound**) innehåller hello information du behöver. 

![information om fel](./media/resource-manager-troubleshoot-tips/error-details.png)

## <a name="enable-debug-logging"></a>Aktivera felsökningsloggning
Ibland behöver mer information om hello förfrågan och svar toodiscover vad som gick fel. Du kan begära att ytterligare information loggas under en distribution med hjälp av PowerShell eller Azure CLI.

- PowerShell

   Ange hello i PowerShell **DeploymentDebugLogLevel** parametern tooAll, ResponseContent eller RequestContent.

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   Undersök hello begär innehåll med hello följande cmdlet:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   Eller hello svar innehåll med:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   Den här informationen kan hjälpa dig att avgöra om ett värde i hello mallen felaktigt anges.

- Azure CLI

   Undersök hello distributionsåtgärder med hello följande kommando:

  ```azurecli
  az group deployment operation list --resource-group ExampleGroup --name vmlinux
  ```

- Kapslad mall

   toolog felsökningsinformation för en kapslad mall, Använd hello **debugSetting** element.

  ```json
  {
      "apiVersion": "2016-09-01",
      "name": "nestedTemplate",
      "type": "Microsoft.Resources/deployments",
      "properties": {
          "mode": "Incremental",
          "templateLink": {
              "uri": "{template-uri}",
              "contentVersion": "1.0.0.0"
          },
          "debugSetting": {
             "detailLevel": "requestContent, responseContent"
          }
      }
  }
  ```


## <a name="create-a-troubleshooting-template"></a>Skapa en mall för felsökning
I vissa fall hello enklaste sättet tootroubleshoot din mall är tootest delar av den. Du kan skapa en förenklad mall som du kan använda toofocus på hello del som du tror orsakas hello-fel. Anta att du får ett fel när du refererar till en resurs. Skapa en mall som returnerar hello del som orsakar problemet i stället för hantering av en hel mall. Det kan hjälpa dig att avgöra om du skickar in hello rätt parametrar, med hjälp av Mallfunktioner korrekt, och hämtar hello resurs du förväntar dig.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "storageName": {
        "type": "string"
    },
    "storageResourceGroup": {
        "type": "string"
    }
  },
  "variables": {},
  "resources": [],
  "outputs": {
    "exampleOutput": {
        "value": "[reference(resourceId(parameters('storageResourceGroup'), 'Microsoft.Storage/storageAccounts', parameters('storageName')), '2016-05-01')]",
        "type" : "object"
    }
  }
}
```

Eller anta att det uppstår distributionsfel som du tror är relaterade tooincorrectly ange beroenden. Testa din mall genom att dela upp den i förenklad mallar. Först skapa en mall som distribuerar en enskild resurs (till exempel en SQL Server). Lägga till en resurs som är beroende av den (till exempel en SQL-databas) när du är säker på att du har den här resursen korrekt definierad. Lägg till andra beroende resurser (till exempel granskningsprinciper) när du har de två resurser korrekt definierad. Ta bort hello grupp toomake att du rätt tester hello resursberoenden mellan varje test-distribution. 

## <a name="check-deployment-sequence"></a>Kontrollera distributionen sekvens

Många distributionsfel inträffa när resurser har distribuerats i ett oväntat sekvens. Dessa fel uppstår när beroenden inte är korrekt inställda. När du saknar ett beroende som krävs, försöker toouse ett värde för en annan resurs men hello andra inte finns ännu en resurs. Du får ett felmeddelande om att en resurs inte hittas. Du kan stöta på den här typen av fel periodvis eftersom hello distributionstiden för varje resurs kan variera. Till exempel din första försöket toodeploy dina resurser lyckas eftersom en nödvändig resurs slumpmässigt har slutförts i tid. Din andra försöket misslyckas dock eftersom hello nödvändiga resursen inte slutfördes i tid. 

Men du vill tooavoid inställningen beroenden som inte behövs. När du har onödiga beroenden förlänga hello varaktighet hello distributionen genom att förhindra att resurser som inte är beroende av varandra från att distribueras parallellt. Dessutom kan du skapa Cirkelberoenden som blockerar hello-distribution. Hej [referens](resource-group-template-functions-resource.md#reference) funktionen skapas en implicit beroende på hello refererar till resursen när den här resursen har distribuerats i hello samma mall. Du kan därför ha flera beroenden än hello beroenden som anges i hello **dependsOn** egenskapen. Hej [resourceId](resource-group-template-functions-resource.md#resourceid) funktionen inte skapa en implicit beroende eller verifiera att hello resursen finns.

När det uppstår problem för beroende måste toogain inblick i hello ordningen för distribution av resursen. tooview hello ordning distributionsåtgärder:

1. Välj hello distributionshistoriken för resursgruppen.

   ![Välj distributionshistoriken](./media/resource-manager-troubleshoot-tips/select-deployment.png)

2. Välj en distribution från hello historik och välj **händelser**.

   ![Välj distributionen händelser](./media/resource-manager-troubleshoot-tips/select-deployment-events.png)

3. Granska hello sekvens av händelser för varje resurs. Betala uppmärksamhet toohello status för varje åtgärd. Hello visar följande bild exempelvis tre storage-konton som har distribuerats parallellt. Observera att hello tre storage-konton har startats på hello samtidigt.

   ![Parallell distribution](./media/resource-manager-troubleshoot-tips/deployment-events-parallel.png)

   hello nästa bild visar tre storage-konton som inte har distribuerats parallellt. hello andra lagringskonto beror på hello första storage-konto, och hello tredje storage-konto för hello andra storage-konto. Därför är hello första storage-konto igång accepteras och slutförs innan hello nästa gång.

   ![sekventiella distribution](./media/resource-manager-troubleshoot-tips/deployment-events-sequence.png)

Även för mer komplicerade scenarier som du kan använda hello samma teknik toodiscover när distributionen är igång och klar för varje resurs. Titta igenom din distribution händelser toosee om hello sekvens skiljer sig än förväntat. I så fall, omvärdera hello beroenden för den här resursen.

Resource Manager identifierar cirkulärt tjänstberoende vid verifiering av mallen. Den returnerar ett felmeddelande som anger ett cirkelberoende finns. toosolve ett cirkulärt beroende:

1. Hitta hello-resurs som identifieras i hello cirkelberoende i mallen. 
2. För den här resursen granska hello **dependsOn** egenskap och några användningsområden för hello **referens** toosee vilka resurser som det beror på att fungera. 
3. Granska dessa resurser toosee vilka resurser de beroende av. Följ hello beroenden tills du ser en resurs som är beroende av hello ursprungliga resursen.
5. Hello resurser som ingår i hello cirkulärt beroende, noggrant undersöka all användning av hello **dependsOn** egenskapen tooidentify eventuella beroenden som inte behövs. Ta bort dessa beroenden. Om du inte vet att ett beroende behövs tar du bort den. 
6. Omdistribuera hello mallen.

Ta bort värden från hello **dependsOn** egenskapen kan orsaka fel när du distribuerar hello mallen. Lägg till hello beroende tillbaka till hello mall om det uppstår ett fel. 

Överväg att flytta en del av logik för distribution till underordnade resurser (till exempel tillägg eller konfigurationsinställningar) om den metoden inte löser hello cirkulärt beroende. Konfigurera de underordnade resurser toodeploy efter hello resurser som ingår i hello cirkulärt beroende. Anta exempelvis att du distribuerar två virtuella datorer, men du måste ange egenskaper för var och en som refererar toohello andra. Du kan distribuera dem i hello följande ordning:

1. vm1
2. vm2
3. Tillägg på vm1 beror på vm1 och vm2. hello-tillägget anger värden för vm1 får från vm2.
4. Tillägg på vm2 beror på vm1 och vm2. hello-tillägget anger värden för vm2 får från vm1.

hello samma metod som fungerar för Apptjänst-appar. Överväg att flytta konfigurationsvärden till en underordnad resurs hello app resurs. Du kan distribuera två web apps i hello följande ordning:

1. webapp1
2. webapp2
3. konfigurationen för webapp1 beror på webapp1 och webapp2. Den innehåller app-inställningar med värden från webapp2.
4. konfigurationen för webapp2 beror på webapp1 och webapp2. Den innehåller app-inställningar med värden från webapp1.


## <a name="next-steps"></a>Nästa steg
* Distributionsfel för lösningar toocommon finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).
* toolearn om granskning åtgärder, se [granskningsåtgärder med Resource Manager](resource-group-audit.md).
* toolearn om åtgärder toodetermine hello fel under distributionen, se [visa distributionsåtgärder](resource-manager-deployment-operations.md).
