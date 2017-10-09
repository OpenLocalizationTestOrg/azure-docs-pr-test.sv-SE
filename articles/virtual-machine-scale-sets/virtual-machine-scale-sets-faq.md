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
# <a name="azure-virtual-machine-scale-sets-faqs"></a>Skala virtuell Azure-dator Anger vanliga frågor och svar

Få svar toofrequently frågor och svar om virtuella datorn anger i Azure.

## <a name="autoscale"></a>Automatisk skalning

### <a name="what-are-best-practices-for-azure-autoscale"></a>Vad är bästa praxis för Azure Autoskala?

Bästa praxis för Autoskala finns [bästa praxis för autoskalning virtuella datorer](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-best-practices).

### <a name="where-do-i-find-metric-names-for-autoscaling-that-uses-host-based-metrics"></a>Var hittar jag mått namn för autoskalning som använder värdbaserad mått?

Tjänstmåttets namn för autoskalning som använder värdbaserad mått finns [stöds mått med Azure-Monitor](https://azure.microsoft.com/documentation/articles/monitoring-supported-metrics/).

### <a name="are-there-any-examples-of-autoscaling-based-on-an-azure-service-bus-topic-and-queue-length"></a>Finns det några exempel på autoskalning baserat på en Azure Service Bus-avsnittet och kön längd?

Ja. Exempel på autoskalning baserat på en Azure Service Bus-avsnittet och kön längd finns [Azure-Monitor autoskalning vanliga mått](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/).

Använd hello följande JSON för Service Bus-kö:

```json
"metricName": "MessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ServiceBus/namespaces/mySB/queues/myqueue"
```

Använd hello följande JSON för en storage-kö:

```json
"metricName": "ApproximateMessageCount",
"metricNamespace": "",
"metricResourceUri": "/subscriptions/s1/resourceGroups/rg1/providers/Microsoft.ClassicStorage/storageAccounts/mystorage/services/queue/queues/mystoragequeue"
```

Ersätt exempelvärden med din resurs Uniform Resource Identifier (URI: er).


### <a name="should-i-autoscale-by-using-host-based-metrics-or-a-diagnostics-extension"></a>Bör jag Autoskala med hjälp av värdbaserad mått eller ett tillägg för diagnostik?

Du kan skapa en autoskalningsinställning på en VM toouse värdnivå mått eller gäst-OS-baserade mått.

En lista över stöds mått finns [Azure-Monitor autoskalning vanliga mått](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-autoscale-common-metrics). 

En fullständig exempelfil för skalningsuppsättningar i virtuella datorer finns [avancerade Autoskala konfiguration med hjälp av Resource Manager-mallar för virtuella datorer](https://docs.microsoft.com/azure/monitoring-and-diagnostics/insights-advanced-autoscale-virtual-machine-scale-sets). 

hello används hello värdnivå CPU mått och ett meddelande count-mått.



### <a name="how-do-i-set-alert-rules-on-a-virtual-machine-scale-set"></a>Hur ställer jag in Varningsregler i en skaluppsättning för virtuell dator?

Du kan skapa aviseringar för mått för skalningsuppsättningar i virtuella datorer via PowerShell eller Azure CLI. Mer information finns i [Azure-Monitor PowerShell snabb start prover](https://azure.microsoft.com/documentation/articles/insights-powershell-samples/#create-alert-rules) och [Azure-Monitor plattformsoberoende CLI snabb start exempel](https://azure.microsoft.com/documentation/articles/insights-cli-samples/#work-with-alerts).

Hej TargetResourceId för hello skaluppsättning för virtuell dator ser ut så här: 

/subscriptions/yoursubscriptionid/resourceGroups/yourresourcegroup/providers/Microsoft.Compute/virtualMachineScaleSets/yourvmssname

Du kan välja alla VM-prestandaräknaren som hello mått tooset en avisering för. Mer information finns i [Gästoperativsystem mätvärden för Resource Manager-baserade virtuella Windows-datorer](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-resource-manager-based-windows-vms) och [Gästoperativsystem mätvärden för virtuella Linux-datorer](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/#guest-os-metrics-linux-vms) i hello [Azure-Monitor autoskalning vanliga mått](https://azure.microsoft.com/documentation/articles/insights-autoscale-common-metrics/)artikel.

### <a name="how-do-i-set-up-autoscale-on-a-virtual-machine-scale-set-by-using-powershell"></a>Hur ställer jag in Autoskala på en virtuell dator skala in med hjälp av PowerShell?

tooset in Autoskala på en virtuell dator skala in med hjälp av PowerShell finns i blogginlägget hello [hur tooadd Autoskala tooan virtuella Azure-datorn skala in](https://msftstack.wordpress.com/2017/03/05/how-to-add-autoscale-to-an-azure-vm-scale-set/).




## <a name="certificates"></a>Certifikat

### <a name="how-do-i-securely-ship-a-certificate-toohello-vm-how-do-i-provision-a-virtual-machine-scale-set-toorun-a-website-where-hello-ssl-for-hello-website-is-shipped-securely-from-a-certificate-configuration-hello-common-certificate-rotation-operation-would-be-almost-hello-same-as-a-configuration-update-operation-do-you-have-an-example-of-how-toodo-this"></a>Hur skickar jag en certifikat-toohello VM på ett säkert sätt? Hur jag för att etablera en virtuell dator scale set toorun en webbplats som hello SSL för hello webbplats levereras på ett säkert sätt från en konfiguration? (hello vanliga certifikat rotation åtgärden skulle vara nästan hello samma som en uppdateringsåtgärd konfiguration.) Har du ett exempel på hur toodo detta? 

toosecurely levereras certifikat-toohello VM kan du installera ett certifikat för kunden direkt till ett Windows-certifikatarkiv från hello kundens nyckelvalvet.

Använd följande JSON hello:

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

hello koden har stöd för Windows och Linux.

Mer information finns i [skapa eller uppdatera en virtuell dator skaluppsättning](https://msdn.microsoft.com/library/mt589035.aspx).


### <a name="example-of-self-signed-certificate"></a>Exempel på självsignerat certifikat

1.  Skapa ett självsignerat certifikat i en nyckelvalvet.

    Använd hello följande PowerShell-kommandon:

    ```powershell
    Import-Module "C:\Users\mikhegn\Downloads\Service-Fabric-master\Scripts\ServiceFabricRPHelpers\ServiceFabricRPHelpers.psm1"

    Login-AzureRmAccount

    Invoke-AddCertToKeyVault -SubscriptionId <Your SubID> -ResourceGroupName KeyVault -Location westus -VaultName MikhegnVault -CertificateName VMSSCert -Password VmssCert -CreateSelfSignedCertificate -DnsName vmss.mikhegn.azure.com -OutputPath c:\users\mikhegn\desktop\
    ```

    Det här kommandot ger du hello indata för hello Azure Resource Manager-mall.

    Ett exempel på hur toocreate ett självsignerat certifikat i nyckelvalvet, se [säkerhetsscenarier för Service Fabric-kluster](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/).

2.  Ändra hello Resource Manager-mall.

    Lägg till den här egenskapen för**virtualMachineProfile**, som en del av hello virtuella skaluppsättning för resursen:

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
  

### <a name="can-i-specify-an-ssh-key-pair-toouse-for-ssh-authentication-with-a-linux-virtual-machine-scale-set-from-a-resource-manager-template"></a>Kan jag ange en SSH-nyckelpar toouse för SSH-autentisering med en Linux skaluppsättningen för virtuell dator från en Resource Manager-mall  

Ja. hello REST API för **osProfile** är liknande toohello standard VM-REST API. 

Inkludera **osProfile** i mallen:

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
 
Det här JSON-blocket används i [hello 101-vm-sshkey GitHub Snabbstart mallen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).
 
hello OS-profil används också i [hello grelayhost.json GitHub quick start mallen](https://github.com/ExchMaster/gadgetron/blob/master/Gadgetron/Templates/grelayhost.json).

Mer information finns i [skapa eller uppdatera en virtuell dator skaluppsättning](https://msdn.microsoft.com/library/azure/mt589035.aspx#linuxconfiguration).
  

### <a name="how-do-i-remove-deprecated-certificates"></a>Hur tar jag bort föråldrad certifikat? 

tooremove föråldrad certifikat, ta bort gammalt certifikat för hello hello valvet certifikat listan. Lämna alla hello certifikat som du vill tooremain på datorn i hello-listan. Hello certifikatet tas inte bort från dina virtuella datorer. Också läggs inte hello certifikat toonew virtuella datorer som skapas i hello skaluppsättning för virtuell dator. 

tooremove hello certifikat från befintliga virtuella datorer, skriva ett anpassat skript för tillägget toomanually ta bort hello certifikat från certifikatarkivet.
 
### <a name="how-do-i-inject-an-existing-ssh-public-key-into-hello-virtual-machine-scale-set-ssh-layer-during-provisioning-i-want-toostore-hello-ssh-public-key-values-in-azure-key-vault-and-then-use-them-in-my-resource-manager-template"></a>Hur jag mata in en befintlig offentlig SSH-nyckel i hello virtuella scale set SSH layer under etablering? Jag vill toostore hello SSH offentlig nyckelvärdena i Azure Key Vault och använda dem i Resource Manager-mall.

Om du tillhandahåller hello virtuella datorer bara med en offentlig SSH-nyckel, behöver du inte tooput hello offentliga nycklar i Nyckelvalvet. Offentliga nycklar är inte hemliga.
 
Du kan ange offentliga SSH-nycklar i klartext när du skapar en Linux VM:

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
 
linuxConfiguration elementnamn | Krävs | Typ | Beskrivning
--- | --- | --- | --- |  ---
SSH | Nej | Samling | Anger hello SSH-nyckeln är konfigurerad för ett Linux-operativsystem
Sökväg | Ja | Sträng | Anger hello Linux sökväg där hello SSH-nycklar eller certifikat ska hittas
nyckeldata | Ja | Sträng | Anger en base64-kodad SSH offentlig nyckel

Ett exempel finns [hello 101-vm-sshkey GitHub Snabbstart mallen](https://github.com/Azure/azure-quickstart-templates/blob/master/101-vm-sshkey/azuredeploy.json).

 
### <a name="when-i-run-update-azurermvmss-after-adding-more-than-one-certificate-from-hello-same-key-vault-i-see-hello-following-message"></a>När kör `Update-AzureRmVmss` efter att lägga till fler än ett certifikat från hello samma nyckel valvet, visas hello följande meddelande:
 
>Update-AzureRmVmss: Listan hemlighet innehåller upprepade förekomster av /subscriptions/ < min-prenumerations-id > / resourceGroups/internal-rg-dev/providers/Microsoft.KeyVault/vaults/internal-keyvault-dev, vilket inte är tillåtet.
 
Detta kan inträffa om du försöker toore-Lägg till hello samma valvet istället för att använda ett nytt certifikat för valvet för hello befintliga källvalv. Hej `Add-AzureRmVmssSecret` kommandot fungerar inte korrekt om du lägger till ytterligare hemligheter.
 
tooadd mer hemligheter från hello samma nyckelvalvet, uppdatera hello $vmss.properties.osProfile.secrets[0].vaultCertificates lista.
 
Hello förväntade indatastrukturen finns [skapa eller uppdatera en virtuell dator ange](https://msdn.microsoft.com/library/azure/mt589035.aspx).
 
Hitta hello hemlighet i hello virtuella Skala uppsättningsobjekt som ingår i hello nyckelvalvet. Lägg sedan till din referens (hello URL och hello hemliga store-namn) toohello lista över certifikat som är associerade med hello-valvet.

> [!NOTE] 
> För närvarande kan du ta bort certifikat från virtuella datorer med hjälp av hello virtuella scale set API.
>

Nya virtuella datorer inte hello gammalt certifikat. Virtuella datorer som har hello certifikat och som redan har distribuerats har dock hello gammalt certifikat.
 
### <a name="can-i-push-certificates-toohello-virtual-machine-scale-set-without-providing-hello-password-when-hello-certificate-is-in-hello-secret-store"></a>Kan jag push-certifikat toohello virtuella skaluppsättningen utan att ange hello lösenord när hello certifikat i hello hemliga store?

Du behöver inte toohard kod lösenord i skript. Du kan hämta lösenord dynamiskt med hello behörigheter du använder toorun hello distributions-skriptet. Om du har ett skript som flyttar ett certifikat från hello hemliga store nyckelvalv hello hemliga store `get certificate` hello lösenordet för PFX-fil som hello kommando också.
 
### <a name="how-does-hello-secrets-property-of-virtualmachineprofileosprofile-for-a-virtual-machine-scale-set-work-why-do-i-need-hello-sourcevault-value-when-i-have-toospecify-hello-absolute-uri-for-a-certificate-by-using-hello-certificateurl-property"></a>Hur ange arbete i hello hemligheter-egenskapen för virtualMachineProfile.osProfile för en virtuell dator skala? Varför måste jag göra hello sourceVault värde när jag har toospecify hello absolut URI för ett certifikat med hjälp av hello certificateUrl egenskapen? 

En referens för Windows Remote Management (WinRM)-certifikat måste finnas i hello hemligheter-egenskapen för hello OS-profil. 

hello syftar som visar hello källvalv tooenforce åtkomstkontrollistan (ACL) principer för åtkomstkontroll som finns i en användares Azure Cloud Service model. Om hello källvalv har inte angetts, är användare som inte har behörigheter toodeploy eller åtkomst hemligheter tooa nyckelvalv kan toothrough en Compute Resource Provider (CRP). ACL: er finns även för resurser som inte finns.

Om du anger ett felaktigt källa valvet ID men en giltig key vault-URL, rapporteras ett fel när du avsöka hello igen.
 
### <a name="if-i-add-secrets-tooan-existing-virtual-machine-scale-set-are-hello-secrets-injected-into-existing-vms-or-only-into-new-ones"></a>Om jag lägger till hemligheter tooan befintlig skaluppsättning för virtuell dator är hello hemligheter matas in i befintliga virtuella datorer eller bara nya? 

Certifikat läggs tooall dina virtuella datorer, även redan befintliga viktiga. Om din virtuella skaluppsättning upgradePolicy egenskap är inställd för**manuell**, hello certifikatet läggs toohello VM när du utför en manuell uppdatering på hello VM.
 
### <a name="where-do-i-put-certificates-for-linux-vms"></a>Där placera certifikat för Linux virtuella datorer?

hur toodeploy certifikat för Linux virtuella datorer kan se toolearn [distribuera certifikat tooVMs från en kundhanterad nyckelvalv](https://blogs.technet.microsoft.com/kv/2015/07/14/deploy-certificates-to-vms-from-customer-managed-key-vault/).
  
### <a name="how-do-i-add-a-new-vault-certificate-tooa-new-certificate-object"></a>Hur lägger jag till en ny valvet tooa nya certifikat certifikatobjekt?

tooadd en valvet certifikat tooan befintliga hemlighet, se hello följande PowerShell-exempel. Använd endast hemliga objekt.
 
```powershell
$newVaultCertificate = New-AzureRmVmssVaultCertificateConfig -CertificateStore MY -CertificateUrl https://sansunallapps1.vault.azure.net:443/secrets/dg-private-enc/55fa0332edc44a84ad655298905f1809
 
$vmss.VirtualMachineProfile.OsProfile.Secrets[0].VaultCertificates.Add($newVaultCertificate)
 
Update-AzureRmVmss -VirtualMachineScaleSet $vmss -ResourceGroup $rg -Name $vmssName
```
 
### <a name="what-happens-toocertificates-if-you-reimage-a-vm"></a>Vad händer toocertificates om du återavbilda en virtuell dator?

Om du återavbilda en virtuell dator tas certifikat bort. Återställning av avbildning borttagningar hello hela OS-disken. 
 
### <a name="what-happens-if-you-delete-a-certificate-from-hello-key-vault"></a>Vad händer om du tar bort ett certifikat från hello nyckelvalv?

Om hello hemlighet tas bort från hello nyckelvalvet och kör du `stop deallocate` för dina virtuella datorer och starta sedan om dem igen, inträffar ett fel. hello felet inträffar eftersom hello CRP behöver tooretrieve hello hemligheter från hello nyckelvalvet, men den inte kan. I det här scenariot kan du ta bort hello certifikat hello virtuella skala modellen. 

hello CRP komponenten inte har behållits kunden hemligheter. Om du kör `stop deallocate` för alla virtuella datorer i hello virtuella datorns skaluppsättning hello cache har tagits bort. I det här scenariot hämtas hemligheter från hello nyckelvalvet.

Det uppstår inte här problemet när skala ut eftersom det inte finns en cachelagrad kopia av hello hemlighet i Azure Service Fabric (i hello enskild fabric klient model).
 
### <a name="why-do-i-have-toospecify-hello-exact-location-for-hello-certificate-url-httpsname-of-hello-vaultvaultazurenet443secretsexact-location-as-indicated-in-service-fabric-cluster-security-scenarioshttpsazuremicrosoftcomdocumentationarticlesservice-fabric-cluster-security"></a>Varför måste jag toospecify hello exakta platsen för Certifikatwebbadress hello (https://<name of hello vault>.vault.azure.net:443/secrets/<exact location>), som anges i [säkerhetsscenarier för Service Fabric-klustret](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security/)?
 
hello Azure Key Vault-dokumentationen om den hello hämta hemligheten REST API ska returnera hello senaste versionen av hello hemlighet om hello-versionen inte har angetts.
 
Metod | URL: EN
--- | ---
HÄMTA | https://mykeyvault.Vault.Azure.NET/secrets/ {hemlighet name} / {hemlighet version}? api-version = {api-version}

Ersätt {*hemlighet namn*} med hello namn och Ersätt {*hemlighet version*} hello hemlighet hello-version du vill använda tooretrieve. hello hemliga versionen kan inte uteslutas. I så fall hämtas hello aktuell version.
  
### <a name="why-do-i-have-toospecify-hello-certificate-version-when-i-use-key-vault"></a>Varför måste jag toospecify hello certifikat version när jag använder Key Vault?

hello syftar hello Key Vault krav toospecify hello certifikat version toomake den Rensa toohello användare för vilka certifikat som har distribuerats på sina virtuella datorer.

Om du skapar en virtuell dator och uppdatera din hemlighet i nyckelvalvet hello hello nya certifikatet är inte hämtad tooyour virtuella datorer. Men dina virtuella datorer visas tooreference och nya virtuella datorer får de hello nya hemligheten. tooavoid, du är obligatoriska tooreference hemliga versionen.

### <a name="my-team-works-with-several-certificates-that-are-distributed-toous-as-cer-public-keys-what-is-hello-recommended-approach-for-deploying-these-certificates-tooa-virtual-machine-scale-set"></a>Min grupp fungerar med flera certifikat som har distribuerats toous som .cer offentliga nycklar. Vad är hello rekommenderat tillvägagångssätt för att distribuera dessa certifikat tooa virtuella datorns skaluppsättning?

skaluppsättning för virtuell dator toodeploy .cer offentliga nycklar tooa, kan du generera en .pfx-fil som innehåller CER-filer. toodo denna, Använd `X509ContentType = Pfx`. Till exempel läsa in hello .cer-fil som x509Certificate2 objekt i C# eller PowerShell och sedan anropa metoden hello. 

Mer information finns i [X509Certificate.Export-metoden (X509ContentType, String)](https://msdn.microsoft.com/library/24ww6yzk(v=vs.110.aspx)).

### <a name="i-do-not-see-an-option-for-users-toopass-in-certificates-as-base64-strings-most-other-resource-providers-have-this-option"></a>Jag ser inte ett alternativ för användare toopass i certifikat som base64-strängar. De flesta andra resursleverantörer har det här alternativet.

tooemulate skicka i ett certifikat som en base64-sträng, kan du extrahera hello senaste version URL i en Resource Manager-mall. Inkludera hello följande JSON-egenskapen i Resource Manager-mall:

```json 
"certificateUrl": "[reference(resourceId(parameters('vaultResourceGroup'), 'Microsoft.KeyVault/vaults/secrets', parameters('vaultName'), parameters('secretName')), '2015-06-01').secretUriWithVersion]"
```
 
### <a name="do-i-have-toowrap-certificates-in-json-objects-in-key-vaults"></a>Måste jag toowrap certifikat i JSON-objekt i nyckelvalv?

I skalningsuppsättningar i virtuella datorer och virtuella datorer, certifikaten omslutas i JSON-objekt. 

Vi stöder också hello innehållstypen application/x-pkcs12. Anvisningar om hur du använder application/x-pkcs12 finns [PFX-certifikat i Azure Key Vault](http://www.rahulpnath.com/blog/pfx-certificate-in-azure-key-vault/).
 
Vi stöder för närvarande inte CER-filer. toouse CER-filer, exportera dem till .pfx-behållare.



## <a name="compliance"></a>Efterlevnad

### <a name="are-virtual-machine-scale-sets-pci-compliant"></a>Är virtuella datorn anger PCI-kompatibel?

Skaluppsättningar för den virtuella datorn är en tunn API skikt som ligger ovanpå hello CRP. Båda komponenterna är en del av hello beräkningsplattform i hello Azure service-trädet.

Skaluppsättningar för den virtuella datorn är en grundläggande del av hello Azure beräkningsplattform ur kompatibilitet. De delar ett team, verktyg, processer, distributionsmetoden, säkerhetsåtgärder, just-in-time (JIT) kompilering, övervakning, aviseringar och så vidare med hello CRP sig själv. Skaluppsättningar för den virtuella datorn är PCI Payment Card Industry ()-kompatibel eftersom hello CRP ingår för hello aktuella PCI Data Security Standard (DSS).

Mer information finns i [hello Microsoft Trust Center](https://www.microsoft.com/TrustCenter/Compliance/PCI).






## <a name="extensions"></a>Tillägg

### <a name="how-do-i-delete-a-virtual-machine-scale-set-extension"></a>Hur tar jag bort en virtuell dator skala utökas?

toodelete en virtuell dator skala Ange filnamnstillägget, Använd hello följande PowerShell-exempel:

```powershell
$vmss = Get-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" 

$vmss=Remove-AzureRmVmssExtension -VirtualMachineScaleSet $vmss -Name "extensionName"

Update-AzureRmVmss -ResourceGroupName "resource_group_name" -VMScaleSetName "vmssName" -VirtualMacineScaleSet $vmss
```
 
Du kan hitta hello Tilläggsnamn värdet i `$vmss`.
   
### <a name="is-there-a-virtual-machine-scale-set-template-example-that-integrates-with-operations-management-suite"></a>Finns det en virtuell dator skala ange mall-exempel som kan integreras med Operations Management Suite?

För en virtuell dator skala har angetts exempel som kan integreras med Operations Management Suite finns hello andra exemplet i [distribuera ett Azure Service Fabric-kluster och aktivera övervakning genom att använda Log Analytics](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/ServiceFabric).
   
### <a name="extensions-seem-toorun-in-parallel-on-virtual-machine-scale-sets-this-causes-my-custom-script-extension-toofail-what-can-i-do-toofix-this"></a>Tillägg verka toorun parallellt på virtuella datorer. Detta leder till min tillägget toofail för anpassat skript. Vad kan jag göra toofix detta?

toolearn om tillägget ordningsföljd i skalningsuppsättningar i virtuella datorer, se [tillägget ordningsföljd i skalningsuppsättningar i virtuella Azure-datorn](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).
 
 
### <a name="how-do-i-reset-hello-password-for-vms-in-my-virtual-machine-scale-set"></a>Hur Återställ hello lösenord för virtuella datorer i min skaluppsättning för virtuell dator?

tooreset hello lösenordet för virtuella datorer i din virtuella skala inställd och använda VM access-tillägg. 

Använd följande PowerShell-exempel hello:

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
 
 
### <a name="how-do-i-add-an-extension-tooall-vms-in-my-virtual-machine-scale-set"></a>Hur lägger jag till ett tillägg tooall virtuella datorer i min skaluppsättning för virtuell dator?

Om principen anges för**automatisk**, hello omdistribuera mallen med hello nya egenskaper för webbtjänsttillägg uppdaterar alla virtuella datorer.

Om principen anges för**manuell**först uppdatera hello tillägg och sedan manuellt uppdatera alla instanser i dina virtuella datorer.

  
### <a name="if-hello-extensions-associated-with-an-existing-virtual-machine-scale-set-are-updated-are-existing-vms-affected-that-is-will-hello-vms-not-match-hello-virtual-machine-scale-set-model-or-are-they-ignored-when-an-existing-machine-is-service-healed-or-reimaged-are-hello-scripts-that-are-currently-configured-on-hello-virtual-machine-scale-set-executed-or-are-hello-scripts-that-were-configured-when-hello-vm-was-first-created-used"></a>Om hello-tillägg som är associerade med en befintlig skaluppsättning för virtuell dator uppdateras befintliga virtuella datorer som påverkas? (Det vill säga kommer hello VMs *inte* matchar hello virtuella skala modellen?) Eller ignoreras de? När en befintlig dator tjänsten lagats eller avbildade, är hello-skript som är konfigurerade på hello virtuella datorns skaluppsättning körs eller är hello-skript som konfigurerades när hello VM skapades används?

Om hello resurstilläggsdefinitionen i hello virtuella skala modellen uppdateras och hello upgradePolicy egenskapen för**automatisk**, uppdaterar den hello virtuella datorer. Om hello upgradePolicy egenskapen för**manuell**, tillägg har flaggats som inte matchar hello modellen. 

Om en befintlig virtuell dator är tjänsten lagats, visas den som en omstart och hello tillägg är inte igen. Om det avbildade, är det som ersätter hello OS-enhet med hello Källavbildningen. Alla specialisering från hello senaste modell, till exempel tillägg, körs.
 
### <a name="how-do-i-join-a-virtual-machine-scale-set-tooan-azure-ad-domain"></a>Hur jag ansluta till en virtuell dator skala set tooan Azure AD-domän

toojoin en virtuell dator scale set tooan Azure Active Directory (AD Azure) domän, kan du definiera ett tillägg. 

toodefine filnamnstillägget, Använd hello JsonADDomainExtension egenskapen:

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
 
### <a name="my-virtual-machine-scale-set-extension-is-trying-tooinstall-something-that-requires-a-reboot-for-example-commandtoexecute-powershellexe--executionpolicy-unrestricted-install-windowsfeature-name-fs-resource-manager-includemanagementtools"></a>Min virtuella skala utökas försöker tooinstall något som kräver en omstart. Till exempel ”commandToExecute” ”: powershell.exe - ExecutionPolicy obegränsad Install-WindowsFeature-Name FS-Resource-Manager – IncludeManagementTools”

Om din virtuella skala utökas försöker tooinstall något som kräver omstart, kan du använda hello Azure Automation Desired State Configuration (Automation DSC)-tillägg. Om hello operativsystemet är Windows Server 2012 R2, Azure tar emot hello-inställningar för Windows Management Framework (WMF) 5.0, omstarter, och fortsätter sedan med hello konfiguration. 
 
### <a name="how-do-i-turn-on-antimalware-in-my-virtual-machine-scale-set"></a>Hur aktiverar jag program mot skadlig kod i min skaluppsättning för virtuell dator?

tooturn för program mot skadlig kod på din virtuella skala ange använder hello följande PowerShell-exempel:

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

### <a name="i-need-tooexecute-a-custom-script-thats-hosted-in-a-private-storage-account-hello-script-runs-successfully-when-hello-storage-is-public-but-when-i-try-toouse-a-shared-access-signature-sas-it-fails-this-message-is-displayed-missing-mandatory-parameters-for-valid-shared-access-signature-linksas-works-fine-from-my-local-browser"></a>Jag behöver tooexecute ett anpassat skript som finns i ett privat storage-konto. hello skriptet körs utan problem när hello lagring är offentlig, men när jag försöker toouse en delad signatur åtkomst (SAS), misslyckas. Det här meddelandet visas: ”obligatoriska parametrar saknas för giltig signatur för delad åtkomst”. Länken + SAS fungerar bra i min lokala webbläsare.

tooexecute ett anpassat skript som finns i ett privat storage-konto för att konfigurera skyddade inställningar med hello lagringskontonyckel och namn. Mer information finns i [anpassade skript tillägget för Windows](https://azure.microsoft.com/documentation/articles/virtual-machines-windows-extensions-customscript/#template-example-for-a-windows-vm-with-protected-settings).







## <a name="networking"></a>Nätverk
 
### <a name="is-it-possible-tooassign-a-network-security-group-nsg-tooa-scale-set-so-that-it-will-apply-tooall-hello-vm-nics-in-hello-set"></a>Är det möjligt tooassign en Nätverkssäkerhetsgrupp (NSG) tooa skala har angetts så att tillämpa tooall hello Virtuella nätverkskort i hello uppsättningen?

Ja. En Nätverkssäkerhetsgrupp kan tillämpas direkt tooa skala som refererar till den i hello networkInterfaceConfigurations avsnitt av hello nätverksprofil. Exempel:

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

### <a name="how-do-i-do-a-vip-swap-for-virtual-machine-scale-sets-in-hello-same-subscription-and-same-region"></a>Hur gör jag en VIP-växling för virtuella datorn anger i hello samma prenumeration och samma region?

Om du har två virtuella datorn anger med Azure belastningsutjämnare framför-servrar och de finns i Hej samma prenumeration och region, du kan frigöra hello offentliga IP-adresser från var och en och tilldela toohello andra. Se [VIP-växling: blå-grön distribution i Azure Resource Manager](https://msftstack.wordpress.com/2017/02/24/vip-swap-blue-green-deployment-in-azure-resource-manager/) t.ex. Detta innebär en fördröjning som hello resurser är frigjord/allokerade på hello nätverksnivån. Ett snabbare alternativ är toouse Azure Application Gateway med två serverdelspooler och en regel för vidarebefordran. Du kan också värd för ditt program med [Azure App service](https://azure.microsoft.com/en-us/services/app-service/) som ger stöd för snabb växling mellan olika platser för mellanlagring och produktion.
 
### <a name="how-do-i-specify-a-range-of-private-ip-addresses-toouse-for-static-private-ip-address-allocation"></a>Hur jag för att ange ett intervall med privata IP-adresser toouse för statisk privat IP-adressallokering?

IP-adresser har markerats från ett undernät som du anger. 

hello fördelning av virtuella skala uppsättning IP-adresser är alltid ”dynamic”, men det betyder att du kan ändra dessa IP-adresser. I det här fallet betyder ”dynamic” bara att du inte anger hello IP-adress i en PUT-begäran. Ange hello statiska in med hjälp av hello undernät. 
    
### <a name="how-do-i-deploy-a-virtual-machine-scale-set-tooan-existing-azure-virtual-network"></a>Hur distribuerar en virtuell dator scale set tooan befintliga virtuella Azure-nätverket? 

toodeploy en virtuell dator skala tooan befintliga Azure-nätverk finns i avsnittet [distribuera en virtuell dator scale set tooan befintligt virtuellt nätverk](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-existing-vnet). 

### <a name="how-do-i-add-hello-ip-address-of-hello-first-vm-in-a-virtual-machine-scale-set-toohello-output-of-a-template"></a>Hur lägger jag till hello IP-adress för hello första virtuella datorn i en virtuell dator skala ange toohello utdata från en mall?

tooadd hello IP-adressen för hello första virtuella datorn i en virtuell dator scale set toohello utdata från en mall finns [ARM: hämta VMSS privata IP-adresser](http://stackoverflow.com/questions/42790392/arm-get-vmsss-private-ips).

### <a name="can-i-use-scale-sets-with-accelerated-networking"></a>Kan jag använda skaluppsättningar med snabbare nätverk?

Ja. toouse snabbare nätverk, ange enableAcceleratedNetworking tootrue i din skala uppsättningens networkInterfaceConfigurations inställningar. T.ex.
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

### <a name="how-can-i-configure-hello-dns-servers-used-by-a-scale-set"></a>Hur konfigurerar jag hello DNS-servrar som används av en skaluppsättning

toocreate en skaluppsättning för virtuell dator med en anpassad DNS-konfiguration, lägga till en dnsSettings JSON paket toohello skala networkInterfaceConfigurations avsnitt. Exempel:
```json
    "dnsSettings":{
        "dnsServers":["10.0.0.6", "10.0.0.5"]
    }
```

### <a name="how-can-i-configure-a-scale-set-tooassign-a-public-ip-address-tooeach-vm"></a>Hur konfigurerar en scale set tooassign en offentlig IP-adress tooeach VM?

toocreate en skaluppsättning för virtuell dator som tilldelar en offentlig IP-adress tooeach VM, kontrollera hello API-versionen av hello Microsoft.Compute/virtualMAchineScaleSets resursen är 2017-03-30 och lägga till en _publicipaddressconfiguration_ JSON-paket toohello skaluppsättning ipConfigurations avsnitt. Exempel:

```json
    "publicipaddressconfiguration": {
        "name": "pub1",
        "properties": {
        "idleTimeoutInMinutes": 15
        }
    }
```

### <a name="can-i-configure-a-scale-set-toowork-with-multiple-application-gateways"></a>Konfigurerar en scale set toowork med flera Programgatewayer?

Ja. Du kan lägga till hello resurs-id's för flera Programgateway backend adresspooler toohello _applicationGatewayBackendAddressPools_ listan i hello _ipConfigurations_ avsnitt i din skaluppsättning nätverksprofil.

## <a name="scale"></a>Skala

### <a name="in-what-case-would-i-create-a-virtual-machine-scale-set-with-fewer-than-two-vms"></a>I vilket fall skulle jag skapa en virtuell dator-skala med färre än två virtuella datorer?

En orsak toocreate en virtuell dator skaluppsättning med färre än två virtuella datorer toouse hello elastisk egenskaperna för en virtuell dator skala anges. Du kan till exempel distribuera en skaluppsättning för virtuell dator med noll VMs toodefine infrastrukturen utan att betala VM driftskostnader. Sedan, när du är klar toodeploy VMs, öka hello ”kapacitet” hello virtual machine scale set toohello produktion instansantal.

En annan orsak kan du skapa en virtuell dator-skala med färre än två virtuella datorer är om du vill göra mindre med tillgänglighet än med en tillgänglighetsuppsättning med diskreta virtuella datorer. Skaluppsättningar för den virtuella datorn får du ett sätt toowork med odifferentierad beräknings-enheter som är fungibla. Denna enhetlighet är nyckelfaktorn för skalningsuppsättningar i virtuella datorer jämfört med tillgänglighetsuppsättningar. Många tillståndslösa arbetsbelastningar spåra inte enskilda enheter. Om hello arbetsbelastning släpper du skala ned tooone beräkning enhet och skala upp toomany när hello arbetsbelastningen ökar.

### <a name="how-do-i-change-hello-number-of-vms-in-a-virtual-machine-scale-set"></a>Hur ändrar jag hello antal virtuella datorer i en skaluppsättning för virtuell dator?

toochange hello antal virtuella datorer i en skaluppsättning för virtuell dator finns [ändra hello instanser av en virtuella datorns skaluppsättning](https://msftstack.wordpress.com/2016/05/13/change-the-instance-count-of-an-azure-vm-scale-set/).

### <a name="how-do-i-define-custom-alerts-for-when-certain-thresholds-are-reached"></a>Hur jag för att definiera anpassade aviseringar för när vissa trösklar har uppnåtts?

Har du viss flexibilitet i hur du hanterar aviseringar för angivna tröskelvärden. Du kan till exempel definiera anpassade webhooks. hello följande webhook exempel är från en Resource Manager-mall:

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

I det här exemplet skickas en avisering tooPagerduty.com när ett tröskelvärde uppnås.



## <a name="patching-and-operations"></a>Korrigering och åtgärder

### <a name="how-do-i-create-a-scale-set-in-an-existing-resource-group"></a>Hur skapar jag en skala som angetts i en befintlig resursgrupp

Skapa skalningsuppsättningar i en befintlig resurs grupp är ännu inte möjligt från hello Azure-portalen, men du kan ange en befintlig resursgrupp när distribuera en skala från en Azure Resource Manager-mall. Du kan också ange en befintlig resursgrupp när du skapar en skala som anges med Azure PowerShell eller CLI.

### <a name="can-we-move-a-scale-set-tooanother-resource-group"></a>Kan vi flytta skaluppsättning tooanother resursgrupp?

Ja, kan du flytta skala uppsättning resurser tooa ny prenumeration eller resursgrupp.

### <a name="how-tooi-update-my-virtual-machine-scale-set-tooa-new-image-how-do-i-manage-patching"></a>Hur tooI uppdatera skaluppsättning min virtuella tooa ny avbildning? Hur hanterar jag korrigering?

tooupdate din virtuella skaluppsättning tooa ny avbildning och toomanage korrigering, se [uppgradera en virtuella datorns skaluppsättning](https://docs.microsoft.com/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-upgrade-scale-set).

### <a name="can-i-use-hello-reimage-operation-tooreset-a-vm-without-changing-hello-image-that-is-i-want-reset-a-vm-toofactory-settings-rather-than-tooa-new-image"></a>Kan jag använda hello avbildningsåterställning åtgärden tooreset en virtuell dator utan att ändra hello avbildning? (Det vill säga jag vill Återställ en VM toofactory inställningar i stället för tooa ny avbildning.)

Ja, kan du använda hello avbildningsåterställning åtgärden tooreset en virtuell dator utan att ändra hello avbildningen. Men om din virtuella skalan refererar till en plattformsavbildning med `version = latest`, den virtuella datorn kan uppdatera tooa senare OS-avbildningen när du anropar `reimage`.

Mer information finns i [hantera alla virtuella datorer i en skaluppsättning för virtuell dator](https://docs.microsoft.com/rest/api/virtualmachinescalesets/manage-all-vms-in-a-set).



## <a name="troubleshooting"></a>Felsökning

### <a name="how-do-i-turn-on-boot-diagnostics"></a>Hur aktiverar jag startdiagnostikinställningar?

tooturn på startdiagnostikinställningar, först skapar ett lagringskonto. Placera sedan JSON blocket i din virtuella datorns skaluppsättning **virtualMachineProfile**, och uppdatera hello skaluppsättning för virtuell dator:

```json
"diagnosticsProfile": {
    "bootDiagnostics": {
        "enabled": true,
        "storageUri": "http://yourstorageaccount.blob.core.windows.net"
    }
}
```

När en ny virtuell dator skapas visar hello InstanceView-egenskapen för hello VM hello information för hello skärmbild och så vidare. Här är ett exempel:
 
```json
"bootDiagnostics": {
    "consoleScreenshotBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.screenshot.bmp",
    "serialConsoleLogBlobUri": "https://o0sz3nhtbmkg6geswarm5.blob.core.windows.net/bootdiagnostics-swarmagen-4157d838-8335-4f78-bf0e-b616a99bc8bd/swarm-agent-9574AE92vmss-0_2.4157d838-8335-4f78-bf0e-b616a99bc8bd.serialconsole.log"
  }
```


## <a name="virtual-machine-properties"></a>Egenskaper för virtuell dator

### <a name="how-do-i-get-property-information-for-each-vm-without-making-multiple-calls-for-example-how-would-i-get-hello-fault-domain-for-each-of-hello-100-vms-in-my-virtual-machine-scale-set"></a>Hur skaffar egenskapsinformation för varje virtuell dator utan att göra flera anrop? Till exempel hur kan jag hello feldomän för varje hello 100 virtuella datorer i min skaluppsättning för virtuell dator?

information om tooget egenskaper för varje virtuell dator utan att göra flera anrop, kan du anropa `ListVMInstanceViews` genom att göra ett REST-API `GET` på hello efter resurs-URI:

/subscriptions/ < PRENUMERATIONSID > /resourceGroups/ < resource_group_name > /providers/Microsoft.Compute/virtualMachineScaleSets/ < scaleset_name > / virtualMachines? $expand = instanceView & $select = instanceView

### <a name="can-i-pass-different-extension-arguments-toodifferent-vms-in-a-virtual-machine-scale-set"></a>Kan jag skicka annat tillägg argument toodifferent virtuella datorer i en skaluppsättning för virtuell dator?

Du kan inte Nej, Skicka annat tillägg argument toodifferent virtuella datorer i en skaluppsättning för virtuell dator. Tillägg kan dock fungera baserat på hello unika egenskaper för hello VM som körs på, så som på hello datornamnet. Tillägg kan också fråga instans metadata på http://169.254.169.254 tooget mer information om hello VM.

### <a name="why-are-there-gaps-between-my-virtual-machine-scale-set-vm-machine-names-and-vm-ids-for-example-0-1-3"></a>Varför är det glapp mellan min Virtuella datornamn på virtuell dator skala och VM-ID: N? Till exempel: 0, 1, 3...

Det förekommer glapp mellan din Virtuella datornamn på virtuell dator skala och VM-ID: N eftersom skalan för din virtuella **overprovision** toohello standardvärdet anges i egenskapen **SANT**. Om överetablering har angetts för**SANT**, flera virtuella datorer än begärt skapas. Extra VM: ar så tas bort. I det här fallet du få bättre distribution tillförlitlighet, men på bekostnad av hello sammanhängande namngivning och sammanhängande (Network Address Translation Translation) regler. 

Du kan ange egenskapen för**FALSKT**. För små virtuella skaluppsättningar påverkar detta avsevärt inte distribution tillförlitlighet.

### <a name="what-is-hello-difference-between-deleting-a-vm-in-a-virtual-machine-scale-set-and-deallocating-hello-vm-when-should-i-choose-one-over-hello-other"></a>Vad är hello skillnaden mellan att ta bort en virtuell dator i en skaluppsättning för virtuell dator och det har frigjorts hello VM? När ska jag välja en över hello andra?

hello största skillnaden mellan att ta bort en virtuell dator i en skaluppsättning för virtuell dator och det har frigjorts hello VM är att `deallocate` tas inte bort hello virtuella hårddiskar (VHD). Finns kostnader för lagring som är associerade med körs `stop deallocate`. Du kan använda en eller hello andra för en av följande orsaker hello:

- Du vill toostop betalar beräkning kostnader, men du vill tookeep hello diskstatusen av hello virtuella datorer.
- Vill du toostart en uppsättning virtuella datorer snabbare än vad du kan skala ut en skaluppsättning för virtuell dator.
  - Relaterade toothis scenario du har skapat egna Autoskala motor och vill en snabbare skala för slutpunkt till slutpunkt.
- Du har en skaluppsättning för virtuell dator som är ojämnt fördelad över feldomäner eller uppdatera domäner. Detta kan bero på att du selektivt ta bort virtuella datorer eller eftersom virtuella datorer har tagits bort efter överetablering. Kör `stop deallocate` följt av `start` på hello virtuell dator distribuerar skaluppsättningen jämnt hello virtuella datorer över feldomäner eller uppdatera domäner.

