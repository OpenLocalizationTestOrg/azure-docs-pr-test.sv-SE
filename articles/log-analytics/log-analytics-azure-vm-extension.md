---
title: aaaConnect virtuella Azure-datorer tooLog Analytics | Microsoft Docs
description: "För Windows och Linux virtuella datorer som körs i Azure, hello rekommenderat sätt insamlade loggar och mått är genom att installera hello Log Analytics Azure VM-tillägget. Du kan använda hello Azure-portalen eller PowerShell tooinstall hello logganalys tillägg för virtuell dator på virtuella Azure-datorer."
services: log-analytics
documentationcenter: 
author: richrundmsft
manager: ewinner
editor: 
ms.assetid: ca39e586-a6af-42fe-862e-80978a58d9b1
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2017
ms.author: richrund
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ac96c242d03ed3a22ca96368e5a8cc53f9a993db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-azure-virtual-machines-toolog-analytics-with-a-log-analytics-agent"></a><span data-ttu-id="51ec1-104">Anslut virtuella datorer i Azure tooLog Analytics med en Log Analytics-agent</span><span class="sxs-lookup"><span data-stu-id="51ec1-104">Connect Azure virtual machines tooLog Analytics with a Log Analytics agent</span></span>

<span data-ttu-id="51ec1-105">Hello rekommenderad metod för insamling av loggar och mått är genom att installera hello logganalys agent för Windows och Linux-datorer.</span><span class="sxs-lookup"><span data-stu-id="51ec1-105">For Windows and Linux computers, hello recommended method for collecting logs and metrics is by installing hello Log Analytics agent.</span></span>

<span data-ttu-id="51ec1-106">hello enklaste sättet tooinstall hello logganalys-agenten på Azure virtual machines är via hello Log Analytics VM-tillägget.</span><span class="sxs-lookup"><span data-stu-id="51ec1-106">hello easiest way tooinstall hello Log Analytics agent on Azure virtual machines is through hello Log Analytics VM Extension.</span></span>  <span data-ttu-id="51ec1-107">Med hjälp av hello tillägget förenklar hello installationsprocessen och konfigurerar automatiskt hello agent toosend data toohello logganalys-arbetsytan som du anger.</span><span class="sxs-lookup"><span data-stu-id="51ec1-107">Using hello extension simplifies hello installation process and automatically configures hello agent toosend data toohello Log Analytics workspace that you specify.</span></span> <span data-ttu-id="51ec1-108">hello-agenten uppgraderas också automatiskt, se till att du har hello senaste funktioner och korrigeringar.</span><span class="sxs-lookup"><span data-stu-id="51ec1-108">hello agent is also upgraded automatically, ensuring that you have hello latest features and fixes.</span></span>

<span data-ttu-id="51ec1-109">För Windows-datorer som du aktiverar hello *Microsoft Monitoring Agent* tillägg för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="51ec1-109">For Windows virtual machines, you enable hello *Microsoft Monitoring Agent* virtual machine extension.</span></span>
<span data-ttu-id="51ec1-110">För Linux virtuella datorer som du aktiverar hello *OMS-Agent för Linux* tillägg för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="51ec1-110">For Linux virtual machines, you enable hello *OMS Agent For Linux* virtual machine extension.</span></span>

<span data-ttu-id="51ec1-111">Lär dig mer om [Azure virtuella datortillägg](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) och hello [Linux-agenten](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="51ec1-111">Learn more about [Azure virtual machine extensions](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) and hello [Linux agent](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="51ec1-112">När du använder agent-baserade samling för loggdata, måste du konfigurera [datakällor i logganalys](log-analytics-data-sources.md) toospecify hello loggar och mått som du vill toocollect.</span><span class="sxs-lookup"><span data-stu-id="51ec1-112">When you use agent-based collection for log data, you must configure [data sources in Log Analytics](log-analytics-data-sources.md) toospecify hello logs and metrics that you want toocollect.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="51ec1-113">Om du konfigurerar logganalys tooindex loggdata genom [Azure diagnostics](log-analytics-azure-storage.md), och du kan konfigurera hello agent toocollect hello samma loggar sedan hello loggar samlas två gånger.</span><span class="sxs-lookup"><span data-stu-id="51ec1-113">If you configure Log Analytics tooindex log data by using [Azure diagnostics](log-analytics-azure-storage.md), and you configure hello agent toocollect hello same logs, then hello logs are collected twice.</span></span> <span data-ttu-id="51ec1-114">Du debiteras för båda datakällor.</span><span class="sxs-lookup"><span data-stu-id="51ec1-114">You are charged for both data sources.</span></span> <span data-ttu-id="51ec1-115">Om du har hello-agenten är installerad kan samla in loggdata med hjälp av hello-agenten - Konfigurera inte logganalys toocollect loggdata från Azure-diagnostik.</span><span class="sxs-lookup"><span data-stu-id="51ec1-115">If you have hello agent installed, then collect log data by using only hello agent - don't configure Log Analytics toocollect log data from Azure diagnostics.</span></span>
>
>

<span data-ttu-id="51ec1-116">Det finns tre sätt tillägg för virtuell dator tooenable hello logganalys:</span><span class="sxs-lookup"><span data-stu-id="51ec1-116">There are three easy ways tooenable hello Log Analytics virtual machine extension:</span></span>

* <span data-ttu-id="51ec1-117">Med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="51ec1-117">By using hello Azure portal</span></span>
* <span data-ttu-id="51ec1-118">Med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="51ec1-118">By using Azure PowerShell</span></span>
* <span data-ttu-id="51ec1-119">Med hjälp av en Azure Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="51ec1-119">By using an Azure Resource Manager template</span></span>

## <a name="enable-hello-vm-extension-in-hello-azure-portal"></a><span data-ttu-id="51ec1-120">Aktivera hello VM-tillägget hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="51ec1-120">Enable hello VM extension in hello Azure portal</span></span>
<span data-ttu-id="51ec1-121">Du kan installera hello agent för Log Analytics och ansluta hello Azure-dator som körs med hjälp av hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="51ec1-121">You can install hello agent for Log Analytics and connect hello Azure virtual machine that it runs on by using hello [Azure portal](https://portal.azure.com).</span></span>

### <a name="tooinstall-hello-log-analytics-agent-and-connect-hello-virtual-machine-tooa-log-analytics-workspace"></a><span data-ttu-id="51ec1-122">tooinstall hello logganalys agent och ansluta hello virtuella tooa logganalys-arbetsytan</span><span class="sxs-lookup"><span data-stu-id="51ec1-122">tooinstall hello Log Analytics agent and connect hello virtual machine tooa Log Analytics workspace</span></span>
1. <span data-ttu-id="51ec1-123">Logga in på hello [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="51ec1-123">Sign into hello [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="51ec1-124">Välj **Bläddra** på hello vänster sida av hello portalen och sedan gå för**logganalys (OMS)** och markera den.</span><span class="sxs-lookup"><span data-stu-id="51ec1-124">Select **Browse** on hello left side of hello portal, and then go too**Log Analytics (OMS)** and select it.</span></span>
3. <span data-ttu-id="51ec1-125">Välj hello något som du vill toouse med hello Azure VM i din lista över logganalys arbetsytor.</span><span class="sxs-lookup"><span data-stu-id="51ec1-125">In your list of Log Analytics workspaces, select hello one that you want toouse with hello Azure VM.</span></span>  
   ![OMS arbetsytor](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4. <span data-ttu-id="51ec1-127">Under **logga analytics management**väljer **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="51ec1-127">Under **Log analytics management**, select **Virtual machines**.</span></span>  
   <span data-ttu-id="51ec1-128">![Virtuella datorer](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)</span><span class="sxs-lookup"><span data-stu-id="51ec1-128">![Virtual machines](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)</span></span>
5. <span data-ttu-id="51ec1-129">I hello lista över **virtuella datorer**, Välj hello virtuell dator som du vill använda tooinstall hello agent.</span><span class="sxs-lookup"><span data-stu-id="51ec1-129">In hello list of **Virtual machines**, select hello virtual machine on which you want tooinstall hello agent.</span></span> <span data-ttu-id="51ec1-130">Hej **OMS anslutningsstatus** för hello VM anger att den är **inte ansluten**.</span><span class="sxs-lookup"><span data-stu-id="51ec1-130">hello **OMS connection status** for hello VM indicates that it is **Not connected**.</span></span>  
   <span data-ttu-id="51ec1-131">![VM inte ansluten](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)</span><span class="sxs-lookup"><span data-stu-id="51ec1-131">![VM not connected](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)</span></span>
6. <span data-ttu-id="51ec1-132">I hello information för den virtuella datorn, väljer **Anslut**.</span><span class="sxs-lookup"><span data-stu-id="51ec1-132">In hello details for your virtual machine, select **Connect**.</span></span> <span data-ttu-id="51ec1-133">hello-agent installeras och konfigureras för logganalys-arbetsytan automatiskt.</span><span class="sxs-lookup"><span data-stu-id="51ec1-133">hello agent is automatically installed and configured for your Log Analytics workspace.</span></span> <span data-ttu-id="51ec1-134">Den här processen tar några minuter, under vilken tid hello OMS Anslutningsstatus är *ansluta...*</span><span class="sxs-lookup"><span data-stu-id="51ec1-134">This process takes a few minutes, during which time hello OMS Connection status is *Connecting...*</span></span>  
   <span data-ttu-id="51ec1-135">![Anslut VM](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)</span><span class="sxs-lookup"><span data-stu-id="51ec1-135">![Connect VM](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)</span></span>
7. <span data-ttu-id="51ec1-136">När du installerar och ansluta hello agent hello **OMS anslutning** status kommer att vara uppdaterade tooshow **arbetsytan**.</span><span class="sxs-lookup"><span data-stu-id="51ec1-136">After you install and connect hello agent, hello **OMS connection** status will be updated tooshow **This workspace**.</span></span>  
   <span data-ttu-id="51ec1-137">![Ansluten](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)</span><span class="sxs-lookup"><span data-stu-id="51ec1-137">![Connected](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)</span></span>

## <a name="enable-hello-vm-extension-using-powershell"></a><span data-ttu-id="51ec1-138">Aktivera hello VM-tillägget med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="51ec1-138">Enable hello VM extension using PowerShell</span></span>
<span data-ttu-id="51ec1-139">När du konfigurerar den virtuella datorn med hjälp av PowerShell måste tooprovide hello **workspaceId** och **workspaceKey**.</span><span class="sxs-lookup"><span data-stu-id="51ec1-139">When you configure your virtual machine by using PowerShell, you need tooprovide hello **workspaceId** and **workspaceKey**.</span></span> <span data-ttu-id="51ec1-140">hello egenskapsnamnen i json-konfigurationen är **skiftlägeskänsliga**.</span><span class="sxs-lookup"><span data-stu-id="51ec1-140">hello property names in your json configuration are **case-sensitive**.</span></span>

<span data-ttu-id="51ec1-141">Du kan hitta hello Id och nyckel på hello **inställningar** hello OMS-portalen eller med hjälp av PowerShell som visas i föregående exempel hello.</span><span class="sxs-lookup"><span data-stu-id="51ec1-141">You can find hello Id and key on hello **Settings** page of hello OMS portal, or by using PowerShell as shown in hello preceding example.</span></span>

![Arbetsyte-ID och primärnyckel](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

<span data-ttu-id="51ec1-143">Det finns olika kommandon för Azure klassiska virtuella datorer och hanteraren för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="51ec1-143">There are different commands for Azure classic virtual machines and Resource Manager virtual machines.</span></span> <span data-ttu-id="51ec1-144">Följande är exempel på både virtuella datorer för Resource Manager och klassisk.</span><span class="sxs-lookup"><span data-stu-id="51ec1-144">Following are examples for both classic and Resource Manager virtual machines.</span></span>

<span data-ttu-id="51ec1-145">Använd hello följande PowerShell-exempel för klassiska virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="51ec1-145">For classic virtual machines, use hello following PowerShell example:</span></span>

```PowerShell
Add-AzureAccount

$workspaceId = "enter workspace ID here"
$workspaceKey = "enter workspace key here"
$hostedService = "enter hosted service here"

$vm = Get-AzureVM –ServiceName $hostedService

# For Windows VM uncomment hello following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'MicrosoftMonitoringAgent' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose

# For Linux VM uncomment hello following line
# Set-AzureVMExtension -VM $vm -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionName 'OmsAgentForLinux' -Version '1.*' -PublicConfiguration "{'workspaceId': '$workspaceId'}" -PrivateConfiguration "{'workspaceKey': '$workspaceKey' }" | Update-AzureVM -Verbose
```

<span data-ttu-id="51ec1-146">För Resource Manager Linux virtuella datorer med hjälp av hello följande CLI</span><span class="sxs-lookup"><span data-stu-id="51ec1-146">For Resource Manager Linux VMs using hello following CLI</span></span>
```azurecli
az vm extension set --resource-group myRGMonitor --vm-name myMonitorVM --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --version 1.3 --protected-settings ‘{"workspaceKey": "<workspace-key>"}’ --settings ‘{"workspaceId": "<workspace-id>"}’ 
```

<span data-ttu-id="51ec1-147">Använd hello följande PowerShell-exempel för hanteraren för virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="51ec1-147">For Resource Manager virtual machines, use hello following PowerShell example:</span></span>

```PowerShell
Login-AzureRMAccount
Select-AzureRMSubscription -SubscriptionId "**"

$workspaceName = "your workspace name"
$VMresourcegroup = "**"
$VMresourcename = "**"

$workspace = (Get-AzureRmOperationalInsightsWorkspace).Where({$_.Name -eq $workspaceName})

if ($workspace.Name -ne $workspaceName)
{
    Write-Error "Unable toofind OMS Workspace $workspaceName. Do you need toorun Select-AzureRMSubscription?"
}

$workspaceId = $workspace.CustomerId
$workspaceKey = (Get-AzureRmOperationalInsightsWorkspaceSharedKeys -ResourceGroupName $workspace.ResourceGroupName -Name $workspace.Name).PrimarySharedKey

$vm = Get-AzureRmVM -ResourceGroupName $VMresourcegroup -Name $VMresourcename
$location = $vm.Location

# For Windows VM uncomment hello following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'MicrosoftMonitoringAgent' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'MicrosoftMonitoringAgent' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"

# For Linux VM uncomment hello following line
# Set-AzureRmVMExtension -ResourceGroupName $VMresourcegroup -VMName $VMresourcename -Name 'OmsAgentForLinux' -Publisher 'Microsoft.EnterpriseCloud.Monitoring' -ExtensionType 'OmsAgentForLinux' -TypeHandlerVersion '1.0' -Location $location -SettingString "{'workspaceId': '$workspaceId'}" -ProtectedSettingString "{'workspaceKey': '$workspaceKey'}"


```


## <a name="deploy-hello-vm-extension-using-a-template"></a><span data-ttu-id="51ec1-148">Distribuera hello VM-tillägget med hjälp av en mall</span><span class="sxs-lookup"><span data-stu-id="51ec1-148">Deploy hello VM extension using a template</span></span>
<span data-ttu-id="51ec1-149">Du kan skapa en mall (i JSON-format) som definierar hello distributionen och konfigurationen av ditt program med hjälp av Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="51ec1-149">By using Azure Resource Manager, you can create a template (in JSON format) that defines hello deployment and configuration of your application.</span></span> <span data-ttu-id="51ec1-150">Den här mallen kallas för en Resource Manager-mall och tillhandahåller en deklarativ metod toodefine distribution.</span><span class="sxs-lookup"><span data-stu-id="51ec1-150">This template is known as a Resource Manager template and provides a declarative way toodefine deployment.</span></span> <span data-ttu-id="51ec1-151">Genom att använda en mall kan du upprepade gånger distribuera ditt program i hela hello applivscykeln och vara säker på att dina resurser distribueras i ett konsekvent tillstånd.</span><span class="sxs-lookup"><span data-stu-id="51ec1-151">By using a template, you can repeatedly deploy your application throughout hello app lifecycle and have confidence that your resources are being deployed in a consistent state.</span></span>

<span data-ttu-id="51ec1-152">Genom att ange hello logganalys-agenten som en del av Resource Manager-mall kan du se till att varje virtuell dator är förkonfigurerade tooreport tooyour logganalys-arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="51ec1-152">By including hello Log Analytics agent as part of your Resource Manager template, you can ensure that each virtual machine is pre-configured tooreport tooyour Log Analytics workspace.</span></span>

<span data-ttu-id="51ec1-153">Mer information om Resource Manager-mallar finns [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="51ec1-153">For more information about Resource Manager templates, see [Authoring Azure Resource Manager templates](../azure-resource-manager/resource-group-authoring-templates.md).</span></span>

<span data-ttu-id="51ec1-154">Följande är ett exempel på en Resource Manager-mall som används för att distribuera en virtuell dator som kör Windows hello tillägg för Microsoft Monitoring Agent som installeras.</span><span class="sxs-lookup"><span data-stu-id="51ec1-154">Following is an example of a Resource Manager template that's used for deploying a virtual machine that's running Windows with hello Microsoft Monitoring Agent extension installed.</span></span> <span data-ttu-id="51ec1-155">Den här mallen är en mall för vanliga virtuella datorer med hello följande tillägg:</span><span class="sxs-lookup"><span data-stu-id="51ec1-155">This template is a typical virtual machine template, with hello following additions:</span></span>

* <span data-ttu-id="51ec1-156">parametrarna workspaceId och workspaceName</span><span class="sxs-lookup"><span data-stu-id="51ec1-156">workspaceId and workspaceName parameters</span></span>
* <span data-ttu-id="51ec1-157">Avsnittet Microsoft.EnterpriseCloud.Monitoring tillägg</span><span class="sxs-lookup"><span data-stu-id="51ec1-157">Microsoft.EnterpriseCloud.Monitoring resource extension section</span></span>
* <span data-ttu-id="51ec1-158">Utdata toolook hello workspaceId och workspaceSharedKey</span><span class="sxs-lookup"><span data-stu-id="51ec1-158">Outputs toolook up hello workspaceId and workspaceSharedKey</span></span>

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "adminUsername": {
      "type": "string",
      "metadata": {
        "description": "Username for hello Virtual Machine."
      }
    },
    "adminPassword": {
      "type": "securestring",
      "metadata": {
        "description": "Password for hello Virtual Machine."
      }
    },
    "dnsLabelPrefix": {
       "type": "string",
       "metadata": {
          "description": "DNS Label for hello Public IP. Must be lowercase. It should match with hello following regular expression: ^[a-z][a-z0-9-]{1,61}[a-z0-9]$ or it will raise an error."
       }
    },
    "workspaceId": {
      "type": "string",
      "metadata": {
        "description": "OMS workspace ID"
      }
    },
    "workspaceName": {
      "type": "string",
      "metadata": {
         "description": "OMS workspace name"
      }
    },
    "windowsOSVersion": {
      "type": "string",
      "defaultValue": "2012-R2-Datacenter",
      "allowedValues": [
        "2008-R2-SP1",
        "2012-Datacenter",
        "2012-R2-Datacenter",
        "Windows-Server-Technical-Preview"
      ],
      "metadata": {
        "description": "hello Windows version for hello VM. This will pick a fully patched image of this given Windows version. Allowed values: 2008-R2-SP1, 2012-Datacenter, 2012-R2-Datacenter, Windows-Server-Technical-Preview."
      }
    }
  },
  "variables": {
    "storageAccountName": "[concat(uniquestring(resourceGroup().id), 'standardsa')]",
    "apiVersion": "2015-06-15",
    "imagePublisher": "MicrosoftWindowsServer",
    "imageOffer": "WindowsServer",
    "OSDiskName": "osdiskforwindowssimple",
    "nicName": "myVMNic",
    "addressPrefix": "10.0.0.0/16",
    "subnetName": "Subnet",
    "subnetPrefix": "10.0.0.0/24",
    "storageAccountType": "Standard_LRS",
    "publicIPAddressName": "myPublicIP",
    "publicIPAddressType": "Dynamic",
    "vmStorageAccountContainerName": "vhds",
    "vmName": "MyWindowsVM",
    "vmSize": "Standard_DS1",
    "virtualNetworkName": "MyVNET",
    "resourceId": "[resourceGroup().id]",
    "vnetID": "[resourceId('Microsoft.Network/virtualNetworks',variables('virtualNetworkName'))]",
    "subnetRef": "[concat(variables('vnetID'),'/subnets/',variables('subnetName'))]"
  },
  "resources": [
    {
      "type": "Microsoft.Storage/storageAccounts",
      "name": "[variables('storageAccountName')]",
      "apiVersion": "[variables('apiVersion')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "accountType": "[variables('storageAccountType')]"
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/publicIPAddresses",
      "name": "[variables('publicIPAddressName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "publicIPAllocationMethod": "[variables('publicIPAddressType')]",
        "dnsSettings": {
          "domainNameLabel": "[parameters('dnsLabelPrefix')]"
        }
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/virtualNetworks",
      "name": "[variables('virtualNetworkName')]",
      "location": "[resourceGroup().location]",
      "properties": {
        "addressSpace": {
          "addressPrefixes": [
            "[variables('addressPrefix')]"
          ]
        },
        "subnets": [
          {
            "name": "[variables('subnetName')]",
            "properties": {
              "addressPrefix": "[variables('subnetPrefix')]"
            }
          }
        ]
      }
    },
    {
      "apiVersion": "[variables('apiVersion')]",
      "type": "Microsoft.Network/networkInterfaces",
      "name": "[variables('nicName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Network/publicIPAddresses/', variables('publicIPAddressName'))]",
        "[concat('Microsoft.Network/virtualNetworks/', variables('virtualNetworkName'))]"
      ],
      "properties": {
        "ipConfigurations": [
          {
            "name": "ipconfig1",
            "properties": {
              "privateIPAllocationMethod": "Dynamic",
              "publicIPAddress": {
                "id": "[resourceId('Microsoft.Network/publicIPAddresses',variables('publicIPAddressName'))]"
              },
              "subnet": {
                "id": "[variables('subnetRef')]"
              }
            }
          }
        ]
      }
    },
    {
      "apiVersion": "2015-06-15",
      "type": "Microsoft.Compute/virtualMachines",
      "name": "[variables('vmName')]",
      "location": "[resourceGroup().location]",
      "dependsOn": [
        "[concat('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[concat('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
      ],
      "properties": {
        "hardwareProfile": {
          "vmSize": "[variables('vmSize')]"
        },
        "osProfile": {
          "computername": "[variables('vmName')]",
          "adminUsername": "[parameters('adminUsername')]",
          "adminPassword": "[parameters('adminPassword')]"
        },
        "storageProfile": {
          "imageReference": {
            "publisher": "[variables('imagePublisher')]",
            "offer": "[variables('imageOffer')]",
            "sku": "[parameters('windowsOSVersion')]",
            "version": "latest"
          },
          "osDisk": {
            "name": "osdisk",
            "vhd": {
              "uri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net/',variables('vmStorageAccountContainerName'),'/',variables('OSDiskName'),'.vhd')]"
            },
            "caching": "ReadWrite",
            "createOption": "FromImage"
          }
        },
        "networkProfile": {
          "networkInterfaces": [
            {
              "id": "[resourceId('Microsoft.Network/networkInterfaces',variables('nicName'))]"
            }
          ]
        },
        "diagnosticsProfile": {
          "bootDiagnostics": {
             "enabled": "true",
             "storageUri": "[concat('http://',variables('storageAccountName'),'.blob.core.windows.net')]"
          }
        }
      },
      "resources": [
        {
          "type": "extensions",
          "name": "Microsoft.EnterpriseCloud.Monitoring",
          "apiVersion": "[variables('apiVersion')]",
          "location": "[resourceGroup().location]",
          "dependsOn": [
            "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
          ],
          "properties": {
            "publisher": "Microsoft.EnterpriseCloud.Monitoring",
            "type": "MicrosoftMonitoringAgent",
            "typeHandlerVersion": "1.0",
            "autoUpgradeMinorVersion": true,
            "settings": {
              "workspaceId": "[parameters('workspaceId')]"
            },
            "protectedSettings": {
              "workspaceKey": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces', parameters('workspaceName')), '2015-03-20').primarySharedKey]"
            }
          }
        }
      ]
    }
  ],
  "outputs": {
      "sharedKeyOutput": {
         "value": "[listKeys(resourceId('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').primarySharedKey]",
         "type": "string"
      },
      "workspaceIdOutput": {
         "value": "[reference(concat('Microsoft.OperationalInsights/workspaces/', parameters('workspaceName')), '2015-03-20').customerId]",
        "type" : "string"
      }
  }
}
```

<span data-ttu-id="51ec1-159">Du kan distribuera en mall med hjälp av hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="51ec1-159">You can deploy a template by using hello following PowerShell command:</span></span>

```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-hello-log-analytics-vm-extension"></a><span data-ttu-id="51ec1-160">Felsöka hello Log Analytics VM-tillägget</span><span class="sxs-lookup"><span data-stu-id="51ec1-160">Troubleshooting hello Log Analytics VM extension</span></span>
<span data-ttu-id="51ec1-161">Vanligtvis meddelande ett när saker inte fungerar från Azure-portalen eller Azure powershell.</span><span class="sxs-lookup"><span data-stu-id="51ec1-161">Usually you receive a message when things don't work, from either Azure portal or Azure powershell.</span></span>

1. <span data-ttu-id="51ec1-162">Logga in på hello [Azure-portalen](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="51ec1-162">Sign into hello [Azure portal](http://portal.azure.com).</span></span>
2. <span data-ttu-id="51ec1-163">Hitta hello VM och öppna VM information.</span><span class="sxs-lookup"><span data-stu-id="51ec1-163">Find hello VM and open VM details.</span></span>
3. <span data-ttu-id="51ec1-164">Klicka på **tillägg** toocheck om OMS-tillägget har aktiverats eller inte.</span><span class="sxs-lookup"><span data-stu-id="51ec1-164">Click **Extensions** toocheck if OMS extension is enabled or not.</span></span>

   ![VM-tillägg View](./media/log-analytics-azure-vm-extension/oms-vmview-extensions.png)

4. <span data-ttu-id="51ec1-166">Klicka på hello *MicrosoftMonitoringAgent*(Windows) eller *OmsAgentForLinux*(Linux)-tillägget och visa information.</span><span class="sxs-lookup"><span data-stu-id="51ec1-166">Click hello *MicrosoftMonitoringAgent*(Windows) or *OmsAgentForLinux*(Linux) extension and view details.</span></span> 

   ![Information för VM-tillägg](./media/log-analytics-azure-vm-extension/oms-vmview-extensiondetails.png)

### <a name="troubleshooting-windows-virtual-machines"></a><span data-ttu-id="51ec1-168">Felsöka Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="51ec1-168">Troubleshooting Windows Virtual Machines</span></span>
<span data-ttu-id="51ec1-169">Om hello *Microsoft Monitoring Agent* tillägg för virtuell dator-agenten installeras inte eller rapporter, kan du utföra följande steg tootroubleshoot hello problemet hello.</span><span class="sxs-lookup"><span data-stu-id="51ec1-169">If hello *Microsoft Monitoring Agent* VM agent extension is not installing or reporting, you can perform hello following steps tootroubleshoot hello issue.</span></span>

1. <span data-ttu-id="51ec1-170">Kontrollera om hello Azure VM-agenten är installerad och fungerar korrekt med hello stegen i [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).</span><span class="sxs-lookup"><span data-stu-id="51ec1-170">Check if hello Azure VM agent is installed and working correctly by using hello steps in [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).</span></span>
   * <span data-ttu-id="51ec1-171">Du kan också granska loggfilen för hello VM-agent`C:\WindowsAzure\logs\WaAppAgent.log`</span><span class="sxs-lookup"><span data-stu-id="51ec1-171">You can also review hello VM agent log file `C:\WindowsAzure\logs\WaAppAgent.log`</span></span>
   * <span data-ttu-id="51ec1-172">Hello VM-agenten installeras inte om hello loggen inte finns.</span><span class="sxs-lookup"><span data-stu-id="51ec1-172">If hello log does not exist, hello VM agent is not installed.</span></span>
     * [<span data-ttu-id="51ec1-173">Installera hello Azure VM-agenten på klassiska virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="51ec1-173">Install hello Azure VM Agent on classic VMs</span></span>](../virtual-machines/windows/classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
2. <span data-ttu-id="51ec1-174">Bekräfta hello Microsoft Monitoring Agent tillägget pulsslag aktiviteten körs med hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="51ec1-174">Confirm hello Microsoft Monitoring Agent extension heartbeat task is running using hello following steps:</span></span>
   * <span data-ttu-id="51ec1-175">Logga in toohello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="51ec1-175">Log in toohello virtual machine</span></span>
   * <span data-ttu-id="51ec1-176">Öppna Schemaläggaren och hitta hello `update_azureoperationalinsight_agent_heartbeat` aktivitet</span><span class="sxs-lookup"><span data-stu-id="51ec1-176">Open task scheduler and find hello `update_azureoperationalinsight_agent_heartbeat` task</span></span>
   * <span data-ttu-id="51ec1-177">Bekräfta hello aktiviteten är aktiverad och körs varje en minut</span><span class="sxs-lookup"><span data-stu-id="51ec1-177">Confirm hello task is enabled and is running every one minute</span></span>
   * <span data-ttu-id="51ec1-178">Kontrollera hello pulsslag logfile i`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`</span><span class="sxs-lookup"><span data-stu-id="51ec1-178">Check hello heartbeat logfile in `C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`</span></span>
3. <span data-ttu-id="51ec1-179">Granska loggfilerna för hello Microsoft Monitoring Agent VM-tillägget i`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`</span><span class="sxs-lookup"><span data-stu-id="51ec1-179">Review hello Microsoft Monitoring Agent VM extension log files in `C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`</span></span>
4. <span data-ttu-id="51ec1-180">Se till att hello virtuella datorn kan köra PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="51ec1-180">Ensure hello virtual machine can run PowerShell scripts</span></span>
5. <span data-ttu-id="51ec1-181">Kontrollera behörigheter på C:\Windows\temp inte har ändrats</span><span class="sxs-lookup"><span data-stu-id="51ec1-181">Ensure permissions on C:\Windows\temp haven’t been changed</span></span>
6. <span data-ttu-id="51ec1-182">Visa hello status för hello Microsoft Monitoring Agent genom att skriva följande kommando i ett upphöjt PowerShell-fönster på den virtuella datorn hello hello`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`</span><span class="sxs-lookup"><span data-stu-id="51ec1-182">View hello status of hello Microsoft Monitoring Agent by typing hello following command in an elevated PowerShell window on hello virtual machine `  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`</span></span>
7. <span data-ttu-id="51ec1-183">Granska installationsloggfilerna för hello Microsoft Monitoring Agent i`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`</span><span class="sxs-lookup"><span data-stu-id="51ec1-183">Review hello Microsoft Monitoring Agent setup log files in `C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`</span></span>

<span data-ttu-id="51ec1-184">Mer information finns i [felsökning av Windows-tillägg](../virtual-machines/windows/extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="51ec1-184">For more information, see [troubleshooting Windows extensions](../virtual-machines/windows/extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

### <a name="troubleshooting-linux-virtual-machines"></a><span data-ttu-id="51ec1-185">Felsökning av virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="51ec1-185">Troubleshooting Linux Virtual Machines</span></span>
<span data-ttu-id="51ec1-186">Om hello *OMS-Agent för Linux* tillägg för virtuell dator-agenten installeras inte eller rapporter, kan du utföra följande steg tootroubleshoot hello problemet hello.</span><span class="sxs-lookup"><span data-stu-id="51ec1-186">If hello *OMS Agent for Linux* VM agent extension is not installing or reporting, you can perform hello following steps tootroubleshoot hello issue.</span></span>

1. <span data-ttu-id="51ec1-187">Om hello tillståndets status är *okänd* kontrollera om hello Azure VM-agenten är installerat och fungerar korrekt genom att granska loggfilen för hello VM-agent`/var/log/waagent.log`</span><span class="sxs-lookup"><span data-stu-id="51ec1-187">If hello extension status is *Unknown* check if hello Azure VM agent is installed and working correctly by reviewing hello VM agent log file `/var/log/waagent.log`</span></span>
   * <span data-ttu-id="51ec1-188">Hello VM-agenten installeras inte om hello loggen inte finns.</span><span class="sxs-lookup"><span data-stu-id="51ec1-188">If hello log does not exist, hello VM agent is not installed.</span></span>
   * [<span data-ttu-id="51ec1-189">Installera hello Azure VM-agenten på virtuella Linux-datorer</span><span class="sxs-lookup"><span data-stu-id="51ec1-189">Install hello Azure VM Agent on Linux VMs</span></span>](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
2. <span data-ttu-id="51ec1-190">För andra ohälsosamt statustyper granska hello OMS-Agent för Linux VM-tillägget loggfiler `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` och`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`</span><span class="sxs-lookup"><span data-stu-id="51ec1-190">For other unhealthy statuses, review hello OMS Agent for Linux VM extension logs files in `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` and `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`</span></span>
3. <span data-ttu-id="51ec1-191">Om hello tillståndets status är felfri, men data överförs inte granska hello OMS-Agent för Linux-loggfiler i`/var/opt/microsoft/omsagent/log/omsagent.log`</span><span class="sxs-lookup"><span data-stu-id="51ec1-191">If hello extension status is healthy, but data is not being uploaded review hello OMS Agent for Linux log files in `/var/opt/microsoft/omsagent/log/omsagent.log`</span></span>

## <a name="next-steps"></a><span data-ttu-id="51ec1-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="51ec1-192">Next steps</span></span>
* <span data-ttu-id="51ec1-193">Konfigurera [datakällor i logganalys](log-analytics-data-sources.md) toospecify hello loggar och mått toocollect.</span><span class="sxs-lookup"><span data-stu-id="51ec1-193">Configure [data sources in Log Analytics](log-analytics-data-sources.md) toospecify hello logs and metrics toocollect.</span></span>
* <span data-ttu-id="51ec1-194">toogather data från virtuella datorer [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).</span><span class="sxs-lookup"><span data-stu-id="51ec1-194">toogather data from virtual machines [Add Log Analytics solutions from hello Solutions Gallery](log-analytics-add-solutions.md).</span></span>
* <span data-ttu-id="51ec1-195">[Samla in data med hjälp av Azure-diagnostik](log-analytics-azure-storage.md) andra resurser som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="51ec1-195">[Collect data by using Azure Diagnostics](log-analytics-azure-storage.md) for other resources that are running in Azure.</span></span>

<span data-ttu-id="51ec1-196">För datorer som inte ingår i Azure, kan du installera hello logganalys agenten med hjälp av hello-metoder som beskrivs i följande artiklar hello:</span><span class="sxs-lookup"><span data-stu-id="51ec1-196">For computers that are not in Azure, you can install hello Log Analytics agent by using hello methods that are described in hello following articles:</span></span>

* [<span data-ttu-id="51ec1-197">Ansluta Windows-datorer tooLog Analytics</span><span class="sxs-lookup"><span data-stu-id="51ec1-197">Connect Windows computers tooLog Analytics</span></span>](log-analytics-windows-agents.md)
* [<span data-ttu-id="51ec1-198">Ansluta Linux-datorer tooLog Analytics</span><span class="sxs-lookup"><span data-stu-id="51ec1-198">Connect Linux computers tooLog Analytics</span></span>](log-analytics-linux-agents.md)
