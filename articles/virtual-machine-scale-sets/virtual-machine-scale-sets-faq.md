---
title: "Skala virtuell Azure-dator Anger vanliga frågor och svar | Microsoft Docs"
description: "Få svar på vanliga frågor och svar om virtuella datorer."
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
ms.openlocfilehash: f320dd5d1f8c99317792f4ae9e09bc5adaf79e25
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="azure-virtual-machine-scale-sets-faqs"></a><span data-ttu-id="a5e29-103">Skala virtuell Azure-dator Anger vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="a5e29-103">Azure virtual machine scale sets FAQs</span></span>

<span data-ttu-id="a5e29-104">Få svar på vanliga frågor och svar om virtuella datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="a5e29-104">Get answers to frequently asked questions about virtual machine scale sets in Azure.</span></span>

## <a name="autoscale"></a><span data-ttu-id="a5e29-105">Automatisk skalning</span><span class="sxs-lookup"><span data-stu-id="a5e29-105">Autoscale</span></span>

### <a name="what-are-best-practices-for-azure-autoscale"></a><span data-ttu-id="a5e29-106">Vad är bästa praxis för Azure Autoskala?</span><span class="sxs-lookup"><span data-stu-id="a5e29-106">What are best practices for Azure Autoscale?</span></span>

<span data-ttu-id="a5e29-107">Bästa praxis för Autoskala finns [bästa praxis för autoskalning virtuella datorer](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).</span><span class="sxs-lookup"><span data-stu-id="a5e29-107">For best practices for Autoscale, see [Best practices for autoscaling virtual machines](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).</span></span>

### <a name="where-do-i-find-metric-names-for-autoscaling-that-uses-host-based-metrics"></a><span data-ttu-id="a5e29-108">Var hittar jag mått namn för autoskalning som använder värdbaserad mått?</span><span class="sxs-lookup"><span data-stu-id="a5e29-108">Where do I find metric names for autoscaling that uses host-based metrics?</span></span>

<span data-ttu-id="a5e29-109">Tjänstmåttets namn för autoskalning som använder värdbaserad mått finns [stöds mått med Azure-Monitor](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).</span><span class="sxs-lookup"><span data-stu-id="a5e29-109">For metric names for autoscaling that uses host-based metrics, see [Supported metrics with Azure Monitor](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).</span></span>

### <a name="are-there-any-examples-of-autoscaling-based-on-an-azure-service-bus-topic-and-queue-length"></a><span data-ttu-id="a5e29-110">Finns det några exempel på autoskalning baserat på en Azure Service Bus-avsnittet och kön längd?</span><span class="sxs-lookup"><span data-stu-id="a5e29-110">Are there any examples of autoscaling based on an Azure Service Bus topic and queue length?</span></span>

<span data-ttu-id="a5e29-111">Ja.</span><span class="sxs-lookup"><span data-stu-id="a5e29-111">Yes.</span></span> <span data-ttu-id="a5e29-112">Exempel på autoskalning baserat på en Azure Service Bus-avsnittet och kön längd finns [Azure-Monitor autoskalning vanliga mått](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).</span><span class="sxs-lookup"><span data-stu-id="a5e29-112">For examples of autoscaling based on an Azure Service Bus topic and queue length, see [Azure Monitor autoscaling common metrics](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).</span></span>

<span data-ttu-id="a5e29-113">Använd följande JSON för Service Bus-kö:</span><span class="sxs-lookup"><span data-stu-id="a5e29-113">For a Service Bus queue, use the following JSON:</span></span>

```json
"metricName": "MessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

<span data-ttu-id="a5e29-114">För en kö för lagring, använder du följande JSON:</span><span class="sxs-lookup"><span data-stu-id="a5e29-114">For a storage queue, use the following JSON:</span></span>

```json
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

<span data-ttu-id="a5e29-115">Ersätt exempelvärden med din resurs Uniform Resource Identifier (URI: er).</span><span class="sxs-lookup"><span data-stu-id="a5e29-115">Replace example values with your resource Uniform Resource Identifiers (URIs).</span></span>


### <a name="should-i-autoscale-by-using-host-based-metrics-or-a-diagnostics-extension"></a><span data-ttu-id="a5e29-116">Bör jag Autoskala med hjälp av värdbaserad mått eller ett tillägg för diagnostik?</span><span class="sxs-lookup"><span data-stu-id="a5e29-116">Should I autoscale by using host-based metrics or a diagnostics extension?</span></span>

<span data-ttu-id="a5e29-117">Du kan skapa en autoskalningsinställning på en virtuell dator att använda värdnivå mått eller gäst-OS-baserade mått.</span><span class="sxs-lookup"><span data-stu-id="a5e29-117">You can create an autoscale setting on a VM to use host-level metrics or guest OS-based metrics.</span></span>

<span data-ttu-id="a5e29-118">En lista över stöds mått finns [Azure-Monitor autoskalning vanliga mått](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics).</span><span class="sxs-lookup"><span data-stu-id="a5e29-118">For a list of supported metrics, see [Azure Monitor autoscaling common metrics](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics).</span></span> 

<span data-ttu-id="a5e29-119">En fullständig exempelfil för skalningsuppsättningar i virtuella datorer finns [avancerade Autoskala konfiguration med hjälp av Resource Manager-mallar för virtuella datorer](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets).</span><span class="sxs-lookup"><span data-stu-id="a5e29-119">For a full sample for virtual machine scale sets, see [Advanced autoscale configuration by using Resource Manager templates for virtual machine scale sets](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets).</span></span> 

<span data-ttu-id="a5e29-120">Används på värdnivå CPU måttet och ett meddelande count-mått.</span><span class="sxs-lookup"><span data-stu-id="a5e29-120">The sample uses the host-level CPU metric and a message count metric.</span></span>



### <a name="how-do-i-set-alert-rules-on-a-virtual-machine-scale-set"></a><span data-ttu-id="a5e29-121">Hur ställer jag in Varningsregler i en skaluppsättning för virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="a5e29-121">How do I set alert rules on a virtual machine scale set?</span></span>

<span data-ttu-id="a5e29-122">Du kan skapa aviseringar för mått för skalningsuppsättningar i virtuella datorer via PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="a5e29-122">You can create alerts on metrics for virtual machine scale sets via PowerShell or Azure CLI.</span></span> <span data-ttu-id="a5e29-123">Mer information finns i [Azure-Monitor PowerShell snabb start prover](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) och [Azure-Monitor plattformsoberoende CLI snabb start exempel](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).</span><span class="sxs-lookup"><span data-stu-id="a5e29-123">For more information, see [Azure Monitor PowerShell quick start samples](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) and [Azure Monitor cross-platform CLI quick start samples](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).</span></span>

<span data-ttu-id="a5e29-124">TargetResourceId för virtuella datorns skaluppsättning ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="a5e29-124">The TargetResourceId of the virtual machine scale set looks like this:</span></span> 

<span data-ttu-id="a5e29-125">/subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmssname</span><span class="sxs-lookup"><span data-stu-id="a5e29-125">/subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmssname</span></span>

<span data-ttu-id="a5e29-126">Du kan välja alla VM-prestandaräknaren som mått för att aktivera en avisering för.</span><span class="sxs-lookup"><span data-stu-id="a5e29-126">You can choose any VM performance counter as the metric to set an alert for.</span></span> <span data-ttu-id="a5e29-127">Mer information finns i [Gästoperativsystem mätvärden för Resource Manager-baserade virtuella Windows-datorer](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) och [Gästoperativsystem mätvärden för virtuella Linux-datorer](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) i den [Azure-Monitor autoskalning vanliga mått](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/) artikel.</span><span class="sxs-lookup"><span data-stu-id="a5e29-127">For more information, see [Guest OS metrics for Resource Manager-based Windows VMs](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) and [Guest OS metrics for Linux VMs](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) in the [Azure Monitor autoscaling common metrics](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/) article.</span></span>

### <a name="how-do-i-set-up-autoscale-on-a-virtual-machine-scale-set-by-using-powershell"></a><span data-ttu-id="a5e29-128">Hur ställer jag in Autoskala på en virtuell dator skala in med hjälp av PowerShell?</span><span class="sxs-lookup"><span data-stu-id="a5e29-128">How do I set up autoscale on a virtual machine scale set by using PowerShell?</span></span>

<span data-ttu-id="a5e29-129">Om du vill konfigurera Autoskala på en virtuell dator skala in med hjälp av PowerShell finns i bloggposten [lägga till Autoskala i en skaluppsättning för virtuell dator i Azure](https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/).</span><span class="sxs-lookup"><span data-stu-id="a5e29-129">To set up autoscale on a virtual machine scale set by using PowerShell, see the blog post [How to add autoscale to an Azure virtual machine scale set](https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/).</span></span>




## <a name="certificates"></a><span data-ttu-id="a5e29-130">Certifikat</span><span class="sxs-lookup"><span data-stu-id="a5e29-130">Certificates</span></span>

### <a name="how-do-i-securely-ship-a-certificate-to-the-vm-how-do-i-provision-a-virtual-machine-scale-set-to-run-a-website-where-the-ssl-for-the-website-is-shipped-securely-from-a-certificate-configuration-the-common-certificate-rotation-operation-would-be-almost-the-same-as-a-configuration-update-operation-do-you-have-an-example-of-how-to-do-this"></a><span data-ttu-id="a5e29-131">Hur skickar jag ett certifikat för att den virtuella datorn på ett säkert sätt?</span><span class="sxs-lookup"><span data-stu-id="a5e29-131">How do I securely ship a certificate to the VM?</span></span> <span data-ttu-id="a5e29-132">Hur jag för att etablera en virtuell dator skala inställd på att köras en webbplats där SSL för webbplats levereras på ett säkert sätt från en Certifikatkonfiguration för?</span><span class="sxs-lookup"><span data-stu-id="a5e29-132">How do I provision a virtual machine scale set to run a website where the SSL for the website is shipped securely from a certificate configuration?</span></span> <span data-ttu-id="a5e29-133">(Vanliga certifikat rotation åtgärden skulle vara nästan samma sätt som en uppdateringsåtgärd konfiguration.) Har du ett exempel på hur du gör detta?</span><span class="sxs-lookup"><span data-stu-id="a5e29-133">(The common certificate rotation operation would be almost the same as a configuration update operation.) Do you have an example of how to do this?</span></span> 

<span data-ttu-id="a5e29-134">För att leverera ett certifikat till den virtuella datorn på ett säkert sätt, kan ett kund-certifikat installeras direkt till ett Windows-certifikatarkiv från kundens nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="a5e29-134">To securely ship a certificate to the VM, you can install a customer certificate directly into a Windows certificate store from the customer's key vault.</span></span>

<span data-ttu-id="a5e29-135">Använd följande JSON:</span><span class="sxs-lookup"><span data-stu-id="a5e29-135">Use the following JSON:</span></span>

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

<span data-ttu-id="a5e29-136">Kod som har stöd för Windows och Linux.</span><span class="sxs-lookup"><span data-stu-id="a5e29-136">The code supports Windows and Linux.</span></span>

<span data-ttu-id="a5e29-137">Mer information finns i [skapa eller uppdatera en virtuell dator skaluppsättning](https://msdn.microsoft.com/library/mt589035.aspx).</span><span class="sxs-lookup"><span data-stu-id="a5e29-137">For more information, see [Create or update a virtual machine scale set](https://msdn.microsoft.com/library/mt589035.aspx).</span></span>


### <a name="example-of-self-signed-certificate"></a><span data-ttu-id="a5e29-138">Exempel på självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="a5e29-138">Example of Self-signed certificate</span></span>

1.  <span data-ttu-id="a5e29-139">Skapa ett självsignerat certifikat i en nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="a5e29-139">Create a self-signed certificate in a key vault.</span></span>

    <span data-ttu-id="a5e29-140">Använd följande PowerShell-kommandon:</span><span class="sxs-lookup"><span data-stu-id="a5e29-140">Use the following PowerShell commands:</span></span>

    ```powershell
    Import-Module "C:\Users\mikhegn\Downloads\Service-Fabric-master\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

    Login-AzureRmAccount

    Invoke-AddCertToKeyVault -SubscriptionId <Your SubID> -ResourceGroupName KeyVault -Location westus -VaultName MikhegnVault -CertificateName VMSSCert -Password VmssCert -CreateSelfSignedCertificate -DnsName vmss.mikhegn.azure.com -OutputPath c:\users\mikhegn\desktop\
    ```

    <span data-ttu-id="a5e29-141">Det här kommandot ger indata för Azure Resource Manager-mallen.</span><span class="sxs-lookup"><span data-stu-id="a5e29-141">This command gives you the input for the Azure Resource Manager template.</span></span>

    <span data-ttu-id="a5e29-142">Ett exempel på hur du skapar ett självsignerat certifikat i en nyckelvalvet finns [säkerhetsscenarier för Service Fabric-kluster](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).</span><span class="sxs-lookup"><span data-stu-id="a5e29-142">For an example of how to create a self-signed certificate in a key vault, see [Service Fabric cluster security scenarios](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).</span></span>

2.  <span data-ttu-id="a5e29-143">Ändra Resource Manager-mallen.</span><span class="sxs-lookup"><span data-stu-id="a5e29-143">Change the Resource Manager template.</span></span>

    <span data-ttu-id="a5e29-144">Lägg till den här egenskapen till **virtualMachineProfile**, som en del av den virtuella datorn skaluppsättning för resursen:</span><span class="sxs-lookup"><span data-stu-id="a5e29-144">Add this property to **virtualMachineProfile**, as part of the virtual machine scale set resource:</span></span>

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
  

### <a name="can-i-specify-an-ssh-key-pair-to-use-for-ssh-authentication-with-a-linux-virtual-machine-scale-set-from-a-resource-manager-template"></a><span data-ttu-id="a5e29-145">Kan jag ange en SSH-nyckel ska användas för SSH-autentisering med en Linux skaluppsättningen för virtuell dator från en Resource Manager-mall?</span><span class="sxs-lookup"><span data-stu-id="a5e29-145">Can I specify an SSH key pair to use for SSH authentication with a Linux virtual machine scale set from a Resource Manager template?</span></span>  

<span data-ttu-id="a5e29-146">Ja.</span><span class="sxs-lookup"><span data-stu-id="a5e29-146">Yes.</span></span> <span data-ttu-id="a5e29-147">REST API för **osProfile** liknar standard VM REST API.</span><span class="sxs-lookup"><span data-stu-id="a5e29-147">The REST API for **osProfile** is similar to the standard VM REST API.</span></span> 

<span data-ttu-id="a5e29-148">Inkludera **osProfile** i mallen:</span><span class="sxs-lookup"><span data-stu-id="a5e29-148">Include **osProfile** in your template:</span></span>

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
 
<span data-ttu-id="a5e29-149">Det här JSON-blocket används i [101-vm-sshkey GitHub Snabbstart mallen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="a5e29-149">This JSON block is used in [the 101-vm-sshkey GitHub quick start template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span></span>
 
<span data-ttu-id="a5e29-150">OS-profil används också i [grelayhost.json GitHub quick start mallen](https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json).</span><span class="sxs-lookup"><span data-stu-id="a5e29-150">The OS profile also is used in [the grelayhost.json GitHub quick start template](https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json).</span></span>

<span data-ttu-id="a5e29-151">Mer information finns i [skapa eller uppdatera en virtuell dator skaluppsättning](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration).</span><span class="sxs-lookup"><span data-stu-id="a5e29-151">For more information, see [Create or update a virtual machine scale set](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration).</span></span>
  

### <a name="how-do-i-remove-deprecated-certificates"></a><span data-ttu-id="a5e29-152">Hur tar jag bort föråldrad certifikat?</span><span class="sxs-lookup"><span data-stu-id="a5e29-152">How do I remove deprecated certificates?</span></span> 

<span data-ttu-id="a5e29-153">Ta bort det gamla certifikatet från listan över certifikat valvet för att ta bort föråldrade certifikat.</span><span class="sxs-lookup"><span data-stu-id="a5e29-153">To remove deprecated certificates, remove the old certificate from the vault certificates list.</span></span> <span data-ttu-id="a5e29-154">Lämna de certifikat som du vill ska finnas kvar på datorn i listan.</span><span class="sxs-lookup"><span data-stu-id="a5e29-154">Leave all the certificates that you want to remain on your computer in the list.</span></span> <span data-ttu-id="a5e29-155">Certifikatet tas inte bort från dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a5e29-155">This does not remove the certificate from all your VMs.</span></span> <span data-ttu-id="a5e29-156">Dessutom lägger inte till certifikatet till nya virtuella datorer som skapas i virtuella datorns skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="a5e29-156">It also does not add the certificate to new VMs that are created in the virtual machine scale set.</span></span> 

<span data-ttu-id="a5e29-157">Ta bort certifikatet från befintliga virtuella datorer genom att skriva ett tillägg för anpassat skript för att ta bort certifikaten manuellt från certifikatarkivet.</span><span class="sxs-lookup"><span data-stu-id="a5e29-157">To remove the certificate from existing VMs, write a custom script extension to manually remove the certificates from your certificate store.</span></span>
 
### <a name="how-do-i-inject-an-existing-ssh-public-key-into-the-virtual-machine-scale-set-ssh-layer-during-provisioning-i-want-to-store-the-ssh-public-key-values-in-azure-key-vault-and-then-use-them-in-my-resource-manager-template"></a><span data-ttu-id="a5e29-158">Hur jag mata in en befintlig offentlig SSH-nyckel i virtuella scale set SSH lagret under etablering?</span><span class="sxs-lookup"><span data-stu-id="a5e29-158">How do I inject an existing SSH public key into the virtual machine scale set SSH layer during provisioning?</span></span> <span data-ttu-id="a5e29-159">Jag vill lagra SSH offentlig nyckelvärdena i Azure Key Vault och använda dem i Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="a5e29-159">I want to store the SSH public key values in Azure Key Vault, and then use them in my Resource Manager template.</span></span>

<span data-ttu-id="a5e29-160">Om du anger de virtuella datorerna med en offentlig SSH-nyckel, behöver du inte placera de offentliga nycklarna i Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="a5e29-160">If you are providing the VMs only with a public SSH key, you don't need to put the public keys in Key Vault.</span></span> <span data-ttu-id="a5e29-161">Offentliga nycklar är inte hemliga.</span><span class="sxs-lookup"><span data-stu-id="a5e29-161">Public keys are not secret.</span></span>
 
<span data-ttu-id="a5e29-162">Du kan ange offentliga SSH-nycklar i klartext när du skapar en Linux VM:</span><span class="sxs-lookup"><span data-stu-id="a5e29-162">You can provide SSH public keys in plain text when you create a Linux VM:</span></span>

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
 
<span data-ttu-id="a5e29-163">linuxConfiguration elementnamn</span><span class="sxs-lookup"><span data-stu-id="a5e29-163">linuxConfiguration element name</span></span> | <span data-ttu-id="a5e29-164">Krävs</span><span class="sxs-lookup"><span data-stu-id="a5e29-164">Required</span></span> | <span data-ttu-id="a5e29-165">Typ</span><span class="sxs-lookup"><span data-stu-id="a5e29-165">Type</span></span> | <span data-ttu-id="a5e29-166">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="a5e29-166">Description</span></span>
--- | --- | --- | --- |  ---
<span data-ttu-id="a5e29-167">SSH</span><span class="sxs-lookup"><span data-stu-id="a5e29-167">ssh</span></span> | <span data-ttu-id="a5e29-168">Nej</span><span class="sxs-lookup"><span data-stu-id="a5e29-168">No</span></span> | <span data-ttu-id="a5e29-169">Samling</span><span class="sxs-lookup"><span data-stu-id="a5e29-169">Collection</span></span> | <span data-ttu-id="a5e29-170">Anger den nyckel SSH-konfigurationen för ett Linux-operativsystem</span><span class="sxs-lookup"><span data-stu-id="a5e29-170">Specifies the SSH key configuration for a Linux OS</span></span>
<span data-ttu-id="a5e29-171">Sökväg</span><span class="sxs-lookup"><span data-stu-id="a5e29-171">path</span></span> | <span data-ttu-id="a5e29-172">Ja</span><span class="sxs-lookup"><span data-stu-id="a5e29-172">Yes</span></span> | <span data-ttu-id="a5e29-173">Sträng</span><span class="sxs-lookup"><span data-stu-id="a5e29-173">String</span></span> | <span data-ttu-id="a5e29-174">Anger sökväg till Linux där SSH-nycklar eller certifikat ska hittas</span><span class="sxs-lookup"><span data-stu-id="a5e29-174">Specifies the Linux file path where the SSH keys or certificate should be located</span></span>
<span data-ttu-id="a5e29-175">nyckeldata</span><span class="sxs-lookup"><span data-stu-id="a5e29-175">keyData</span></span> | <span data-ttu-id="a5e29-176">Ja</span><span class="sxs-lookup"><span data-stu-id="a5e29-176">Yes</span></span> | <span data-ttu-id="a5e29-177">Sträng</span><span class="sxs-lookup"><span data-stu-id="a5e29-177">String</span></span> | <span data-ttu-id="a5e29-178">Anger en base64-kodad SSH offentlig nyckel</span><span class="sxs-lookup"><span data-stu-id="a5e29-178">Specifies a base64-encoded SSH public key</span></span>

<span data-ttu-id="a5e29-179">Ett exempel finns [101-vm-sshkey GitHub Snabbstart mallen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span><span class="sxs-lookup"><span data-stu-id="a5e29-179">For an example, see [the 101-vm-sshkey GitHub quick start template](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).</span></span>

 
### <a name="when-i-run-update-azurermvmss-after-adding-more-than-one-certificate-from-the-same-key-vault-i-see-the-following-message"></a><span data-ttu-id="a5e29-180">När kör `Update-AzureRmVmss` efter att lägga till fler än ett certifikat från samma nyckelvalvet, visas följande meddelande:</span><span class="sxs-lookup"><span data-stu-id="a5e29-180">When I run `Update-AzureRmVmss` after adding more than one certificate from the same key vault, I see the following message:</span></span>
 
><span data-ttu-id="a5e29-181">Update-AzureRmVmss: Listan hemlighet innehåller upprepade förekomster av /subscriptions/ < min-prenumerations-id > / resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev, vilket inte är tillåtet.</span><span class="sxs-lookup"><span data-stu-id="a5e29-181">Update-AzureRmVmss: List secret contains repeated instances of /subscriptions/<my-subscription-id>/resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev, which is disallowed.</span></span>
 
<span data-ttu-id="a5e29-182">Detta kan inträffa om du försöker lägga till samma valvet istället för att använda ett nytt certifikat för valvet för befintliga källa valvet igen.</span><span class="sxs-lookup"><span data-stu-id="a5e29-182">This can happen if you try to re-add the same vault instead of using a new vault certificate for the existing source vault.</span></span> <span data-ttu-id="a5e29-183">Den `Add-AzureRmVmssSecret` kommandot fungerar inte korrekt om du lägger till ytterligare hemligheter.</span><span class="sxs-lookup"><span data-stu-id="a5e29-183">The `Add-AzureRmVmssSecret` command does not work correctly if you are adding additional secrets.</span></span>
 
<span data-ttu-id="a5e29-184">Uppdatera listan över $vmss.properties.osProfile.secrets[0].vaultCertificates för att lägga till flera hemligheter från samma nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="a5e29-184">To add more secrets from the same key vault, update the $vmss.properties.osProfile.secrets[0].vaultCertificates list.</span></span>
 
<span data-ttu-id="a5e29-185">Förväntade indatastrukturen finns [skapa eller uppdatera en virtuell dator ange](https://msdn.microsoft.com/library/azure/mt589035.aspx).</span><span class="sxs-lookup"><span data-stu-id="a5e29-185">For the expected input structure, see [Create or update a virtual machine set](https://msdn.microsoft.com/library/azure/mt589035.aspx).</span></span>
 
<span data-ttu-id="a5e29-186">Hitta hemligheten i virtuella Skala uppsättningsobjekt som finns i nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="a5e29-186">Find the secret in the virtual machine scale set object that is in the key vault.</span></span> <span data-ttu-id="a5e29-187">Lägg sedan till Certifikatreferens (URL: en och hemliga store-namn) i listan som är kopplad till valvet.</span><span class="sxs-lookup"><span data-stu-id="a5e29-187">Then, add your certificate reference (the URL and the secret store name) to the list associated with the vault.</span></span>

> [!NOTE] 
> <span data-ttu-id="a5e29-188">För närvarande kan du ta bort certifikat från virtuella datorer med virtuella scale set API.</span><span class="sxs-lookup"><span data-stu-id="a5e29-188">Currently, you cannot remove certificates from VMs by using the virtual machine scale set API.</span></span>
>

<span data-ttu-id="a5e29-189">Nya virtuella datorer har inte det gamla certifikatet.</span><span class="sxs-lookup"><span data-stu-id="a5e29-189">New VMs will not have the old certificate.</span></span> <span data-ttu-id="a5e29-190">Virtuella datorer som har certifikatet och som redan har distribuerats har dock det gamla certifikatet.</span><span class="sxs-lookup"><span data-stu-id="a5e29-190">However, VMs that have the certificate and which are already deployed will have the old certificate.</span></span>
 
### <a name="can-i-push-certificates-to-the-virtual-machine-scale-set-without-providing-the-password-when-the-certificate-is-in-the-secret-store"></a><span data-ttu-id="a5e29-191">Kan jag push-certifikat till skaluppsättningen för virtuell dator utan att ange lösenord, när certifikatet finns i arkivet hemliga?</span><span class="sxs-lookup"><span data-stu-id="a5e29-191">Can I push certificates to the virtual machine scale set without providing the password, when the certificate is in the secret store?</span></span>

<span data-ttu-id="a5e29-192">Du behöver inte hårdkoda lösenord i skript.</span><span class="sxs-lookup"><span data-stu-id="a5e29-192">You do not need to hard-code passwords in scripts.</span></span> <span data-ttu-id="a5e29-193">Du kan dynamiskt hämta lösenord med de behörigheter som används för att köra skriptet för distribution.</span><span class="sxs-lookup"><span data-stu-id="a5e29-193">You can dynamically retrieve passwords with the permissions you use to run the deployment script.</span></span> <span data-ttu-id="a5e29-194">Om du har ett skript som flyttar ett certifikat från den hemliga Lagernyckeln valvet, arkivet hemliga `get certificate` kommandot också matar ut lösenordet för PFX-filen.</span><span class="sxs-lookup"><span data-stu-id="a5e29-194">If you have a script that moves a certificate from the secret store key vault, the secret store `get certificate` command also outputs the password of the .pfx file.</span></span>
 
### <a name="how-does-the-secrets-property-of-virtualmachineprofileosprofile-for-a-virtual-machine-scale-set-work-why-do-i-need-the-sourcevault-value-when-i-have-to-specify-the-absolute-uri-for-a-certificate-by-using-the-certificateurl-property"></a><span data-ttu-id="a5e29-195">Hur in arbete i egenskapen hemligheter för virtualMachineProfile.osProfile för en virtuell dator skala?</span><span class="sxs-lookup"><span data-stu-id="a5e29-195">How does the Secrets property of virtualMachineProfile.osProfile for a virtual machine scale set work?</span></span> <span data-ttu-id="a5e29-196">Varför behöver jag sourceVault värdet när jag måste ange en absolut URI för ett certifikat med hjälp av egenskapen certificateUrl?</span><span class="sxs-lookup"><span data-stu-id="a5e29-196">Why do I need the sourceVault value when I have to specify the absolute URI for a certificate by using the certificateUrl property?</span></span> 

<span data-ttu-id="a5e29-197">En referens för Windows Remote Management (WinRM)-certifikat måste finnas i egenskapen hemligheter för OS-profil.</span><span class="sxs-lookup"><span data-stu-id="a5e29-197">A Windows Remote Management (WinRM) certificate reference must be present in the Secrets property of the OS profile.</span></span> 

<span data-ttu-id="a5e29-198">Syftet med som anger valvet källa är att genomdriva åtkomstkontrollistan (ACL) principer för åtkomstkontroll som finns i en användares Azure Cloud Service model.</span><span class="sxs-lookup"><span data-stu-id="a5e29-198">The purpose of indicating the source vault is to enforce access control list (ACL) policies that exist in a user's Azure Cloud Service model.</span></span> <span data-ttu-id="a5e29-199">Om käll-valvet har inte angetts, skulle användare som inte har behörighet att distribuera eller få åtkomst till nyckeln till en nyckelvalv kunna via en Compute Resource Provider (CRP).</span><span class="sxs-lookup"><span data-stu-id="a5e29-199">If the source vault isn't specified, users who do not have permissions to deploy or access secrets to a key vault would be able to through a Compute Resource Provider (CRP).</span></span> <span data-ttu-id="a5e29-200">ACL: er finns även för resurser som inte finns.</span><span class="sxs-lookup"><span data-stu-id="a5e29-200">ACLs exist even for resources that do not exist.</span></span>

<span data-ttu-id="a5e29-201">Om du anger ett felaktigt källa valvet ID men en giltig key vault-URL, rapporteras ett fel när du söka igen.</span><span class="sxs-lookup"><span data-stu-id="a5e29-201">If you provide an incorrect source vault ID but a valid key vault URL, an error is reported when you poll the operation.</span></span>
 
### <a name="if-i-add-secrets-to-an-existing-virtual-machine-scale-set-are-the-secrets-injected-into-existing-vms-or-only-into-new-ones"></a><span data-ttu-id="a5e29-202">Om jag lägger till hemligheter i en befintlig ange virtuella datorn, är hemligheter som läggs in i befintliga virtuella datorer eller bara nya?</span><span class="sxs-lookup"><span data-stu-id="a5e29-202">If I add secrets to an existing virtual machine scale set, are the secrets injected into existing VMs, or only into new ones?</span></span> 

<span data-ttu-id="a5e29-203">Certifikat har lagts till alla dina virtuella datorer, även redan befintliga viktiga.</span><span class="sxs-lookup"><span data-stu-id="a5e29-203">Certificates are added to all your VMs, even preexisting ones.</span></span> <span data-ttu-id="a5e29-204">Om din virtuella skaluppsättning upgradePolicy egenskap är inställd på **manuell**, certifikatet har lagts till den virtuella datorn när du utför en manuell uppdatering på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a5e29-204">If your virtual machine scale set upgradePolicy property is set to **manual**, the certificate is added to the VM when you perform a manual update on the VM.</span></span>
 
### <a name="where-do-i-put-certificates-for-linux-vms"></a><span data-ttu-id="a5e29-205">Där placera certifikat för Linux virtuella datorer?</span><span class="sxs-lookup"><span data-stu-id="a5e29-205">Where do I put certificates for Linux VMs?</span></span>

<span data-ttu-id="a5e29-206">Om du vill lära dig mer om att distribuera certifikat för Linux virtuella datorer, se [distribuera certifikat till virtuella datorer från en kundhanterad nyckelvalv](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/).</span><span class="sxs-lookup"><span data-stu-id="a5e29-206">To learn how to deploy certificates for Linux VMs, see [Deploy certificates to VMs from a customer-managed key vault](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/).</span></span>
  
### <a name="how-do-i-add-a-new-vault-certificate-to-a-new-certificate-object"></a><span data-ttu-id="a5e29-207">Hur lägger jag till ett nytt certifikat för valvet till ett nytt certifikatobjekt?</span><span class="sxs-lookup"><span data-stu-id="a5e29-207">How do I add a new vault certificate to a new certificate object?</span></span>

<span data-ttu-id="a5e29-208">Om du vill lägga till ett certifikat för valvet i en befintlig hemlighet finns i följande PowerShell-exempel.</span><span class="sxs-lookup"><span data-stu-id="a5e29-208">To add a vault certificate to an existing secret, see the following PowerShell example.</span></span> <span data-ttu-id="a5e29-209">Använd endast hemliga objekt.</span><span class="sxs-lookup"><span data-stu-id="a5e29-209">Use only one secret object.</span></span>
 
```powershell
$newVaultCertificate = New-AzureRmVmssVaultCertificateConfig -CertificateStore MY -CertificateUrl https://sansunallapps1.vault.azure.net:443/secrets/dg-private-enc/55fa0332edc44a84ad655298905f1809
 
$vmss.VirtualMachineProfile.OsProfile.Secrets[0].VaultCertificates.Add($newVaultCertificate)
 
Update-AzureRmVmss -VirtualMachineScaleSet $vmss -ResourceGroup $rg -Name $vmssName
```
 
### <a name="what-happens-to-certificates-if-you-reimage-a-vm"></a><span data-ttu-id="a5e29-210">Vad händer med certifikat om du återavbilda en virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="a5e29-210">What happens to certificates if you reimage a VM?</span></span>

<span data-ttu-id="a5e29-211">Om du återavbilda en virtuell dator tas certifikat bort.</span><span class="sxs-lookup"><span data-stu-id="a5e29-211">If you reimage a VM, certificates are deleted.</span></span> <span data-ttu-id="a5e29-212">Återställning av avbildning borttagningar i hela OS-disk.</span><span class="sxs-lookup"><span data-stu-id="a5e29-212">Reimaging deletes the entire OS disk.</span></span> 
 
### <a name="what-happens-if-you-delete-a-certificate-from-the-key-vault"></a><span data-ttu-id="a5e29-213">Vad händer om du tar bort ett certifikat från nyckelvalvet?</span><span class="sxs-lookup"><span data-stu-id="a5e29-213">What happens if you delete a certificate from the key vault?</span></span>

<span data-ttu-id="a5e29-214">Om hemligheten som tas bort från nyckelvalvet och sedan kör `stop deallocate` för dina virtuella datorer och starta sedan om dem igen, inträffar ett fel.</span><span class="sxs-lookup"><span data-stu-id="a5e29-214">If the secret is deleted from the key vault, and then you run `stop deallocate` for all your VMs and then start them again, you will encounter a failure.</span></span> <span data-ttu-id="a5e29-215">Felet beror på att CRP måste hämta hemligheterna från nyckelvalvet, men den inte kan.</span><span class="sxs-lookup"><span data-stu-id="a5e29-215">The failure occurs because the CRP needs to retrieve the secrets from the key vault, but it cannot.</span></span> <span data-ttu-id="a5e29-216">I det här scenariot kan du ta bort certifikat från den virtuella datorn skala modellen.</span><span class="sxs-lookup"><span data-stu-id="a5e29-216">In this scenario, you can delete the certificates from the virtual machine scale set model.</span></span> 

<span data-ttu-id="a5e29-217">CRP-komponenten inte har behållits kunden hemligheter.</span><span class="sxs-lookup"><span data-stu-id="a5e29-217">The CRP component does not persist customer secrets.</span></span> <span data-ttu-id="a5e29-218">Om du kör `stop deallocate` för alla virtuella datorer i virtuella datorns skaluppsättning cacheminnet har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="a5e29-218">If you run `stop deallocate` for all VMs in the virtual machine scale set, the cache is deleted.</span></span> <span data-ttu-id="a5e29-219">I det här scenariot hämtas hemligheter från nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="a5e29-219">In this scenario, secrets are retrieved from the key vault.</span></span>

<span data-ttu-id="a5e29-220">Det uppstår inte här problemet när skala ut eftersom det inte finns en cachelagrad kopia av hemlighet i Azure Service Fabric (i en fabric-klient-modellen).</span><span class="sxs-lookup"><span data-stu-id="a5e29-220">You don't encounter this problem when scaling out because there is a cached copy of the secret in Azure Service Fabric (in the single-fabric tenant model).</span></span>
 
### <a name="why-do-i-have-to-specify-the-exact-location-for-the-certificate-url-httpsname-of-the-vaultvaultazurenet443secretsexact-location-as-indicated-in-service-fabric-cluster-security-scenarioshttpsazuremicrosoftcomdocumentationarticlesservice-fabric-cluster-security"></a><span data-ttu-id="a5e29-221">Varför måste jag ange den exakta platsen för certifikatet URL (https://<name of the vault>.vault.azure.net:443/secrets/<exact location>), som anges i [säkerhetsscenarier för Service Fabric-klustret](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/)?</span><span class="sxs-lookup"><span data-stu-id="a5e29-221">Why do I have to specify the exact location for the certificate URL (https://<name of the vault>.vault.azure.net:443/secrets/<exact location>), as indicated in [Service Fabric cluster security scenarios](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/)?</span></span>
 
<span data-ttu-id="a5e29-222">Azure Key Vault-dokumentationen om att hämta hemligheten REST API ska returnera den senaste versionen av hemligheten som om versionen inte har angetts.</span><span class="sxs-lookup"><span data-stu-id="a5e29-222">The Azure Key Vault documentation states that the Get Secret REST API should return the latest version of the secret if the version is not specified.</span></span>
 
<span data-ttu-id="a5e29-223">Metod</span><span class="sxs-lookup"><span data-stu-id="a5e29-223">Method</span></span> | <span data-ttu-id="a5e29-224">URL: EN</span><span class="sxs-lookup"><span data-stu-id="a5e29-224">URL</span></span>
--- | ---
<span data-ttu-id="a5e29-225">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="a5e29-225">GET</span></span> | <span data-ttu-id="a5e29-226">https://mykeyvault.Vault.Azure.NET/secrets/ {hemlighet name} / {hemlighet version}? api-version = {api-version}</span><span class="sxs-lookup"><span data-stu-id="a5e29-226">https://mykeyvault.vault.azure.net/secrets/{secret-name}/{secret-version}?api-version={api-version}</span></span>

<span data-ttu-id="a5e29-227">Ersätt {*hemlighet namn*} med namnet och Ersätt {*hemlighet version*} med versionen av den hemlighet som du vill hämta.</span><span class="sxs-lookup"><span data-stu-id="a5e29-227">Replace {*secret-name*} with the name, and replace {*secret-version*} with the version of the secret you want to retrieve.</span></span> <span data-ttu-id="a5e29-228">Den hemliga versionen kan inte uteslutas.</span><span class="sxs-lookup"><span data-stu-id="a5e29-228">The secret version might be excluded.</span></span> <span data-ttu-id="a5e29-229">I så fall hämtas den aktuella versionen.</span><span class="sxs-lookup"><span data-stu-id="a5e29-229">In that case, the current version is retrieved.</span></span>
  
### <a name="why-do-i-have-to-specify-the-certificate-version-when-i-use-key-vault"></a><span data-ttu-id="a5e29-230">Varför måste jag ange certifikatet version när jag använder Key Vault?</span><span class="sxs-lookup"><span data-stu-id="a5e29-230">Why do I have to specify the certificate version when I use Key Vault?</span></span>

<span data-ttu-id="a5e29-231">Syftet med Key Vault-krav anger versionen certifikat är att göra det användarna vilka certifikat som har distribuerats på sina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a5e29-231">The purpose of the Key Vault requirement to specify the certificate version is to make it clear to the user what certificate is deployed on their VMs.</span></span>

<span data-ttu-id="a5e29-232">Om du skapar en virtuell dator och uppdatera din hemlighet i nyckelvalvet hämtas inte det nya certifikatet till dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a5e29-232">If you create a VM and then update your secret in the key vault, the new certificate is not downloaded to your VMs.</span></span> <span data-ttu-id="a5e29-233">Men dina virtuella datorer visas att referera till den, och nya virtuella datorer får den nya hemligheten.</span><span class="sxs-lookup"><span data-stu-id="a5e29-233">But your VMs appear to reference it, and new VMs get the new secret.</span></span> <span data-ttu-id="a5e29-234">Om du vill undvika detta krävs att referera till en hemliga versionen.</span><span class="sxs-lookup"><span data-stu-id="a5e29-234">To avoid this, you are required to reference a secret version.</span></span>

### <a name="my-team-works-with-several-certificates-that-are-distributed-to-us-as-cer-public-keys-what-is-the-recommended-approach-for-deploying-these-certificates-to-a-virtual-machine-scale-set"></a><span data-ttu-id="a5e29-235">Min grupp fungerar med flera certifikat som har distribuerats till oss som .cer offentliga nycklar.</span><span class="sxs-lookup"><span data-stu-id="a5e29-235">My team works with several certificates that are distributed to us as .cer public keys.</span></span> <span data-ttu-id="a5e29-236">Vad är den rekommenderade metoden för att distribuera dessa certifikat till en virtuell dator skala inställd?</span><span class="sxs-lookup"><span data-stu-id="a5e29-236">What is the recommended approach for deploying these certificates to a virtual machine scale set?</span></span>

<span data-ttu-id="a5e29-237">Offentliga nycklar till en virtuell dator skala inställd för att distribuera .cer och du kan skapa en .pfx-fil som innehåller CER-filer.</span><span class="sxs-lookup"><span data-stu-id="a5e29-237">To deploy .cer public keys to a virtual machine scale set, you can generate a .pfx file that contains only .cer files.</span></span> <span data-ttu-id="a5e29-238">Det gör du genom att använda `X509ContentType = Pfx`.</span><span class="sxs-lookup"><span data-stu-id="a5e29-238">To do this, use `X509ContentType = Pfx`.</span></span> <span data-ttu-id="a5e29-239">Till exempel läsa in den .cer-fil som x509Certificate2 objekt i C# eller PowerShell och sedan anropa metoden.</span><span class="sxs-lookup"><span data-stu-id="a5e29-239">For example, load the .cer file as an x509Certificate2 object in C# or PowerShell, and then call the method.</span></span> 

<span data-ttu-id="a5e29-240">Mer information finns i [X509Certificate.Export-metoden (X509ContentType, String)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).</span><span class="sxs-lookup"><span data-stu-id="a5e29-240">For more information, see [X509Certificate.Export Method (X509ContentType, String)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).</span></span>

### <a name="i-do-not-see-an-option-for-users-to-pass-in-certificates-as-base64-strings-most-other-resource-providers-have-this-option"></a><span data-ttu-id="a5e29-241">Jag ser inte ett alternativ för användare att skicka in certifikat som base64-strängar.</span><span class="sxs-lookup"><span data-stu-id="a5e29-241">I do not see an option for users to pass in certificates as base64 strings.</span></span> <span data-ttu-id="a5e29-242">De flesta andra resursleverantörer har det här alternativet.</span><span class="sxs-lookup"><span data-stu-id="a5e29-242">Most other resource providers have this option.</span></span>

<span data-ttu-id="a5e29-243">Du kan extrahera senaste version URL-Adressen i en Resource Manager-mall för att emulera om ett certifikat som en base64-sträng.</span><span class="sxs-lookup"><span data-stu-id="a5e29-243">To emulate passing in a certificate as a base64 string, you can extract the latest versioned URL in a Resource Manager template.</span></span> <span data-ttu-id="a5e29-244">Inkludera följande JSON-egenskap i hanteraren för filserverresurser mallen:</span><span class="sxs-lookup"><span data-stu-id="a5e29-244">Include the following JSON property in your Resource Manager template:</span></span>

```json 
"certificateUrl": "[reference(resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults/secrets', parameters('vaultName'), parameters('secretName')), '2015-06-01').secretUriWithVersion]"
```
 
### <a name="do-i-have-to-wrap-certificates-in-json-objects-in-key-vaults"></a><span data-ttu-id="a5e29-245">Måste jag omsluter certifikat i JSON-objekt i nyckelvalv?</span><span class="sxs-lookup"><span data-stu-id="a5e29-245">Do I have to wrap certificates in JSON objects in key vaults?</span></span>

<span data-ttu-id="a5e29-246">I skalningsuppsättningar i virtuella datorer och virtuella datorer, certifikaten omslutas i JSON-objekt.</span><span class="sxs-lookup"><span data-stu-id="a5e29-246">In virtual machine scale sets and VMs, certificates must be wrapped in JSON objects.</span></span> 

<span data-ttu-id="a5e29-247">Vi stöder också innehållstypen application/x-pkcs12.</span><span class="sxs-lookup"><span data-stu-id="a5e29-247">We also support the content type application/x-pkcs12.</span></span> <span data-ttu-id="a5e29-248">Anvisningar om hur du använder application/x-pkcs12 finns [PFX-certifikat i Azure Key Vault](http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/).</span><span class="sxs-lookup"><span data-stu-id="a5e29-248">For instructions on using application/x-pkcs12, see [PFX certificates in Azure Key Vault](http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/).</span></span>
 
<span data-ttu-id="a5e29-249">Vi stöder för närvarande inte CER-filer.</span><span class="sxs-lookup"><span data-stu-id="a5e29-249">We currently do not support .cer files.</span></span> <span data-ttu-id="a5e29-250">Om du vill använda CER-filer, exportera dem till .pfx-behållare.</span><span class="sxs-lookup"><span data-stu-id="a5e29-250">To use .cer files, export them into .pfx containers.</span></span>



## <a name="compliance"></a><span data-ttu-id="a5e29-251">Efterlevnad</span><span class="sxs-lookup"><span data-stu-id="a5e29-251">Compliance</span></span>

### <a name="are-virtual-machine-scale-sets-pci-compliant"></a><span data-ttu-id="a5e29-252">Är virtuella datorn anger PCI-kompatibel?</span><span class="sxs-lookup"><span data-stu-id="a5e29-252">Are virtual machine scale sets PCI-compliant?</span></span>

<span data-ttu-id="a5e29-253">VM-skalningsuppsättningar är ett tunt API-läger ovanpå CRP.</span><span class="sxs-lookup"><span data-stu-id="a5e29-253">Virtual machine scale sets are a thin API layer on top of the CRP.</span></span> <span data-ttu-id="a5e29-254">Båda komponenterna är en del av beräkningsplattformen i Azure-tjänsteträdet.</span><span class="sxs-lookup"><span data-stu-id="a5e29-254">Both components are part of the compute platform in the Azure service tree.</span></span>

<span data-ttu-id="a5e29-255">Från ett kompatibilitetsperspektiv är VM-skalningsuppsättningar en grundläggande del av Azure Compute-plattformen.</span><span class="sxs-lookup"><span data-stu-id="a5e29-255">From a compliance perspective, virtual machine scale sets are a fundamental part of the Azure compute platform.</span></span> <span data-ttu-id="a5e29-256">De delar ett team, verktyg, processer, distributionsmetodik, säkerhetskontroller, JIT-kompilering (just-in-time), övervakning, aviseringar och så vidare med själva CRP:n.</span><span class="sxs-lookup"><span data-stu-id="a5e29-256">They share a team, tools, processes, deployment methodology, security controls, just-in-time (JIT) compilation, monitoring, alerting, and so on, with the CRP itself.</span></span> <span data-ttu-id="a5e29-257">VM-skalningsuppsättningar är PCI-kompatibla (Payment Card Industry) eftersom CRP:n är en del av den aktuella attesteringen för PCI Data Security Standard (DSS).</span><span class="sxs-lookup"><span data-stu-id="a5e29-257">Virtual machine scale sets are Payment Card Industry (PCI)-compliant because the CRP is part of the current PCI Data Security Standard (DSS) attestation.</span></span>

<span data-ttu-id="a5e29-258">Mer information finns i [Microsoft Trust Center](https://www.microsoft.com/TrustCenter/Compliance/PCI).</span><span class="sxs-lookup"><span data-stu-id="a5e29-258">For more information, see [the Microsoft Trust Center](https://www.microsoft.com/TrustCenter/Compliance/PCI).</span></span>






## <a name="extensions"></a><span data-ttu-id="a5e29-259">Tillägg</span><span class="sxs-lookup"><span data-stu-id="a5e29-259">Extensions</span></span>

### <a name="how-do-i-delete-a-virtual-machine-scale-set-extension"></a><span data-ttu-id="a5e29-260">Hur tar jag bort en virtuell dator skala utökas?</span><span class="sxs-lookup"><span data-stu-id="a5e29-260">How do I delete a virtual machine scale set extension?</span></span>

<span data-ttu-id="a5e29-261">Använd följande PowerShell-exempel för att ta bort en virtuell dator skala utökas:</span><span class="sxs-lookup"><span data-stu-id="a5e29-261">To delete a virtual machine scale set extension, use the following PowerShell example:</span></span>

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" 

$vmss=Remove-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "extensionName"

Update-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" -VirtualMacineScaleSet $vmss
```
 
<span data-ttu-id="a5e29-262">Du hittar en Tilläggsnamn värdet i `$vmss`.</span><span class="sxs-lookup"><span data-stu-id="a5e29-262">You can find the extensionName value in `$vmss`.</span></span>
   
### <a name="is-there-a-virtual-machine-scale-set-template-example-that-integrates-with-operations-management-suite"></a><span data-ttu-id="a5e29-263">Finns det en virtuell dator skala ange mall-exempel som kan integreras med Operations Management Suite?</span><span class="sxs-lookup"><span data-stu-id="a5e29-263">Is there a virtual machine scale set template example that integrates with Operations Management Suite?</span></span>

<span data-ttu-id="a5e29-264">En virtuell dator skala ange mall-exempel som kan integreras med Operations Management Suite, finns det andra exemplet i [distribuera ett Azure Service Fabric-kluster och aktivera övervakning genom att använda Log Analytics](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric).</span><span class="sxs-lookup"><span data-stu-id="a5e29-264">For a virtual machine scale set template example that integrates with Operations Management Suite, see the second example in [Deploy an Azure Service Fabric cluster and enable monitoring by using Log Analytics](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric).</span></span>
   
### <a name="extensions-seem-to-run-in-parallel-on-virtual-machine-scale-sets-this-causes-my-custom-script-extension-to-fail-what-can-i-do-to-fix-this"></a><span data-ttu-id="a5e29-265">Det verkar som om tillägg körs parallellt på virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a5e29-265">Extensions seem to run in parallel on virtual machine scale sets.</span></span> <span data-ttu-id="a5e29-266">Detta leder till min tillägget för anpassat skript misslyckas.</span><span class="sxs-lookup"><span data-stu-id="a5e29-266">This causes my custom script extension to fail.</span></span> <span data-ttu-id="a5e29-267">Vad kan jag göra för att åtgärda detta?</span><span class="sxs-lookup"><span data-stu-id="a5e29-267">What can I do to fix this?</span></span>

<span data-ttu-id="a5e29-268">Mer information om tillägg ordningsföljd i skalningsuppsättningar i virtuella datorer, se [tillägget ordningsföljd i skalningsuppsättningar i virtuella Azure-datorn](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).</span><span class="sxs-lookup"><span data-stu-id="a5e29-268">To learn about extension sequencing in virtual machine scale sets, see [Extension sequencing in Azure virtual machine scale sets](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).</span></span>
 
 
### <a name="how-do-i-reset-the-password-for-vms-in-my-virtual-machine-scale-set"></a><span data-ttu-id="a5e29-269">Hur Återställ lösenordet för virtuella datorer i min skaluppsättning för virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="a5e29-269">How do I reset the password for VMs in my virtual machine scale set?</span></span>

<span data-ttu-id="a5e29-270">Om du vill återställa lösenordet för virtuella datorer i din skaluppsättning för virtuell dator, Använd VM access-tillägg.</span><span class="sxs-lookup"><span data-stu-id="a5e29-270">To reset the password for VMs in your virtual machine scale set, use VM access extensions.</span></span> 

<span data-ttu-id="a5e29-271">Använd följande PowerShell-exempel:</span><span class="sxs-lookup"><span data-stu-id="a5e29-271">Use the following PowerShell example:</span></span>

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
 
 
### <a name="how-do-i-add-an-extension-to-all-vms-in-my-virtual-machine-scale-set"></a><span data-ttu-id="a5e29-272">Hur lägger jag till ett tillägg till alla virtuella datorer i min skaluppsättning för virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="a5e29-272">How do I add an extension to all VMs in my virtual machine scale set?</span></span>

<span data-ttu-id="a5e29-273">Om principen anges till **automatisk**, omdistribuera mallen med de nya egenskaperna för webbtjänsttillägg uppdaterar alla virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a5e29-273">If update policy is set to **automatic**, redeploying the template with the new extension properties updates all VMs.</span></span>

<span data-ttu-id="a5e29-274">Om principen anges till **manuell**först uppdatera tillägget och uppdatera alla instanser i dina virtuella datorer manuellt.</span><span class="sxs-lookup"><span data-stu-id="a5e29-274">If update policy is set to **manual**, first update the extension, and then manually update all instances in your VMs.</span></span>

  
### <a name="if-the-extensions-associated-with-an-existing-virtual-machine-scale-set-are-updated-are-existing-vms-affected-that-is-will-the-vms-not-match-the-virtual-machine-scale-set-model-or-are-they-ignored-when-an-existing-machine-is-service-healed-or-reimaged-are-the-scripts-that-are-currently-configured-on-the-virtual-machine-scale-set-executed-or-are-the-scripts-that-were-configured-when-the-vm-was-first-created-used"></a><span data-ttu-id="a5e29-275">Om de tillägg som är associerade med en befintlig skaluppsättning för virtuell dator uppdateras befintliga virtuella datorer som påverkas?</span><span class="sxs-lookup"><span data-stu-id="a5e29-275">If the extensions associated with an existing virtual machine scale set are updated, are existing VMs affected?</span></span> <span data-ttu-id="a5e29-276">(Det vill säga att de virtuella datorerna *inte* matchar den virtuella datorn skala modellen?) Eller ignoreras de?</span><span class="sxs-lookup"><span data-stu-id="a5e29-276">(That is, will the VMs *not* match the virtual machine scale set model?) Or are they ignored?</span></span> <span data-ttu-id="a5e29-277">När en befintlig dator har service-läkt eller avbildade, är de skript som konfigurerats på virtuella datorns skaluppsättning körs eller är de skript som konfigurerades när den virtuella datorn skapades användas?</span><span class="sxs-lookup"><span data-stu-id="a5e29-277">When an existing machine is service-healed or reimaged, are the scripts that are currently configured on the virtual machine scale set executed, or are the scripts that were configured when the VM was first created used?</span></span>

<span data-ttu-id="a5e29-278">Om resurstilläggsdefinitionen i virtuella datorns skaluppsättning modellen uppdateras och upgradePolicy-egenskap är inställd på **automatisk**, uppdateras de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="a5e29-278">If the extension definition in the virtual machine scale set model is updated and the upgradePolicy property is set to **automatic**, it updates the VMs.</span></span> <span data-ttu-id="a5e29-279">Om egenskapen upgradePolicy anges till **manuell**, tillägg har flaggats som inte matchar modellen.</span><span class="sxs-lookup"><span data-stu-id="a5e29-279">If the upgradePolicy property is set to **manual**, extensions are flagged as not matching the model.</span></span> 

<span data-ttu-id="a5e29-280">Om en befintlig virtuell dator är tjänsten lagats, visas den som en omstart och tillägg finns inte på nytt.</span><span class="sxs-lookup"><span data-stu-id="a5e29-280">If an existing VM is service-healed, it appears as a reboot, and the extensions are not rerun.</span></span> <span data-ttu-id="a5e29-281">Om det avbildade, är det som ersätter enhetens OS med källbilden.</span><span class="sxs-lookup"><span data-stu-id="a5e29-281">If it is reimaged, it's like replacing the OS drive with the source image.</span></span> <span data-ttu-id="a5e29-282">Alla specialisering från den senaste modellen, till exempel tillägg, körs.</span><span class="sxs-lookup"><span data-stu-id="a5e29-282">Any specialization from the latest model, such as extensions, are run.</span></span>
 
### <a name="how-do-i-join-a-virtual-machine-scale-set-to-an-azure-ad-domain"></a><span data-ttu-id="a5e29-283">Hur jag ansluta till en virtuell dator skala en Azure AD-domän</span><span class="sxs-lookup"><span data-stu-id="a5e29-283">How do I join a virtual machine scale set to an Azure AD domain?</span></span>

<span data-ttu-id="a5e29-284">Du kan definiera ett tillägg för att ansluta till en virtuell dator skala ett Azure Active Directory (Azure AD)-domän.</span><span class="sxs-lookup"><span data-stu-id="a5e29-284">To join a virtual machine scale set to an Azure Active Directory (Azure AD) domain, you can define an extension.</span></span> 

<span data-ttu-id="a5e29-285">Definiera ett tillägg genom att använda egenskapen JsonADDomainExtension:</span><span class="sxs-lookup"><span data-stu-id="a5e29-285">To define an extension, use the JsonADDomainExtension property:</span></span>

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
 
### <a name="my-virtual-machine-scale-set-extension-is-trying-to-install-something-that-requires-a-reboot-for-example-commandtoexecute-powershellexe--executionpolicy-unrestricted-install-windowsfeature-name-fs-resource-manager-includemanagementtools"></a><span data-ttu-id="a5e29-286">Min virtuella skala utökas försöker installera något som kräver en omstart.</span><span class="sxs-lookup"><span data-stu-id="a5e29-286">My virtual machine scale set extension is trying to install something that requires a reboot.</span></span> <span data-ttu-id="a5e29-287">Till exempel ”commandToExecute” ”: powershell.exe - ExecutionPolicy obegränsad Install-WindowsFeature-Name FS-Resource-Manager – IncludeManagementTools”</span><span class="sxs-lookup"><span data-stu-id="a5e29-287">For example, "commandToExecute": "powershell.exe -ExecutionPolicy Unrestricted Install-WindowsFeature –Name FS-Resource-Manager –IncludeManagementTools"</span></span>

<span data-ttu-id="a5e29-288">Om din virtuella skala utökas försöker installera något som kräver omstart, kan du använda Azure Automation Desired State Configuration (Automation DSC)-tillägget.</span><span class="sxs-lookup"><span data-stu-id="a5e29-288">If your virtual machine scale set extension is trying to install something that requires a reboot, you can use the Azure Automation Desired State Configuration (Automation DSC) extension.</span></span> <span data-ttu-id="a5e29-289">Om operativsystemet är Windows Server 2012 R2, Azure hämtar i installationsprogrammet för Windows Management Framework (WMF) 5.0, omstarter, och sedan fortsätta med konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="a5e29-289">If the operating system is Windows Server 2012 R2, Azure pulls in the Windows Management Framework (WMF) 5.0 setup, reboots, and then continues with the configuration.</span></span> 
 
### <a name="how-do-i-turn-on-antimalware-in-my-virtual-machine-scale-set"></a><span data-ttu-id="a5e29-290">Hur aktiverar jag program mot skadlig kod i min skaluppsättning för virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="a5e29-290">How do I turn on antimalware in my virtual machine scale set?</span></span>

<span data-ttu-id="a5e29-291">Om du vill aktivera program mot skadlig kod på virtuella datorns skaluppsättning, Använd följande PowerShell-exempel:</span><span class="sxs-lookup"><span data-stu-id="a5e29-291">To turn on antimalware on your virtual machine scale set, use the following PowerShell example:</span></span>

```powershell
$rgname = 'autolap'
$vmssname = 'autolapbr'
$location = 'eastus'
 
# Retrieve the most recent version number of the extension.
$allVersions= (Get-AzureRmVMExtensionImage -Location $location -PublisherName "Microsoft.Azure.Security" -Type "IaaSAntimalware").Version
$versionString = $allVersions[($allVersions.count)-1].Split(".")[0] + "." + $allVersions[($allVersions.count)-1].Split(".")[1]
 
$VMSS = Get-AzureRmVmss -ResourceGroupName $rgname -VMScaleSetName $vmssname
echo $VMSS
Add-AzureRmVmssExtension -VirtualMachineScaleSet $VMSS -Name "IaaSAntimalware" -Publisher "Microsoft.Azure.Security" -Type "IaaSAntimalware" -TypeHandlerVersion $versionString
Update-AzureRmVmss -ResourceGroupName $rgname -Name $vmssname -VirtualMachineScaleSet $VMSS 
```

### <a name="i-need-to-execute-a-custom-script-thats-hosted-in-a-private-storage-account-the-script-runs-successfully-when-the-storage-is-public-but-when-i-try-to-use-a-shared-access-signature-sas-it-fails-this-message-is-displayed-missing-mandatory-parameters-for-valid-shared-access-signature-linksas-works-fine-from-my-local-browser"></a><span data-ttu-id="a5e29-292">Jag behöver köra ett anpassat skript som finns i ett privat storage-konto.</span><span class="sxs-lookup"><span data-stu-id="a5e29-292">I need to execute a custom script that's hosted in a private storage account.</span></span> <span data-ttu-id="a5e29-293">Skriptet har körts när lagring är offentlig, men när jag försöker använda en delad signatur åtkomst (SAS), misslyckas.</span><span class="sxs-lookup"><span data-stu-id="a5e29-293">The script runs successfully when the storage is public, but when I try to use a Shared Access Signature (SAS), it fails.</span></span> <span data-ttu-id="a5e29-294">Det här meddelandet visas: ”obligatoriska parametrar saknas för giltig signatur för delad åtkomst”.</span><span class="sxs-lookup"><span data-stu-id="a5e29-294">This message is displayed: “Missing mandatory parameters for valid Shared Access Signature”.</span></span> <span data-ttu-id="a5e29-295">Länken + SAS fungerar bra i min lokala webbläsare.</span><span class="sxs-lookup"><span data-stu-id="a5e29-295">Link+SAS works fine from my local browser.</span></span>

<span data-ttu-id="a5e29-296">Om du vill köra ett anpassat skript som finns i ett privat lagringskonto konfigurerat skyddade inställningarna lagringskontonyckel och namn.</span><span class="sxs-lookup"><span data-stu-id="a5e29-296">To execute a custom script that's hosted in a private storage account, set up protected settings with the storage account key and name.</span></span> <span data-ttu-id="a5e29-297">Mer information finns i [anpassade skript tillägget för Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).</span><span class="sxs-lookup"><span data-stu-id="a5e29-297">For more information, see [Custom Script Extension for Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).</span></span>







## <a name="networking"></a><span data-ttu-id="a5e29-298">Nätverk</span><span class="sxs-lookup"><span data-stu-id="a5e29-298">Networking</span></span>
 
### <a name="is-it-possible-to-assign-a-network-security-group-nsg-to-a-scale-set-so-that-it-will-apply-to-all-the-vm-nics-in-the-set"></a><span data-ttu-id="a5e29-299">Är det möjligt att tilldela en Nätverkssäkerhetsgrupp (NSG) till en skalningsuppsättning så att den gäller för alla Virtuella nätverkskort i uppsättningen?</span><span class="sxs-lookup"><span data-stu-id="a5e29-299">Is it possible to assign a Network Security Group (NSG) to a scale set, so that it will apply to all the VM NICs in the set?</span></span>

<span data-ttu-id="a5e29-300">Ja.</span><span class="sxs-lookup"><span data-stu-id="a5e29-300">Yes.</span></span> <span data-ttu-id="a5e29-301">En Nätverkssäkerhetsgrupp kan tillämpas direkt på en skala som refererar till den i avsnittet networkInterfaceConfigurations i nätverksprofilen.</span><span class="sxs-lookup"><span data-stu-id="a5e29-301">A Network Security Group can be applied directly to a scale set by referencing it in the networkInterfaceConfigurations section of the network profile.</span></span> <span data-ttu-id="a5e29-302">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a5e29-302">Example:</span></span>

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

### <a name="how-do-i-do-a-vip-swap-for-virtual-machine-scale-sets-in-the-same-subscription-and-same-region"></a><span data-ttu-id="a5e29-303">Hur gör jag en VIP-växling för skalningsuppsättningar i virtuella datorer i samma prenumeration och samma region?</span><span class="sxs-lookup"><span data-stu-id="a5e29-303">How do I do a VIP swap for virtual machine scale sets in the same subscription and same region?</span></span>

<span data-ttu-id="a5e29-304">Om du har två skalningsuppsättningar i virtuella datorn med Azure belastningsutjämnare framför-servrar och de finns i samma prenumeration och region, kan du frigöra de offentliga IP-adresserna från var och en och tilldela till den andra.</span><span class="sxs-lookup"><span data-stu-id="a5e29-304">If you have two virtual machine scale sets with Azure Load Balancer front-ends, and they are in the same subscription and region, you could deallocate the public IP addresses from each one, and assign to the other.</span></span> <span data-ttu-id="a5e29-305">Se [VIP-växling: blå-grön distribution i Azure Resource Manager](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) t.ex.</span><span class="sxs-lookup"><span data-stu-id="a5e29-305">See [VIP Swap: Blue-green deployment in Azure Resource Manager](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) for example.</span></span> <span data-ttu-id="a5e29-306">Detta innebär en fördröjning nivå om resurserna är frigjord/fördelas på nätverket.</span><span class="sxs-lookup"><span data-stu-id="a5e29-306">This does imply a delay though as the resources are deallocated/allocated at the network level.</span></span> <span data-ttu-id="a5e29-307">Ett snabbare alternativ är att använda Azure Application Gateway med två serverdelspooler och en regel för vidarebefordran.</span><span class="sxs-lookup"><span data-stu-id="a5e29-307">A faster option is to use Azure Application Gateway with two backend pools, and a routing rule.</span></span> <span data-ttu-id="a5e29-308">Du kan också värd för ditt program med [Azure App service](https://azure.microsoft.com/en-us/services/app-service/) som ger stöd för snabb växling mellan olika platser för mellanlagring och produktion.</span><span class="sxs-lookup"><span data-stu-id="a5e29-308">Alternatively, you could host your application with [Azure App service](https://azure.microsoft.com/en-us/services/app-service/) which provides support for fast switching between staging and production slots.</span></span>
 
### <a name="how-do-i-specify-a-range-of-private-ip-addresses-to-use-for-static-private-ip-address-allocation"></a><span data-ttu-id="a5e29-309">Hur jag för att ange ett intervall med privata IP-adresser som ska användas för statisk privat IP-adressallokering?</span><span class="sxs-lookup"><span data-stu-id="a5e29-309">How do I specify a range of private IP addresses to use for static private IP address allocation?</span></span>

<span data-ttu-id="a5e29-310">IP-adresser har markerats från ett undernät som du anger.</span><span class="sxs-lookup"><span data-stu-id="a5e29-310">IP addresses are selected from a subnet that you specify.</span></span> 

<span data-ttu-id="a5e29-311">Allokeringsmetoden virtuella skala uppsättning IP-adresser är alltid ”dynamic”, men det betyder att du kan ändra dessa IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="a5e29-311">The allocation method of virtual machine scale set IP addresses is always “dynamic,” but that doesn't mean that these IP addresses can change.</span></span> <span data-ttu-id="a5e29-312">I det här fallet betyder ”dynamic” bara att du inte anger IP-adressen i en PUT-begäran.</span><span class="sxs-lookup"><span data-stu-id="a5e29-312">In this case, "dynamic" only means that you do not specify the IP address in a PUT request.</span></span> <span data-ttu-id="a5e29-313">Ange den statiska ange med undernätet.</span><span class="sxs-lookup"><span data-stu-id="a5e29-313">Specify the static set by using the subnet.</span></span> 
    
### <a name="how-do-i-deploy-a-virtual-machine-scale-set-to-an-existing-azure-virtual-network"></a><span data-ttu-id="a5e29-314">Hur distribuerar en virtuell dator skala inställd på befintliga Azure-nätverk?</span><span class="sxs-lookup"><span data-stu-id="a5e29-314">How do I deploy a virtual machine scale set to an existing Azure virtual network?</span></span> 

<span data-ttu-id="a5e29-315">För att distribuera en virtuell dator skala inställd på befintliga Azure-nätverk finns [distribuera en virtuell dator skala har angetts till ett befintligt virtuellt nätverk](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet).</span><span class="sxs-lookup"><span data-stu-id="a5e29-315">To deploy a virtual machine scale set to an existing Azure virtual network, see [Deploy a virtual machine scale set to an existing virtual network](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet).</span></span> 

### <a name="how-do-i-add-the-ip-address-of-the-first-vm-in-a-virtual-machine-scale-set-to-the-output-of-a-template"></a><span data-ttu-id="a5e29-316">Hur lägger jag till IP-adressen för den första virtuella datorn i en virtuell dator skala inställd på utdata från en mall</span><span class="sxs-lookup"><span data-stu-id="a5e29-316">How do I add the IP address of the first VM in a virtual machine scale set to the output of a template?</span></span>

<span data-ttu-id="a5e29-317">Om du vill lägga till IP-adressen för den första virtuella datorn i en virtuell dator skala inställd på utdata från en mall finns [ARM: hämta VMSS privata IP-adresser](http://stackoverflow.com/questions/42790392/arm-get-vmsss-private-ips).</span><span class="sxs-lookup"><span data-stu-id="a5e29-317">To add the IP address of the first VM in a virtual machine scale set to the output of a template, see [ARM: Get VMSS's private IPs](http://stackoverflow.com/questions/42790392/arm-get-vmsss-private-ips).</span></span>

### <a name="can-i-use-scale-sets-with-accelerated-networking"></a><span data-ttu-id="a5e29-318">Kan jag använda skaluppsättningar med snabbare nätverk?</span><span class="sxs-lookup"><span data-stu-id="a5e29-318">Can I use scale sets with Accelerated Networking?</span></span>

<span data-ttu-id="a5e29-319">Ja.</span><span class="sxs-lookup"><span data-stu-id="a5e29-319">Yes.</span></span> <span data-ttu-id="a5e29-320">Om du vill använda snabbare nätverksfunktioner, ange enableAcceleratedNetworking till true i ditt skala uppsättningens networkInterfaceConfigurations inställningar.</span><span class="sxs-lookup"><span data-stu-id="a5e29-320">To use accelerated networking, set enableAcceleratedNetworking to true in your scale set's networkInterfaceConfigurations settings.</span></span> <span data-ttu-id="a5e29-321">T.ex.</span><span class="sxs-lookup"><span data-stu-id="a5e29-321">E.g.</span></span>
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

### <a name="how-can-i-configure-the-dns-servers-used-by-a-scale-set"></a><span data-ttu-id="a5e29-322">Hur kan jag konfigurera DNS-servrar som används av en skalningsuppsättning?</span><span class="sxs-lookup"><span data-stu-id="a5e29-322">How can I configure the DNS servers used by a scale set?</span></span>

<span data-ttu-id="a5e29-323">Lägg till ett dnsSettings JSON-paket till avsnittet scale set networkInterfaceConfigurations för att skapa en VM-skala med en anpassad DNS-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="a5e29-323">To create a VM scale set with a custom DNS configuration, add a dnsSettings JSON packet to the scale set networkInterfaceConfigurations section.</span></span> <span data-ttu-id="a5e29-324">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a5e29-324">Example:</span></span>
```json
    "dnsSettings":{
        "dnsServers":["10.0.0.6", "10.0.0.5"]
    }
```

### <a name="how-can-i-configure-a-scale-set-to-assign-a-public-ip-address-to-each-vm"></a><span data-ttu-id="a5e29-325">Hur konfigurerar en skala som anger att tilldela en offentlig IP-adress till varje virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="a5e29-325">How can I configure a scale set to assign a public IP address to each VM?</span></span>

<span data-ttu-id="a5e29-326">Om du vill skapa en skaluppsättning för virtuell dator som tilldelar en offentlig IP-adress till varje virtuell dator, se till att API-versionen av resursen Microsoft.Compute/virtualMAchineScaleSets 2017-03-30 och Lägg till en _publicipaddressconfiguration_ JSON-paketet till skalan ange ipConfigurations avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a5e29-326">To create a VM scale set that assigns a public IP address to each VM, make sure the API version of the Microsoft.Compute/virtualMAchineScaleSets resource is 2017-03-30, and add a _publicipaddressconfiguration_ JSON packet to the scale set ipConfigurations section.</span></span> <span data-ttu-id="a5e29-327">Exempel:</span><span class="sxs-lookup"><span data-stu-id="a5e29-327">Example:</span></span>

```json
    "publicipaddressconfiguration": {
        "name": "pub1",
        "properties": {
        "idleTimeoutInMinutes": 15
        }
    }
```

### <a name="can-i-configure-a-scale-set-to-work-with-multiple-application-gateways"></a><span data-ttu-id="a5e29-328">Konfigurerar en skala som ska fungera med flera Programgatewayer?</span><span class="sxs-lookup"><span data-stu-id="a5e29-328">Can I configure a scale set to work with multiple Application Gateways?</span></span>

<span data-ttu-id="a5e29-329">Ja.</span><span class="sxs-lookup"><span data-stu-id="a5e29-329">Yes.</span></span> <span data-ttu-id="a5e29-330">Du kan lägga till resurs-id's för flera Programgateway serverdelsadresspooler till den _applicationGatewayBackendAddressPools_ lista i den _ipConfigurations_ delen av din skala inställd nätverksprofil.</span><span class="sxs-lookup"><span data-stu-id="a5e29-330">You can add the resource id's for multiple Application Gateway backend address pools to the _applicationGatewayBackendAddressPools_ list in the _ipConfigurations_ section of your scale set network profile.</span></span>

## <a name="scale"></a><span data-ttu-id="a5e29-331">Skala</span><span class="sxs-lookup"><span data-stu-id="a5e29-331">Scale</span></span>

### <a name="in-what-case-would-i-create-a-virtual-machine-scale-set-with-fewer-than-two-vms"></a><span data-ttu-id="a5e29-332">I vilket fall skulle jag skapa en virtuell dator-skala med färre än två virtuella datorer?</span><span class="sxs-lookup"><span data-stu-id="a5e29-332">In what case would I create a virtual machine scale set with fewer than two VMs?</span></span>

<span data-ttu-id="a5e29-333">En orsak till att skapa en virtuell dator-skala med färre än två virtuella datorer är att använda elastisk egenskaperna för en skaluppsättning för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a5e29-333">One reason to create a virtual machine scale set with fewer than two VMs would be to use the elastic properties of a virtual machine scale set.</span></span> <span data-ttu-id="a5e29-334">Du kan till exempel distribuera en virtuell dator skala ange med noll virtuella datorer för att definiera din infrastruktur utan att betala VM driftskostnader.</span><span class="sxs-lookup"><span data-stu-id="a5e29-334">For example, you could deploy a virtual machine scale set with zero VMs to define your infrastructure without paying VM running costs.</span></span> <span data-ttu-id="a5e29-335">Sedan när du är redo att distribuera virtuella datorer, öka ”kapaciteten” för virtuella datorns skaluppsättning inställd på instansantalet produktion.</span><span class="sxs-lookup"><span data-stu-id="a5e29-335">Then, when you are ready to deploy VMs, increase the “capacity” of the virtual machine scale set to the production instance count.</span></span>

<span data-ttu-id="a5e29-336">En annan orsak kan du skapa en virtuell dator-skala med färre än två virtuella datorer är om du vill göra mindre med tillgänglighet än med en tillgänglighetsuppsättning med diskreta virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a5e29-336">Another reason you might create a virtual machine scale set with fewer than two VMs is if you're concerned less with availability than in using an availability set with discrete VMs.</span></span> <span data-ttu-id="a5e29-337">Skaluppsättningar för den virtuella datorn ger dig möjlighet att arbeta med odifferentierad beräknings-enheter som är fungibla.</span><span class="sxs-lookup"><span data-stu-id="a5e29-337">Virtual machine scale sets give you a way to work with undifferentiated compute units that are fungible.</span></span> <span data-ttu-id="a5e29-338">Denna enhetlighet är nyckelfaktorn för skalningsuppsättningar i virtuella datorer jämfört med tillgänglighetsuppsättningar.</span><span class="sxs-lookup"><span data-stu-id="a5e29-338">This uniformity is a key differentiator for virtual machine scale sets versus availability sets.</span></span> <span data-ttu-id="a5e29-339">Många tillståndslösa arbetsbelastningar spåra inte enskilda enheter.</span><span class="sxs-lookup"><span data-stu-id="a5e29-339">Many stateless workloads do not track individual units.</span></span> <span data-ttu-id="a5e29-340">Om arbetsbelastningen släpper du skala till en datornod enhet och sedan att skala upp många när belastningen ökar.</span><span class="sxs-lookup"><span data-stu-id="a5e29-340">If the workload drops, you can scale down to one compute unit, and then scale up to many when the workload increases.</span></span>

### <a name="how-do-i-change-the-number-of-vms-in-a-virtual-machine-scale-set"></a><span data-ttu-id="a5e29-341">Hur ändrar numret för virtuella datorer i en skaluppsättning för virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="a5e29-341">How do I change the number of VMs in a virtual machine scale set?</span></span>

<span data-ttu-id="a5e29-342">Om du vill ändra hur många virtuella datorer i en skaluppsättning för virtuell dator finns [ändra instansantalet för en virtuella datorns skaluppsättning](https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/).</span><span class="sxs-lookup"><span data-stu-id="a5e29-342">To change the number of VMs in a virtual machine scale set, see [Change the instance count of a virtual machine scale set](https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/).</span></span>

### <a name="how-do-i-define-custom-alerts-for-when-certain-thresholds-are-reached"></a><span data-ttu-id="a5e29-343">Hur jag för att definiera anpassade aviseringar för när vissa trösklar har uppnåtts?</span><span class="sxs-lookup"><span data-stu-id="a5e29-343">How do I define custom alerts for when certain thresholds are reached?</span></span>

<span data-ttu-id="a5e29-344">Har du viss flexibilitet i hur du hanterar aviseringar för angivna tröskelvärden.</span><span class="sxs-lookup"><span data-stu-id="a5e29-344">You have some flexibility in how you handle alerts for specified thresholds.</span></span> <span data-ttu-id="a5e29-345">Du kan till exempel definiera anpassade webhooks.</span><span class="sxs-lookup"><span data-stu-id="a5e29-345">For example, you can define customized webhooks.</span></span> <span data-ttu-id="a5e29-346">I följande exempel webhook är från en Resource Manager-mall:</span><span class="sxs-lookup"><span data-stu-id="a5e29-346">The following webhook example is from a Resource Manager template:</span></span>

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

<span data-ttu-id="a5e29-347">I det här exemplet skickas en avisering till Pagerduty.com när ett tröskelvärde uppnås.</span><span class="sxs-lookup"><span data-stu-id="a5e29-347">In this example, an alert goes to Pagerduty.com when a threshold is reached.</span></span>



## <a name="patching-and-operations"></a><span data-ttu-id="a5e29-348">Korrigering och åtgärder</span><span class="sxs-lookup"><span data-stu-id="a5e29-348">Patching and operations</span></span>

### <a name="how-do-i-create-a-scale-set-in-an-existing-resource-group"></a><span data-ttu-id="a5e29-349">Hur skapar jag en skala som angetts i en befintlig resursgrupp</span><span class="sxs-lookup"><span data-stu-id="a5e29-349">How do I create a scale set in an existing resource group?</span></span>

<span data-ttu-id="a5e29-350">Skapa skalningsuppsättningar i en befintlig resurs grupp är ännu inte möjligt från Azure-portalen, men du kan ange en befintlig resursgrupp när distribuera en skala från en Azure Resource Manager-mall.</span><span class="sxs-lookup"><span data-stu-id="a5e29-350">Creating scale sets in an existing resource group is not yet possible from the Azure portal, but you can specify an existing resource group when deploying a scale set from an Azure Resource Manager template.</span></span> <span data-ttu-id="a5e29-351">Du kan också ange en befintlig resursgrupp när du skapar en skala som anges med Azure PowerShell eller CLI.</span><span class="sxs-lookup"><span data-stu-id="a5e29-351">You can also specify an existing resource group when creating a scale set using Azure PowerShell or CLI.</span></span>

### <a name="can-we-move-a-scale-set-to-another-resource-group"></a><span data-ttu-id="a5e29-352">Kan vi flytta en skala som har angetts till en annan resursgrupp?</span><span class="sxs-lookup"><span data-stu-id="a5e29-352">Can we move a scale set to another resource group?</span></span>

<span data-ttu-id="a5e29-353">Ja, kan du flytta skaluppsättning resurser till en ny prenumeration eller resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="a5e29-353">Yes, you can move scale set resources to a new subscription or resource group.</span></span>

### <a name="how-to-i-update-my-virtual-machine-scale-set-to-a-new-image-how-do-i-manage-patching"></a><span data-ttu-id="a5e29-354">Jag kan hur Uppdatera mina virtuella skaluppsättningen till en ny avbildning till?</span><span class="sxs-lookup"><span data-stu-id="a5e29-354">How to I update my virtual machine scale set to a new image?</span></span> <span data-ttu-id="a5e29-355">Hur hanterar jag korrigering?</span><span class="sxs-lookup"><span data-stu-id="a5e29-355">How do I manage patching?</span></span>

<span data-ttu-id="a5e29-356">Uppdatera din virtuella skaluppsättningen till en ny avbildning och hantera korrigering finns [uppgradera en virtuella datorns skaluppsättning](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).</span><span class="sxs-lookup"><span data-stu-id="a5e29-356">To update your virtual machine scale set to a new image, and to manage patching, see [Upgrade a virtual machine scale set](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).</span></span>

### <a name="can-i-use-the-reimage-operation-to-reset-a-vm-without-changing-the-image-that-is-i-want-reset-a-vm-to-factory-settings-rather-than-to-a-new-image"></a><span data-ttu-id="a5e29-357">Kan jag använda avbildningsåterställning igen för att återställa en virtuell dator utan att ändra bilden?</span><span class="sxs-lookup"><span data-stu-id="a5e29-357">Can I use the reimage operation to reset a VM without changing the image?</span></span> <span data-ttu-id="a5e29-358">(Det vill säga jag vill återställa en virtuell dator till fabriksinställningarna i stället för till en ny avbildning.)</span><span class="sxs-lookup"><span data-stu-id="a5e29-358">(That is, I want reset a VM to factory settings rather than to a new image.)</span></span>

<span data-ttu-id="a5e29-359">Ja, du kan använda avbildningsåterställning åtgärden för att återställa en virtuell dator utan att ändra bilden.</span><span class="sxs-lookup"><span data-stu-id="a5e29-359">Yes, you can use the reimage operation to reset a VM without changing the image.</span></span> <span data-ttu-id="a5e29-360">Men om din virtuella skalan refererar till en plattformsavbildning med `version = latest`, den virtuella datorn kan uppdatera till en senare OS-avbildningen när du anropar `reimage`.</span><span class="sxs-lookup"><span data-stu-id="a5e29-360">However, if your virtual machine scale set references a platform image with `version = latest`, your VM can update to a later OS image when you call `reimage`.</span></span>

<span data-ttu-id="a5e29-361">Mer information finns i [hantera alla virtuella datorer i en skaluppsättning för virtuell dator](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set).</span><span class="sxs-lookup"><span data-stu-id="a5e29-361">For more information, see [Manage all VMs in a virtual machine scale set](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set).</span></span>



## <a name="troubleshooting"></a><span data-ttu-id="a5e29-362">Felsökning</span><span class="sxs-lookup"><span data-stu-id="a5e29-362">Troubleshooting</span></span>

### <a name="how-do-i-turn-on-boot-diagnostics"></a><span data-ttu-id="a5e29-363">Hur aktiverar jag startdiagnostikinställningar?</span><span class="sxs-lookup"><span data-stu-id="a5e29-363">How do I turn on boot diagnostics?</span></span>

<span data-ttu-id="a5e29-364">Om du vill aktivera startdiagnostikinställningar först skapa ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="a5e29-364">To turn on boot diagnostics, first, create a storage account.</span></span> <span data-ttu-id="a5e29-365">Placera sedan JSON blocket i din virtuella datorns skaluppsättning **virtualMachineProfile**, och uppdatera virtuella datorns skaluppsättning:</span><span class="sxs-lookup"><span data-stu-id="a5e29-365">Then, put this JSON block in your virtual machine scale set **virtualMachineProfile**, and update the virtual machine scale set:</span></span>

```json
"diagnosticsProfile": {
    "bootDiagnostics": {
        "enabled": true,
        "storageUri": "http://yourstorageaccount.blob.core.windows.net"
    }
}
```

<span data-ttu-id="a5e29-366">När en ny virtuell dator skapas visar egenskapen InstanceView för den virtuella datorn för skärmbild och så vidare.</span><span class="sxs-lookup"><span data-stu-id="a5e29-366">When a new VM is created, the InstanceView property of the VM shows the details for the screenshot, and so on.</span></span> <span data-ttu-id="a5e29-367">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="a5e29-367">Here's an example:</span></span>
 
```json
"bootDiagnostics": {
    "consoleScreenshotBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.screenshot.bmp",
    "serialConsoleLogBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.serialconsole.log"
  }
```


## <a name="virtual-machine-properties"></a><span data-ttu-id="a5e29-368">Egenskaper för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a5e29-368">Virtual machine properties</span></span>

### <a name="how-do-i-get-property-information-for-each-vm-without-making-multiple-calls-for-example-how-would-i-get-the-fault-domain-for-each-of-the-100-vms-in-my-virtual-machine-scale-set"></a><span data-ttu-id="a5e29-369">Hur skaffar egenskapsinformation för varje virtuell dator utan att göra flera anrop?</span><span class="sxs-lookup"><span data-stu-id="a5e29-369">How do I get property information for each VM without making multiple calls?</span></span> <span data-ttu-id="a5e29-370">Till exempel hur kan jag feldomänen för varje 100 virtuella datorer i min skaluppsättning för virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="a5e29-370">For example, how would I get the fault domain for each of the 100 VMs in my virtual machine scale set?</span></span>

<span data-ttu-id="a5e29-371">För att få information om egenskaper för varje virtuell dator utan att göra flera anrop, kan du anropa `ListVMInstanceViews` genom att göra ett REST-API `GET` på följande resurs-URI:</span><span class="sxs-lookup"><span data-stu-id="a5e29-371">To get property information for each VM without making multiple calls, you can call `ListVMInstanceViews` by doing a REST API `GET` on the following resource URI:</span></span>

<span data-ttu-id="a5e29-372">/subscriptions/ < PRENUMERATIONSID > /resourceGroups/ < resource_group_name > /providers/Microsoft.Compute/virtualMachineScaleSets/ < scaleset_name > / virtualMachines? $expand = instanceView & $select = instanceView</span><span class="sxs-lookup"><span data-stu-id="a5e29-372">/subscriptions/<subscription_id>/resourceGroups/<resource_group_name>/providers/Microsoft.Compute/virtualMachineScaleSets/<scaleset_name>/virtualMachines?$expand=instanceView&$select=instanceView</span></span>

### <a name="can-i-pass-different-extension-arguments-to-different-vms-in-a-virtual-machine-scale-set"></a><span data-ttu-id="a5e29-373">Kan jag skicka annat tillägg argument till olika virtuella datorer i en skaluppsättning för virtuell dator?</span><span class="sxs-lookup"><span data-stu-id="a5e29-373">Can I pass different extension arguments to different VMs in a virtual machine scale set?</span></span>

<span data-ttu-id="a5e29-374">Nej, kan du ange olika tillägg argument för olika virtuella datorer i en skaluppsättning för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a5e29-374">No, you cannot pass different extension arguments to different VMs in a virtual machine scale set.</span></span> <span data-ttu-id="a5e29-375">Tillägg kan dock fungera baserat på unika egenskaperna för den virtuella datorn körs på, så som på namnet på datorn.</span><span class="sxs-lookup"><span data-stu-id="a5e29-375">However, extensions can act based on the unique properties of the VM they are running on, such as on the machine name.</span></span> <span data-ttu-id="a5e29-376">Tillägg kan också fråga instans metadata på http://169.254.169.254 för mer information om den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="a5e29-376">Extensions also can query instance metadata on http://169.254.169.254 to get more information about the VM.</span></span>

### <a name="why-are-there-gaps-between-my-virtual-machine-scale-set-vm-machine-names-and-vm-ids-for-example-0-1-3"></a><span data-ttu-id="a5e29-377">Varför är det glapp mellan min Virtuella datornamn på virtuell dator skala och VM-ID: N?</span><span class="sxs-lookup"><span data-stu-id="a5e29-377">Why are there gaps between my virtual machine scale set VM machine names and VM IDs?</span></span> <span data-ttu-id="a5e29-378">Till exempel: 0, 1, 3...</span><span class="sxs-lookup"><span data-stu-id="a5e29-378">For example: 0, 1, 3...</span></span>

<span data-ttu-id="a5e29-379">Det förekommer glapp mellan din Virtuella datornamn på virtuell dator skala och VM-ID: N eftersom skalan för din virtuella **overprovision** egenskap är inställd på standardvärdet **SANT**.</span><span class="sxs-lookup"><span data-stu-id="a5e29-379">There are gaps between your virtual machine scale set VM machine names and VM IDs because your virtual machine scale set **overprovision** property is set to the default value of **true**.</span></span> <span data-ttu-id="a5e29-380">Om överetablering anges till **SANT**, flera virtuella datorer än begärt skapas.</span><span class="sxs-lookup"><span data-stu-id="a5e29-380">If overprovisioning is set to **true**, more VMs than requested are created.</span></span> <span data-ttu-id="a5e29-381">Extra VM: ar så tas bort.</span><span class="sxs-lookup"><span data-stu-id="a5e29-381">Extra VMs are then deleted.</span></span> <span data-ttu-id="a5e29-382">I så fall måste du få bättre distribution tillförlitlighet, men på bekostnad av sammanhängande namngivning och sammanhängande (Network Address Translation Translation) regler.</span><span class="sxs-lookup"><span data-stu-id="a5e29-382">In this case, you gain increased deployment reliability, but at the expense of contiguous naming and contiguous Network Address Translation (NAT) rules.</span></span> 

<span data-ttu-id="a5e29-383">Du kan ange egenskapen **FALSKT**.</span><span class="sxs-lookup"><span data-stu-id="a5e29-383">You can set this property to **false**.</span></span> <span data-ttu-id="a5e29-384">För små virtuella skaluppsättningar påverkar detta avsevärt inte distribution tillförlitlighet.</span><span class="sxs-lookup"><span data-stu-id="a5e29-384">For small virtual machine scale sets, this doesn't significantly affect deployment reliability.</span></span>

### <a name="what-is-the-difference-between-deleting-a-vm-in-a-virtual-machine-scale-set-and-deallocating-the-vm-when-should-i-choose-one-over-the-other"></a><span data-ttu-id="a5e29-385">Vad är skillnaden mellan att ta bort en virtuell dator i en skaluppsättning för virtuell dator och det har frigjorts den virtuella datorn?</span><span class="sxs-lookup"><span data-stu-id="a5e29-385">What is the difference between deleting a VM in a virtual machine scale set and deallocating the VM?</span></span> <span data-ttu-id="a5e29-386">När ska jag välja rätt scenario?</span><span class="sxs-lookup"><span data-stu-id="a5e29-386">When should I choose one over the other?</span></span>

<span data-ttu-id="a5e29-387">Den största skillnaden mellan att ta bort en virtuell dator i en skaluppsättning för virtuell dator och det har frigjorts den virtuella datorn är att `deallocate` inte ta bort de virtuella hårddiskarna (VHD).</span><span class="sxs-lookup"><span data-stu-id="a5e29-387">The main difference between deleting a VM in a virtual machine scale set and deallocating the VM is that `deallocate` doesn’t delete the virtual hard disks (VHDs).</span></span> <span data-ttu-id="a5e29-388">Finns kostnader för lagring som är associerade med körs `stop deallocate`.</span><span class="sxs-lookup"><span data-stu-id="a5e29-388">There are storage costs associated with running `stop deallocate`.</span></span> <span data-ttu-id="a5e29-389">Du kan använda en av av något av följande skäl:</span><span class="sxs-lookup"><span data-stu-id="a5e29-389">You might use one or the other for one of the following reasons:</span></span>

- <span data-ttu-id="a5e29-390">Vill du avbryta betalar beräkning kostnader, men du vill behålla diskstatusen på de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="a5e29-390">You want to stop paying compute costs, but you want to keep the disk state of the VMs.</span></span>
- <span data-ttu-id="a5e29-391">Du vill starta en uppsättning virtuella datorer snabbare än vad du kan skala ut en skaluppsättning för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a5e29-391">You want to start a set of VMs more quickly than you could scale out a virtual machine scale set.</span></span>
  - <span data-ttu-id="a5e29-392">Relaterad till det här scenariot kan har du skapat egna Autoskala motor och vill en snabbare skala för slutpunkt till slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="a5e29-392">Related to this scenario, you might have created your own autoscale engine and want a faster end-to-end scale.</span></span>
- <span data-ttu-id="a5e29-393">Du har en skaluppsättning för virtuell dator som är ojämnt fördelad över feldomäner eller uppdatera domäner.</span><span class="sxs-lookup"><span data-stu-id="a5e29-393">You have a virtual machine scale set that is unevenly distributed across fault domains or update domains.</span></span> <span data-ttu-id="a5e29-394">Detta kan bero på att du selektivt ta bort virtuella datorer eller eftersom virtuella datorer har tagits bort efter överetablering.</span><span class="sxs-lookup"><span data-stu-id="a5e29-394">This might be because you selectively deleted VMs, or because VMs were deleted after overprovisioning.</span></span> <span data-ttu-id="a5e29-395">Kör `stop deallocate` följt av `start` på den virtuella datorn skaluppsättningen jämnt distribuerar de virtuella datorerna i feldomäner eller uppdatera domäner.</span><span class="sxs-lookup"><span data-stu-id="a5e29-395">Running `stop deallocate` followed by `start` on the virtual machine scale set evenly distributes the VMs across fault domains or update domains.</span></span>

