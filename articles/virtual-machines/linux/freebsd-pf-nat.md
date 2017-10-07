---
title: "aaaUse FreeBSD paketfilter toocreate en brandvägg i Azure | Microsoft Docs"
description: "Lär dig hur toodeploy en NAT-brandväggen med hjälp av Freebsd's PF i Azure."
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/20/2017
ms.author: kyliel
ms.openlocfilehash: 3d3a5dde2ca03ba6fc384581c786f5eb746e6d92
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-freebsds-packet-filter-toocreate-a-secure-firewall-in-azure"></a>Hur toouse FreeBSD paketfilter toocreate en säker brandvägg i Azure
Den här artikeln beskriver hur toodeploy NAT-brandvägg med Freebsds förpackaren Filter via Azure Resource Manager-mall för web server ovanligt.

## <a name="what-is-pf"></a>Vad är PF?
PF (paketfilter, också skrivs pf) är ett BSD licensierade tillståndskänslig paketfilter, en central del av programvaran för firewalling. PF sedan har utvecklats snabbt och nu har flera fördelar jämfört med andra tillgängliga brandväggar. NAT (Network Address Translation) är i PF ända sedan paketschemaläggaren och aktiva köhantering har integrerats i PF, genom att integrera hello ALTQ och göra den konfigureras via PFS konfiguration. Funktioner som pfsync och KARP för växling vid fel och redundans, authpf för sessionsautentisering och ftp-proxy tooease firewalling hello svårt FTP-protokollet har också utökat PF. Kort sagt: PF är en kraftfull och funktionsrika brandvägg. 

## <a name="get-started"></a>Kom igång
Om du är intresserad av att skapa en säker brandvägg i hello molnet för webbservrar, nu sätter vi igång. Du kan också använda hello-skript som används i den här Azure Resource Manager-mall tooset upp din nätverkstopologin.
hello Azure Resource Manager-mall som ställer in en FreeBSD virtuell dator som utför NAT /redirection med PF och två FreeBSD-virtuella datorer med hello Nginx-webbservern installeras och konfigureras. Dessutom tooperforming NAT för hello två webbservrar utgående trafik, hello NAT/omdirigering av virtuell dator fångar upp HTTP-begäranden och omdirigerar dem toohello två webbservrar på resursallokering sätt. Hej VNet använder hello privata icke-dirigerbara IP-adressutrymme 10.0.0.2/24 och du kan ändra hello parametrarna för hello mallen. hello Azure Resource Manager-mall definierar också en routningstabell för hello hela VNet, som är en samling individuella vägare som används för toooverride Azure standardvägar baserat på hello mål-IP-adress. 

![pf_topology](./media/freebsd-pf-nat/pf_topology.jpg)
    
### <a name="deploy-through-azure-cli"></a>Distribuera via Azure CLI
Du behöver hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login). Skapa en resursgrupp med [az group create](/cli/azure/group#create). hello följande exempel skapas ett Resursgruppsnamn `myResourceGroup` i hello `West US` plats.

```azurecli
az group create --name myResourceGroup --location westus
```

Distribuera mallen hello [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup) med [az distribution skapa](/cli/azure/group/deployment#create). Hämta [azuredeploy.parameters.json](https://github.com/Azure/azure-quickstart-templates/blob/master/pf-freebsd-setup/azuredeploy.parameters.json) under hello samma sökväg och definiera en egen resurs-värden som `adminPassword`, `networkPrefix`, och `domainNamePrefix`. 

```azurecli
az group deployment create --resource-group myResourceGroup --name myDeploymentName \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/pf-freebsd-setup/azuredeploy.json \
    --parameters '@azuredeploy.parameters.json' --verbose
```

Efter cirka fem minuter kommer du få hello information för `"provisioningState": "Succeeded"`. Sedan kan du ssh toohello klientdel VM (NAT) eller öppna Nginx webbserver i en webbläsare som använder hello offentlig IP-adress eller fullständigt domännamn för hello klientdel VM (NAT). hello följande exempel visar FQDN och offentliga IP-adress som tilldelats toohello klientdel VM (NAT) i hello `myResourceGroup` resursgruppen. 

```azurecli
az network public-ip list --resource-group myResourceGroup
```
    
## <a name="next-steps"></a>Nästa steg
Vill du tooset upp egna NAT i Azure? Öppna datakällan, kostnadsfritt men kraftfulla? Sedan är PF ett bra alternativ. Med hjälp av mallen hello [pf-freebsd-setup](https://github.com/Azure/azure-quickstart-templates/tree/master/pf-freebsd-setup), behöver du bara fem minuter tooset upp en NAT-brandvägg med belastningsutjämning för resursallokering med Freebsd's PF i Azure för web server ovanligt. 

Om du vill toolearn hello erbjuds FreeBSD i Azure finns för[introduktion tooFreeBSD på Azure](freebsd-intro-on-azure.md).

Om du vill tooknow mer om PF finns för[FreeBSD handboken](https://www.freebsd.org/doc/handbook/firewalls-pf.html) eller [PF-Användarhandbok](https://www.freebsd.org/doc/handbook/firewalls-pf.html).
