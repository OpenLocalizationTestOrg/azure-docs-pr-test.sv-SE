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
# <a name="understand-azure-deployment-errors"></a><span data-ttu-id="2084f-104">Förstå Azure distributionsfel</span><span class="sxs-lookup"><span data-stu-id="2084f-104">Understand Azure deployment errors</span></span>
<span data-ttu-id="2084f-105">Det här avsnittet beskriver distributionsfel och hur du kan hitta mer information om ett fel.</span><span class="sxs-lookup"><span data-stu-id="2084f-105">This topic describes deployment errors and how you can discover more information about an error.</span></span> <span data-ttu-id="2084f-106">Distributionsfel för lösningar toocommon finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="2084f-106">For resolutions toocommon deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>

## <a name="two-types-of-errors"></a><span data-ttu-id="2084f-107">Två typer av fel</span><span class="sxs-lookup"><span data-stu-id="2084f-107">Two types of errors</span></span>
<span data-ttu-id="2084f-108">Det finns två typer av fel som du kan ta emot:</span><span class="sxs-lookup"><span data-stu-id="2084f-108">There are two types of errors you can receive:</span></span>

* <span data-ttu-id="2084f-109">Verifieringsfel</span><span class="sxs-lookup"><span data-stu-id="2084f-109">validation errors</span></span>
* <span data-ttu-id="2084f-110">Distributionsfel</span><span class="sxs-lookup"><span data-stu-id="2084f-110">deployment errors</span></span>

<span data-ttu-id="2084f-111">hello följande bild visar hello aktivitetsloggen för en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2084f-111">hello following image shows hello activity log for a subscription.</span></span> <span data-ttu-id="2084f-112">Den representerar två distributioner.</span><span class="sxs-lookup"><span data-stu-id="2084f-112">It represents two deployments.</span></span> <span data-ttu-id="2084f-113">I en distribution hello mallen gick inte att validera (**verifiera**) och fortsatte inte.</span><span class="sxs-lookup"><span data-stu-id="2084f-113">In one deployment, hello template failed validation (**Validate**) and did not proceed.</span></span> <span data-ttu-id="2084f-114">I Hej annan distribution, hello mallen valideras men det gick inte att skapa hello resurser (**skriva distributioner**).</span><span class="sxs-lookup"><span data-stu-id="2084f-114">In hello other deployment, hello template passed validation but failed when creating hello resources (**Write Deployments**).</span></span> 

![Visa felkod](./media/resource-manager-troubleshoot-tips/show-activity-log.png)

<span data-ttu-id="2084f-116">Verifieringsfel uppstår scenarier som kan fastställas innan distribution.</span><span class="sxs-lookup"><span data-stu-id="2084f-116">Validation errors arise from scenarios that can be determined before deployment.</span></span> <span data-ttu-id="2084f-117">De omfattar syntaxfel i mallen, eller försök toodeploy resurser som skulle överskrida kvoter för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2084f-117">They include syntax errors in your template, or trying toodeploy resources that would exceed your subscription quotas.</span></span> <span data-ttu-id="2084f-118">Distributionsfel uppkommer villkor som kan inträffa under hello distribueringen.</span><span class="sxs-lookup"><span data-stu-id="2084f-118">Deployment errors arise from conditions that occur during hello deployment process.</span></span> <span data-ttu-id="2084f-119">De omfattar försök tooaccess en resurs som distribueras parallellt.</span><span class="sxs-lookup"><span data-stu-id="2084f-119">They include trying tooaccess a resource that is being deployed in parallel.</span></span>

<span data-ttu-id="2084f-120">Båda typerna av fel returnerar en felkod som du använder tootroubleshoot hello distribution.</span><span class="sxs-lookup"><span data-stu-id="2084f-120">Both types of errors return an error code that you use tootroubleshoot hello deployment.</span></span> <span data-ttu-id="2084f-121">Båda typerna av fel visas i hello [aktivitetsloggen](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="2084f-121">Both types of errors appear in hello [activity log](resource-group-audit.md).</span></span> <span data-ttu-id="2084f-122">Dock visas inte valideringsfel i distributionshistoriken eftersom hello distributionen aldrig har startats.</span><span class="sxs-lookup"><span data-stu-id="2084f-122">However, validation errors do not appear in your deployment history because hello deployment never started.</span></span>

## <a name="determine-error-code"></a><span data-ttu-id="2084f-123">Fastställa felkod:</span><span class="sxs-lookup"><span data-stu-id="2084f-123">Determine error code</span></span>

<span data-ttu-id="2084f-124">Du kan lära dig om ett fel genom att titta på hello felmeddelande och hello-felkod.</span><span class="sxs-lookup"><span data-stu-id="2084f-124">You can learn about an error by looking at hello error message and hello error code.</span></span> <span data-ttu-id="2084f-125">Hej [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md) artikeln visar lösningar av felkod.</span><span class="sxs-lookup"><span data-stu-id="2084f-125">hello [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md) article lists resolutions by error code.</span></span> <span data-ttu-id="2084f-126">Det här avsnittet visar hur toouse hello Azure portal toodiscover hello-felkod.</span><span class="sxs-lookup"><span data-stu-id="2084f-126">This topic shows how toouse hello Azure portal toodiscover hello error code.</span></span>

### <a name="validation-errors"></a><span data-ttu-id="2084f-127">Verifieringsfel</span><span class="sxs-lookup"><span data-stu-id="2084f-127">Validation errors</span></span>

<span data-ttu-id="2084f-128">När du distribuerar via hello portal finns ett verifieringsfel efter att ha skickat värdena.</span><span class="sxs-lookup"><span data-stu-id="2084f-128">When deploying through hello portal, you see a validation error after submitting your values.</span></span>

![Visa portal verifieringsfel](./media/resource-manager-troubleshoot-tips/validation-error.png)

<span data-ttu-id="2084f-130">Välj hello-meddelande för mer information.</span><span class="sxs-lookup"><span data-stu-id="2084f-130">Select hello message for more details.</span></span> <span data-ttu-id="2084f-131">I följande bild hello, visas en **InvalidTemplateDeployment** fel och ett meddelande som anger en princip blockeras distributionen.</span><span class="sxs-lookup"><span data-stu-id="2084f-131">In hello following image, you see an **InvalidTemplateDeployment** error and a message that indicates a policy blocked deployment.</span></span>

![Visa valideringsinformation](./media/resource-manager-troubleshoot-tips/validation-details.png)

### <a name="deployment-errors"></a><span data-ttu-id="2084f-133">Distributionsfel</span><span class="sxs-lookup"><span data-stu-id="2084f-133">Deployment errors</span></span>

<span data-ttu-id="2084f-134">När hello åtgärden klarar valideringen, men inte fungerar under distributionen, ser du hello fel i hello meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2084f-134">When hello operation passes validation, but fails during deployment, you see hello error in hello notifications.</span></span> <span data-ttu-id="2084f-135">Välj hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="2084f-135">Select hello notification.</span></span>

![meddelandet-fel](./media/resource-manager-troubleshoot-tips/notification.png)

<span data-ttu-id="2084f-137">Du kan visa mer information om hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="2084f-137">You see more details about hello deployment.</span></span> <span data-ttu-id="2084f-138">Välj hello alternativet toofind mer information om hello felet.</span><span class="sxs-lookup"><span data-stu-id="2084f-138">Select hello option toofind more information about hello error.</span></span>

![distributionen misslyckades](./media/resource-manager-troubleshoot-tips/deployment-failed.png)

<span data-ttu-id="2084f-140">Du ser hello meddelandet och fel felkoder.</span><span class="sxs-lookup"><span data-stu-id="2084f-140">You see hello error message and error codes.</span></span> <span data-ttu-id="2084f-141">Observera att det finns två felkoder.</span><span class="sxs-lookup"><span data-stu-id="2084f-141">Notice there are two error codes.</span></span> <span data-ttu-id="2084f-142">Hej första felkoden (**DeploymentFailed**) är ett allmänt fel som inte innehåller hello information du behöver toosolve hello fel.</span><span class="sxs-lookup"><span data-stu-id="2084f-142">hello first error code (**DeploymentFailed**) is a general error that does not provide hello details you need toosolve hello error.</span></span> <span data-ttu-id="2084f-143">Hej andra felkoden (**StorageAccountNotFound**) innehåller hello information du behöver.</span><span class="sxs-lookup"><span data-stu-id="2084f-143">hello second error code (**StorageAccountNotFound**) provides hello details you need.</span></span> 

![information om fel](./media/resource-manager-troubleshoot-tips/error-details.png)

## <a name="enable-debug-logging"></a><span data-ttu-id="2084f-145">Aktivera felsökningsloggning</span><span class="sxs-lookup"><span data-stu-id="2084f-145">Enable debug logging</span></span>
<span data-ttu-id="2084f-146">Ibland behöver mer information om hello förfrågan och svar toodiscover vad som gick fel.</span><span class="sxs-lookup"><span data-stu-id="2084f-146">Sometimes you need more information about hello request and response toodiscover what went wrong.</span></span> <span data-ttu-id="2084f-147">Du kan begära att ytterligare information loggas under en distribution med hjälp av PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="2084f-147">By using PowerShell or Azure CLI, you can request that additional information is logged during a deployment.</span></span>

- <span data-ttu-id="2084f-148">PowerShell</span><span class="sxs-lookup"><span data-stu-id="2084f-148">PowerShell</span></span>

   <span data-ttu-id="2084f-149">Ange hello i PowerShell **DeploymentDebugLogLevel** parametern tooAll, ResponseContent eller RequestContent.</span><span class="sxs-lookup"><span data-stu-id="2084f-149">In PowerShell, set hello **DeploymentDebugLogLevel** parameter tooAll, ResponseContent, or RequestContent.</span></span>

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   <span data-ttu-id="2084f-150">Undersök hello begär innehåll med hello följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="2084f-150">Examine hello request content with hello following cmdlet:</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   <span data-ttu-id="2084f-151">Eller hello svar innehåll med:</span><span class="sxs-lookup"><span data-stu-id="2084f-151">Or, hello response content with:</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   <span data-ttu-id="2084f-152">Den här informationen kan hjälpa dig att avgöra om ett värde i hello mallen felaktigt anges.</span><span class="sxs-lookup"><span data-stu-id="2084f-152">This information can help you determine whether a value in hello template is being incorrectly set.</span></span>

- <span data-ttu-id="2084f-153">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="2084f-153">Azure CLI</span></span>

   <span data-ttu-id="2084f-154">Undersök hello distributionsåtgärder med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="2084f-154">Examine hello deployment operations with hello following command:</span></span>

  ```azurecli
  az group deployment operation list --resource-group ExampleGroup --name vmlinux
  ```

- <span data-ttu-id="2084f-155">Kapslad mall</span><span class="sxs-lookup"><span data-stu-id="2084f-155">Nested template</span></span>

   <span data-ttu-id="2084f-156">toolog felsökningsinformation för en kapslad mall, Använd hello **debugSetting** element.</span><span class="sxs-lookup"><span data-stu-id="2084f-156">toolog debug information for a nested template, use hello **debugSetting** element.</span></span>

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


## <a name="create-a-troubleshooting-template"></a><span data-ttu-id="2084f-157">Skapa en mall för felsökning</span><span class="sxs-lookup"><span data-stu-id="2084f-157">Create a troubleshooting template</span></span>
<span data-ttu-id="2084f-158">I vissa fall hello enklaste sättet tootroubleshoot din mall är tootest delar av den.</span><span class="sxs-lookup"><span data-stu-id="2084f-158">In some cases, hello easiest way tootroubleshoot your template is tootest parts of it.</span></span> <span data-ttu-id="2084f-159">Du kan skapa en förenklad mall som du kan använda toofocus på hello del som du tror orsakas hello-fel.</span><span class="sxs-lookup"><span data-stu-id="2084f-159">You can create a simplified template that enables you toofocus on hello part that you believe is causing hello error.</span></span> <span data-ttu-id="2084f-160">Anta att du får ett fel när du refererar till en resurs.</span><span class="sxs-lookup"><span data-stu-id="2084f-160">For example, suppose you are receiving an error when referencing a resource.</span></span> <span data-ttu-id="2084f-161">Skapa en mall som returnerar hello del som orsakar problemet i stället för hantering av en hel mall.</span><span class="sxs-lookup"><span data-stu-id="2084f-161">Rather than dealing with an entire template, create a template that returns hello part that may be causing your problem.</span></span> <span data-ttu-id="2084f-162">Det kan hjälpa dig att avgöra om du skickar in hello rätt parametrar, med hjälp av Mallfunktioner korrekt, och hämtar hello resurs du förväntar dig.</span><span class="sxs-lookup"><span data-stu-id="2084f-162">It can help you determine whether you are passing in hello right parameters, using template functions correctly, and getting hello resource you expect.</span></span>

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

<span data-ttu-id="2084f-163">Eller anta att det uppstår distributionsfel som du tror är relaterade tooincorrectly ange beroenden.</span><span class="sxs-lookup"><span data-stu-id="2084f-163">Or, suppose you are encountering deployment errors that you believe are related tooincorrectly set dependencies.</span></span> <span data-ttu-id="2084f-164">Testa din mall genom att dela upp den i förenklad mallar.</span><span class="sxs-lookup"><span data-stu-id="2084f-164">Test your template by breaking it into simplified templates.</span></span> <span data-ttu-id="2084f-165">Först skapa en mall som distribuerar en enskild resurs (till exempel en SQL Server).</span><span class="sxs-lookup"><span data-stu-id="2084f-165">First, create a template that deploys only a single resource (like a SQL Server).</span></span> <span data-ttu-id="2084f-166">Lägga till en resurs som är beroende av den (till exempel en SQL-databas) när du är säker på att du har den här resursen korrekt definierad.</span><span class="sxs-lookup"><span data-stu-id="2084f-166">When you are sure you have that resource correctly defined, add a resource that depends on it (like a SQL Database).</span></span> <span data-ttu-id="2084f-167">Lägg till andra beroende resurser (till exempel granskningsprinciper) när du har de två resurser korrekt definierad.</span><span class="sxs-lookup"><span data-stu-id="2084f-167">When you have those two resources correctly defined, add other dependent resources (like auditing policies).</span></span> <span data-ttu-id="2084f-168">Ta bort hello grupp toomake att du rätt tester hello resursberoenden mellan varje test-distribution.</span><span class="sxs-lookup"><span data-stu-id="2084f-168">In between each test deployment, delete hello resource group toomake sure you adequately testing hello dependencies.</span></span> 

## <a name="check-deployment-sequence"></a><span data-ttu-id="2084f-169">Kontrollera distributionen sekvens</span><span class="sxs-lookup"><span data-stu-id="2084f-169">Check deployment sequence</span></span>

<span data-ttu-id="2084f-170">Många distributionsfel inträffa när resurser har distribuerats i ett oväntat sekvens.</span><span class="sxs-lookup"><span data-stu-id="2084f-170">Many deployment errors happen when resources are deployed in an unexpected sequence.</span></span> <span data-ttu-id="2084f-171">Dessa fel uppstår när beroenden inte är korrekt inställda.</span><span class="sxs-lookup"><span data-stu-id="2084f-171">These errors arise when dependencies are not correctly set.</span></span> <span data-ttu-id="2084f-172">När du saknar ett beroende som krävs, försöker toouse ett värde för en annan resurs men hello andra inte finns ännu en resurs.</span><span class="sxs-lookup"><span data-stu-id="2084f-172">When you are missing a needed dependency, one resource attempts toouse a value for another resource but hello other does not yet exist.</span></span> <span data-ttu-id="2084f-173">Du får ett felmeddelande om att en resurs inte hittas.</span><span class="sxs-lookup"><span data-stu-id="2084f-173">You get an error stating that a resource is not found.</span></span> <span data-ttu-id="2084f-174">Du kan stöta på den här typen av fel periodvis eftersom hello distributionstiden för varje resurs kan variera.</span><span class="sxs-lookup"><span data-stu-id="2084f-174">You may encounter this type of error intermittently because hello deployment time for each resource can vary.</span></span> <span data-ttu-id="2084f-175">Till exempel din första försöket toodeploy dina resurser lyckas eftersom en nödvändig resurs slumpmässigt har slutförts i tid.</span><span class="sxs-lookup"><span data-stu-id="2084f-175">For example, your first attempt toodeploy your resources succeeds because a required resource randomly completes in time.</span></span> <span data-ttu-id="2084f-176">Din andra försöket misslyckas dock eftersom hello nödvändiga resursen inte slutfördes i tid.</span><span class="sxs-lookup"><span data-stu-id="2084f-176">However, your second attempt fails because hello required resource did not complete in time.</span></span> 

<span data-ttu-id="2084f-177">Men du vill tooavoid inställningen beroenden som inte behövs.</span><span class="sxs-lookup"><span data-stu-id="2084f-177">But, you want tooavoid setting dependencies that are not needed.</span></span> <span data-ttu-id="2084f-178">När du har onödiga beroenden förlänga hello varaktighet hello distributionen genom att förhindra att resurser som inte är beroende av varandra från att distribueras parallellt.</span><span class="sxs-lookup"><span data-stu-id="2084f-178">When you have unnecessary dependencies, you prolong hello duration of hello deployment by preventing resources that are not dependent on each other from being deployed in parallel.</span></span> <span data-ttu-id="2084f-179">Dessutom kan du skapa Cirkelberoenden som blockerar hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="2084f-179">In addition, you may create circular dependencies that block hello deployment.</span></span> <span data-ttu-id="2084f-180">Hej [referens](resource-group-template-functions-resource.md#reference) funktionen skapas en implicit beroende på hello refererar till resursen när den här resursen har distribuerats i hello samma mall.</span><span class="sxs-lookup"><span data-stu-id="2084f-180">hello [reference](resource-group-template-functions-resource.md#reference) function creates an implicit dependency on hello referenced resource, when that resource is deployed in hello same template.</span></span> <span data-ttu-id="2084f-181">Du kan därför ha flera beroenden än hello beroenden som anges i hello **dependsOn** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="2084f-181">Therefore, you may have more dependencies than hello dependencies specified in hello **dependsOn** property.</span></span> <span data-ttu-id="2084f-182">Hej [resourceId](resource-group-template-functions-resource.md#resourceid) funktionen inte skapa en implicit beroende eller verifiera att hello resursen finns.</span><span class="sxs-lookup"><span data-stu-id="2084f-182">hello [resourceId](resource-group-template-functions-resource.md#resourceid) function does not create an implicit dependency or validate that hello resource exists.</span></span>

<span data-ttu-id="2084f-183">När det uppstår problem för beroende måste toogain inblick i hello ordningen för distribution av resursen.</span><span class="sxs-lookup"><span data-stu-id="2084f-183">When you encounter dependency problems, you need toogain insight into hello order of resource deployment.</span></span> <span data-ttu-id="2084f-184">tooview hello ordning distributionsåtgärder:</span><span class="sxs-lookup"><span data-stu-id="2084f-184">tooview hello order of deployment operations:</span></span>

1. <span data-ttu-id="2084f-185">Välj hello distributionshistoriken för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="2084f-185">Select hello deployment history for your resource group.</span></span>

   ![Välj distributionshistoriken](./media/resource-manager-troubleshoot-tips/select-deployment.png)

2. <span data-ttu-id="2084f-187">Välj en distribution från hello historik och välj **händelser**.</span><span class="sxs-lookup"><span data-stu-id="2084f-187">Select a deployment from hello history, and select **Events**.</span></span>

   ![Välj distributionen händelser](./media/resource-manager-troubleshoot-tips/select-deployment-events.png)

3. <span data-ttu-id="2084f-189">Granska hello sekvens av händelser för varje resurs.</span><span class="sxs-lookup"><span data-stu-id="2084f-189">Examine hello sequence of events for each resource.</span></span> <span data-ttu-id="2084f-190">Betala uppmärksamhet toohello status för varje åtgärd.</span><span class="sxs-lookup"><span data-stu-id="2084f-190">Pay attention toohello status of each operation.</span></span> <span data-ttu-id="2084f-191">Hello visar följande bild exempelvis tre storage-konton som har distribuerats parallellt.</span><span class="sxs-lookup"><span data-stu-id="2084f-191">For example, hello following image shows three storage accounts that deployed in parallel.</span></span> <span data-ttu-id="2084f-192">Observera att hello tre storage-konton har startats på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="2084f-192">Notice that hello three storage accounts are started at hello same time.</span></span>

   ![Parallell distribution](./media/resource-manager-troubleshoot-tips/deployment-events-parallel.png)

   <span data-ttu-id="2084f-194">hello nästa bild visar tre storage-konton som inte har distribuerats parallellt.</span><span class="sxs-lookup"><span data-stu-id="2084f-194">hello next image shows three storage accounts that are not deployed in parallel.</span></span> <span data-ttu-id="2084f-195">hello andra lagringskonto beror på hello första storage-konto, och hello tredje storage-konto för hello andra storage-konto.</span><span class="sxs-lookup"><span data-stu-id="2084f-195">hello second storage account depends on hello first storage account, and hello third storage account depends on hello second storage account.</span></span> <span data-ttu-id="2084f-196">Därför är hello första storage-konto igång accepteras och slutförs innan hello nästa gång.</span><span class="sxs-lookup"><span data-stu-id="2084f-196">Therefore, hello first storage account is started, accepted, and completed before hello next is started.</span></span>

   ![sekventiella distribution](./media/resource-manager-troubleshoot-tips/deployment-events-sequence.png)

<span data-ttu-id="2084f-198">Även för mer komplicerade scenarier som du kan använda hello samma teknik toodiscover när distributionen är igång och klar för varje resurs.</span><span class="sxs-lookup"><span data-stu-id="2084f-198">Even for more complicated scenarios, you can use hello same technique toodiscover when deployment is started and completed for each resource.</span></span> <span data-ttu-id="2084f-199">Titta igenom din distribution händelser toosee om hello sekvens skiljer sig än förväntat.</span><span class="sxs-lookup"><span data-stu-id="2084f-199">Look through your deployment events toosee if hello sequence is different than you would expect.</span></span> <span data-ttu-id="2084f-200">I så fall, omvärdera hello beroenden för den här resursen.</span><span class="sxs-lookup"><span data-stu-id="2084f-200">If so, reevaluate hello dependencies for this resource.</span></span>

<span data-ttu-id="2084f-201">Resource Manager identifierar cirkulärt tjänstberoende vid verifiering av mallen.</span><span class="sxs-lookup"><span data-stu-id="2084f-201">Resource Manager identifies circular dependencies during template validation.</span></span> <span data-ttu-id="2084f-202">Den returnerar ett felmeddelande som anger ett cirkelberoende finns.</span><span class="sxs-lookup"><span data-stu-id="2084f-202">It returns an error message that specifically states a circular dependency exists.</span></span> <span data-ttu-id="2084f-203">toosolve ett cirkulärt beroende:</span><span class="sxs-lookup"><span data-stu-id="2084f-203">toosolve a circular dependency:</span></span>

1. <span data-ttu-id="2084f-204">Hitta hello-resurs som identifieras i hello cirkelberoende i mallen.</span><span class="sxs-lookup"><span data-stu-id="2084f-204">In your template, find hello resource identified in hello circular dependency.</span></span> 
2. <span data-ttu-id="2084f-205">För den här resursen granska hello **dependsOn** egenskap och några användningsområden för hello **referens** toosee vilka resurser som det beror på att fungera.</span><span class="sxs-lookup"><span data-stu-id="2084f-205">For that resource, examine hello **dependsOn** property and any uses of hello **reference** function toosee which resources it depends on.</span></span> 
3. <span data-ttu-id="2084f-206">Granska dessa resurser toosee vilka resurser de beroende av.</span><span class="sxs-lookup"><span data-stu-id="2084f-206">Examine those resources toosee which resources they depend on.</span></span> <span data-ttu-id="2084f-207">Följ hello beroenden tills du ser en resurs som är beroende av hello ursprungliga resursen.</span><span class="sxs-lookup"><span data-stu-id="2084f-207">Follow hello dependencies until you notice a resource that depends on hello original resource.</span></span>
5. <span data-ttu-id="2084f-208">Hello resurser som ingår i hello cirkulärt beroende, noggrant undersöka all användning av hello **dependsOn** egenskapen tooidentify eventuella beroenden som inte behövs.</span><span class="sxs-lookup"><span data-stu-id="2084f-208">For hello resources involved in hello circular dependency, carefully examine all uses of hello **dependsOn** property tooidentify any dependencies that are not needed.</span></span> <span data-ttu-id="2084f-209">Ta bort dessa beroenden.</span><span class="sxs-lookup"><span data-stu-id="2084f-209">Remove those dependencies.</span></span> <span data-ttu-id="2084f-210">Om du inte vet att ett beroende behövs tar du bort den.</span><span class="sxs-lookup"><span data-stu-id="2084f-210">If you are unsure that a dependency is needed, try removing it.</span></span> 
6. <span data-ttu-id="2084f-211">Omdistribuera hello mallen.</span><span class="sxs-lookup"><span data-stu-id="2084f-211">Redeploy hello template.</span></span>

<span data-ttu-id="2084f-212">Ta bort värden från hello **dependsOn** egenskapen kan orsaka fel när du distribuerar hello mallen.</span><span class="sxs-lookup"><span data-stu-id="2084f-212">Removing values from hello **dependsOn** property can cause errors when you deploy hello template.</span></span> <span data-ttu-id="2084f-213">Lägg till hello beroende tillbaka till hello mall om det uppstår ett fel.</span><span class="sxs-lookup"><span data-stu-id="2084f-213">If you encounter an error, add hello dependency back into hello template.</span></span> 

<span data-ttu-id="2084f-214">Överväg att flytta en del av logik för distribution till underordnade resurser (till exempel tillägg eller konfigurationsinställningar) om den metoden inte löser hello cirkulärt beroende.</span><span class="sxs-lookup"><span data-stu-id="2084f-214">If that approach does not solve hello circular dependency, consider moving part of your deployment logic into child resources (such as extensions or configuration settings).</span></span> <span data-ttu-id="2084f-215">Konfigurera de underordnade resurser toodeploy efter hello resurser som ingår i hello cirkulärt beroende.</span><span class="sxs-lookup"><span data-stu-id="2084f-215">Configure those child resources toodeploy after hello resources involved in hello circular dependency.</span></span> <span data-ttu-id="2084f-216">Anta exempelvis att du distribuerar två virtuella datorer, men du måste ange egenskaper för var och en som refererar toohello andra.</span><span class="sxs-lookup"><span data-stu-id="2084f-216">For example, suppose you are deploying two virtual machines but you must set properties on each one that refer toohello other.</span></span> <span data-ttu-id="2084f-217">Du kan distribuera dem i hello följande ordning:</span><span class="sxs-lookup"><span data-stu-id="2084f-217">You can deploy them in hello following order:</span></span>

1. <span data-ttu-id="2084f-218">vm1</span><span class="sxs-lookup"><span data-stu-id="2084f-218">vm1</span></span>
2. <span data-ttu-id="2084f-219">vm2</span><span class="sxs-lookup"><span data-stu-id="2084f-219">vm2</span></span>
3. <span data-ttu-id="2084f-220">Tillägg på vm1 beror på vm1 och vm2.</span><span class="sxs-lookup"><span data-stu-id="2084f-220">Extension on vm1 depends on vm1 and vm2.</span></span> <span data-ttu-id="2084f-221">hello-tillägget anger värden för vm1 får från vm2.</span><span class="sxs-lookup"><span data-stu-id="2084f-221">hello extension sets values on vm1 that it gets from vm2.</span></span>
4. <span data-ttu-id="2084f-222">Tillägg på vm2 beror på vm1 och vm2.</span><span class="sxs-lookup"><span data-stu-id="2084f-222">Extension on vm2 depends on vm1 and vm2.</span></span> <span data-ttu-id="2084f-223">hello-tillägget anger värden för vm2 får från vm1.</span><span class="sxs-lookup"><span data-stu-id="2084f-223">hello extension sets values on vm2 that it gets from vm1.</span></span>

<span data-ttu-id="2084f-224">hello samma metod som fungerar för Apptjänst-appar.</span><span class="sxs-lookup"><span data-stu-id="2084f-224">hello same approach works for App Service apps.</span></span> <span data-ttu-id="2084f-225">Överväg att flytta konfigurationsvärden till en underordnad resurs hello app resurs.</span><span class="sxs-lookup"><span data-stu-id="2084f-225">Consider moving configuration values into a child resource of hello app resource.</span></span> <span data-ttu-id="2084f-226">Du kan distribuera två web apps i hello följande ordning:</span><span class="sxs-lookup"><span data-stu-id="2084f-226">You can deploy two web apps in hello following order:</span></span>

1. <span data-ttu-id="2084f-227">webapp1</span><span class="sxs-lookup"><span data-stu-id="2084f-227">webapp1</span></span>
2. <span data-ttu-id="2084f-228">webapp2</span><span class="sxs-lookup"><span data-stu-id="2084f-228">webapp2</span></span>
3. <span data-ttu-id="2084f-229">konfigurationen för webapp1 beror på webapp1 och webapp2.</span><span class="sxs-lookup"><span data-stu-id="2084f-229">config for webapp1 depends on webapp1 and webapp2.</span></span> <span data-ttu-id="2084f-230">Den innehåller app-inställningar med värden från webapp2.</span><span class="sxs-lookup"><span data-stu-id="2084f-230">It contains app settings with values from webapp2.</span></span>
4. <span data-ttu-id="2084f-231">konfigurationen för webapp2 beror på webapp1 och webapp2.</span><span class="sxs-lookup"><span data-stu-id="2084f-231">config for webapp2 depends on webapp1 and webapp2.</span></span> <span data-ttu-id="2084f-232">Den innehåller app-inställningar med värden från webapp1.</span><span class="sxs-lookup"><span data-stu-id="2084f-232">It contains app settings with values from webapp1.</span></span>


## <a name="next-steps"></a><span data-ttu-id="2084f-233">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2084f-233">Next steps</span></span>
* <span data-ttu-id="2084f-234">Distributionsfel för lösningar toocommon finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="2084f-234">For resolutions toocommon deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="2084f-235">toolearn om granskning åtgärder, se [granskningsåtgärder med Resource Manager](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="2084f-235">toolearn about auditing actions, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="2084f-236">toolearn om åtgärder toodetermine hello fel under distributionen, se [visa distributionsåtgärder](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="2084f-236">toolearn about actions toodetermine hello errors during deployment, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
