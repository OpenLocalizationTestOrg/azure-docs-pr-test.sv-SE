---
title: "Förstå Azure distributionsfel | Microsoft Docs"
description: "Beskriver hur du lär dig mer om Azure distributionsfel."
services: azure-resource-manager
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
keywords: "distributionsfel för azure-distribution distribuera till azure"
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: support-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/12/2017
ms.author: tomfitz
ms.openlocfilehash: b67bb30fa259fa08e37e11afec724c8b8c3eb633
ms.sourcegitcommit: 422efcbac5b6b68295064bd545132fcc98349d01
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/29/2017
---
# <a name="understand-azure-deployment-errors"></a><span data-ttu-id="f271d-104">Förstå Azure distributionsfel</span><span class="sxs-lookup"><span data-stu-id="f271d-104">Understand Azure deployment errors</span></span>
<span data-ttu-id="f271d-105">Det här avsnittet beskriver distributionsfel och hur du kan hitta mer information om ett fel.</span><span class="sxs-lookup"><span data-stu-id="f271d-105">This topic describes deployment errors and how you can discover more information about an error.</span></span> <span data-ttu-id="f271d-106">Lösningar på vanliga distributionsfel finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="f271d-106">For resolutions to common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>

## <a name="two-types-of-errors"></a><span data-ttu-id="f271d-107">Två typer av fel</span><span class="sxs-lookup"><span data-stu-id="f271d-107">Two types of errors</span></span>
<span data-ttu-id="f271d-108">Det finns två typer av fel som du kan ta emot:</span><span class="sxs-lookup"><span data-stu-id="f271d-108">There are two types of errors you can receive:</span></span>

* <span data-ttu-id="f271d-109">Verifieringsfel</span><span class="sxs-lookup"><span data-stu-id="f271d-109">validation errors</span></span>
* <span data-ttu-id="f271d-110">Distributionsfel</span><span class="sxs-lookup"><span data-stu-id="f271d-110">deployment errors</span></span>

<span data-ttu-id="f271d-111">Följande bild visar aktivitetsloggen för en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f271d-111">The following image shows the activity log for a subscription.</span></span> <span data-ttu-id="f271d-112">Den representerar två distributioner.</span><span class="sxs-lookup"><span data-stu-id="f271d-112">It represents two deployments.</span></span> <span data-ttu-id="f271d-113">I en distribution av mallen gick inte att validera (**verifiera**) och fortsatte inte.</span><span class="sxs-lookup"><span data-stu-id="f271d-113">In one deployment, the template failed validation (**Validate**) and did not proceed.</span></span> <span data-ttu-id="f271d-114">I den andra distributionen mallen valideras men det gick inte att skapa resurser (**skriva distributioner**).</span><span class="sxs-lookup"><span data-stu-id="f271d-114">In the other deployment, the template passed validation but failed when creating the resources (**Write Deployments**).</span></span> 

![Visa felkod](./media/resource-manager-troubleshoot-tips/show-activity-log.png)

<span data-ttu-id="f271d-116">Verifieringsfel uppstår scenarier som kan fastställas innan distribution.</span><span class="sxs-lookup"><span data-stu-id="f271d-116">Validation errors arise from scenarios that can be determined before deployment.</span></span> <span data-ttu-id="f271d-117">De omfattar syntaxfel i mallen, eller försök att distribuera resurser som skulle överskrida kvoter för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="f271d-117">They include syntax errors in your template, or trying to deploy resources that would exceed your subscription quotas.</span></span> <span data-ttu-id="f271d-118">Distributionsfel uppkommer villkor som kan inträffa under distributionsprocessen.</span><span class="sxs-lookup"><span data-stu-id="f271d-118">Deployment errors arise from conditions that occur during the deployment process.</span></span> <span data-ttu-id="f271d-119">De omfattar försöker komma åt en resurs som distribueras parallellt.</span><span class="sxs-lookup"><span data-stu-id="f271d-119">They include trying to access a resource that is being deployed in parallel.</span></span>

<span data-ttu-id="f271d-120">Båda typerna av fel returnerar en felkod som används för att felsöka distributionen.</span><span class="sxs-lookup"><span data-stu-id="f271d-120">Both types of errors return an error code that you use to troubleshoot the deployment.</span></span> <span data-ttu-id="f271d-121">Båda typerna av fel visas i den [aktivitetsloggen](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="f271d-121">Both types of errors appear in the [activity log](resource-group-audit.md).</span></span> <span data-ttu-id="f271d-122">Dock visas inte valideringsfel i distributionshistoriken eftersom distributionen har startat.</span><span class="sxs-lookup"><span data-stu-id="f271d-122">However, validation errors do not appear in your deployment history because the deployment never started.</span></span>

## <a name="determine-error-code"></a><span data-ttu-id="f271d-123">Fastställa felkod:</span><span class="sxs-lookup"><span data-stu-id="f271d-123">Determine error code</span></span>

<span data-ttu-id="f271d-124">Du kan lära dig om ett fel genom att titta på felmeddelandet och felkoden.</span><span class="sxs-lookup"><span data-stu-id="f271d-124">You can learn about an error by looking at the error message and the error code.</span></span> <span data-ttu-id="f271d-125">Den [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md) artikeln visar lösningar av felkod.</span><span class="sxs-lookup"><span data-stu-id="f271d-125">The [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md) article lists resolutions by error code.</span></span> <span data-ttu-id="f271d-126">Det här avsnittet visar hur du använder Azure-portalen för att identifiera felkoden.</span><span class="sxs-lookup"><span data-stu-id="f271d-126">This topic shows how to use the Azure portal to discover the error code.</span></span>

### <a name="validation-errors"></a><span data-ttu-id="f271d-127">Verifieringsfel</span><span class="sxs-lookup"><span data-stu-id="f271d-127">Validation errors</span></span>

<span data-ttu-id="f271d-128">När du distribuerar via portalen finns ett verifieringsfel efter att ha skickat värdena.</span><span class="sxs-lookup"><span data-stu-id="f271d-128">When deploying through the portal, you see a validation error after submitting your values.</span></span>

![Visa portal verifieringsfel](./media/resource-manager-troubleshoot-tips/validation-error.png)

<span data-ttu-id="f271d-130">Välj meddelandet för mer information.</span><span class="sxs-lookup"><span data-stu-id="f271d-130">Select the message for more details.</span></span> <span data-ttu-id="f271d-131">I följande bild visas en **InvalidTemplateDeployment** fel och ett meddelande som anger en princip blockeras distributionen.</span><span class="sxs-lookup"><span data-stu-id="f271d-131">In the following image, you see an **InvalidTemplateDeployment** error and a message that indicates a policy blocked deployment.</span></span>

![Visa valideringsinformation](./media/resource-manager-troubleshoot-tips/validation-details.png)

### <a name="deployment-errors"></a><span data-ttu-id="f271d-133">Distributionsfel</span><span class="sxs-lookup"><span data-stu-id="f271d-133">Deployment errors</span></span>

<span data-ttu-id="f271d-134">När åtgärden har klarat valideringen men misslyckas under distributionen, visas fel i meddelanden.</span><span class="sxs-lookup"><span data-stu-id="f271d-134">When the operation passes validation, but fails during deployment, you see the error in the notifications.</span></span> <span data-ttu-id="f271d-135">Markera meddelandet.</span><span class="sxs-lookup"><span data-stu-id="f271d-135">Select the notification.</span></span>

![meddelandet-fel](./media/resource-manager-troubleshoot-tips/notification.png)

<span data-ttu-id="f271d-137">Du kan visa mer information om hur du distribuerar.</span><span class="sxs-lookup"><span data-stu-id="f271d-137">You see more details about the deployment.</span></span> <span data-ttu-id="f271d-138">Välj alternativet för att få mer information om felet.</span><span class="sxs-lookup"><span data-stu-id="f271d-138">Select the option to find more information about the error.</span></span>

![distributionen misslyckades](./media/resource-manager-troubleshoot-tips/deployment-failed.png)

<span data-ttu-id="f271d-140">Du ser felmeddelandet och felkoder.</span><span class="sxs-lookup"><span data-stu-id="f271d-140">You see the error message and error codes.</span></span> <span data-ttu-id="f271d-141">Observera att det finns två felkoder.</span><span class="sxs-lookup"><span data-stu-id="f271d-141">Notice there are two error codes.</span></span> <span data-ttu-id="f271d-142">Första felkoden (**DeploymentFailed**) är ett allmänt fel som inte innehåller de information du behöver för att lösa felet.</span><span class="sxs-lookup"><span data-stu-id="f271d-142">The first error code (**DeploymentFailed**) is a general error that does not provide the details you need to solve the error.</span></span> <span data-ttu-id="f271d-143">Andra felkoden (**StorageAccountNotFound**) innehåller de information du behöver.</span><span class="sxs-lookup"><span data-stu-id="f271d-143">The second error code (**StorageAccountNotFound**) provides the details you need.</span></span> 

![information om fel](./media/resource-manager-troubleshoot-tips/error-details.png)

## <a name="enable-debug-logging"></a><span data-ttu-id="f271d-145">Aktivera felsökningsloggning</span><span class="sxs-lookup"><span data-stu-id="f271d-145">Enable debug logging</span></span>
<span data-ttu-id="f271d-146">Ibland behöver mer information om begäran och svar för att identifiera vad som gick fel.</span><span class="sxs-lookup"><span data-stu-id="f271d-146">Sometimes you need more information about the request and response to discover what went wrong.</span></span> <span data-ttu-id="f271d-147">Du kan begära att ytterligare information loggas under en distribution med hjälp av PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="f271d-147">By using PowerShell or Azure CLI, you can request that additional information is logged during a deployment.</span></span>

- <span data-ttu-id="f271d-148">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f271d-148">PowerShell</span></span>

   <span data-ttu-id="f271d-149">I PowerShell, ange den **DeploymentDebugLogLevel** parameter till alla, ResponseContent eller RequestContent.</span><span class="sxs-lookup"><span data-stu-id="f271d-149">In PowerShell, set the **DeploymentDebugLogLevel** parameter to All, ResponseContent, or RequestContent.</span></span>

  ```powershell
  New-AzureRmResourceGroupDeployment -ResourceGroupName examplegroup -TemplateFile c:\Azure\Templates\storage.json -DeploymentDebugLogLevel All
  ```

   <span data-ttu-id="f271d-150">Kontrollera begäran innehåll med följande cmdlet:</span><span class="sxs-lookup"><span data-stu-id="f271d-150">Examine the request content with the following cmdlet:</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.request | ConvertTo-Json
  ```

   <span data-ttu-id="f271d-151">Eller innehåll med svaret:</span><span class="sxs-lookup"><span data-stu-id="f271d-151">Or, the response content with:</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName storageonly -ResourceGroupName startgroup).Properties.response | ConvertTo-Json
  ```

   <span data-ttu-id="f271d-152">Den här informationen kan hjälpa dig att avgöra om ett värde i mallen felaktigt anges.</span><span class="sxs-lookup"><span data-stu-id="f271d-152">This information can help you determine whether a value in the template is being incorrectly set.</span></span>

- <span data-ttu-id="f271d-153">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="f271d-153">Azure CLI</span></span>

   <span data-ttu-id="f271d-154">Undersök distributionsåtgärder med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f271d-154">Examine the deployment operations with the following command:</span></span>

  ```azurecli
  az group deployment operation list --resource-group ExampleGroup --name vmlinux
  ```

- <span data-ttu-id="f271d-155">Kapslad mall</span><span class="sxs-lookup"><span data-stu-id="f271d-155">Nested template</span></span>

   <span data-ttu-id="f271d-156">Logga felsökningsinformation för en kapslad mall med det **debugSetting** element.</span><span class="sxs-lookup"><span data-stu-id="f271d-156">To log debug information for a nested template, use the **debugSetting** element.</span></span>

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


## <a name="create-a-troubleshooting-template"></a><span data-ttu-id="f271d-157">Skapa en mall för felsökning</span><span class="sxs-lookup"><span data-stu-id="f271d-157">Create a troubleshooting template</span></span>
<span data-ttu-id="f271d-158">I vissa fall är det enklaste sättet att felsöka mallen för att testa delar av den.</span><span class="sxs-lookup"><span data-stu-id="f271d-158">In some cases, the easiest way to troubleshoot your template is to test parts of it.</span></span> <span data-ttu-id="f271d-159">Du kan skapa en förenklad mall som gör det möjligt att fokusera på den del som du tror orsakar felet.</span><span class="sxs-lookup"><span data-stu-id="f271d-159">You can create a simplified template that enables you to focus on the part that you believe is causing the error.</span></span> <span data-ttu-id="f271d-160">Anta att du får ett fel när du refererar till en resurs.</span><span class="sxs-lookup"><span data-stu-id="f271d-160">For example, suppose you are receiving an error when referencing a resource.</span></span> <span data-ttu-id="f271d-161">Skapa en mall som returnerar den del som orsakar problemet i stället för hantering av en hel mall.</span><span class="sxs-lookup"><span data-stu-id="f271d-161">Rather than dealing with an entire template, create a template that returns the part that may be causing your problem.</span></span> <span data-ttu-id="f271d-162">Det kan hjälpa dig att avgöra om du skickar in rätt parametrar med hjälp av Mallfunktioner korrekt, och hämtar resursen du förväntar dig.</span><span class="sxs-lookup"><span data-stu-id="f271d-162">It can help you determine whether you are passing in the right parameters, using template functions correctly, and getting the resource you expect.</span></span>

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

<span data-ttu-id="f271d-163">Eller anta att det uppstår distributionsfel som du tror är relaterade till felaktigt angiven beroenden.</span><span class="sxs-lookup"><span data-stu-id="f271d-163">Or, suppose you are encountering deployment errors that you believe are related to incorrectly set dependencies.</span></span> <span data-ttu-id="f271d-164">Testa din mall genom att dela upp den i förenklad mallar.</span><span class="sxs-lookup"><span data-stu-id="f271d-164">Test your template by breaking it into simplified templates.</span></span> <span data-ttu-id="f271d-165">Först skapa en mall som distribuerar en enskild resurs (till exempel en SQL Server).</span><span class="sxs-lookup"><span data-stu-id="f271d-165">First, create a template that deploys only a single resource (like a SQL Server).</span></span> <span data-ttu-id="f271d-166">Lägga till en resurs som är beroende av den (till exempel en SQL-databas) när du är säker på att du har den här resursen korrekt definierad.</span><span class="sxs-lookup"><span data-stu-id="f271d-166">When you are sure you have that resource correctly defined, add a resource that depends on it (like a SQL Database).</span></span> <span data-ttu-id="f271d-167">Lägg till andra beroende resurser (till exempel granskningsprinciper) när du har de två resurser korrekt definierad.</span><span class="sxs-lookup"><span data-stu-id="f271d-167">When you have those two resources correctly defined, add other dependent resources (like auditing policies).</span></span> <span data-ttu-id="f271d-168">Ta bort resursgruppen för att se till att du testar rätt beroenden mellan varje test-distribution.</span><span class="sxs-lookup"><span data-stu-id="f271d-168">In between each test deployment, delete the resource group to make sure you adequately testing the dependencies.</span></span> 

## <a name="check-deployment-sequence"></a><span data-ttu-id="f271d-169">Kontrollera distributionen sekvens</span><span class="sxs-lookup"><span data-stu-id="f271d-169">Check deployment sequence</span></span>

<span data-ttu-id="f271d-170">Många distributionsfel inträffa när resurser har distribuerats i ett oväntat sekvens.</span><span class="sxs-lookup"><span data-stu-id="f271d-170">Many deployment errors happen when resources are deployed in an unexpected sequence.</span></span> <span data-ttu-id="f271d-171">Dessa fel uppstår när beroenden inte är korrekt inställda.</span><span class="sxs-lookup"><span data-stu-id="f271d-171">These errors arise when dependencies are not correctly set.</span></span> <span data-ttu-id="f271d-172">När du saknar ett beroende som krävs en resurs som försöker använda ett värde för en annan resurs, men den andra ännu finns inte.</span><span class="sxs-lookup"><span data-stu-id="f271d-172">When you are missing a needed dependency, one resource attempts to use a value for another resource but the other does not yet exist.</span></span> <span data-ttu-id="f271d-173">Du får ett felmeddelande om att en resurs inte hittas.</span><span class="sxs-lookup"><span data-stu-id="f271d-173">You get an error stating that a resource is not found.</span></span> <span data-ttu-id="f271d-174">Du kan stöta på den här typen av fel periodvis eftersom tidpunkten för distribution för varje resurs kan variera.</span><span class="sxs-lookup"><span data-stu-id="f271d-174">You may encounter this type of error intermittently because the deployment time for each resource can vary.</span></span> <span data-ttu-id="f271d-175">Till exempel lyckas din första försöket att distribuera dina resurser eftersom en nödvändig resurs slumpmässigt har slutförts i tid.</span><span class="sxs-lookup"><span data-stu-id="f271d-175">For example, your first attempt to deploy your resources succeeds because a required resource randomly completes in time.</span></span> <span data-ttu-id="f271d-176">Däremot misslyckas din andra försöket eftersom den begärda resursen inte slutfördes i tid.</span><span class="sxs-lookup"><span data-stu-id="f271d-176">However, your second attempt fails because the required resource did not complete in time.</span></span> 

<span data-ttu-id="f271d-177">Men om du vill undvika ange beroenden som inte behövs.</span><span class="sxs-lookup"><span data-stu-id="f271d-177">But, you want to avoid setting dependencies that are not needed.</span></span> <span data-ttu-id="f271d-178">När du har onödiga beroenden förlänga varaktigheten för distributionen genom att förhindra att resurser som inte är beroende av varandra från att distribueras parallellt.</span><span class="sxs-lookup"><span data-stu-id="f271d-178">When you have unnecessary dependencies, you prolong the duration of the deployment by preventing resources that are not dependent on each other from being deployed in parallel.</span></span> <span data-ttu-id="f271d-179">Dessutom kan du skapa Cirkelberoenden som blockerar distributionen.</span><span class="sxs-lookup"><span data-stu-id="f271d-179">In addition, you may create circular dependencies that block the deployment.</span></span> <span data-ttu-id="f271d-180">Den [referens](resource-group-template-functions-resource.md#reference) funktionen skapas en implicit beroende på den refererade resursen när resursen distribueras i samma mall.</span><span class="sxs-lookup"><span data-stu-id="f271d-180">The [reference](resource-group-template-functions-resource.md#reference) function creates an implicit dependency on the referenced resource, when that resource is deployed in the same template.</span></span> <span data-ttu-id="f271d-181">Du kan därför ha flera beroenden än beroenden i den **dependsOn** egenskapen.</span><span class="sxs-lookup"><span data-stu-id="f271d-181">Therefore, you may have more dependencies than the dependencies specified in the **dependsOn** property.</span></span> <span data-ttu-id="f271d-182">Den [resourceId](resource-group-template-functions-resource.md#resourceid) funktionen inte skapa en implicit beroende eller verifiera att resursen finns.</span><span class="sxs-lookup"><span data-stu-id="f271d-182">The [resourceId](resource-group-template-functions-resource.md#resourceid) function does not create an implicit dependency or validate that the resource exists.</span></span>

<span data-ttu-id="f271d-183">När du har beroende problem, som du behöver få insyn i ordningen för distribution av resursen.</span><span class="sxs-lookup"><span data-stu-id="f271d-183">When you encounter dependency problems, you need to gain insight into the order of resource deployment.</span></span> <span data-ttu-id="f271d-184">Visa ordningen på distributionsåtgärder:</span><span class="sxs-lookup"><span data-stu-id="f271d-184">To view the order of deployment operations:</span></span>

1. <span data-ttu-id="f271d-185">Välj distributionshistoriken för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="f271d-185">Select the deployment history for your resource group.</span></span>

   ![Välj distributionshistoriken](./media/resource-manager-troubleshoot-tips/select-deployment.png)

2. <span data-ttu-id="f271d-187">Välj en distribution från historiken och välj **händelser**.</span><span class="sxs-lookup"><span data-stu-id="f271d-187">Select a deployment from the history, and select **Events**.</span></span>

   ![Välj distributionen händelser](./media/resource-manager-troubleshoot-tips/select-deployment-events.png)

3. <span data-ttu-id="f271d-189">Granska sekvens av händelser för varje resurs.</span><span class="sxs-lookup"><span data-stu-id="f271d-189">Examine the sequence of events for each resource.</span></span> <span data-ttu-id="f271d-190">Vara uppmärksam på status för varje åtgärd.</span><span class="sxs-lookup"><span data-stu-id="f271d-190">Pay attention to the status of each operation.</span></span> <span data-ttu-id="f271d-191">Följande bild visar exempelvis tre storage-konton som har distribuerats parallellt.</span><span class="sxs-lookup"><span data-stu-id="f271d-191">For example, the following image shows three storage accounts that deployed in parallel.</span></span> <span data-ttu-id="f271d-192">Observera att tre storage-konton har startats på samma gång.</span><span class="sxs-lookup"><span data-stu-id="f271d-192">Notice that the three storage accounts are started at the same time.</span></span>

   ![Parallell distribution](./media/resource-manager-troubleshoot-tips/deployment-events-parallel.png)

   <span data-ttu-id="f271d-194">Nästa bild visar tre storage-konton som inte har distribuerats parallellt.</span><span class="sxs-lookup"><span data-stu-id="f271d-194">The next image shows three storage accounts that are not deployed in parallel.</span></span> <span data-ttu-id="f271d-195">Andra lagringskontot beror på det första storage-kontot och tredje lagringskontot är beroende av andra lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="f271d-195">The second storage account depends on the first storage account, and the third storage account depends on the second storage account.</span></span> <span data-ttu-id="f271d-196">Därför första lagringskontot startas, accepteras och slutförs innan nästa startas.</span><span class="sxs-lookup"><span data-stu-id="f271d-196">Therefore, the first storage account is started, accepted, and completed before the next is started.</span></span>

   ![sekventiella distribution](./media/resource-manager-troubleshoot-tips/deployment-events-sequence.png)

<span data-ttu-id="f271d-198">Du kan använda samma teknik även för mer komplicerade scenarier för att identifiera när distributionen är igång och klar för varje resurs.</span><span class="sxs-lookup"><span data-stu-id="f271d-198">Even for more complicated scenarios, you can use the same technique to discover when deployment is started and completed for each resource.</span></span> <span data-ttu-id="f271d-199">Titta igenom din distribution händelser att se om sekvensen är annorlunda än förväntat.</span><span class="sxs-lookup"><span data-stu-id="f271d-199">Look through your deployment events to see if the sequence is different than you would expect.</span></span> <span data-ttu-id="f271d-200">I så fall, omvärdera beroenden för den här resursen.</span><span class="sxs-lookup"><span data-stu-id="f271d-200">If so, reevaluate the dependencies for this resource.</span></span>

<span data-ttu-id="f271d-201">Resource Manager identifierar cirkulärt tjänstberoende vid verifiering av mallen.</span><span class="sxs-lookup"><span data-stu-id="f271d-201">Resource Manager identifies circular dependencies during template validation.</span></span> <span data-ttu-id="f271d-202">Den returnerar ett felmeddelande som anger ett cirkelberoende finns.</span><span class="sxs-lookup"><span data-stu-id="f271d-202">It returns an error message that specifically states a circular dependency exists.</span></span> <span data-ttu-id="f271d-203">Du löser ett cirkulärt beroende:</span><span class="sxs-lookup"><span data-stu-id="f271d-203">To solve a circular dependency:</span></span>

1. <span data-ttu-id="f271d-204">Hitta den resurs som identifieras i cirkulärt beroende i mallen.</span><span class="sxs-lookup"><span data-stu-id="f271d-204">In your template, find the resource identified in the circular dependency.</span></span> 
2. <span data-ttu-id="f271d-205">Resursen måste undersöka den **dependsOn** egenskap och några användningsområden för den **referens** funktion för att se vilka resurser som den är beroende av.</span><span class="sxs-lookup"><span data-stu-id="f271d-205">For that resource, examine the **dependsOn** property and any uses of the **reference** function to see which resources it depends on.</span></span> 
3. <span data-ttu-id="f271d-206">Granska dessa resurser om du vill se vilka resurser de beroende av.</span><span class="sxs-lookup"><span data-stu-id="f271d-206">Examine those resources to see which resources they depend on.</span></span> <span data-ttu-id="f271d-207">Följ beroenden tills du ser en resurs som är beroende av den ursprungliga resursen.</span><span class="sxs-lookup"><span data-stu-id="f271d-207">Follow the dependencies until you notice a resource that depends on the original resource.</span></span>
5. <span data-ttu-id="f271d-208">För resurserna som ingår i cirkulärt beroende noggrant undersöka all användning av den **dependsOn** egenskapen för att identifiera eventuella beroenden som inte behövs.</span><span class="sxs-lookup"><span data-stu-id="f271d-208">For the resources involved in the circular dependency, carefully examine all uses of the **dependsOn** property to identify any dependencies that are not needed.</span></span> <span data-ttu-id="f271d-209">Ta bort dessa beroenden.</span><span class="sxs-lookup"><span data-stu-id="f271d-209">Remove those dependencies.</span></span> <span data-ttu-id="f271d-210">Om du inte vet att ett beroende behövs tar du bort den.</span><span class="sxs-lookup"><span data-stu-id="f271d-210">If you are unsure that a dependency is needed, try removing it.</span></span> 
6. <span data-ttu-id="f271d-211">Distribuera mallen.</span><span class="sxs-lookup"><span data-stu-id="f271d-211">Redeploy the template.</span></span>

<span data-ttu-id="f271d-212">Ta bort värden från den **dependsOn** egenskapen kan orsaka fel när du distribuerar mallen.</span><span class="sxs-lookup"><span data-stu-id="f271d-212">Removing values from the **dependsOn** property can cause errors when you deploy the template.</span></span> <span data-ttu-id="f271d-213">Om det uppstår ett fel, lägger du till beroende tillbaka till mallen.</span><span class="sxs-lookup"><span data-stu-id="f271d-213">If you encounter an error, add the dependency back into the template.</span></span> 

<span data-ttu-id="f271d-214">Överväg att flytta en del av logik för distribution till underordnade resurser (till exempel tillägg eller konfigurationsinställningar) om den metoden inte löser cirkulärt beroende.</span><span class="sxs-lookup"><span data-stu-id="f271d-214">If that approach does not solve the circular dependency, consider moving part of your deployment logic into child resources (such as extensions or configuration settings).</span></span> <span data-ttu-id="f271d-215">Konfigurera de underordnade resurserna att distribuera efter resurser som är inblandade i cirkulärt beroende.</span><span class="sxs-lookup"><span data-stu-id="f271d-215">Configure those child resources to deploy after the resources involved in the circular dependency.</span></span> <span data-ttu-id="f271d-216">Anta exempelvis att du distribuerar två virtuella datorer, men du måste ange egenskaper för var och en som refererar till den andra.</span><span class="sxs-lookup"><span data-stu-id="f271d-216">For example, suppose you are deploying two virtual machines but you must set properties on each one that refer to the other.</span></span> <span data-ttu-id="f271d-217">Du kan distribuera dem i följande ordning:</span><span class="sxs-lookup"><span data-stu-id="f271d-217">You can deploy them in the following order:</span></span>

1. <span data-ttu-id="f271d-218">vm1</span><span class="sxs-lookup"><span data-stu-id="f271d-218">vm1</span></span>
2. <span data-ttu-id="f271d-219">vm2</span><span class="sxs-lookup"><span data-stu-id="f271d-219">vm2</span></span>
3. <span data-ttu-id="f271d-220">Tillägg på vm1 beror på vm1 och vm2.</span><span class="sxs-lookup"><span data-stu-id="f271d-220">Extension on vm1 depends on vm1 and vm2.</span></span> <span data-ttu-id="f271d-221">Tillägget anger värden för vm1 får från vm2.</span><span class="sxs-lookup"><span data-stu-id="f271d-221">The extension sets values on vm1 that it gets from vm2.</span></span>
4. <span data-ttu-id="f271d-222">Tillägg på vm2 beror på vm1 och vm2.</span><span class="sxs-lookup"><span data-stu-id="f271d-222">Extension on vm2 depends on vm1 and vm2.</span></span> <span data-ttu-id="f271d-223">Tillägget anger värden för vm2 får från vm1.</span><span class="sxs-lookup"><span data-stu-id="f271d-223">The extension sets values on vm2 that it gets from vm1.</span></span>

<span data-ttu-id="f271d-224">Samma metod fungerar för Apptjänst-appar.</span><span class="sxs-lookup"><span data-stu-id="f271d-224">The same approach works for App Service apps.</span></span> <span data-ttu-id="f271d-225">Överväg att flytta konfigurationsvärden till en underordnad resurs för app-resurs.</span><span class="sxs-lookup"><span data-stu-id="f271d-225">Consider moving configuration values into a child resource of the app resource.</span></span> <span data-ttu-id="f271d-226">Du kan distribuera två web apps i följande ordning:</span><span class="sxs-lookup"><span data-stu-id="f271d-226">You can deploy two web apps in the following order:</span></span>

1. <span data-ttu-id="f271d-227">webapp1</span><span class="sxs-lookup"><span data-stu-id="f271d-227">webapp1</span></span>
2. <span data-ttu-id="f271d-228">webapp2</span><span class="sxs-lookup"><span data-stu-id="f271d-228">webapp2</span></span>
3. <span data-ttu-id="f271d-229">konfigurationen för webapp1 beror på webapp1 och webapp2.</span><span class="sxs-lookup"><span data-stu-id="f271d-229">config for webapp1 depends on webapp1 and webapp2.</span></span> <span data-ttu-id="f271d-230">Den innehåller app-inställningar med värden från webapp2.</span><span class="sxs-lookup"><span data-stu-id="f271d-230">It contains app settings with values from webapp2.</span></span>
4. <span data-ttu-id="f271d-231">konfigurationen för webapp2 beror på webapp1 och webapp2.</span><span class="sxs-lookup"><span data-stu-id="f271d-231">config for webapp2 depends on webapp1 and webapp2.</span></span> <span data-ttu-id="f271d-232">Den innehåller app-inställningar med värden från webapp1.</span><span class="sxs-lookup"><span data-stu-id="f271d-232">It contains app settings with values from webapp1.</span></span>


## <a name="next-steps"></a><span data-ttu-id="f271d-233">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f271d-233">Next steps</span></span>
* <span data-ttu-id="f271d-234">Lösningar på vanliga distributionsfel finns [felsöka vanliga Azure-distribution med Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="f271d-234">For resolutions to common deployment errors, see [Troubleshoot common Azure deployment errors with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="f271d-235">Läs om granskning åtgärder i [granskningsåtgärder med Resource Manager](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="f271d-235">To learn about auditing actions, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="f271d-236">Mer information om åtgärder för att avgöra felen under distributionen, se [visa distributionsåtgärder](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="f271d-236">To learn about actions to determine the errors during deployment, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
