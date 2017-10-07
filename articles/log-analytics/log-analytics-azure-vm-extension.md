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
# <a name="connect-azure-virtual-machines-toolog-analytics-with-a-log-analytics-agent"></a>Anslut virtuella datorer i Azure tooLog Analytics med en Log Analytics-agent

Hello rekommenderad metod för insamling av loggar och mått är genom att installera hello logganalys agent för Windows och Linux-datorer.

hello enklaste sättet tooinstall hello logganalys-agenten på Azure virtual machines är via hello Log Analytics VM-tillägget.  Med hjälp av hello tillägget förenklar hello installationsprocessen och konfigurerar automatiskt hello agent toosend data toohello logganalys-arbetsytan som du anger. hello-agenten uppgraderas också automatiskt, se till att du har hello senaste funktioner och korrigeringar.

För Windows-datorer som du aktiverar hello *Microsoft Monitoring Agent* tillägg för virtuell dator.
För Linux virtuella datorer som du aktiverar hello *OMS-Agent för Linux* tillägg för virtuell dator.

Lär dig mer om [Azure virtuella datortillägg](../virtual-machines/windows/extensions-features.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) och hello [Linux-agenten](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

När du använder agent-baserade samling för loggdata, måste du konfigurera [datakällor i logganalys](log-analytics-data-sources.md) toospecify hello loggar och mått som du vill toocollect.

> [!IMPORTANT]
> Om du konfigurerar logganalys tooindex loggdata genom [Azure diagnostics](log-analytics-azure-storage.md), och du kan konfigurera hello agent toocollect hello samma loggar sedan hello loggar samlas två gånger. Du debiteras för båda datakällor. Om du har hello-agenten är installerad kan samla in loggdata med hjälp av hello-agenten - Konfigurera inte logganalys toocollect loggdata från Azure-diagnostik.
>
>

Det finns tre sätt tillägg för virtuell dator tooenable hello logganalys:

* Med hjälp av hello Azure-portalen
* Med hjälp av Azure PowerShell
* Med hjälp av en Azure Resource Manager-mall

## <a name="enable-hello-vm-extension-in-hello-azure-portal"></a>Aktivera hello VM-tillägget hello Azure-portalen
Du kan installera hello agent för Log Analytics och ansluta hello Azure-dator som körs med hjälp av hello [Azure-portalen](https://portal.azure.com).

### <a name="tooinstall-hello-log-analytics-agent-and-connect-hello-virtual-machine-tooa-log-analytics-workspace"></a>tooinstall hello logganalys agent och ansluta hello virtuella tooa logganalys-arbetsytan
1. Logga in på hello [Azure-portalen](http://portal.azure.com).
2. Välj **Bläddra** på hello vänster sida av hello portalen och sedan gå för**logganalys (OMS)** och markera den.
3. Välj hello något som du vill toouse med hello Azure VM i din lista över logganalys arbetsytor.  
   ![OMS arbetsytor](./media/log-analytics-azure-vm-extension/oms-connect-azure-01.png)
4. Under **logga analytics management**väljer **virtuella datorer**.  
   ![Virtuella datorer](./media/log-analytics-azure-vm-extension/oms-connect-azure-02.png)
5. I hello lista över **virtuella datorer**, Välj hello virtuell dator som du vill använda tooinstall hello agent. Hej **OMS anslutningsstatus** för hello VM anger att den är **inte ansluten**.  
   ![VM inte ansluten](./media/log-analytics-azure-vm-extension/oms-connect-azure-03.png)
6. I hello information för den virtuella datorn, väljer **Anslut**. hello-agent installeras och konfigureras för logganalys-arbetsytan automatiskt. Den här processen tar några minuter, under vilken tid hello OMS Anslutningsstatus är *ansluta...*  
   ![Anslut VM](./media/log-analytics-azure-vm-extension/oms-connect-azure-04.png)
7. När du installerar och ansluta hello agent hello **OMS anslutning** status kommer att vara uppdaterade tooshow **arbetsytan**.  
   ![Ansluten](./media/log-analytics-azure-vm-extension/oms-connect-azure-05.png)

## <a name="enable-hello-vm-extension-using-powershell"></a>Aktivera hello VM-tillägget med hjälp av PowerShell
När du konfigurerar den virtuella datorn med hjälp av PowerShell måste tooprovide hello **workspaceId** och **workspaceKey**. hello egenskapsnamnen i json-konfigurationen är **skiftlägeskänsliga**.

Du kan hitta hello Id och nyckel på hello **inställningar** hello OMS-portalen eller med hjälp av PowerShell som visas i föregående exempel hello.

![Arbetsyte-ID och primärnyckel](./media/log-analytics-azure-vm-extension/oms-analyze-azure-sources.png)

Det finns olika kommandon för Azure klassiska virtuella datorer och hanteraren för virtuella datorer. Följande är exempel på både virtuella datorer för Resource Manager och klassisk.

Använd hello följande PowerShell-exempel för klassiska virtuella datorer:

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

För Resource Manager Linux virtuella datorer med hjälp av hello följande CLI
```azurecli
az vm extension set --resource-group myRGMonitor --vm-name myMonitorVM --name OmsAgentForLinux --publisher Microsoft.EnterpriseCloud.Monitoring --version 1.3 --protected-settings ‘{"workspaceKey": "<workspace-key>"}’ --settings ‘{"workspaceId": "<workspace-id>"}’ 
```

Använd hello följande PowerShell-exempel för hanteraren för virtuella datorer:

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


## <a name="deploy-hello-vm-extension-using-a-template"></a>Distribuera hello VM-tillägget med hjälp av en mall
Du kan skapa en mall (i JSON-format) som definierar hello distributionen och konfigurationen av ditt program med hjälp av Azure Resource Manager. Den här mallen kallas för en Resource Manager-mall och tillhandahåller en deklarativ metod toodefine distribution. Genom att använda en mall kan du upprepade gånger distribuera ditt program i hela hello applivscykeln och vara säker på att dina resurser distribueras i ett konsekvent tillstånd.

Genom att ange hello logganalys-agenten som en del av Resource Manager-mall kan du se till att varje virtuell dator är förkonfigurerade tooreport tooyour logganalys-arbetsytan.

Mer information om Resource Manager-mallar finns [redigera Azure Resource Manager-mallar](../azure-resource-manager/resource-group-authoring-templates.md).

Följande är ett exempel på en Resource Manager-mall som används för att distribuera en virtuell dator som kör Windows hello tillägg för Microsoft Monitoring Agent som installeras. Den här mallen är en mall för vanliga virtuella datorer med hello följande tillägg:

* parametrarna workspaceId och workspaceName
* Avsnittet Microsoft.EnterpriseCloud.Monitoring tillägg
* Utdata toolook hello workspaceId och workspaceSharedKey

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

Du kan distribuera en mall med hjälp av hello följande PowerShell-kommando:

```PowerShell
New-AzureRmResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateFile $templateFilePath
```

## <a name="troubleshooting-hello-log-analytics-vm-extension"></a>Felsöka hello Log Analytics VM-tillägget
Vanligtvis meddelande ett när saker inte fungerar från Azure-portalen eller Azure powershell.

1. Logga in på hello [Azure-portalen](http://portal.azure.com).
2. Hitta hello VM och öppna VM information.
3. Klicka på **tillägg** toocheck om OMS-tillägget har aktiverats eller inte.

   ![VM-tillägg View](./media/log-analytics-azure-vm-extension/oms-vmview-extensions.png)

4. Klicka på hello *MicrosoftMonitoringAgent*(Windows) eller *OmsAgentForLinux*(Linux)-tillägget och visa information. 

   ![Information för VM-tillägg](./media/log-analytics-azure-vm-extension/oms-vmview-extensiondetails.png)

### <a name="troubleshooting-windows-virtual-machines"></a>Felsöka Windows-datorer
Om hello *Microsoft Monitoring Agent* tillägg för virtuell dator-agenten installeras inte eller rapporter, kan du utföra följande steg tootroubleshoot hello problemet hello.

1. Kontrollera om hello Azure VM-agenten är installerad och fungerar korrekt med hello stegen i [KB 2965986](https://support.microsoft.com/kb/2965986#mt1).
   * Du kan också granska loggfilen för hello VM-agent`C:\WindowsAzure\logs\WaAppAgent.log`
   * Hello VM-agenten installeras inte om hello loggen inte finns.
     * [Installera hello Azure VM-agenten på klassiska virtuella datorer](../virtual-machines/windows/classic/agents-and-extensions.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)
2. Bekräfta hello Microsoft Monitoring Agent tillägget pulsslag aktiviteten körs med hello följande steg:
   * Logga in toohello virtuell dator
   * Öppna Schemaläggaren och hitta hello `update_azureoperationalinsight_agent_heartbeat` aktivitet
   * Bekräfta hello aktiviteten är aktiverad och körs varje en minut
   * Kontrollera hello pulsslag logfile i`C:\WindowsAzure\Logs\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent\heartbeat.log`
3. Granska loggfilerna för hello Microsoft Monitoring Agent VM-tillägget i`C:\Packages\Plugins\Microsoft.EnterpriseCloud.Monitoring.MicrosoftMonitoringAgent`
4. Se till att hello virtuella datorn kan köra PowerShell-skript
5. Kontrollera behörigheter på C:\Windows\temp inte har ändrats
6. Visa hello status för hello Microsoft Monitoring Agent genom att skriva följande kommando i ett upphöjt PowerShell-fönster på den virtuella datorn hello hello`  (New-Object -ComObject 'AgentConfigManager.MgmtSvcCfg').GetCloudWorkspaces() | Format-List`
7. Granska installationsloggfilerna för hello Microsoft Monitoring Agent i`C:\Windows\System32\config\systemprofile\AppData\Local\SCOM\Logs`

Mer information finns i [felsökning av Windows-tillägg](../virtual-machines/windows/extensions-troubleshoot.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="troubleshooting-linux-virtual-machines"></a>Felsökning av virtuella Linux-datorer
Om hello *OMS-Agent för Linux* tillägg för virtuell dator-agenten installeras inte eller rapporter, kan du utföra följande steg tootroubleshoot hello problemet hello.

1. Om hello tillståndets status är *okänd* kontrollera om hello Azure VM-agenten är installerat och fungerar korrekt genom att granska loggfilen för hello VM-agent`/var/log/waagent.log`
   * Hello VM-agenten installeras inte om hello loggen inte finns.
   * [Installera hello Azure VM-agenten på virtuella Linux-datorer](../virtual-machines/linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
2. För andra ohälsosamt statustyper granska hello OMS-Agent för Linux VM-tillägget loggfiler `/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/extension.log` och`/var/log/azure/Microsoft.EnterpriseCloud.Monitoring.OmsAgentForLinux/*/CommandExecution.log`
3. Om hello tillståndets status är felfri, men data överförs inte granska hello OMS-Agent för Linux-loggfiler i`/var/opt/microsoft/omsagent/log/omsagent.log`

## <a name="next-steps"></a>Nästa steg
* Konfigurera [datakällor i logganalys](log-analytics-data-sources.md) toospecify hello loggar och mått toocollect.
* toogather data från virtuella datorer [lägga till logganalys lösningar från hello lösningar galleriet](log-analytics-add-solutions.md).
* [Samla in data med hjälp av Azure-diagnostik](log-analytics-azure-storage.md) andra resurser som körs i Azure.

För datorer som inte ingår i Azure, kan du installera hello logganalys agenten med hjälp av hello-metoder som beskrivs i följande artiklar hello:

* [Ansluta Windows-datorer tooLog Analytics](log-analytics-windows-agents.md)
* [Ansluta Linux-datorer tooLog Analytics](log-analytics-linux-agents.md)
