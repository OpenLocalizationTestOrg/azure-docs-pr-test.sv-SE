---
title: "aaaDeployment åtgärder med Azure Resource Manager | Microsoft Docs"
description: "Beskriver hur tooview Azure Resource Manager distributionsåtgärder med hello portal, PowerShell, Azure CLI och REST-API."
services: azure-resource-manager,virtual-machines
documentationcenter: 
tags: top-support-issue
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-multiple
ms.workload: infrastructure
ms.date: 01/13/2017
ms.author: tomfitz
ms.openlocfilehash: ba4823ca73caca83dfc07c99d736344ef8b7b54d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="view-deployment-operations-with-azure-resource-manager"></a><span data-ttu-id="71b6f-103">Visa distributionsåtgärder med Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="71b6f-103">View deployment operations with Azure Resource Manager</span></span>


<span data-ttu-id="71b6f-104">Du kan visa hello åtgärder för en distribution via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="71b6f-104">You can view hello operations for a deployment through hello Azure portal.</span></span> <span data-ttu-id="71b6f-105">Du kanske är mest intresserad visar hello åtgärder när du har tagit emot ett fel under distributionen så att den här artikeln fokuserar på Visa åtgärder som har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="71b6f-105">You may be most interested in viewing hello operations when you have received an error during deployment so this article focuses on viewing operations that have failed.</span></span> <span data-ttu-id="71b6f-106">hello portal tillhandahåller ett gränssnitt som gör att du tooeasily hitta hello fel och fastställa eventuella korrigeringar.</span><span class="sxs-lookup"><span data-stu-id="71b6f-106">hello portal provides an interface that enables you tooeasily find hello errors and determine potential fixes.</span></span>

[!INCLUDE [resource-manager-troubleshoot-introduction](../../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="portal"></a><span data-ttu-id="71b6f-107">Portalen</span><span class="sxs-lookup"><span data-stu-id="71b6f-107">Portal</span></span>
<span data-ttu-id="71b6f-108">toosee hello distributionsåtgärder, Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="71b6f-108">toosee hello deployment operations, use hello following steps:</span></span>

1. <span data-ttu-id="71b6f-109">Hello resursgrupp ingår i hello distribution, Observera i hello status för senaste hello-distributionen.</span><span class="sxs-lookup"><span data-stu-id="71b6f-109">For hello resource group involved in hello deployment, notice hello status of hello last deployment.</span></span> <span data-ttu-id="71b6f-110">Du kan välja den här statusen tooget mer information.</span><span class="sxs-lookup"><span data-stu-id="71b6f-110">You can select this status tooget more details.</span></span>
   
    ![Distributionsstatus](./media/resource-manager-deployment-operations/deployment-status.png)
2. <span data-ttu-id="71b6f-112">Du ser hello senaste distributionshistoriken.</span><span class="sxs-lookup"><span data-stu-id="71b6f-112">You see hello recent deployment history.</span></span> <span data-ttu-id="71b6f-113">Välj hello-distribution som har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="71b6f-113">Select hello deployment that failed.</span></span>
   
    ![Distributionsstatus](./media/resource-manager-deployment-operations/select-deployment.png)
3. <span data-ttu-id="71b6f-115">Välj hello länken toosee en beskrivning av varför hello distributionen misslyckades.</span><span class="sxs-lookup"><span data-stu-id="71b6f-115">Select hello link toosee a description of why hello deployment failed.</span></span> <span data-ttu-id="71b6f-116">Hello DNS-posten är inte unikt i hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="71b6f-116">In hello image below, hello DNS record is not unique.</span></span>  
   
    ![Visa misslyckad distribution](./media/resource-manager-deployment-operations/view-error.png)
   
    <span data-ttu-id="71b6f-118">Det här felmeddelandet bör vara tillräckligt för att du toobegin felsökning.</span><span class="sxs-lookup"><span data-stu-id="71b6f-118">This error message should be enough for you toobegin troubleshooting.</span></span> <span data-ttu-id="71b6f-119">Om du vill ha mer information om vilka uppgifter har slutförts kan du visa hello åtgärder enligt hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="71b6f-119">However, if you need more details about which tasks were completed, you can view hello operations as shown in hello following steps.</span></span>
4. <span data-ttu-id="71b6f-120">Du kan visa alla hello distributionsåtgärder i hello **distribution** bladet.</span><span class="sxs-lookup"><span data-stu-id="71b6f-120">You can view all hello deployment operations in hello **Deployment** blade.</span></span> <span data-ttu-id="71b6f-121">Välj alla åtgärden toosee mer information.</span><span class="sxs-lookup"><span data-stu-id="71b6f-121">Select any operation toosee more details.</span></span>
   
    ![Visa operationer](./media/resource-manager-deployment-operations/view-operations.png)
   
    <span data-ttu-id="71b6f-123">I så fall måste se du att hello lagringskonto, virtuella nätverk och tillgänglighetsuppsättning har skapats.</span><span class="sxs-lookup"><span data-stu-id="71b6f-123">In this case, you see that hello storage account, virtual network, and availability set were successfully created.</span></span> <span data-ttu-id="71b6f-124">hello offentliga IP-adressen misslyckades och andra resurser har inte använts.</span><span class="sxs-lookup"><span data-stu-id="71b6f-124">hello public IP address failed, and other resources were not attempted.</span></span>
5. <span data-ttu-id="71b6f-125">Du kan visa händelser för hello distribution genom att välja **händelser**.</span><span class="sxs-lookup"><span data-stu-id="71b6f-125">You can view events for hello deployment by selecting **Events**.</span></span>
   
    ![Visa händelser](./media/resource-manager-deployment-operations/view-events.png)
6. <span data-ttu-id="71b6f-127">Du se alla hello händelser för hello distribution och välja något mer information.</span><span class="sxs-lookup"><span data-stu-id="71b6f-127">You see all hello events for hello deployment and select any one for more details.</span></span> <span data-ttu-id="71b6f-128">Observera för hello Korrelations-ID: N.</span><span class="sxs-lookup"><span data-stu-id="71b6f-128">Notice too hello correlation IDs.</span></span> <span data-ttu-id="71b6f-129">Det här värdet kan vara användbart när du arbetar med teknisk support tootroubleshoot en distribution.</span><span class="sxs-lookup"><span data-stu-id="71b6f-129">This value can be helpful when working with technical support tootroubleshoot a deployment.</span></span>
   
    ![Visa händelser](./media/resource-manager-deployment-operations/see-all-events.png)

## <a name="powershell"></a><span data-ttu-id="71b6f-131">PowerShell</span><span class="sxs-lookup"><span data-stu-id="71b6f-131">PowerShell</span></span>
1. <span data-ttu-id="71b6f-132">tooget hello övergripande status för en distribution, Använd hello **Get-AzureRmResourceGroupDeployment** kommando.</span><span class="sxs-lookup"><span data-stu-id="71b6f-132">tooget hello overall status of a deployment, use hello **Get-AzureRmResourceGroupDeployment** command.</span></span> 

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup
  ```

   <span data-ttu-id="71b6f-133">Eller så kan du filtrera hello resultat för dessa distributioner som har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="71b6f-133">Or, you can filter hello results for only those deployments that have failed.</span></span>

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
  ```
   
2. <span data-ttu-id="71b6f-134">Varje distribution innehåller flera åtgärder.</span><span class="sxs-lookup"><span data-stu-id="71b6f-134">Each deployment includes multiple operations.</span></span> <span data-ttu-id="71b6f-135">Varje åtgärd representerar ett steg i processen för distribution av hello.</span><span class="sxs-lookup"><span data-stu-id="71b6f-135">Each operation represents a step in hello deployment process.</span></span> <span data-ttu-id="71b6f-136">toodiscover vad som gick fel med en distribution, vanligtvis måste toosee information om hello distributionsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="71b6f-136">toodiscover what went wrong with a deployment, you usually need toosee details about hello deployment operations.</span></span> <span data-ttu-id="71b6f-137">Du kan se status för hello hello åtgärder med **Get-AzureRmResourceGroupDeploymentOperation**.</span><span class="sxs-lookup"><span data-stu-id="71b6f-137">You can see hello status of hello operations with **Get-AzureRmResourceGroupDeploymentOperation**.</span></span>

  ```powershell 
  Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName vmDeployment
  ```

    <span data-ttu-id="71b6f-138">Som returnerar flera åtgärder med var och en i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="71b6f-138">Which returns multiple operations with each one in hello following format:</span></span>

  ```powershell
  Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
  OperationId    : A3EB2DA598E0A780
  Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                   duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                   serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
  PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                   serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}
  ```

3. <span data-ttu-id="71b6f-139">tooget mer information om misslyckade åtgärder hämta hello egenskaper för åtgärder med **misslyckades** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="71b6f-139">tooget more details about failed operations, retrieve hello properties for operations with **Failed** state.</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
  ```
   
    <span data-ttu-id="71b6f-140">Som returnerar alla hello misslyckade åtgärder med var och en i hello följande format:</span><span class="sxs-lookup"><span data-stu-id="71b6f-140">Which returns all hello failed operations with each one in hello following format:</span></span>

  ```powershell
  provisioningOperation : Create
  provisioningState     : Failed
  timestamp             : 2016-06-14T21:54:55.1468068Z
  duration              : PT3.1449887S
  trackingId            : f4ed72f8-4203-43dc-958a-15d041e8c233
  serviceRequestId      : a426f689-5d5a-448d-a2f0-9784d14c900a
  statusCode            : BadRequest
  statusMessage         : @{error=}
  targetResource        : @{id=/subscriptions/{guid}/resourceGroups/ExampleGroup/providers/
                          Microsoft.Network/publicIPAddresses/myPublicIP;
                          resourceType=Microsoft.Network/publicIPAddresses; resourceName=myPublicIP}
  ```

    <span data-ttu-id="71b6f-141">Observera hello serviceRequestId och hello trackingId för hello åtgärden.</span><span class="sxs-lookup"><span data-stu-id="71b6f-141">Note hello serviceRequestId and hello trackingId for hello operation.</span></span> <span data-ttu-id="71b6f-142">Hej serviceRequestId kan vara användbart när du arbetar med teknisk support tootroubleshoot en distribution.</span><span class="sxs-lookup"><span data-stu-id="71b6f-142">hello serviceRequestId can be helpful when working with technical support tootroubleshoot a deployment.</span></span> <span data-ttu-id="71b6f-143">Du använder hello trackingId i hello nästa steg toofocus på en viss åtgärd.</span><span class="sxs-lookup"><span data-stu-id="71b6f-143">You will use hello trackingId in hello next step toofocus on a particular operation.</span></span>
4. <span data-ttu-id="71b6f-144">tooget hello statusmeddelande för en viss misslyckade operation, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="71b6f-144">tooget hello status message of a particular failed operation, use hello following command:</span></span>

  ```powershell
  ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
  ```

    <span data-ttu-id="71b6f-145">Som returnerar:</span><span class="sxs-lookup"><span data-stu-id="71b6f-145">Which returns:</span></span>

  ```powershell
  code           message                                                                        details
  ----           -------                                                                        -------
  DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}
  ```
4. <span data-ttu-id="71b6f-146">Alla distributionsåtgärder i Azure inkluderar innehåll för förfrågan och svar.</span><span class="sxs-lookup"><span data-stu-id="71b6f-146">Every deployment operation in Azure includes request and response content.</span></span> <span data-ttu-id="71b6f-147">hello begär innehåll är vad du skickat tooAzure under distributionen (till exempel skapa en virtuell dator, OS-disken och andra resurser).</span><span class="sxs-lookup"><span data-stu-id="71b6f-147">hello request content is what you sent tooAzure during deployment (for example, create a VM, OS disk, and other resources).</span></span> <span data-ttu-id="71b6f-148">hello svar innehållet är Azure skickas tillbaka från din distributionsbegäran om.</span><span class="sxs-lookup"><span data-stu-id="71b6f-148">hello response content is what Azure sent back from your deployment request.</span></span> <span data-ttu-id="71b6f-149">Under distributionen kan du använda **DeploymentDebugLogLevel** paramenter toospecify som hello begäran och/eller svar bevaras i hello-loggen.</span><span class="sxs-lookup"><span data-stu-id="71b6f-149">During deployment, you can use **DeploymentDebugLogLevel** paramenter toospecify that hello request and/or response are retained in hello log.</span></span> 

  <span data-ttu-id="71b6f-150">Du hämta informationen från hello-loggen och spara den lokalt genom att använda hello följande PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="71b6f-150">You get that information from hello log, and save it locally by using hello following PowerShell commands:</span></span>

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.request | ConvertTo-Json |  Out-File -FilePath <PathToFile>

  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.response | ConvertTo-Json |  Out-File -FilePath <PathToFile>
  ```

## <a name="azure-cli"></a><span data-ttu-id="71b6f-151">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="71b6f-151">Azure CLI</span></span>

1. <span data-ttu-id="71b6f-152">Hämta hello övergripande status för en distribution med hello **azure-grupp distribution visa** kommando.</span><span class="sxs-lookup"><span data-stu-id="71b6f-152">Get hello overall status of a deployment with hello **azure group deployment show** command.</span></span>

  ```azurecli
  azure group deployment show --resource-group ExampleGroup --name ExampleDeployment --json
  ```
  
  <span data-ttu-id="71b6f-153">En av hello returnerade värden är hello **correlationId**.</span><span class="sxs-lookup"><span data-stu-id="71b6f-153">One of hello returned values is hello **correlationId**.</span></span> <span data-ttu-id="71b6f-154">Det här värdet används tootrack relaterade händelser och kan vara användbart när arbetar med teknisk support tootroubleshoot en distribution.</span><span class="sxs-lookup"><span data-stu-id="71b6f-154">This value is used tootrack related events, and can be helpful when working with technical support tootroubleshoot a deployment.</span></span>

  ```azurecli
  "properties": {
    "provisioningState": "Failed",
    "correlationId": "4002062a-a506-4b5e-aaba-4147036b771a",
  ```

2. <span data-ttu-id="71b6f-155">toosee hello åtgärder för en distribution med:</span><span class="sxs-lookup"><span data-stu-id="71b6f-155">toosee hello operations for a deployment, use:</span></span>

  ```azurecli
  azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json
  ```

## <a name="rest"></a><span data-ttu-id="71b6f-156">REST</span><span class="sxs-lookup"><span data-stu-id="71b6f-156">REST</span></span>

1. <span data-ttu-id="71b6f-157">Hämta information om en distribution med hello [få information om en för malldistribution](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) igen.</span><span class="sxs-lookup"><span data-stu-id="71b6f-157">Get information about a deployment with hello [Get information about a template deployment](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) operation.</span></span>

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
  ```

    <span data-ttu-id="71b6f-158">Observera särskilt hello svar hello **provisioningState**, **correlationId**, och **fel** element.</span><span class="sxs-lookup"><span data-stu-id="71b6f-158">In hello response, note in particular hello **provisioningState**, **correlationId**, and **error** elements.</span></span> <span data-ttu-id="71b6f-159">Hej **correlationId** används tootrack relaterade händelser och kan vara användbart när arbetar med teknisk support tootroubleshoot en distribution.</span><span class="sxs-lookup"><span data-stu-id="71b6f-159">hello **correlationId** is used tootrack related events, and can be helpful when working with technical support tootroubleshoot a deployment.</span></span>

  ```json
  { 
    ...
    "properties": {
      "provisioningState":"Failed",
      "correlationId":"d5062e45-6e9f-4fd3-a0a0-6b2c56b15757",
      ...
      "error":{
        "code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see http://aka.ms/arm-debug for usage details.",
        "details":[{"code":"Conflict","message":"{\r\n  \"error\": {\r\n    \"message\": \"Conflict\",\r\n    \"code\": \"Conflict\"\r\n  }\r\n}"}]
      }  
    }
  }
  ```

2. <span data-ttu-id="71b6f-160">Hämta information om distributionsåtgärder med hello [visa alla distributionsåtgärder för mallen](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List) igen.</span><span class="sxs-lookup"><span data-stu-id="71b6f-160">Get information about deployment operations with hello [List all template deployment operations](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List) operation.</span></span> 

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}
  ```
   
    <span data-ttu-id="71b6f-161">hello svaret innehåller begäran och/eller svar information baserat på vad du har angett i hello **debugSetting** egenskapen under distributionen.</span><span class="sxs-lookup"><span data-stu-id="71b6f-161">hello response includes request and/or response information based on what you specified in hello **debugSetting** property during deployment.</span></span>

  ```json
  {
    ...
    "properties": 
    {
      ...
      "request":{
        "content":{
          "location":"West US",
          "properties":{
            "accountType": "Standard_LRS"
          }
        }
      },
      "response":{
        "content":{
          "error":{
            "message":"Conflict","code":"Conflict"
          }
        }
      }
    }
  }
  ```


## <a name="next-steps"></a><span data-ttu-id="71b6f-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="71b6f-162">Next steps</span></span>
* <span data-ttu-id="71b6f-163">Hjälp med att lösa viss distributionsfel finns [Lös vanliga fel när du distribuerar tooAzure resurser med Azure Resource Manager](resource-manager-common-deployment-errors.md).</span><span class="sxs-lookup"><span data-stu-id="71b6f-163">For help with resolving particular deployment errors, see [Resolve common errors when deploying resources tooAzure with Azure Resource Manager](resource-manager-common-deployment-errors.md).</span></span>
* <span data-ttu-id="71b6f-164">toolearn om hur du använder hello aktivitet loggar toomonitor andra typer av åtgärder, se [visa aktivitet loggar toomanage Azure resurser](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="71b6f-164">toolearn about using hello activity logs toomonitor other types of actions, see [View activity logs toomanage Azure resources](resource-group-audit.md).</span></span>
* <span data-ttu-id="71b6f-165">toovalidate distributionen innan du kör den, se [distribuera en resursgrupp med Azure Resource Manager-mall](resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="71b6f-165">toovalidate your deployment before executing it, see [Deploy a resource group with Azure Resource Manager template](resource-group-template-deploy.md).</span></span>

