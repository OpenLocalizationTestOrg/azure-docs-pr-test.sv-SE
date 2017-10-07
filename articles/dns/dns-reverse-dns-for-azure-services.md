---
title: "aaaReverse DNS för Azure-tjänster | Microsoft Docs"
description: "Lär dig hur tooconfigure omvänd DNS-sökning för tjänster i Azure"
services: dns
documentationcenter: na
author: jtuliani
manager: timlt
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/29/2017
ms.author: jonatul
ms.openlocfilehash: c6fe1d80232f124be86dd7fc57abc20699be7eba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-reverse-dns-for-services-hosted-in-azure"></a>Konfigurera omvänd DNS för tjänster i Azure

Den här artikeln förklarar hur tooconfigure omvänd DNS-sökning för tjänster i Azure.

Tjänster i Azure använda IP-adresser tilldelas av Azure och som ägs av Microsoft. Dessa omvänd DNS-poster (PTR-poster) måste skapas i hello motsvarande ägs av Microsoft omvänd DNS-zoner för sökning. Den här artikeln förklarar hur toodo detta.

Det här scenariot ska inte förväxlas med hello möjligheten för[värd hello omvänd DNS-sökningszoner för din tilldelade IP-adressintervall i Azure DNS](dns-reverse-dns-hosting.md). I det här fallet måste hello IP-adressintervall som representeras av hello zon för omvänd sökning tilldelas tooyour organisation, vanligtvis av din Internetleverantör.

Innan du läser den här artikeln bör du vara bekant med den här [översikt över omvänd DNS- och support i Azure](dns-reverse-dns-overview.md).

Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).
* I hello Resource Manager-distributionsmodellen exponeras beräkningsresurser (till exempel virtuella datorer, virtuella datorer eller Service Fabric-kluster) via en PublicIpAddress-resurs. Omvänd DNS-sökning konfigureras med hello 'ReverseFqdn'-egenskapen för hello PublicIpAddress.
* I hello klassiska distributionsmodellen exponeras beräkningsresurser med molntjänster. Omvänd DNS-sökning konfigureras med hello 'ReverseDnsFqdn'-egenskapen för hello tjänst i molnet.

Omvänd DNS stöds inte för hello Azure App Service.

## <a name="validation-of-reverse-dns-records"></a>Validering av omvänd DNS-poster

En tredje part får inte vara kan toocreate omvänd DNS-poster för sina Azure-tjänsten mappning tooyour DNS-domäner. tooprevent, Azure kan bara hello skapandet av en omvänd DNS-post där domännamnet som anges i hello omvänd DNS-posten är hello samma som eller matchar, hello DNS-namn eller IP-adressen för en PublicIpAddress eller moln-tjänsten i hello samma Azure-prenumeration.

Verifieringen utförs endast när hello omvänd DNS-post anges eller ändras. Periodiska ny verifiering utförs inte.

Exempel: Anta att hello PublicIpAddress resurs har contosoapp1.northus.cloudapp.azure.com för hello DNS-namn och IP-adress 23.96.52.53. Hej ReverseFqdn för hello PublicIpAddress kan anges som:
* hello DNS-namn för hello PublicIpAddress, contosoapp1.northus.cloudapp.azure.com
* hello DNS-namn för en annan PublicIpAddress i hello samma prenumeration, till exempel contosoapp2.westus.cloudapp.azure.com
* En alternativa DNS-namn, till exempel app1.contoso.com, så länge det här namnet är *första* som konfigurerats som en CNAME-toocontosoapp1.northus.cloudapp.azure.com eller tooa olika PublicIpAddress i hello samma prenumeration.
* En alternativa DNS-namn, till exempel app1.contoso.com, så länge det här namnet är *första* som konfigurerats som en A-poster toohello IP-adress 23.96.52.53 eller toohello IP-adressen för en annan PublicIpAddress i hello samma prenumeration.

hello samma begränsningar gäller tooreverse DNS för molntjänster.


## <a name="reverse-dns-for-publicipaddress-resources"></a>Omvänd DNS för PublicIpAddress-resurser

Det här avsnittet innehåller detaljerade anvisningar om hur tooconfigure omvänd DNS för PublicIpAddress resurser i hello Resource Manager-distributionsmodellen, med hjälp av Azure PowerShell, Azure CLI 1.0 eller 2.0 för Azure CLI. Konfigurera omvänd DNS för PublicIpAddress resurser stöds inte för närvarande via hello Azure-portalen.

Azure för närvarande har stöd för omvänd DNS endast för IPv4 PublicIpAddress resurser. Det finns inte stöd för IPv6.

### <a name="add-reverse-dns-tooan-existing-publicipaddresses"></a>Lägg till omvänd DNS-tooan befintliga PublicIpAddresses

#### <a name="powershell"></a>PowerShell

tooadd omvänd DNS-tooan befintliga PublicIpAddress:

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

tooadd omvänd DNS-tooan befintliga PublicIpAddress som inte redan har ett DNS-namn, måste du också ange ett DNS-namn:

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings = New-Object -TypeName "Microsoft.Azure.Commands.Network.Models.PSPublicIpAddressDnsSettings"
$pip.DnsSettings.DomainNameLabel = "contosoapp1"
$pip.DnsSettings.ReverseFqdn = "contosoapp1.westus.cloudapp.azure.com."
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

tooadd omvänd DNS-tooan befintliga PublicIpAddress:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -f contosoapp1.westus.cloudapp.azure.com.
```

tooadd omvänd DNS-tooan befintliga PublicIpAddress som inte redan har ett DNS-namn, måste du också ange ett DNS-namn:

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup -d contosoapp1 -f contosoapp1.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

tooadd omvänd DNS-tooan befintliga PublicIpAddress:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com.
```

tooadd omvänd DNS-tooan befintliga PublicIpAddress som inte redan har ett DNS-namn, måste du också ange ett DNS-namn:

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn contosoapp1.westus.cloudapp.azure.com --dns-name contosoapp1
```

### <a name="create-a-public-ip-address-with-reverse-dns"></a>Skapa en offentlig IP-adress med omvänd DNS

toocreate en ny PublicIpAddress hello omvänd DNS-egenskapen har redan angetts:

#### <a name="powershell"></a>PowerShell

```powershell
New-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup" -Location "WestUS" -AllocationMethod Dynamic -DomainNameLabel "contosoapp2" -ReverseFqdn "contosoapp2.westus.cloudapp.azure.com."
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network public-ip create -n PublicIp -g MyResourceGroup -l westus -d contosoapp3 -f contosoapp3.westus.cloudapp.azure.com.
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip create --name PublicIp --resource-group MyResourceGroup --location westcentralus --dns-name contosoapp1 --reverse-fqdn contosoapp1.westcentralus.cloudapp.azure.com
```

### <a name="view-reverse-dns-for-an-existing-publicipaddress"></a>Visa omvända DNS för en befintlig PublicIpAddress

tooview hello konfigurerat värde för en befintlig PublicIpAddress:

#### <a name="powershell"></a>PowerShell

```powershell
Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network public-ip show -n PublicIp -g MyResourceGroup
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip show --name PublicIp --resource-group MyResourceGroup
```

### <a name="remove-reverse-dns-from-existing-public-ip-addresses"></a>Ta bort omvänd DNS från befintliga offentliga IP-adresser

tooremove en omvänd DNS-egenskap från en befintlig PublicIpAddress:

#### <a name="powershell"></a>PowerShell

```powershell
$pip = Get-AzureRmPublicIpAddress -Name "PublicIp" -ResourceGroupName "MyResourceGroup"
$pip.DnsSettings.ReverseFqdn = ""
Set-AzureRmPublicIpAddress -PublicIpAddress $pip
```

#### <a name="azure-cli-10"></a>Azure CLI 1.0

```azurecli
azure network public-ip set -n PublicIp -g MyResourceGroup –f ""
```

#### <a name="azure-cli-20"></a>Azure CLI 2.0

```azurecli
az network public-ip update --resource-group MyResourceGroup --name PublicIp --reverse-fqdn ""
```


## <a name="configure-reverse-dns-for-cloud-services"></a>Konfigurera omvänd DNS för molntjänster

Det här avsnittet innehåller detaljerade anvisningar om hur tooconfigure omvänd DNS för molntjänster i hello klassiska distributionsmodellen, med hjälp av Azure PowerShell. Konfigurera omvänd DNS för molntjänster stöds inte via hello Azure-portalen, Azure CLI 1.0 eller 2.0 för Azure CLI.

### <a name="add-reverse-dns-tooexisting-cloud-services"></a>Lägg till omvänd DNS-tooexisting molntjänster

tooadd en omvänd DNS registrera tooan befintlig tjänst i molnet:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="create-a-cloud-service-with-reverse-dns"></a>Skapa en molntjänst med omvänd DNS

toocreate en ny molntjänst hello omvänd DNS-egenskapen har redan angetts:

```powershell
New-AzureService –ServiceName "contosoapp1" –Location "West US" –Description "App1 with Reverse DNS" –ReverseDnsFqdn "contosoapp1.cloudapp.net."
```

### <a name="view-reverse-dns-for-existing-cloud-services"></a>Visa omvända DNS för befintliga molntjänster

tooview hello omvänd DNS-egenskapen för en befintlig molntjänst:

```powershell
Get-AzureService "contosoapp1"
```

### <a name="remove-reverse-dns-from-existing-cloud-services"></a>Ta bort omvänd DNS från befintliga molntjänster

tooremove en omvänd DNS-egenskap från en befintlig molntjänst:

```powershell
Set-AzureService –ServiceName "contosoapp1" –Description "App1 with Reverse DNS" –ReverseDnsFqdn ""
```

## <a name="faq"></a>VANLIGA FRÅGOR OCH SVAR

### <a name="how-much-do-reverse-dns-records-cost"></a>Hur mycket omvänd DNS-poster kostnaden?

De är kostnadsfritt!  Det finns utan extra kostnad för omvänd DNS-poster eller frågor.

### <a name="will-my-reverse-dns-records-resolve-from-hello-internet"></a>Kommer min omvänd DNS-poster Lös från hello internet?

Ja. När du har angett hello omvänd DNS-egenskapen för din Azure-tjänst Azure hanterar alla hello DNS-delegeringar och DNS-zoner krävs tooensure som omvänd DNS-post matchas för alla användare på Internet.

### <a name="are-default-reverse-dns-records-created-for-my-azure-services"></a>Skapas standard omvänd DNS-poster för min Azure-tjänster?

Nej. Omvänd DNS är en opt-funktion. Ingen standard omvänd DNS-poster skapas om du väljer att inte tooconfigure dem.

### <a name="what-is-hello-format-for-hello-fully-qualified-domain-name-fqdn"></a>Vad är hello format för hello fullständigt kvalificerat domännamn (FQDN)?

FQDN anges i vanlig ordning och måste avslutas med en punkt (till exempel ”app1.contoso.com”.).

### <a name="what-happens-if-hello-validation-check-for-hello-reverse-dns-ive-specified-fails"></a>Vad händer om hello valideringskontrollen för hello omvänd DNS som jag har angett misslyckas?

Om hello omvänd DNS-verifieringen misslyckas, misslyckas hello åtgärden tooconfigure hello omvänd DNS-post. Korrigera hello omvänd DNS-värde som krävs och försök igen.

### <a name="can-i-configure-reverse-dns-for-azure-app-service"></a>Kan jag konfigurera omvänd DNS för Azure App Service?

Nej. Omvänd DNS stöds inte för hello Azure App Service.

### <a name="can-i-configure-multiple-reverse-dns-records-for-my-azure-service"></a>Kan jag konfigurera flera omvänd DNS-poster för min Azure-tjänsten?

Nej. Azure stöder en omvänd DNS-post för varje Azure-molntjänst eller en PublicIpAddress.

### <a name="can-i-configure-reverse-dns-for-ipv6-publicipaddress-resources"></a>Kan jag konfigurera omvänd DNS för IPv6-PublicIpAddress resurser?

Nej. Azure för närvarande har stöd för omvänd DNS endast för IPv4 PublicIpAddress resurser och molntjänster.

### <a name="can-i-send-emails-tooexternal-domains-from-my-azure-compute-services"></a>Kan jag skicka e-postmeddelanden tooexternal domäner från min Azure Compute-tjänster?

Nej. [Skicka e-postmeddelanden tooexternal domäner har inte stöd för Azure Compute-tjänster](https://blogs.msdn.microsoft.com/mast/2016/04/04/sending-e-mail-from-azure-compute-resource-to-external-domains/)

## <a name="next-steps"></a>Nästa steg

Läs mer om omvänd DNS [omvänd DNS-sökning på Wikipedia](http://en.wikipedia.org/wiki/Reverse_DNS_lookup).
<br>
Lär dig hur för[värden hello zon för omvänd sökning för din ISP-tilldelad IP-adressintervall i Azure DNS](dns-reverse-dns-for-azure-services.md).

