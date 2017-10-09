---
title: "aaaAzure virtuella skalningsuppsättningarna vanliga frågor och svar | Microsoft Docs"
description: "Få svar toofrequently frågor och svar om virtuella datorer."
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/20/2017
ms.author: negat
ms.custom: na
ms.openlocfilehash: 0deb9e2bb79f87f17bbf748397b94dc53070cfbb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-virtual-machine-scale-sets-faqs"></a><span data-ttu-id="0f1b7-103">Skala virtuell Azure-dator Anger vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="0f1b7-103">Azure virtual machine scale sets FAQs</span></span>

<span data-ttu-id="0f1b7-104">Få svar toofrequently frågor och svar om virtuella datorn anger i Azure.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-104">Get answers toofrequently asked questions about virtual machine scale sets in Azure.</span></span>

## <a name="autoscale"></a><span data-ttu-id="0f1b7-105">Automatisk skalning</span><span class="sxs-lookup"><span data-stu-id="0f1b7-105">Autoscale</span></span>

### <a name="what-are-best-practices-for-azure-autoscale"></a><span data-ttu-id="0f1b7-106">Vad är bästa praxis för Azure Autoskala?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-106">What are best practices for Azure Autoscale?</span></span>

<span data-ttu-id="0f1b7-107">Bästa praxis för Autoskala finns [bästa praxis för autoskalning virtuella datorer](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-107">For best practices for Autoscale, see [Best practices for autoscaling virtual machines](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).</span></span>

### <a name="where-do-i-find-metric-names-for-autoscaling-that-uses-host-based-metrics"></a><span data-ttu-id="0f1b7-108">Var hittar jag mått namn för autoskalning som använder värdbaserad mått?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-108">Where do I find metric names for autoscaling that uses host-based metrics?</span></span>

<span data-ttu-id="0f1b7-109">Tjänstmåttets namn för autoskalning som använder värdbaserad mått finns [stöds mått med Azure-Monitor](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-109">For metric names for autoscaling that uses host-based metrics, see [Supported metrics with Azure Monitor](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).</span></span>

### <a name="are-there-any-examples-of-autoscaling-based-on-an-azure-service-bus-topic-and-queue-length"></a><span data-ttu-id="0f1b7-110">Finns det några exempel på autoskalning baserat på en Azure Service Bus-avsnittet och kön längd?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-110">Are there any examples of autoscaling based on an Azure Service Bus topic and queue length?</span></span>

<span data-ttu-id="0f1b7-111">Ja.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-111">Yes.</span></span> <span data-ttu-id="0f1b7-112">Exempel på autoskalning baserat på en Azure Service Bus-avsnittet och kön längd finns [Azure-Monitor autoskalning vanliga mått](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-112">For examples of autoscaling based on an Azure Service Bus topic and queue length, see [Azure Monitor autoscaling common metrics](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).</span></span>

<span data-ttu-id="0f1b7-113">Använd hello följande JSON för Service Bus-kö:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-113">For a Service Bus queue, use hello following JSON:</span></span>

```json
"metricName": "MessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

<span data-ttu-id="0f1b7-114">Använd hello följande JSON för en storage-kö:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-114">For a storage queue, use hello following JSON:</span></span>

```json
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

<span data-ttu-id="0f1b7-115">Ersätt exempelvärden med din resurs Uniform Resource Identifier (URI: er).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-115">Replace example values with your resource Uniform Resource Identifiers (URIs).</span></span>


### <a name="should-i-autoscale-by-using-host-based-metrics-or-a-diagnostics-extension"></a><span data-ttu-id="0f1b7-116">Bör jag Autoskala med hjälp av värdbaserad mått eller ett tillägg för diagnostik?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-116">Should I autoscale by using host-based metrics or a diagnostics extension?</span></span>

<span data-ttu-id="0f1b7-117">Du kan skapa en autoskalningsinställning på en VM toouse värdnivå mått eller gäst-OS-baserade mått.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-117">You can create an autoscale setting on a VM toouse host-level metrics or guest OS-based metrics.</span></span>

<span data-ttu-id="0f1b7-118">En lista över stöds mått finns [Azure-Monitor autoskalning vanliga mått](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-118">For a list of supported metrics, see [Azure Monitor autoscaling common metrics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics).</span></span> 

<span data-ttu-id="0f1b7-119">En fullständig exempelfil för skalningsuppsättningar i virtuella datorer finns [avancerade Autoskala konfiguration med hjälp av Resource Manager-mallar för virtuella datorer](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-119">For a full sample for virtual machine scale sets, see [Advanced autoscale configuration by using Resource Manager templates for virtual machine scale sets](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets).</span></span> 

<span data-ttu-id="0f1b7-120">hello används hello värdnivå CPU mått och ett meddelande count-mått.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-120">hello sample uses hello host-level CPU metric and a message count metric.</span></span>



### <a name="how-do-i-set-alert-rules-on-a-virtual-machine-scale-set"></a><span data-ttu-id="0f1b7-121">Hur ställer jag in Varningsregler i en skaluppsättning för virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-121">How do I set alert rules on a virtual machine scale set?</span></span>

<span data-ttu-id="0f1b7-122">Du kan skapa aviseringar för mått för skalningsuppsättningar i virtuella datorer via PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-122">You can create alerts on metrics for virtual machine scale sets via PowerShell or Azure CLI.</span></span> <span data-ttu-id="0f1b7-123">Mer information finns i [Azure-Monitor PowerShell snabb start prover](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) och [Azure-Monitor plattformsoberoende CLI snabb start exempel](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-123">For more information, see [Azure Monitor PowerShell quick start samples](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) and [Azure Monitor cross-platform CLI quick start samples](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).</span></span>

<span data-ttu-id="0f1b7-124">Hej TargetResourceId för hello skaluppsättning för virtuell dator ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-124">hello TargetResourceId of hello virtual machine scale set looks like this:</span></span> 

<span data-ttu-id="0f1b7-125">/subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmssname</span><span class="sxs-lookup"><span data-stu-id="0f1b7-125">/subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmssname</span></span>

<span data-ttu-id="0f1b7-126">Du kan välja alla VM-prestandaräknaren som hello mått tooset en avisering för.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-126">You can choose any VM performance counter as hello metric tooset an alert for.</span></span> <span data-ttu-id="0f1b7-127">Mer information finns i [Gästoperativsystem mätvärden för Resource Manager-baserade virtuella Windows-datorer](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) och [Gästoperativsystem mätvärden för virtuella Linux-datorer](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) i hello [Azure-Monitor autoskalning vanliga mått](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/)artikel.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-127">For more information, see [Guest OS metrics for Resource Manager-based Windows VMs](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) and [Guest OS metrics for Linux VMs](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) in hello [Azure Monitor autoscaling common metrics](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/) article.</span></span>

### <a name="how-do-i-set-up-autoscale-on-a-virtual-machine-scale-set-by-using-powershell"></a><span data-ttu-id="0f1b7-128">Hur ställer jag in Autoskala på en virtuell dator skala in med hjälp av PowerShell?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-128">How do I set up autoscale on a virtual machine scale set by using PowerShell?</span></span>

<span data-ttu-id="0f1b7-129">tooset in Autoskala på en virtuell dator skala in med hjälp av PowerShell finns i blogginlägget hello [hur tooadd Autoskala tooan virtuella Azure-datorn skala in](https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-129">tooset up autoscale on a virtual machine scale set by using PowerShell, see hello blog post [How tooadd autoscale tooan Azure virtual machine scale set](https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/).</span></span>




## <a name="certificates"></a><span data-ttu-id="0f1b7-130">Certifikat</span><span class="sxs-lookup"><span data-stu-id="0f1b7-130">Certificates</span></span>

### <a name="how-do-i-securely-ship-a-certificate-toohello-vm-how-do-i-provision-a-virtual-machine-scale-set-toorun-a-website-where-hello-ssl-for-hello-website-is-shipped-securely-from-a-certificate-configuration-hello-common-certificate-rotation-operation-would-be-almost-hello-same-as-a-configuration-update-operation-do-you-have-an-example-of-how-toodo-this"></a><span data-ttu-id="0f1b7-131">Hur skickar jag en certifikat-toohello VM på ett säkert sätt?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-131">How do I securely ship a certificate toohello VM?</span></span> <span data-ttu-id="0f1b7-132">Hur jag för att etablera en virtuell dator scale set toorun en webbplats som hello SSL för hello webbplats levereras på ett säkert sätt från en konfiguration?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-132">How do I provision a virtual machine scale set toorun a website where hello SSL for hello website is shipped securely from a certificate configuration?</span></span> <span data-ttu-id="0f1b7-133">(hello vanliga certifikat rotation åtgärden skulle vara nästan hello samma som en uppdateringsåtgärd konfiguration.) Har du ett exempel på hur toodo detta?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-133">(hello common certificate rotation operation would be almost hello same as a configuration update operation.) Do you have an example of how toodo this?</span></span> 

<span data-ttu-id="0f1b7-134">toosecurely levereras certifikat-toohello VM kan du installera ett certifikat för kunden direkt till ett Windows-certifikatarkiv från hello kundens nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-134">toosecurely ship a certificate toohello VM, you can install a customer certificate directly into a Windows certificate store from hello customer's key vault.</span></span>

<span data-ttu-id="0f1b7-135">Använd följande JSON hello:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-135">Use hello following JSON:</span></span>

```json
"secrets": [
    {
        "sourceVault": {
            "id": "/subscriptions/{subscriptionid}/resourceGroups/myrg1/providers/Microsoft.KeyVault/vaults/mykeyvault1"
        },
        "vaultCertificates": [
            {
                "certificateUrl": "https://mykeyvault1.vault.azure.net/secrets/{secretname}/{secret-version}",
                "certificateStore": "certificateStoreName"
            }
        ]
    }
]
```

<span data-ttu-id="0f1b7-136">hello koden har stöd för Windows och Linux.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-136">hello code supports Windows and Linux.</span></span>

<span data-ttu-id="0f1b7-137">Mer information finns i [skapa eller uppdatera en virtuell dator skaluppsättning](https://msdn.microsoft.com/library/mt589035.aspx).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-137">For more information, see [Create or update a virtual machine scale set](https://msdn.microsoft.com/library/mt589035.aspx).</span></span>


### <a name="example-of-self-signed-certificate"></a><span data-ttu-id="0f1b7-138">Exempel på självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="0f1b7-138">Example of Self-signed certificate</span></span>

1.  <span data-ttu-id="0f1b7-139">Skapa ett självsignerat certifikat i en nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-139">Create a self-signed certificate in a key vault.</span></span>

    <span data-ttu-id="0f1b7-140">Använd hello följande PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-140">Use hello following PowerShell commands:</span></span>

    ```powershell
    Import-Module "C:\Users\mikhegn\Downloads\Service-Fabric-master\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

    Login-AzureRmAccount

    Invoke-AddCertToKeyVault -SubscriptionId <Your SubID> -ResourceGroupName KeyVault -Location westus -VaultName MikhegnVault -CertificateName VMSSCert -Password VmssCert -CreateSelfSignedCertificate -DnsName vmss.mikhegn.azure.com -OutputPath c:\users\mikhegn\desktop\
    ```

    <span data-ttu-id="0f1b7-141">Det här kommandot ger du hello indata för hello Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-141">This command gives you hello input for hello Azure Resource Manager template.</span></span>

    <span data-ttu-id="0f1b7-142">Ett exempel på hur toocreate ett självsignerat certifikat i nyckelvalvet, se [säkerhetsscenarier för Service Fabric-kluster](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-142">For an example of how toocreate a self-signed certificate in a key vault, see [Service Fabric cluster security scenarios](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).</span></span>

2.  <span data-ttu-id="0f1b7-143">Ändra hello Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-143">Change hello Resource Manager template.</span></span>

    <span data-ttu-id="0f1b7-144">Lägg till den här egenskapen för**virtualMachineProfile**, som en del av hello virtuella skaluppsättning för resursen:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-144">Add this property too**virtualMachineProfile**, as part of hello virtual machine scale set resource:</span></span>

    ```json 
    "osProfile": {
        "computerNamePrefix": "[variables('namingInfix')]",
        "adminUsername": "[parameters('adminUsername')]",
        "adminPassword": "[parameters('adminPassword')]",
        "secrets": [
            {
                "sourceVault": {
                    "id": "[resourceId('KeyVault', 'Microsoft.KeyVault/vaults', 'MikhegnVault')]"
                },
                "vaultCertificates": [
                    {
                        "certificateUrl": "https://mikhegnvault.vault.azure.net:443/secrets/VMSSCert/20709ca8faee4abb84bc6f4611b088a4",
                        "certificateStore": "My"
                    }
                ]
            }
        ]
    }
    ```
  

### <a name="can-i-specify-an-ssh-key-pair-toouse-for-ssh-authentication-with-a-linux-virtual-machine-scale-set-from-a-resource-manager-template"></a><span data-ttu-id="0f1b7-145">Kan jag ange en SSH-nyckelpar toouse för SSH-autentisering med en Linux skaluppsättningen för virtuell dator från en Resource Manager-mall</span><span class="sxs-lookup"><span data-stu-id="0f1b7-145">Can I specify an SSH key pair toouse for SSH authentication with a Linux virtual machine scale set from a Resource Manager template?</span></span>  

<span data-ttu-id="0f1b7-146">Ja.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-146">Yes.</span></span> <span data-ttu-id="0f1b7-147">hello REST API för **osProfile** är liknande toohello standard VM-REST API.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-147">hello REST API for **osProfile** is similar toohello standard VM REST API.</span></span> 

<span data-ttu-id="0f1b7-148">Inkludera **osProfile** i mallen:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-148">Include **osProfile** in your template:</span></span>

```json 
"osProfile": {
    "computerName": "[variables('vmName')]",
    "adminUsername": "[parameters('adminUserName')]",
    "linuxConfiguration": {
        "disablePasswordAuthentication": "true",
        "ssh": {
            "publicKeys": [
                {
                    "path": "[variables('sshKeyPath')]",
                    "keyData": "[parameters('sshKeyData')]"
                }
            ]
        }
    }
}
```
 
<span data-ttu-id="0f1b7-149">Det här JSON-blocket används i [hello 101-vm-sshkey GitHub Snabbstart mallen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-149">This JSON block is used in [hello 101-vm-sshkey GitHub quick start template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span></span>
 
<span data-ttu-id="0f1b7-150">hello OS-profil används också i [hello grelayhost.json GitHub quick start mallen](https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-150">hello OS profile also is used in [hello grelayhost.json GitHub quick start template](https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json).</span></span>

<span data-ttu-id="0f1b7-151">Mer information finns i [skapa eller uppdatera en virtuell dator skaluppsättning](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-151">For more information, see [Create or update a virtual machine scale set](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration).</span></span>
  

### <a name="how-do-i-remove-deprecated-certificates"></a><span data-ttu-id="0f1b7-152">Hur tar jag bort föråldrad certifikat?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-152">How do I remove deprecated certificates?</span></span> 

<span data-ttu-id="0f1b7-153">tooremove föråldrad certifikat, ta bort gammalt certifikat för hello hello valvet certifikat listan.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-153">tooremove deprecated certificates, remove hello old certificate from hello vault certificates list.</span></span> <span data-ttu-id="0f1b7-154">Lämna alla hello certifikat som du vill tooremain på datorn i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-154">Leave all hello certificates that you want tooremain on your computer in hello list.</span></span> <span data-ttu-id="0f1b7-155">Hello certifikatet tas inte bort från dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-155">This does not remove hello certificate from all your VMs.</span></span> <span data-ttu-id="0f1b7-156">Också läggs inte hello certifikat toonew virtuella datorer som skapas i hello skaluppsättning för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-156">It also does not add hello certificate toonew VMs that are created in hello virtual machine scale set.</span></span> 

<span data-ttu-id="0f1b7-157">tooremove hello certifikat från befintliga virtuella datorer, skriva ett anpassat skript för tillägget toomanually ta bort hello certifikat från certifikatarkivet.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-157">tooremove hello certificate from existing VMs, write a custom script extension toomanually remove hello certificates from your certificate store.</span></span>
 
### <a name="how-do-i-inject-an-existing-ssh-public-key-into-hello-virtual-machine-scale-set-ssh-layer-during-provisioning-i-want-toostore-hello-ssh-public-key-values-in-azure-key-vault-and-then-use-them-in-my-resource-manager-template"></a><span data-ttu-id="0f1b7-158">Hur jag mata in en befintlig offentlig SSH-nyckel i hello virtuella scale set SSH layer under etablering?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-158">How do I inject an existing SSH public key into hello virtual machine scale set SSH layer during provisioning?</span></span> <span data-ttu-id="0f1b7-159">Jag vill toostore hello SSH offentlig nyckelvärdena i Azure Key Vault och använda dem i Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-159">I want toostore hello SSH public key values in Azure Key Vault, and then use them in my Resource Manager template.</span></span>

<span data-ttu-id="0f1b7-160">Om du tillhandahåller hello virtuella datorer bara med en offentlig SSH-nyckel, behöver du inte tooput hello offentliga nycklar i Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-160">If you are providing hello VMs only with a public SSH key, you don't need tooput hello public keys in Key Vault.</span></span> <span data-ttu-id="0f1b7-161">Offentliga nycklar är inte hemliga.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-161">Public keys are not secret.</span></span>
 
<span data-ttu-id="0f1b7-162">Du kan ange offentliga SSH-nycklar i klartext när du skapar en Linux VM:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-162">You can provide SSH public keys in plain text when you create a Linux VM:</span></span>

```json
"linuxConfiguration": {
    "ssh": {
        "publicKeys": [
            {
                "path": "path",
                "keyData": "publickey"
            }
        ]
    }
```
 
<span data-ttu-id="0f1b7-163">linuxConfiguration elementnamn</span><span class="sxs-lookup"><span data-stu-id="0f1b7-163">linuxConfiguration element name</span></span> | <span data-ttu-id="0f1b7-164">Krävs</span><span class="sxs-lookup"><span data-stu-id="0f1b7-164">Required</span></span> | <span data-ttu-id="0f1b7-165">Typ</span><span class="sxs-lookup"><span data-stu-id="0f1b7-165">Type</span></span> | <span data-ttu-id="0f1b7-166">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0f1b7-166">Description</span></span>
--- | --- | --- | --- |  ---
<span data-ttu-id="0f1b7-167">SSH</span><span class="sxs-lookup"><span data-stu-id="0f1b7-167">ssh</span></span> | <span data-ttu-id="0f1b7-168">Nej</span><span class="sxs-lookup"><span data-stu-id="0f1b7-168">No</span></span> | <span data-ttu-id="0f1b7-169">Samling</span><span class="sxs-lookup"><span data-stu-id="0f1b7-169">Collection</span></span> | <span data-ttu-id="0f1b7-170">Anger hello SSH-nyckeln är konfigurerad för ett Linux-operativsystem</span><span class="sxs-lookup"><span data-stu-id="0f1b7-170">Specifies hello SSH key configuration for a Linux OS</span></span>
<span data-ttu-id="0f1b7-171">Sökväg</span><span class="sxs-lookup"><span data-stu-id="0f1b7-171">path</span></span> | <span data-ttu-id="0f1b7-172">Ja</span><span class="sxs-lookup"><span data-stu-id="0f1b7-172">Yes</span></span> | <span data-ttu-id="0f1b7-173">Sträng</span><span class="sxs-lookup"><span data-stu-id="0f1b7-173">String</span></span> | <span data-ttu-id="0f1b7-174">Anger hello Linux sökväg där hello SSH-nycklar eller certifikat ska hittas</span><span class="sxs-lookup"><span data-stu-id="0f1b7-174">Specifies hello Linux file path where hello SSH keys or certificate should be located</span></span>
<span data-ttu-id="0f1b7-175">nyckeldata</span><span class="sxs-lookup"><span data-stu-id="0f1b7-175">keyData</span></span> | <span data-ttu-id="0f1b7-176">Ja</span><span class="sxs-lookup"><span data-stu-id="0f1b7-176">Yes</span></span> | <span data-ttu-id="0f1b7-177">Sträng</span><span class="sxs-lookup"><span data-stu-id="0f1b7-177">String</span></span> | <span data-ttu-id="0f1b7-178">Anger en base64-kodad SSH offentlig nyckel</span><span class="sxs-lookup"><span data-stu-id="0f1b7-178">Specifies a base64-encoded SSH public key</span></span>

<span data-ttu-id="0f1b7-179">Ett exempel finns [hello 101-vm-sshkey GitHub Snabbstart mallen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-179">For an example, see [hello 101-vm-sshkey GitHub quick start template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span></span>

 
### <a name="when-i-run-update-azurermvmss-after-adding-more-than-one-certificate-from-hello-same-key-vault-i-see-hello-following-message"></a><span data-ttu-id="0f1b7-180">När kör `Update-AzureRmVmss` efter att lägga till fler än ett certifikat från hello samma nyckel valvet, visas hello följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-180">When I run `Update-AzureRmVmss` after adding more than one certificate from hello same key vault, I see hello following message:</span></span>
 
><span data-ttu-id="0f1b7-181">Update-AzureRmVmss: Listan hemlighet innehåller upprepade förekomster av /subscriptions/ < min-prenumerations-id > / resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev, vilket inte är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-181">Update-AzureRmVmss: List secret contains repeated instances of /subscriptions/<my-subscription-id>/resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev, which is disallowed.</span></span>
 
<span data-ttu-id="0f1b7-182">Detta kan inträffa om du försöker toore-Lägg till hello samma valvet istället för att använda ett nytt certifikat för valvet för hello befintliga källvalv.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-182">This can happen if you try toore-add hello same vault instead of using a new vault certificate for hello existing source vault.</span></span> <span data-ttu-id="0f1b7-183">Hej `Add-AzureRmVmssSecret` kommandot fungerar inte korrekt om du lägger till ytterligare hemligheter.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-183">hello `Add-AzureRmVmssSecret` command does not work correctly if you are adding additional secrets.</span></span>
 
<span data-ttu-id="0f1b7-184">tooadd mer hemligheter från hello samma nyckelvalvet, uppdatera hello $vmss.properties.osProfile.secrets[0].vaultCertificates lista.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-184">tooadd more secrets from hello same key vault, update hello $vmss.properties.osProfile.secrets[0].vaultCertificates list.</span></span>
 
<span data-ttu-id="0f1b7-185">Hello förväntade indatastrukturen finns [skapa eller uppdatera en virtuell dator ange](https://msdn.microsoft.com/library/azure/mt589035.aspx).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-185">For hello expected input structure, see [Create or update a virtual machine set](https://msdn.microsoft.com/library/azure/mt589035.aspx).</span></span>
 
<span data-ttu-id="0f1b7-186">Hitta hello hemlighet i hello virtuella Skala uppsättningsobjekt som ingår i hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-186">Find hello secret in hello virtual machine scale set object that is in hello key vault.</span></span> <span data-ttu-id="0f1b7-187">Lägg sedan till din referens (hello URL och hello hemliga store-namn) toohello lista över certifikat som är associerade med hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-187">Then, add your certificate reference (hello URL and hello secret store name) toohello list associated with hello vault.</span></span>

> [!NOTE] 
> <span data-ttu-id="0f1b7-188">För närvarande kan du ta bort certifikat från virtuella datorer med hjälp av hello virtuella scale set API.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-188">Currently, you cannot remove certificates from VMs by using hello virtual machine scale set API.</span></span>
>

<span data-ttu-id="0f1b7-189">Nya virtuella datorer inte hello gammalt certifikat.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-189">New VMs will not have hello old certificate.</span></span> <span data-ttu-id="0f1b7-190">Virtuella datorer som har hello certifikat och som redan har distribuerats har dock hello gammalt certifikat.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-190">However, VMs that have hello certificate and which are already deployed will have hello old certificate.</span></span>
 
### <a name="can-i-push-certificates-toohello-virtual-machine-scale-set-without-providing-hello-password-when-hello-certificate-is-in-hello-secret-store"></a><span data-ttu-id="0f1b7-191">Kan jag push-certifikat toohello virtuella skaluppsättningen utan att ange hello lösenord när hello certifikat i hello hemliga store?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-191">Can I push certificates toohello virtual machine scale set without providing hello password, when hello certificate is in hello secret store?</span></span>

<span data-ttu-id="0f1b7-192">Du behöver inte toohard kod lösenord i skript.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-192">You do not need toohard-code passwords in scripts.</span></span> <span data-ttu-id="0f1b7-193">Du kan hämta lösenord dynamiskt med hello behörigheter du använder toorun hello distributions-skriptet.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-193">You can dynamically retrieve passwords with hello permissions you use toorun hello deployment script.</span></span> <span data-ttu-id="0f1b7-194">Om du har ett skript som flyttar ett certifikat från hello hemliga store nyckelvalv hello hemliga store `get certificate` hello lösenordet för PFX-fil som hello kommando också.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-194">If you have a script that moves a certificate from hello secret store key vault, hello secret store `get certificate` command also outputs hello password of hello .pfx file.</span></span>
 
### <a name="how-does-hello-secrets-property-of-virtualmachineprofileosprofile-for-a-virtual-machine-scale-set-work-why-do-i-need-hello-sourcevault-value-when-i-have-toospecify-hello-absolute-uri-for-a-certificate-by-using-hello-certificateurl-property"></a><span data-ttu-id="0f1b7-195">Hur ange arbete i hello hemligheter-egenskapen för virtualMachineProfile.osProfile för en virtuell dator skala?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-195">How does hello Secrets property of virtualMachineProfile.osProfile for a virtual machine scale set work?</span></span> <span data-ttu-id="0f1b7-196">Varför måste jag göra hello sourceVault värde när jag har toospecify hello absolut URI för ett certifikat med hjälp av hello certificateUrl egenskapen?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-196">Why do I need hello sourceVault value when I have toospecify hello absolute URI for a certificate by using hello certificateUrl property?</span></span> 

<span data-ttu-id="0f1b7-197">En referens för Windows Remote Management (WinRM)-certifikat måste finnas i hello hemligheter-egenskapen för hello OS-profil.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-197">A Windows Remote Management (WinRM) certificate reference must be present in hello Secrets property of hello OS profile.</span></span> 

<span data-ttu-id="0f1b7-198">hello syftar som visar hello källvalv tooenforce åtkomstkontrollistan (ACL) principer för åtkomstkontroll som finns i en användares Azure Cloud Service model.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-198">hello purpose of indicating hello source vault is tooenforce access control list (ACL) policies that exist in a user's Azure Cloud Service model.</span></span> <span data-ttu-id="0f1b7-199">Om hello källvalv har inte angetts, är användare som inte har behörigheter toodeploy eller åtkomst hemligheter tooa nyckelvalv kan toothrough en Compute Resource Provider (CRP).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-199">If hello source vault isn't specified, users who do not have permissions toodeploy or access secrets tooa key vault would be able toothrough a Compute Resource Provider (CRP).</span></span> <span data-ttu-id="0f1b7-200">ACL: er finns även för resurser som inte finns.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-200">ACLs exist even for resources that do not exist.</span></span>

<span data-ttu-id="0f1b7-201">Om du anger ett felaktigt källa valvet ID men en giltig key vault-URL, rapporteras ett fel när du avsöka hello igen.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-201">If you provide an incorrect source vault ID but a valid key vault URL, an error is reported when you poll hello operation.</span></span>
 
### <a name="if-i-add-secrets-tooan-existing-virtual-machine-scale-set-are-hello-secrets-injected-into-existing-vms-or-only-into-new-ones"></a><span data-ttu-id="0f1b7-202">Om jag lägger till hemligheter tooan befintlig skaluppsättning för virtuell dator är hello hemligheter matas in i befintliga virtuella datorer eller bara nya?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-202">If I add secrets tooan existing virtual machine scale set, are hello secrets injected into existing VMs, or only into new ones?</span></span> 

<span data-ttu-id="0f1b7-203">Certifikat läggs tooall dina virtuella datorer, även redan befintliga viktiga.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-203">Certificates are added tooall your VMs, even preexisting ones.</span></span> <span data-ttu-id="0f1b7-204">Om din virtuella skaluppsättning upgradePolicy egenskap är inställd för**manuell**, hello certifikatet läggs toohello VM när du utför en manuell uppdatering på hello VM.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-204">If your virtual machine scale set upgradePolicy property is set too**manual**, hello certificate is added toohello VM when you perform a manual update on hello VM.</span></span>
 
### <a name="where-do-i-put-certificates-for-linux-vms"></a><span data-ttu-id="0f1b7-205">Där placera certifikat för Linux virtuella datorer?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-205">Where do I put certificates for Linux VMs?</span></span>

<span data-ttu-id="0f1b7-206">hur toodeploy certifikat för Linux virtuella datorer kan se toolearn [distribuera certifikat tooVMs från en kundhanterad nyckelvalv](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-206">toolearn how toodeploy certificates for Linux VMs, see [Deploy certificates tooVMs from a customer-managed key vault](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/).</span></span>
  
### <a name="how-do-i-add-a-new-vault-certificate-tooa-new-certificate-object"></a><span data-ttu-id="0f1b7-207">Hur lägger jag till en ny valvet tooa nya certifikat certifikatobjekt?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-207">How do I add a new vault certificate tooa new certificate object?</span></span>

<span data-ttu-id="0f1b7-208">tooadd en valvet certifikat tooan befintliga hemlighet, se hello följande PowerShell-exempel.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-208">tooadd a vault certificate tooan existing secret, see hello following PowerShell example.</span></span> <span data-ttu-id="0f1b7-209">Använd endast hemliga objekt.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-209">Use only one secret object.</span></span>
 
```powershell
$newVaultCertificate = New-AzureRmVmssVaultCertificateConfig -CertificateStore MY -CertificateUrl https://sansunallapps1.vault.azure.net:443/secrets/dg-private-enc/55fa0332edc44a84ad655298905f1809
 
$vmss.VirtualMachineProfile.OsProfile.Secrets[0].VaultCertificates.Add($newVaultCertificate)
 
Update-AzureRmVmss -VirtualMachineScaleSet $vmss -ResourceGroup $rg -Name $vmssName
```
 
### <a name="what-happens-toocertificates-if-you-reimage-a-vm"></a><span data-ttu-id="0f1b7-210">Vad händer toocertificates om du återavbilda en virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-210">What happens toocertificates if you reimage a VM?</span></span>

<span data-ttu-id="0f1b7-211">Om du återavbilda en virtuell dator tas certifikat bort.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-211">If you reimage a VM, certificates are deleted.</span></span> <span data-ttu-id="0f1b7-212">Återställning av avbildning borttagningar hello hela OS-disken.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-212">Reimaging deletes hello entire OS disk.</span></span> 
 
### <a name="what-happens-if-you-delete-a-certificate-from-hello-key-vault"></a><span data-ttu-id="0f1b7-213">Vad händer om du tar bort ett certifikat från hello nyckelvalv?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-213">What happens if you delete a certificate from hello key vault?</span></span>

<span data-ttu-id="0f1b7-214">Om hello hemlighet tas bort från hello nyckelvalvet och kör du `stop deallocate` för dina virtuella datorer och starta sedan om dem igen, inträffar ett fel.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-214">If hello secret is deleted from hello key vault, and then you run `stop deallocate` for all your VMs and then start them again, you will encounter a failure.</span></span> <span data-ttu-id="0f1b7-215">hello felet inträffar eftersom hello CRP behöver tooretrieve hello hemligheter från hello nyckelvalvet, men den inte kan.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-215">hello failure occurs because hello CRP needs tooretrieve hello secrets from hello key vault, but it cannot.</span></span> <span data-ttu-id="0f1b7-216">I det här scenariot kan du ta bort hello certifikat hello virtuella skala modellen.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-216">In this scenario, you can delete hello certificates from hello virtual machine scale set model.</span></span> 

<span data-ttu-id="0f1b7-217">hello CRP komponenten inte har behållits kunden hemligheter.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-217">hello CRP component does not persist customer secrets.</span></span> <span data-ttu-id="0f1b7-218">Om du kör `stop deallocate` för alla virtuella datorer i hello virtuella datorns skaluppsättning hello cache har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-218">If you run `stop deallocate` for all VMs in hello virtual machine scale set, hello cache is deleted.</span></span> <span data-ttu-id="0f1b7-219">I det här scenariot hämtas hemligheter från hello nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-219">In this scenario, secrets are retrieved from hello key vault.</span></span>

<span data-ttu-id="0f1b7-220">Det uppstår inte här problemet när skala ut eftersom det inte finns en cachelagrad kopia av hello hemlighet i Azure Service Fabric (i hello enskild fabric klient model).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-220">You don't encounter this problem when scaling out because there is a cached copy of hello secret in Azure Service Fabric (in hello single-fabric tenant model).</span></span>
 
### <a name="why-do-i-have-toospecify-hello-exact-location-for-hello-certificate-url-httpsname-of-hello-vaultvaultazurenet443secretsexact-location-as-indicated-in-service-fabric-cluster-security-scenarioshttpsazuremicrosoftcomdocumentationarticlesservice-fabric-cluster-security"></a><span data-ttu-id="0f1b7-221">Varför måste jag toospecify hello exakta platsen för Certifikatwebbadress hello (https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>), som anges i [säkerhetsscenarier för Service Fabric-klustret](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/)?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-221">Why do I have toospecify hello exact location for hello certificate URL (https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>), as indicated in [Service Fabric cluster security scenarios](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/)?</span></span>
 
<span data-ttu-id="0f1b7-222">hello Azure Key Vault-dokumentationen om den hello hämta hemligheten REST API ska returnera hello senaste versionen av hello hemlighet om hello-versionen inte har angetts.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-222">hello Azure Key Vault documentation states that hello Get Secret REST API should return hello latest version of hello secret if hello version is not specified.</span></span>
 
<span data-ttu-id="0f1b7-223">Metod</span><span class="sxs-lookup"><span data-stu-id="0f1b7-223">Method</span></span> | <span data-ttu-id="0f1b7-224">URL: EN</span><span class="sxs-lookup"><span data-stu-id="0f1b7-224">URL</span></span>
--- | ---
<span data-ttu-id="0f1b7-225">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="0f1b7-225">GET</span></span> | <span data-ttu-id="0f1b7-226">https://mykeyvault.Vault.Azure.NET/secrets/ {hemlighet name} / {hemlighet version}? api-version = {api-version}</span><span class="sxs-lookup"><span data-stu-id="0f1b7-226">https://mykeyvault.vault.azure.net/secrets/{secret-name}/{secret-version}?api-version={api-version}</span></span>

<span data-ttu-id="0f1b7-227">Ersätt {*hemlighet namn*} med hello namn och Ersätt {*hemlighet version*} hello hemlighet hello-version du vill använda tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-227">Replace {*secret-name*} with hello name, and replace {*secret-version*} with hello version of hello secret you want tooretrieve.</span></span> <span data-ttu-id="0f1b7-228">hello hemliga versionen kan inte uteslutas.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-228">hello secret version might be excluded.</span></span> <span data-ttu-id="0f1b7-229">I så fall hämtas hello aktuell version.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-229">In that case, hello current version is retrieved.</span></span>
  
### <a name="why-do-i-have-toospecify-hello-certificate-version-when-i-use-key-vault"></a><span data-ttu-id="0f1b7-230">Varför måste jag toospecify hello certifikat version när jag använder Key Vault?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-230">Why do I have toospecify hello certificate version when I use Key Vault?</span></span>

<span data-ttu-id="0f1b7-231">hello syftar hello Key Vault krav toospecify hello certifikat version toomake den Rensa toohello användare för vilka certifikat som har distribuerats på sina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-231">hello purpose of hello Key Vault requirement toospecify hello certificate version is toomake it clear toohello user what certificate is deployed on their VMs.</span></span>

<span data-ttu-id="0f1b7-232">Om du skapar en virtuell dator och uppdatera din hemlighet i nyckelvalvet hello hello nya certifikatet är inte hämtad tooyour virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-232">If you create a VM and then update your secret in hello key vault, hello new certificate is not downloaded tooyour VMs.</span></span> <span data-ttu-id="0f1b7-233">Men dina virtuella datorer visas tooreference och nya virtuella datorer får de hello nya hemligheten.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-233">But your VMs appear tooreference it, and new VMs get hello new secret.</span></span> <span data-ttu-id="0f1b7-234">tooavoid, du är obligatoriska tooreference hemliga versionen.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-234">tooavoid this, you are required tooreference a secret version.</span></span>

### <a name="my-team-works-with-several-certificates-that-are-distributed-toous-as-cer-public-keys-what-is-hello-recommended-approach-for-deploying-these-certificates-tooa-virtual-machine-scale-set"></a><span data-ttu-id="0f1b7-235">Min grupp fungerar med flera certifikat som har distribuerats toous som .cer offentliga nycklar.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-235">My team works with several certificates that are distributed toous as .cer public keys.</span></span> <span data-ttu-id="0f1b7-236">Vad är hello rekommenderat tillvägagångssätt för att distribuera dessa certifikat tooa virtuella datorns skaluppsättning?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-236">What is hello recommended approach for deploying these certificates tooa virtual machine scale set?</span></span>

<span data-ttu-id="0f1b7-237">skaluppsättning för virtuell dator toodeploy .cer offentliga nycklar tooa, kan du generera en .pfx-fil som innehåller CER-filer.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-237">toodeploy .cer public keys tooa virtual machine scale set, you can generate a .pfx file that contains only .cer files.</span></span> <span data-ttu-id="0f1b7-238">toodo denna, Använd `X509ContentType = Pfx`.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-238">toodo this, use `X509ContentType = Pfx`.</span></span> <span data-ttu-id="0f1b7-239">Till exempel läsa in hello .cer-fil som x509Certificate2 objekt i C# eller PowerShell och sedan anropa metoden hello.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-239">For example, load hello .cer file as an x509Certificate2 object in C# or PowerShell, and then call hello method.</span></span> 

<span data-ttu-id="0f1b7-240">Mer information finns i [X509Certificate.Export-metoden (X509ContentType, String)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-240">For more information, see [X509Certificate.Export Method (X509ContentType, String)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).</span></span>

### <a name="i-do-not-see-an-option-for-users-toopass-in-certificates-as-base64-strings-most-other-resource-providers-have-this-option"></a><span data-ttu-id="0f1b7-241">Jag ser inte ett alternativ för användare toopass i certifikat som base64-strängar.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-241">I do not see an option for users toopass in certificates as base64 strings.</span></span> <span data-ttu-id="0f1b7-242">De flesta andra resursleverantörer har det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-242">Most other resource providers have this option.</span></span>

<span data-ttu-id="0f1b7-243">tooemulate skicka i ett certifikat som en base64-sträng, kan du extrahera hello senaste version URL i en Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-243">tooemulate passing in a certificate as a base64 string, you can extract hello latest versioned URL in a Resource Manager template.</span></span> <span data-ttu-id="0f1b7-244">Inkludera hello följande JSON-egenskapen i Resource Manager-mall:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-244">Include hello following JSON property in your Resource Manager template:</span></span>

```json 
"certificateUrl": "[reference(resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults/secrets', parameters('vaultName'), parameters('secretName')), '2015-06-01').secretUriWithVersion]"
```
 
### <a name="do-i-have-toowrap-certificates-in-json-objects-in-key-vaults"></a><span data-ttu-id="0f1b7-245">Måste jag toowrap certifikat i JSON-objekt i nyckelvalv?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-245">Do I have toowrap certificates in JSON objects in key vaults?</span></span>

<span data-ttu-id="0f1b7-246">I skalningsuppsättningar i virtuella datorer och virtuella datorer, certifikaten omslutas i JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-246">In virtual machine scale sets and VMs, certificates must be wrapped in JSON objects.</span></span> 

<span data-ttu-id="0f1b7-247">Vi stöder också hello innehållstypen application/x-pkcs12.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-247">We also support hello content type application/x-pkcs12.</span></span> <span data-ttu-id="0f1b7-248">Anvisningar om hur du använder application/x-pkcs12 finns [PFX-certifikat i Azure Key Vault](http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-248">For instructions on using application/x-pkcs12, see [PFX certificates in Azure Key Vault](http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/).</span></span>
 
<span data-ttu-id="0f1b7-249">Vi stöder för närvarande inte CER-filer.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-249">We currently do not support .cer files.</span></span> <span data-ttu-id="0f1b7-250">toouse CER-filer, exportera dem till .pfx-behållare.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-250">toouse .cer files, export them into .pfx containers.</span></span>



## <a name="compliance"></a><span data-ttu-id="0f1b7-251">Efterlevnad</span><span class="sxs-lookup"><span data-stu-id="0f1b7-251">Compliance</span></span>

### <a name="are-virtual-machine-scale-sets-pci-compliant"></a><span data-ttu-id="0f1b7-252">Är virtuella datorn anger PCI-kompatibel?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-252">Are virtual machine scale sets PCI-compliant?</span></span>

<span data-ttu-id="0f1b7-253">Skaluppsättningar för den virtuella datorn är en tunn API skikt som ligger ovanpå hello CRP.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-253">Virtual machine scale sets are a thin API layer on top of hello CRP.</span></span> <span data-ttu-id="0f1b7-254">Båda komponenterna är en del av hello beräkningsplattform i hello Azure service-trädet.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-254">Both components are part of hello compute platform in hello Azure service tree.</span></span>

<span data-ttu-id="0f1b7-255">Skaluppsättningar för den virtuella datorn är en grundläggande del av hello Azure beräkningsplattform ur kompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-255">From a compliance perspective, virtual machine scale sets are a fundamental part of hello Azure compute platform.</span></span> <span data-ttu-id="0f1b7-256">De delar ett team, verktyg, processer, distributionsmetoden, säkerhetsåtgärder, just-in-time (JIT) kompilering, övervakning, aviseringar och så vidare med hello CRP sig själv.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-256">They share a team, tools, processes, deployment methodology, security controls, just-in-time (JIT) compilation, monitoring, alerting, and so on, with hello CRP itself.</span></span> <span data-ttu-id="0f1b7-257">Skaluppsättningar för den virtuella datorn är PCI Payment Card Industry ()-kompatibel eftersom hello CRP ingår för hello aktuella PCI Data Security Standard (DSS).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-257">Virtual machine scale sets are Payment Card Industry (PCI)-compliant because hello CRP is part of hello current PCI Data Security Standard (DSS) attestation.</span></span>

<span data-ttu-id="0f1b7-258">Mer information finns i [hello Microsoft Trust Center](https://www.microsoft.com/TrustCenter/Compliance/PCI).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-258">For more information, see [hello Microsoft Trust Center](https://www.microsoft.com/TrustCenter/Compliance/PCI).</span></span>






## <a name="extensions"></a><span data-ttu-id="0f1b7-259">Tillägg</span><span class="sxs-lookup"><span data-stu-id="0f1b7-259">Extensions</span></span>

### <a name="how-do-i-delete-a-virtual-machine-scale-set-extension"></a><span data-ttu-id="0f1b7-260">Hur tar jag bort en virtuell dator skala utökas?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-260">How do I delete a virtual machine scale set extension?</span></span>

<span data-ttu-id="0f1b7-261">toodelete en virtuell dator skala Ange filnamnstillägget, Använd hello följande PowerShell-exempel:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-261">toodelete a virtual machine scale set extension, use hello following PowerShell example:</span></span>

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" 

$vmss=Remove-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "extensionName"

Update-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" -VirtualMacineScaleSet $vmss
```
 
<span data-ttu-id="0f1b7-262">Du kan hitta hello Tilläggsnamn värdet i `$vmss`.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-262">You can find hello extensionName value in `$vmss`.</span></span>
   
### <a name="is-there-a-virtual-machine-scale-set-template-example-that-integrates-with-operations-management-suite"></a><span data-ttu-id="0f1b7-263">Finns det en virtuell dator skala ange mall-exempel som kan integreras med Operations Management Suite?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-263">Is there a virtual machine scale set template example that integrates with Operations Management Suite?</span></span>

<span data-ttu-id="0f1b7-264">För en virtuell dator skala har angetts exempel som kan integreras med Operations Management Suite finns hello andra exemplet i [distribuera ett Azure Service Fabric-kluster och aktivera övervakning genom att använda Log Analytics](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-264">For a virtual machine scale set template example that integrates with Operations Management Suite, see hello second example in [Deploy an Azure Service Fabric cluster and enable monitoring by using Log Analytics](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric).</span></span>
   
### <a name="extensions-seem-toorun-in-parallel-on-virtual-machine-scale-sets-this-causes-my-custom-script-extension-toofail-what-can-i-do-toofix-this"></a><span data-ttu-id="0f1b7-265">Tillägg verka toorun parallellt på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-265">Extensions seem toorun in parallel on virtual machine scale sets.</span></span> <span data-ttu-id="0f1b7-266">Detta leder till min tillägget toofail för anpassat skript.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-266">This causes my custom script extension toofail.</span></span> <span data-ttu-id="0f1b7-267">Vad kan jag göra toofix detta?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-267">What can I do toofix this?</span></span>

<span data-ttu-id="0f1b7-268">toolearn om tillägget ordningsföljd i skalningsuppsättningar i virtuella datorer, se [tillägget ordningsföljd i skalningsuppsättningar i virtuella Azure-datorn](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-268">toolearn about extension sequencing in virtual machine scale sets, see [Extension sequencing in Azure virtual machine scale sets](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).</span></span>
 
 
### <a name="how-do-i-reset-hello-password-for-vms-in-my-virtual-machine-scale-set"></a><span data-ttu-id="0f1b7-269">Hur Återställ hello lösenord för virtuella datorer i min skaluppsättning för virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-269">How do I reset hello password for VMs in my virtual machine scale set?</span></span>

<span data-ttu-id="0f1b7-270">tooreset hello lösenordet för virtuella datorer i din virtuella skala inställd och använda VM access-tillägg.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-270">tooreset hello password for VMs in your virtual machine scale set, use VM access extensions.</span></span> 

<span data-ttu-id="0f1b7-271">Använd följande PowerShell-exempel hello:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-271">Use hello following PowerShell example:</span></span>

```powershell
$vmssName = "myvmss"
$vmssResourceGroup = "myvmssrg"
$publicConfig = @{"UserName" = "newuser"}
$privateConfig = @{"Password" = "********"}
 
$extName = "VMAccessAgent"
$publisher = "Microsoft.Compute"
$vmss = Get-AzureRmVmss -ResourceGroupName $vmssResourceGroup -VMScaleSetName $vmssName
$vmss = Add-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name $extName -Publisher $publisher -Setting $publicConfig -ProtectedSetting $privateConfig -Type $extName -TypeHandlerVersion "2.0" -AutoUpgradeMinorVersion $true
Update-AzureRmVmss -ResourceGroupName $vmssResourceGroup -Name $vmssName -VirtualMachineScaleSet $vmss
```
 
 
### <a name="how-do-i-add-an-extension-tooall-vms-in-my-virtual-machine-scale-set"></a><span data-ttu-id="0f1b7-272">Hur lägger jag till ett tillägg tooall virtuella datorer i min skaluppsättning för virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-272">How do I add an extension tooall VMs in my virtual machine scale set?</span></span>

<span data-ttu-id="0f1b7-273">Om principen anges för**automatisk**, hello omdistribuera mallen med hello nya egenskaper för webbtjänsttillägg uppdaterar alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-273">If update policy is set too**automatic**, redeploying hello template with hello new extension properties updates all VMs.</span></span>

<span data-ttu-id="0f1b7-274">Om principen anges för**manuell**först uppdatera hello tillägg och sedan manuellt uppdatera alla instanser i dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-274">If update policy is set too**manual**, first update hello extension, and then manually update all instances in your VMs.</span></span>

  
### <a name="if-hello-extensions-associated-with-an-existing-virtual-machine-scale-set-are-updated-are-existing-vms-affected-that-is-will-hello-vms-not-match-hello-virtual-machine-scale-set-model-or-are-they-ignored-when-an-existing-machine-is-service-healed-or-reimaged-are-hello-scripts-that-are-currently-configured-on-hello-virtual-machine-scale-set-executed-or-are-hello-scripts-that-were-configured-when-hello-vm-was-first-created-used"></a><span data-ttu-id="0f1b7-275">Om hello-tillägg som är associerade med en befintlig skaluppsättning för virtuell dator uppdateras befintliga virtuella datorer som påverkas?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-275">If hello extensions associated with an existing virtual machine scale set are updated, are existing VMs affected?</span></span> <span data-ttu-id="0f1b7-276">(Det vill säga kommer hello VMs *inte* matchar hello virtuella skala modellen?) Eller ignoreras de?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-276">(That is, will hello VMs *not* match hello virtual machine scale set model?) Or are they ignored?</span></span> <span data-ttu-id="0f1b7-277">När en befintlig dator tjänsten lagats eller avbildade, är hello-skript som är konfigurerade på hello virtuella datorns skaluppsättning körs eller är hello-skript som konfigurerades när hello VM skapades används?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-277">When an existing machine is service-healed or reimaged, are hello scripts that are currently configured on hello virtual machine scale set executed, or are hello scripts that were configured when hello VM was first created used?</span></span>

<span data-ttu-id="0f1b7-278">Om hello resurstilläggsdefinitionen i hello virtuella skala modellen uppdateras och hello upgradePolicy egenskapen för**automatisk**, uppdaterar den hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-278">If hello extension definition in hello virtual machine scale set model is updated and hello upgradePolicy property is set too**automatic**, it updates hello VMs.</span></span> <span data-ttu-id="0f1b7-279">Om hello upgradePolicy egenskapen för**manuell**, tillägg har flaggats som inte matchar hello modellen.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-279">If hello upgradePolicy property is set too**manual**, extensions are flagged as not matching hello model.</span></span> 

<span data-ttu-id="0f1b7-280">Om en befintlig virtuell dator är tjänsten lagats, visas den som en omstart och hello tillägg är inte igen.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-280">If an existing VM is service-healed, it appears as a reboot, and hello extensions are not rerun.</span></span> <span data-ttu-id="0f1b7-281">Om det avbildade, är det som ersätter hello OS-enhet med hello Källavbildningen.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-281">If it is reimaged, it's like replacing hello OS drive with hello source image.</span></span> <span data-ttu-id="0f1b7-282">Alla specialisering från hello senaste modell, till exempel tillägg, körs.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-282">Any specialization from hello latest model, such as extensions, are run.</span></span>
 
### <a name="how-do-i-join-a-virtual-machine-scale-set-tooan-azure-ad-domain"></a><span data-ttu-id="0f1b7-283">Hur jag ansluta till en virtuell dator skala set tooan Azure AD-domän</span><span class="sxs-lookup"><span data-stu-id="0f1b7-283">How do I join a virtual machine scale set tooan Azure AD domain?</span></span>

<span data-ttu-id="0f1b7-284">toojoin en virtuell dator scale set tooan Azure Active Directory (AD Azure) domän, kan du definiera ett tillägg.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-284">toojoin a virtual machine scale set tooan Azure Active Directory (Azure AD) domain, you can define an extension.</span></span> 

<span data-ttu-id="0f1b7-285">toodefine filnamnstillägget, Använd hello JsonADDomainExtension egenskapen:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-285">toodefine an extension, use hello JsonADDomainExtension property:</span></span>

```json
"extensionProfile": {
    "extensions": [
        {
            "name": "joindomain",
            "properties": {
                "publisher": "Microsoft.Compute",
                "type": "JsonADDomainExtension",
                "typeHandlerVersion": "1.3",
                "settings": {
                    "Name": "[parameters('domainName')]",
                    "OUPath": "[variables('ouPath')]",
                    "User": "[variables('domainAndUsername')]",
                    "Restart": "true",
                    "Options": "[variables('domainJoinOptions')]"
                },
                "protectedsettings": {
                    "Password": "[parameters('domainJoinPassword')]"
                }
            }
        }
    ]
}
```
 
### <a name="my-virtual-machine-scale-set-extension-is-trying-tooinstall-something-that-requires-a-reboot-for-example-commandtoexecute-powershellexe--executionpolicy-unrestricted-install-windowsfeature-name-fs-resource-manager-includemanagementtools"></a><span data-ttu-id="0f1b7-286">Min virtuella skala utökas försöker tooinstall något som kräver en omstart.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-286">My virtual machine scale set extension is trying tooinstall something that requires a reboot.</span></span> <span data-ttu-id="0f1b7-287">Till exempel ”commandToExecute” ”: powershell.exe - ExecutionPolicy obegränsad Install-WindowsFeature-Name FS-Resource-Manager – IncludeManagementTools”</span><span class="sxs-lookup"><span data-stu-id="0f1b7-287">For example, "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted Install-WindowsFeature –Name FS-Resource-Manager –IncludeManagementTools"</span></span>

<span data-ttu-id="0f1b7-288">Om din virtuella skala utökas försöker tooinstall något som kräver omstart, kan du använda hello Azure Automation Desired State Configuration (Automation DSC)-tillägg.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-288">If your virtual machine scale set extension is trying tooinstall something that requires a reboot, you can use hello Azure Automation Desired State Configuration (Automation DSC) extension.</span></span> <span data-ttu-id="0f1b7-289">Om hello operativsystemet är Windows Server 2012 R2, Azure tar emot hello-inställningar för Windows Management Framework (WMF) 5.0, omstarter, och fortsätter sedan med hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-289">If hello operating system is Windows Server 2012 R2, Azure pulls in hello Windows Management Framework (WMF) 5.0 setup, reboots, and then continues with hello configuration.</span></span> 
 
### <a name="how-do-i-turn-on-antimalware-in-my-virtual-machine-scale-set"></a><span data-ttu-id="0f1b7-290">Hur aktiverar jag program mot skadlig kod i min skaluppsättning för virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-290">How do I turn on antimalware in my virtual machine scale set?</span></span>

<span data-ttu-id="0f1b7-291">tooturn för program mot skadlig kod på din virtuella skala ange använder hello följande PowerShell-exempel:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-291">tooturn on antimalware on your virtual machine scale set, use hello following PowerShell example:</span></span>

```powershell
$rgname = 'autolap'
$vmssname = 'autolapbr'
$location = 'eastus'
 
# Retrieve hello most recent version number of hello extension.
$allVersions= (Get-AzureRmVMExtensionImage -Location $location -PublisherName "Microsoft.Azure.Security" -Type "IaaSAntimalware").Version
$versionString = $allVersions[($allVersions.count)-1].Split(".")[0] + "." + $allVersions[($allVersions.count)-1].Split(".")[1]
 
$VMSS = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname
echo $VMSS
Add-AzureRmVmssExtension -VirtualMachineScaleSet $VMSS -Name "IaaSAntimalware" -Publisher "Microsoft.Azure.Security" -Type "IaaSAntimalware" -TypeHandlerVersion $versionString
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $VMSS 
```

### <a name="i-need-tooexecute-a-custom-script-thats-hosted-in-a-private-storage-account-hello-script-runs-successfully-when-hello-storage-is-public-but-when-i-try-toouse-a-shared-access-signature-sas-it-fails-this-message-is-displayed-missing-mandatory-parameters-for-valid-shared-access-signature-linksas-works-fine-from-my-local-browser"></a><span data-ttu-id="0f1b7-292">Jag behöver tooexecute ett anpassat skript som finns i ett privat storage-konto.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-292">I need tooexecute a custom script that's hosted in a private storage account.</span></span> <span data-ttu-id="0f1b7-293">hello skriptet körs utan problem när hello lagring är offentlig, men när jag försöker toouse en delad signatur åtkomst (SAS), misslyckas.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-293">hello script runs successfully when hello storage is public, but when I try toouse a Shared Access Signature (SAS), it fails.</span></span> <span data-ttu-id="0f1b7-294">Det här meddelandet visas: ”obligatoriska parametrar saknas för giltig signatur för delad åtkomst”.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-294">This message is displayed: “Missing mandatory parameters for valid Shared Access Signature”.</span></span> <span data-ttu-id="0f1b7-295">Länken + SAS fungerar bra i min lokala webbläsare.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-295">Link+SAS works fine from my local browser.</span></span>

<span data-ttu-id="0f1b7-296">tooexecute ett anpassat skript som finns i ett privat storage-konto för att konfigurera skyddade inställningar med hello lagringskontonyckel och namn.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-296">tooexecute a custom script that's hosted in a private storage account, set up protected settings with hello storage account key and name.</span></span> <span data-ttu-id="0f1b7-297">Mer information finns i [anpassade skript tillägget för Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-297">For more information, see [Custom Script Extension for Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).</span></span>







## <a name="networking"></a><span data-ttu-id="0f1b7-298">Nätverk</span><span class="sxs-lookup"><span data-stu-id="0f1b7-298">Networking</span></span>
 
### <a name="is-it-possible-tooassign-a-network-security-group-nsg-tooa-scale-set-so-that-it-will-apply-tooall-hello-vm-nics-in-hello-set"></a><span data-ttu-id="0f1b7-299">Är det möjligt tooassign en Nätverkssäkerhetsgrupp (NSG) tooa skala har angetts så att tillämpa tooall hello Virtuella nätverkskort i hello uppsättningen?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-299">Is it possible tooassign a Network Security Group (NSG) tooa scale set, so that it will apply tooall hello VM NICs in hello set?</span></span>

<span data-ttu-id="0f1b7-300">Ja.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-300">Yes.</span></span> <span data-ttu-id="0f1b7-301">En Nätverkssäkerhetsgrupp kan tillämpas direkt tooa skala som refererar till den i hello networkInterfaceConfigurations avsnitt av hello nätverksprofil.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-301">A Network Security Group can be applied directly tooa scale set by referencing it in hello networkInterfaceConfigurations section of hello network profile.</span></span> <span data-ttu-id="0f1b7-302">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-302">Example:</span></span>

```json
"networkProfile": {
    "networkInterfaceConfigurations": [
        {
            "name": "nic1",
            "properties": {
                "primary": "true",
                "ipConfigurations": [
                    {
                        "name": "ip1",
                        "properties": {
                            "subnet": {
                                "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/virtualNetworks/', variables('vnetName'), '/subnets/subnet1')]"
                            }
                "loadBalancerInboundNatPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/inboundNatPools/natPool1')]"
                                }
                            ],
                            "loadBalancerBackendAddressPools": [
                                {
                                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/loadBalancers/', variables('lbName'), '/backendAddressPools/addressPool1')]"
                                 }
                            ]
                        }
                    }
                ],
                "networkSecurityGroup": {
                    "id": "[concat('/subscriptions/', subscription().subscriptionId,'/resourceGroups/', resourceGroup().name, '/providers/Microsoft.Network/networkSecurityGroups/', variables('nsgName'))]"
                }
            }
        }
    ]
}
```

### <a name="how-do-i-do-a-vip-swap-for-virtual-machine-scale-sets-in-hello-same-subscription-and-same-region"></a><span data-ttu-id="0f1b7-303">Hur gör jag en VIP-växling för virtuella datorn anger i hello samma prenumeration och samma region?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-303">How do I do a VIP swap for virtual machine scale sets in hello same subscription and same region?</span></span>

<span data-ttu-id="0f1b7-304">Om du har två virtuella datorn anger med Azure belastningsutjämnare framför-servrar och de finns i Hej samma prenumeration och region, du kan frigöra hello offentliga IP-adresser från var och en och tilldela toohello andra.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-304">If you have two virtual machine scale sets with Azure Load Balancer front-ends, and they are in hello same subscription and region, you could deallocate hello public IP addresses from each one, and assign toohello other.</span></span> <span data-ttu-id="0f1b7-305">Se [VIP-växling: blå-grön distribution i Azure Resource Manager](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) t.ex.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-305">See [VIP Swap: Blue-green deployment in Azure Resource Manager](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) for example.</span></span> <span data-ttu-id="0f1b7-306">Detta innebär en fördröjning som hello resurser är frigjord/allokerade på hello nätverksnivån.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-306">This does imply a delay though as hello resources are deallocated/allocated at hello network level.</span></span> <span data-ttu-id="0f1b7-307">Ett snabbare alternativ är toouse Azure Application Gateway med två serverdelspooler och en regel för vidarebefordran.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-307">A faster option is toouse Azure Application Gateway with two backend pools, and a routing rule.</span></span> <span data-ttu-id="0f1b7-308">Du kan också värd för ditt program med [Azure App service](https://azure.microsoft.com/en-us/services/app-service/) som ger stöd för snabb växling mellan olika platser för mellanlagring och produktion.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-308">Alternatively, you could host your application with [Azure App service](https://azure.microsoft.com/en-us/services/app-service/) which provides support for fast switching between staging and production slots.</span></span>
 
### <a name="how-do-i-specify-a-range-of-private-ip-addresses-toouse-for-static-private-ip-address-allocation"></a><span data-ttu-id="0f1b7-309">Hur jag för att ange ett intervall med privata IP-adresser toouse för statisk privat IP-adressallokering?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-309">How do I specify a range of private IP addresses toouse for static private IP address allocation?</span></span>

<span data-ttu-id="0f1b7-310">IP-adresser har markerats från ett undernät som du anger.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-310">IP addresses are selected from a subnet that you specify.</span></span> 

<span data-ttu-id="0f1b7-311">hello fördelning av virtuella skala uppsättning IP-adresser är alltid ”dynamic”, men det betyder att du kan ändra dessa IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-311">hello allocation method of virtual machine scale set IP addresses is always “dynamic,” but that doesn't mean that these IP addresses can change.</span></span> <span data-ttu-id="0f1b7-312">I det här fallet betyder ”dynamic” bara att du inte anger hello IP-adress i en PUT-begäran.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-312">In this case, "dynamic" only means that you do not specify hello IP address in a PUT request.</span></span> <span data-ttu-id="0f1b7-313">Ange hello statiska in med hjälp av hello undernät.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-313">Specify hello static set by using hello subnet.</span></span> 
    
### <a name="how-do-i-deploy-a-virtual-machine-scale-set-tooan-existing-azure-virtual-network"></a><span data-ttu-id="0f1b7-314">Hur distribuerar en virtuell dator scale set tooan befintliga virtuella Azure-nätverket?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-314">How do I deploy a virtual machine scale set tooan existing Azure virtual network?</span></span> 

<span data-ttu-id="0f1b7-315">toodeploy en virtuell dator skala tooan befintliga Azure-nätverk finns i avsnittet [distribuera en virtuell dator scale set tooan befintligt virtuellt nätverk](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-315">toodeploy a virtual machine scale set tooan existing Azure virtual network, see [Deploy a virtual machine scale set tooan existing virtual network](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet).</span></span> 

### <a name="how-do-i-add-hello-ip-address-of-hello-first-vm-in-a-virtual-machine-scale-set-toohello-output-of-a-template"></a><span data-ttu-id="0f1b7-316">Hur lägger jag till hello IP-adress för hello första virtuella datorn i en virtuell dator skala ange toohello utdata från en mall?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-316">How do I add hello IP address of hello first VM in a virtual machine scale set toohello output of a template?</span></span>

<span data-ttu-id="0f1b7-317">tooadd hello IP-adressen för hello första virtuella datorn i en virtuell dator scale set toohello utdata från en mall finns [ARM: hämta VMSS privata IP-adresser](http://stackoverflow.com/questions/42790392/arm-get-vmsss-private-ips).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-317">tooadd hello IP address of hello first VM in a virtual machine scale set toohello output of a template, see [ARM: Get VMSS's private IPs](http://stackoverflow.com/questions/42790392/arm-get-vmsss-private-ips).</span></span>

### <a name="can-i-use-scale-sets-with-accelerated-networking"></a><span data-ttu-id="0f1b7-318">Kan jag använda skaluppsättningar med snabbare nätverk?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-318">Can I use scale sets with Accelerated Networking?</span></span>

<span data-ttu-id="0f1b7-319">Ja.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-319">Yes.</span></span> <span data-ttu-id="0f1b7-320">toouse snabbare nätverk, ange enableAcceleratedNetworking tootrue i din skala uppsättningens networkInterfaceConfigurations inställningar.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-320">toouse accelerated networking, set enableAcceleratedNetworking tootrue in your scale set's networkInterfaceConfigurations settings.</span></span> <span data-ttu-id="0f1b7-321">T.ex.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-321">E.g.</span></span>
```json
"networkProfile": {
    "networkInterfaceConfigurations": [
    {
        "name": "niconfig1",
        "properties": {
        "primary": true,
        "enableAcceleratedNetworking" : true,
        "ipConfigurations": [
                ]
            }
            }
        ]
        }
    }
    ]
}
```

### <a name="how-can-i-configure-hello-dns-servers-used-by-a-scale-set"></a><span data-ttu-id="0f1b7-322">Hur konfigurerar jag hello DNS-servrar som används av en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="0f1b7-322">How can I configure hello DNS servers used by a scale set?</span></span>

<span data-ttu-id="0f1b7-323">toocreate en skaluppsättning för virtuell dator med en anpassad DNS-konfiguration, lägga till en dnsSettings JSON paket toohello skala networkInterfaceConfigurations avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-323">toocreate a VM scale set with a custom DNS configuration, add a dnsSettings JSON packet toohello scale set networkInterfaceConfigurations section.</span></span> <span data-ttu-id="0f1b7-324">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-324">Example:</span></span>
```json
    "dnsSettings":{
        "dnsServers":["10.0.0.6", "10.0.0.5"]
    }
```

### <a name="how-can-i-configure-a-scale-set-tooassign-a-public-ip-address-tooeach-vm"></a><span data-ttu-id="0f1b7-325">Hur konfigurerar en scale set tooassign en offentlig IP-adress tooeach VM?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-325">How can I configure a scale set tooassign a public IP address tooeach VM?</span></span>

<span data-ttu-id="0f1b7-326">toocreate en skaluppsättning för virtuell dator som tilldelar en offentlig IP-adress tooeach VM, kontrollera hello API-versionen av hello Microsoft.Compute/virtualMAchineScaleSets resursen är 2017-03-30 och lägga till en _publicipaddressconfiguration_ JSON-paket toohello skaluppsättning ipConfigurations avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-326">toocreate a VM scale set that assigns a public IP address tooeach VM, make sure hello API version of hello Microsoft.Compute/virtualMAchineScaleSets resource is 2017-03-30, and add a _publicipaddressconfiguration_ JSON packet toohello scale set ipConfigurations section.</span></span> <span data-ttu-id="0f1b7-327">Exempel:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-327">Example:</span></span>

```json
    "publicipaddressconfiguration": {
        "name": "pub1",
        "properties": {
        "idleTimeoutInMinutes": 15
        }
    }
```

### <a name="can-i-configure-a-scale-set-toowork-with-multiple-application-gateways"></a><span data-ttu-id="0f1b7-328">Konfigurerar en scale set toowork med flera Programgatewayer?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-328">Can I configure a scale set toowork with multiple Application Gateways?</span></span>

<span data-ttu-id="0f1b7-329">Ja.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-329">Yes.</span></span> <span data-ttu-id="0f1b7-330">Du kan lägga till hello resurs-id's för flera Programgateway backend adresspooler toohello _applicationGatewayBackendAddressPools_ listan i hello _ipConfigurations_ avsnitt i din skaluppsättning nätverksprofil.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-330">You can add hello resource id's for multiple Application Gateway backend address pools toohello _applicationGatewayBackendAddressPools_ list in hello _ipConfigurations_ section of your scale set network profile.</span></span>

## <a name="scale"></a><span data-ttu-id="0f1b7-331">Skala</span><span class="sxs-lookup"><span data-stu-id="0f1b7-331">Scale</span></span>

### <a name="in-what-case-would-i-create-a-virtual-machine-scale-set-with-fewer-than-two-vms"></a><span data-ttu-id="0f1b7-332">I vilket fall skulle jag skapa en virtuell dator-skala med färre än två virtuella datorer?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-332">In what case would I create a virtual machine scale set with fewer than two VMs?</span></span>

<span data-ttu-id="0f1b7-333">En orsak toocreate en virtuell dator skaluppsättning med färre än två virtuella datorer toouse hello elastisk egenskaperna för en virtuell dator skala anges.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-333">One reason toocreate a virtual machine scale set with fewer than two VMs would be toouse hello elastic properties of a virtual machine scale set.</span></span> <span data-ttu-id="0f1b7-334">Du kan till exempel distribuera en skaluppsättning för virtuell dator med noll VMs toodefine infrastrukturen utan att betala VM driftskostnader.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-334">For example, you could deploy a virtual machine scale set with zero VMs toodefine your infrastructure without paying VM running costs.</span></span> <span data-ttu-id="0f1b7-335">Sedan, när du är klar toodeploy VMs, öka hello ”kapacitet” hello virtual machine scale set toohello produktion instansantal.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-335">Then, when you are ready toodeploy VMs, increase hello “capacity” of hello virtual machine scale set toohello production instance count.</span></span>

<span data-ttu-id="0f1b7-336">En annan orsak kan du skapa en virtuell dator-skala med färre än två virtuella datorer är om du vill göra mindre med tillgänglighet än med en tillgänglighetsuppsättning med diskreta virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-336">Another reason you might create a virtual machine scale set with fewer than two VMs is if you're concerned less with availability than in using an availability set with discrete VMs.</span></span> <span data-ttu-id="0f1b7-337">Skaluppsättningar för den virtuella datorn får du ett sätt toowork med odifferentierad beräknings-enheter som är fungibla.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-337">Virtual machine scale sets give you a way toowork with undifferentiated compute units that are fungible.</span></span> <span data-ttu-id="0f1b7-338">Denna enhetlighet är nyckelfaktorn för skalningsuppsättningar i virtuella datorer jämfört med tillgänglighetsuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-338">This uniformity is a key differentiator for virtual machine scale sets versus availability sets.</span></span> <span data-ttu-id="0f1b7-339">Många tillståndslösa arbetsbelastningar spåra inte enskilda enheter.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-339">Many stateless workloads do not track individual units.</span></span> <span data-ttu-id="0f1b7-340">Om hello arbetsbelastning släpper du skala ned tooone beräkning enhet och skala upp toomany när hello arbetsbelastningen ökar.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-340">If hello workload drops, you can scale down tooone compute unit, and then scale up toomany when hello workload increases.</span></span>

### <a name="how-do-i-change-hello-number-of-vms-in-a-virtual-machine-scale-set"></a><span data-ttu-id="0f1b7-341">Hur ändrar jag hello antal virtuella datorer i en skaluppsättning för virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-341">How do I change hello number of VMs in a virtual machine scale set?</span></span>

<span data-ttu-id="0f1b7-342">toochange hello antal virtuella datorer i en skaluppsättning för virtuell dator finns [ändra hello instanser av en virtuella datorns skaluppsättning](https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-342">toochange hello number of VMs in a virtual machine scale set, see [Change hello instance count of a virtual machine scale set](https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/).</span></span>

### <a name="how-do-i-define-custom-alerts-for-when-certain-thresholds-are-reached"></a><span data-ttu-id="0f1b7-343">Hur jag för att definiera anpassade aviseringar för när vissa trösklar har uppnåtts?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-343">How do I define custom alerts for when certain thresholds are reached?</span></span>

<span data-ttu-id="0f1b7-344">Har du viss flexibilitet i hur du hanterar aviseringar för angivna tröskelvärden.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-344">You have some flexibility in how you handle alerts for specified thresholds.</span></span> <span data-ttu-id="0f1b7-345">Du kan till exempel definiera anpassade webhooks.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-345">For example, you can define customized webhooks.</span></span> <span data-ttu-id="0f1b7-346">hello följande webhook exempel är från en Resource Manager-mall:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-346">hello following webhook example is from a Resource Manager template:</span></span>

```json
{
    "type": "Microsoft.Insights/autoscaleSettings",
    "apiVersion": "[variables('insightsApi')]",
    "name": "autoscale",
    "location": "[parameters('resourceLocation')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]"
    ],
    "properties": {
        "name": "autoscale",
        "targetResourceUri": "[concat('/subscriptions/',subscription().subscriptionId, '/resourceGroups/',  resourceGroup().name, '/providers/Microsoft.Compute/virtualMachineScaleSets/', parameters('vmSSName'))]",
        "enabled": true,
        "notifications": [
            {
                "operation": "Scale",
                "email": {
                    "sendToSubscriptionAdministrator": true,
                    "sendToSubscriptionCoAdministrators": true,
                    "customEmails": [
                        "youremail@address.com"
                    ]
                },
                "webhooks": [
                    {
                        "serviceUri": "https://events.pagerduty.com/integration/0b75b57246814149b4d87fa6e1273687/enqueue",
                        "properties": {
                            "key1": "custommetric",
                            "key2": "scalevmss"
                        }
                    }
                ]
            }
        ],
```

<span data-ttu-id="0f1b7-347">I det här exemplet skickas en avisering tooPagerduty.com när ett tröskelvärde uppnås.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-347">In this example, an alert goes tooPagerduty.com when a threshold is reached.</span></span>



## <a name="patching-and-operations"></a><span data-ttu-id="0f1b7-348">Korrigering och åtgärder</span><span class="sxs-lookup"><span data-stu-id="0f1b7-348">Patching and operations</span></span>

### <a name="how-do-i-create-a-scale-set-in-an-existing-resource-group"></a><span data-ttu-id="0f1b7-349">Hur skapar jag en skala som angetts i en befintlig resursgrupp</span><span class="sxs-lookup"><span data-stu-id="0f1b7-349">How do I create a scale set in an existing resource group?</span></span>

<span data-ttu-id="0f1b7-350">Skapa skalningsuppsättningar i en befintlig resurs grupp är ännu inte möjligt från hello Azure-portalen, men du kan ange en befintlig resursgrupp när distribuera en skala från en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-350">Creating scale sets in an existing resource group is not yet possible from hello Azure portal, but you can specify an existing resource group when deploying a scale set from an Azure Resource Manager template.</span></span> <span data-ttu-id="0f1b7-351">Du kan också ange en befintlig resursgrupp när du skapar en skala som anges med Azure PowerShell eller CLI.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-351">You can also specify an existing resource group when creating a scale set using Azure PowerShell or CLI.</span></span>

### <a name="can-we-move-a-scale-set-tooanother-resource-group"></a><span data-ttu-id="0f1b7-352">Kan vi flytta skaluppsättning tooanother resursgrupp?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-352">Can we move a scale set tooanother resource group?</span></span>

<span data-ttu-id="0f1b7-353">Ja, kan du flytta skala uppsättning resurser tooa ny prenumeration eller resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-353">Yes, you can move scale set resources tooa new subscription or resource group.</span></span>

### <a name="how-tooi-update-my-virtual-machine-scale-set-tooa-new-image-how-do-i-manage-patching"></a><span data-ttu-id="0f1b7-354">Hur tooI uppdatera skaluppsättning min virtuella tooa ny avbildning?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-354">How tooI update my virtual machine scale set tooa new image?</span></span> <span data-ttu-id="0f1b7-355">Hur hanterar jag korrigering?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-355">How do I manage patching?</span></span>

<span data-ttu-id="0f1b7-356">tooupdate din virtuella skaluppsättning tooa ny avbildning och toomanage korrigering, se [uppgradera en virtuella datorns skaluppsättning](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-356">tooupdate your virtual machine scale set tooa new image, and toomanage patching, see [Upgrade a virtual machine scale set](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).</span></span>

### <a name="can-i-use-hello-reimage-operation-tooreset-a-vm-without-changing-hello-image-that-is-i-want-reset-a-vm-toofactory-settings-rather-than-tooa-new-image"></a><span data-ttu-id="0f1b7-357">Kan jag använda hello avbildningsåterställning åtgärden tooreset en virtuell dator utan att ändra hello avbildning?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-357">Can I use hello reimage operation tooreset a VM without changing hello image?</span></span> <span data-ttu-id="0f1b7-358">(Det vill säga jag vill Återställ en VM toofactory inställningar i stället för tooa ny avbildning.)</span><span class="sxs-lookup"><span data-stu-id="0f1b7-358">(That is, I want reset a VM toofactory settings rather than tooa new image.)</span></span>

<span data-ttu-id="0f1b7-359">Ja, kan du använda hello avbildningsåterställning åtgärden tooreset en virtuell dator utan att ändra hello avbildningen.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-359">Yes, you can use hello reimage operation tooreset a VM without changing hello image.</span></span> <span data-ttu-id="0f1b7-360">Men om din virtuella skalan refererar till en plattformsavbildning med `version = latest`, den virtuella datorn kan uppdatera tooa senare OS-avbildningen när du anropar `reimage`.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-360">However, if your virtual machine scale set references a platform image with `version = latest`, your VM can update tooa later OS image when you call `reimage`.</span></span>

<span data-ttu-id="0f1b7-361">Mer information finns i [hantera alla virtuella datorer i en skaluppsättning för virtuell dator](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-361">For more information, see [Manage all VMs in a virtual machine scale set](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set).</span></span>



## <a name="troubleshooting"></a><span data-ttu-id="0f1b7-362">Felsökning</span><span class="sxs-lookup"><span data-stu-id="0f1b7-362">Troubleshooting</span></span>

### <a name="how-do-i-turn-on-boot-diagnostics"></a><span data-ttu-id="0f1b7-363">Hur aktiverar jag startdiagnostikinställningar?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-363">How do I turn on boot diagnostics?</span></span>

<span data-ttu-id="0f1b7-364">tooturn på startdiagnostikinställningar, först skapar ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-364">tooturn on boot diagnostics, first, create a storage account.</span></span> <span data-ttu-id="0f1b7-365">Placera sedan JSON blocket i din virtuella datorns skaluppsättning **virtualMachineProfile**, och uppdatera hello skaluppsättning för virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-365">Then, put this JSON block in your virtual machine scale set **virtualMachineProfile**, and update hello virtual machine scale set:</span></span>

```json
"diagnosticsProfile": {
    "bootDiagnostics": {
        "enabled": true,
        "storageUri": "http://yourstorageaccount.blob.core.windows.net"
    }
}
```

<span data-ttu-id="0f1b7-366">När en ny virtuell dator skapas visar hello InstanceView-egenskapen för hello VM hello information för hello skärmbild och så vidare.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-366">When a new VM is created, hello InstanceView property of hello VM shows hello details for hello screenshot, and so on.</span></span> <span data-ttu-id="0f1b7-367">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-367">Here's an example:</span></span>
 
```json
"bootDiagnostics": {
    "consoleScreenshotBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.screenshot.bmp",
    "serialConsoleLogBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.serialconsole.log"
  }
```


## <a name="virtual-machine-properties"></a><span data-ttu-id="0f1b7-368">Egenskaper för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0f1b7-368">Virtual machine properties</span></span>

### <a name="how-do-i-get-property-information-for-each-vm-without-making-multiple-calls-for-example-how-would-i-get-hello-fault-domain-for-each-of-hello-100-vms-in-my-virtual-machine-scale-set"></a><span data-ttu-id="0f1b7-369">Hur skaffar egenskapsinformation för varje virtuell dator utan att göra flera anrop?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-369">How do I get property information for each VM without making multiple calls?</span></span> <span data-ttu-id="0f1b7-370">Till exempel hur kan jag hello feldomän för varje hello 100 virtuella datorer i min skaluppsättning för virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-370">For example, how would I get hello fault domain for each of hello 100 VMs in my virtual machine scale set?</span></span>

<span data-ttu-id="0f1b7-371">information om tooget egenskaper för varje virtuell dator utan att göra flera anrop, kan du anropa `ListVMInstanceViews` genom att göra ett REST-API `GET` på hello efter resurs-URI:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-371">tooget property information for each VM without making multiple calls, you can call `ListVMInstanceViews` by doing a REST API `GET` on hello following resource URI:</span></span>

<span data-ttu-id="0f1b7-372">/subscriptions/ < PRENUMERATIONSID > /resourceGroups/ < resource_group_name > /providers/Microsoft.Compute/virtualMachineScaleSets/ < scaleset_name > / virtualMachines? $expand = instanceView & $select = instanceView</span><span class="sxs-lookup"><span data-stu-id="0f1b7-372">/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.Compute/virtualMachineScaleSets/<scaleset_name>/virtualMachines?$expand=instanceView&$select=instanceView</span></span>

### <a name="can-i-pass-different-extension-arguments-toodifferent-vms-in-a-virtual-machine-scale-set"></a><span data-ttu-id="0f1b7-373">Kan jag skicka annat tillägg argument toodifferent virtuella datorer i en skaluppsättning för virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-373">Can I pass different extension arguments toodifferent VMs in a virtual machine scale set?</span></span>

<span data-ttu-id="0f1b7-374">Du kan inte Nej, Skicka annat tillägg argument toodifferent virtuella datorer i en skaluppsättning för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-374">No, you cannot pass different extension arguments toodifferent VMs in a virtual machine scale set.</span></span> <span data-ttu-id="0f1b7-375">Tillägg kan dock fungera baserat på hello unika egenskaper för hello VM som körs på, så som på hello datornamnet.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-375">However, extensions can act based on hello unique properties of hello VM they are running on, such as on hello machine name.</span></span> <span data-ttu-id="0f1b7-376">Tillägg kan också fråga instans metadata på http://169.254.169.254 tooget mer information om hello VM.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-376">Extensions also can query instance metadata on http://169.254.169.254 tooget more information about hello VM.</span></span>

### <a name="why-are-there-gaps-between-my-virtual-machine-scale-set-vm-machine-names-and-vm-ids-for-example-0-1-3"></a><span data-ttu-id="0f1b7-377">Varför är det glapp mellan min Virtuella datornamn på virtuell dator skala och VM-ID: N?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-377">Why are there gaps between my virtual machine scale set VM machine names and VM IDs?</span></span> <span data-ttu-id="0f1b7-378">Till exempel: 0, 1, 3...</span><span class="sxs-lookup"><span data-stu-id="0f1b7-378">For example: 0, 1, 3...</span></span>

<span data-ttu-id="0f1b7-379">Det förekommer glapp mellan din Virtuella datornamn på virtuell dator skala och VM-ID: N eftersom skalan för din virtuella **overprovision** toohello standardvärdet anges i egenskapen **SANT**.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-379">There are gaps between your virtual machine scale set VM machine names and VM IDs because your virtual machine scale set **overprovision** property is set toohello default value of **true**.</span></span> <span data-ttu-id="0f1b7-380">Om överetablering har angetts för**SANT**, flera virtuella datorer än begärt skapas.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-380">If overprovisioning is set too**true**, more VMs than requested are created.</span></span> <span data-ttu-id="0f1b7-381">Extra VM: ar så tas bort.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-381">Extra VMs are then deleted.</span></span> <span data-ttu-id="0f1b7-382">I det här fallet du få bättre distribution tillförlitlighet, men på bekostnad av hello sammanhängande namngivning och sammanhängande (Network Address Translation Translation) regler.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-382">In this case, you gain increased deployment reliability, but at hello expense of contiguous naming and contiguous Network Address Translation (NAT) rules.</span></span> 

<span data-ttu-id="0f1b7-383">Du kan ange egenskapen för**FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-383">You can set this property too**false**.</span></span> <span data-ttu-id="0f1b7-384">För små virtuella skaluppsättningar påverkar detta avsevärt inte distribution tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-384">For small virtual machine scale sets, this doesn't significantly affect deployment reliability.</span></span>

### <a name="what-is-hello-difference-between-deleting-a-vm-in-a-virtual-machine-scale-set-and-deallocating-hello-vm-when-should-i-choose-one-over-hello-other"></a><span data-ttu-id="0f1b7-385">Vad är hello skillnaden mellan att ta bort en virtuell dator i en skaluppsättning för virtuell dator och det har frigjorts hello VM?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-385">What is hello difference between deleting a VM in a virtual machine scale set and deallocating hello VM?</span></span> <span data-ttu-id="0f1b7-386">När ska jag välja en över hello andra?</span><span class="sxs-lookup"><span data-stu-id="0f1b7-386">When should I choose one over hello other?</span></span>

<span data-ttu-id="0f1b7-387">hello största skillnaden mellan att ta bort en virtuell dator i en skaluppsättning för virtuell dator och det har frigjorts hello VM är att `deallocate` tas inte bort hello virtuella hårddiskar (VHD).</span><span class="sxs-lookup"><span data-stu-id="0f1b7-387">hello main difference between deleting a VM in a virtual machine scale set and deallocating hello VM is that `deallocate` doesn’t delete hello virtual hard disks (VHDs).</span></span> <span data-ttu-id="0f1b7-388">Finns kostnader för lagring som är associerade med körs `stop deallocate`.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-388">There are storage costs associated with running `stop deallocate`.</span></span> <span data-ttu-id="0f1b7-389">Du kan använda en eller hello andra för en av följande orsaker hello:</span><span class="sxs-lookup"><span data-stu-id="0f1b7-389">You might use one or hello other for one of hello following reasons:</span></span>

- <span data-ttu-id="0f1b7-390">Du vill toostop betalar beräkning kostnader, men du vill tookeep hello diskstatusen av hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-390">You want toostop paying compute costs, but you want tookeep hello disk state of hello VMs.</span></span>
- <span data-ttu-id="0f1b7-391">Vill du toostart en uppsättning virtuella datorer snabbare än vad du kan skala ut en skaluppsättning för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-391">You want toostart a set of VMs more quickly than you could scale out a virtual machine scale set.</span></span>
  - <span data-ttu-id="0f1b7-392">Relaterade toothis scenario du har skapat egna Autoskala motor och vill en snabbare skala för slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-392">Related toothis scenario, you might have created your own autoscale engine and want a faster end-to-end scale.</span></span>
- <span data-ttu-id="0f1b7-393">Du har en skaluppsättning för virtuell dator som är ojämnt fördelad över feldomäner eller uppdatera domäner.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-393">You have a virtual machine scale set that is unevenly distributed across fault domains or update domains.</span></span> <span data-ttu-id="0f1b7-394">Detta kan bero på att du selektivt ta bort virtuella datorer eller eftersom virtuella datorer har tagits bort efter överetablering.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-394">This might be because you selectively deleted VMs, or because VMs were deleted after overprovisioning.</span></span> <span data-ttu-id="0f1b7-395">Kör `stop deallocate` följt av `start` på hello virtuell dator distribuerar skaluppsättningen jämnt hello virtuella datorer över feldomäner eller uppdatera domäner.</span><span class="sxs-lookup"><span data-stu-id="0f1b7-395">Running `stop deallocate` followed by `start` on hello virtual machine scale set evenly distributes hello VMs across fault domains or update domains.</span></span>

