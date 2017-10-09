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
# <a name="view-deployment-operations-with-azure-resource-manager"></a>Visa distributionsåtgärder med Azure Resource Manager


Du kan visa hello åtgärder för en distribution via hello Azure-portalen. Du kanske är mest intresserad visar hello åtgärder när du har tagit emot ett fel under distributionen så att den här artikeln fokuserar på Visa åtgärder som har misslyckats. hello portal tillhandahåller ett gränssnitt som gör att du tooeasily hitta hello fel och fastställa eventuella korrigeringar.

[!INCLUDE [resource-manager-troubleshoot-introduction](../../includes/resource-manager-troubleshoot-introduction.md)]

## <a name="portal"></a>Portalen
toosee hello distributionsåtgärder, Använd hello följande steg:

1. Hello resursgrupp ingår i hello distribution, Observera i hello status för senaste hello-distributionen. Du kan välja den här statusen tooget mer information.
   
    ![Distributionsstatus](./media/resource-manager-deployment-operations/deployment-status.png)
2. Du ser hello senaste distributionshistoriken. Välj hello-distribution som har misslyckats.
   
    ![Distributionsstatus](./media/resource-manager-deployment-operations/select-deployment.png)
3. Välj hello länken toosee en beskrivning av varför hello distributionen misslyckades. Hello DNS-posten är inte unikt i hello bilden nedan.  
   
    ![Visa misslyckad distribution](./media/resource-manager-deployment-operations/view-error.png)
   
    Det här felmeddelandet bör vara tillräckligt för att du toobegin felsökning. Om du vill ha mer information om vilka uppgifter har slutförts kan du visa hello åtgärder enligt hello följande steg.
4. Du kan visa alla hello distributionsåtgärder i hello **distribution** bladet. Välj alla åtgärden toosee mer information.
   
    ![Visa operationer](./media/resource-manager-deployment-operations/view-operations.png)
   
    I så fall måste se du att hello lagringskonto, virtuella nätverk och tillgänglighetsuppsättning har skapats. hello offentliga IP-adressen misslyckades och andra resurser har inte använts.
5. Du kan visa händelser för hello distribution genom att välja **händelser**.
   
    ![Visa händelser](./media/resource-manager-deployment-operations/view-events.png)
6. Du se alla hello händelser för hello distribution och välja något mer information. Observera för hello Korrelations-ID: N. Det här värdet kan vara användbart när du arbetar med teknisk support tootroubleshoot en distribution.
   
    ![Visa händelser](./media/resource-manager-deployment-operations/see-all-events.png)

## <a name="powershell"></a>PowerShell
1. tooget hello övergripande status för en distribution, Använd hello **Get-AzureRmResourceGroupDeployment** kommando. 

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup
  ```

   Eller så kan du filtrera hello resultat för dessa distributioner som har misslyckats.

  ```powershell
  Get-AzureRmResourceGroupDeployment -ResourceGroupName ExampleGroup | Where-Object ProvisioningState -eq Failed
  ```
   
2. Varje distribution innehåller flera åtgärder. Varje åtgärd representerar ett steg i processen för distribution av hello. toodiscover vad som gick fel med en distribution, vanligtvis måste toosee information om hello distributionsåtgärder. Du kan se status för hello hello åtgärder med **Get-AzureRmResourceGroupDeploymentOperation**.

  ```powershell 
  Get-AzureRmResourceGroupDeploymentOperation -ResourceGroupName ExampleGroup -DeploymentName vmDeployment
  ```

    Som returnerar flera åtgärder med var och en i hello följande format:

  ```powershell
  Id             : /subscriptions/{guid}/resourceGroups/ExampleGroup/providers/Microsoft.Resources/deployments/Microsoft.Template/operations/A3EB2DA598E0A780
  OperationId    : A3EB2DA598E0A780
  Properties     : @{provisioningOperation=Create; provisioningState=Succeeded; timestamp=2016-06-14T21:55:15.0156208Z;
                   duration=PT23.0227078S; trackingId=11d376e8-5d6d-4da8-847e-6f23c6443fbf;
                   serviceRequestId=0196828d-8559-4bf6-b6b8-8b9057cb0e23; statusCode=OK; targetResource=}
  PropertiesText : {duration:PT23.0227078S, provisioningOperation:Create, provisioningState:Succeeded,
                   serviceRequestId:0196828d-8559-4bf6-b6b8-8b9057cb0e23...}
  ```

3. tooget mer information om misslyckade åtgärder hämta hello egenskaper för åtgärder med **misslyckades** tillstånd.

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object ProvisioningState -eq Failed
  ```
   
    Som returnerar alla hello misslyckade åtgärder med var och en i hello följande format:

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

    Observera hello serviceRequestId och hello trackingId för hello åtgärden. Hej serviceRequestId kan vara användbart när du arbetar med teknisk support tootroubleshoot en distribution. Du använder hello trackingId i hello nästa steg toofocus på en viss åtgärd.
4. tooget hello statusmeddelande för en viss misslyckade operation, Använd hello följande kommando:

  ```powershell
  ((Get-AzureRmResourceGroupDeploymentOperation -DeploymentName Microsoft.Template -ResourceGroupName ExampleGroup).Properties | Where-Object trackingId -eq f4ed72f8-4203-43dc-958a-15d041e8c233).StatusMessage.error
  ```

    Som returnerar:

  ```powershell
  code           message                                                                        details
  ----           -------                                                                        -------
  DnsRecordInUse DNS record dns.westus.cloudapp.azure.com is already used by another public IP. {}
  ```
4. Alla distributionsåtgärder i Azure inkluderar innehåll för förfrågan och svar. hello begär innehåll är vad du skickat tooAzure under distributionen (till exempel skapa en virtuell dator, OS-disken och andra resurser). hello svar innehållet är Azure skickas tillbaka från din distributionsbegäran om. Under distributionen kan du använda **DeploymentDebugLogLevel** paramenter toospecify som hello begäran och/eller svar bevaras i hello-loggen. 

  Du hämta informationen från hello-loggen och spara den lokalt genom att använda hello följande PowerShell-kommandon:

  ```powershell
  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.request | ConvertTo-Json |  Out-File -FilePath <PathToFile>

  (Get-AzureRmResourceGroupDeploymentOperation -DeploymentName "TestDeployment" -ResourceGroupName "Test-RG").Properties.response | ConvertTo-Json |  Out-File -FilePath <PathToFile>
  ```

## <a name="azure-cli"></a>Azure CLI

1. Hämta hello övergripande status för en distribution med hello **azure-grupp distribution visa** kommando.

  ```azurecli
  azure group deployment show --resource-group ExampleGroup --name ExampleDeployment --json
  ```
  
  En av hello returnerade värden är hello **correlationId**. Det här värdet används tootrack relaterade händelser och kan vara användbart när arbetar med teknisk support tootroubleshoot en distribution.

  ```azurecli
  "properties": {
    "provisioningState": "Failed",
    "correlationId": "4002062a-a506-4b5e-aaba-4147036b771a",
  ```

2. toosee hello åtgärder för en distribution med:

  ```azurecli
  azure group deployment operation list --resource-group ExampleGroup --name ExampleDeployment --json
  ```

## <a name="rest"></a>REST

1. Hämta information om en distribution med hello [få information om en för malldistribution](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_Get) igen.

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}?api-version={api-version}
  ```

    Observera särskilt hello svar hello **provisioningState**, **correlationId**, och **fel** element. Hej **correlationId** används tootrack relaterade händelser och kan vara användbart när arbetar med teknisk support tootroubleshoot en distribution.

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

2. Hämta information om distributionsåtgärder med hello [visa alla distributionsåtgärder för mallen](https://docs.microsoft.com/rest/api/resources/deployments#Deployments_List) igen. 

  ```http
  GET https://management.azure.com/subscriptions/{subscription-id}/resourcegroups/{resource-group-name}/providers/microsoft.resources/deployments/{deployment-name}/operations?$skiptoken={skiptoken}&api-version={api-version}
  ```
   
    hello svaret innehåller begäran och/eller svar information baserat på vad du har angett i hello **debugSetting** egenskapen under distributionen.

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


## <a name="next-steps"></a>Nästa steg
* Hjälp med att lösa viss distributionsfel finns [Lös vanliga fel när du distribuerar tooAzure resurser med Azure Resource Manager](resource-manager-common-deployment-errors.md).
* toolearn om hur du använder hello aktivitet loggar toomonitor andra typer av åtgärder, se [visa aktivitet loggar toomanage Azure resurser](resource-group-audit.md).
* toovalidate distributionen innan du kör den, se [distribuera en resursgrupp med Azure Resource Manager-mall](resource-group-template-deploy.md).

