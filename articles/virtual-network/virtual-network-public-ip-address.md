---
title: "Skapa, ändra eller ta bort en Azure offentliga IP-adress | Microsoft Docs"
description: "Lär dig mer om att skapa, ändra eller ta bort en offentlig IP-adress."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: jdial
ms.openlocfilehash: 9a51cd703266e330550a2a2f4e6a4d46722fec09
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-change-or-delete-a-public-ip-address"></a>Skapa, ändra eller ta bort en offentlig IP-adress

Lär dig mer om en offentlig IP-adress och hur du skapar, ändra och ta bort ett. En offentlig IP-adress är en resurs med en egen konfigurerbara inställningar. Tilldela en offentlig IP-adress till andra Azure-resurser kan:
- Inkommande Internet-anslutning till resurser, till exempel Azure virtuella datorer (VM), Azure Virtual Machine-Skalningsuppsättningar, Azure VPN-Gateway, programgatewayer och Internet-riktade Azure belastningsutjämnare. Azure-resurser kan inte ta emot inkommande kommunikation från Internet utan en tilldelad offentliga IP-adress. Medan vissa Azure-resurser är allmänt tillgängligt via offentliga IP-adresser, måste andra resurser ha offentliga IP-adresser som tilldelats är tillgänglig från Internet.
- Utgående anslutning till Internet via en förutsägbar IP-adress. En virtuell dator kan till exempel kommunicera utgående till Internet utan en offentlig IP-adress tilldelas, men dess adress är nätverksadress översättas av Azure till ett oförutsägbart offentlig adress. Tilldela en offentlig IP-adress till en resurs kan du vet vilken IP-adress används för utgående anslutning. Även om förutsägbar, kan adressen ändras beroende på tilldelningsmetod valt. Mer information finns i [skapa en offentlig IP-adress](#create-a-public-ip-address). Mer information om utgående anslutningar från Azure-resurser i [förstå utgående anslutningar](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.

## <a name="before-you-begin"></a>Innan du börjar

Utför följande uppgifter innan du slutför alla steg i alla avsnitt i den här artikeln:

- Granska de [Azure begränsar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikeln innehåller information om begränsningarna för offentliga IP-adresser.
- Logga in på Azure [portal](https://portal.azure.com), Azure-kommandoradsgränssnittet (CLI) eller Azure PowerShell med ett Azure-konto. Om du inte redan har ett Azure-konto, registrera dig för en [ledigt utvärderingskonto](https://azure.microsoft.com/free).
- Om du använder PowerShell-kommandon för att utföra åtgärder i den här artikeln [installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har den senaste versionen av Azure PowerShell-kommandon som är installerad. Om du vill få hjälp med PowerShell-kommandon med exempel på, skriver `get-help <command> -full`.
- Om Azure-kommandoradsgränssnittet (CLI)-kommandon för att utföra åtgärder i den här artikeln [installera och konfigurera Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har den senaste versionen av Azure CLI installerad. Om du vill få hjälp med CLI-kommandon, Skriv `az <command> --help`. Du kan använda Azure Cloud-gränssnittet i stället för att installera CLI och dess krav. Azure Cloud Shell är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i Azure-portalen. Den har Azure CLI förinstallerat och har konfigurerats för användning med ditt konto. Om du vill använda molnet Shell klickar du på molnet Shell **> _** längst upp i den [portal](https://portal.azure.com).

Offentliga IP-adresser har en nominell kostnad. Om du vill visa prissättning, läsa den [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan. 

## <a name="create-a-public-ip-address"></a>Skapa en offentlig IP-adress

1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är tilldelade (minst) behörigheter för rollen Network-deltagare för din prenumeration. Läs den [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikeln om du vill veta mer om hur du tilldelar roller och behörigheter till konton.
2. I rutan som innehåller texten *söka resurser* längst upp i Azure-portalen, Skriv *offentliga ip-adressen*. När **offentliga IP-adresser** visas i sökresultaten klickar du på den.
3. Klicka på **+ Lägg till** i den **offentliga IP-adressen** bladet som visas.
4. Ange eller Välj värden för följande inställningar i den **skapa offentlig IP-adress** bladet som visas, klicka sedan på **skapa**:

    |Inställning|Krävs?|Information|
    |---|---|---|
    |Namn|Ja|Namnet måste vara unikt inom resursgruppen som du väljer.|
    |IP-Version|Ja| Välj IPv4 eller IPv6. Medan offentliga IPv4-adresser kan tilldelas till flera Azure-resurser, kan en IPv6-offentliga IP-adress endast tilldelas en Internetriktade belastningsutjämnare. Belastningsutjämnaren kan belastningsutjämna IPv6-trafik till virtuella Azure-datorer. Lär dig mer om [IPv6-trafik till virtuella datorer för belastningsutjämning](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    |IP-adresstilldelning|Ja|**Dynamiska:** dynamiska adresser tilldelas endast när den offentliga IP-adressen är kopplad till ett nätverkskort anslutet till en virtuell dator och den virtuella datorn startas för första gången. Dynamiska adresser kan ändras om den virtuella dator som det virtuella nätverkskortet är kopplat till stoppas (frigörs). Adressen förblir detsamma om den virtuella datorn har startats om eller stoppats (men inte frigöra). **Statiskt:** statiska adresser tilldelas när den offentliga IP-adressen har skapats. Statiska adresser ändras inte även om den virtuella datorn stoppas (frigörs). Adressen frigörs endast om nätverkskortet tas bort. Du kan ändra tilldelningsmetoden när nätverkskortet har skapats. Om du väljer IPv6 för den **IP version**, är den enda tillgängliga tilldelningsmetod **dynamiska**.|
    |Tidsgränsen för inaktivitet (minuter)|Nej|Hur många minuter att hålla en TCP eller HTTP-anslutning öppen utan klienter skickar keep-alive-meddelanden. Om du väljer IPv6 för **IP Version**, det här värdet kan inte ändras. |
    |DNS-namnetikett|Nej|Måste vara unika inom den Azure-plats som du skapar namnet i (över alla prenumerationer och alla kunder). Offentliga DNS-tjänsten för Azure registrerar automatiskt namn och IP-adress så att du kan ansluta till en resurs med namnet. Lägger till Azure *location.cloudapp.azure.com* (där platsen är platsen du väljer) med namnet du anger för att skapa det fullständigt kvalificerade DNS-namnet. Om du väljer att skapa båda versionerna av adress tilldelas samma DNS-namn till både IPv4 och IPv6-adresser. Azure DNS-tjänsten innehåller poster för både IPv4-A- och IPv6-AAAA namn och svarar med båda posterna när DNS-namnet slås upp. Klienten väljer vilken adress (IPv4 eller IPv6) ska kunna kommunicera med.|
    |Skapa en IPv6 (eller IPv4) adress|Nej| Om IPv6 och IPv4 visas beror på vad du väljer för **IP Version**. Om du väljer exempelvis **IPv4** för **IP Version**, **IPv6** visas här.
    |Namnet (endast synliga när du har markerat den **skapar en IPv6 (eller IPv4) adress** kryssrutan)|Ja, om du väljer den **skapa en IPv6** (eller IPv4) kryssrutan.|Namnet måste vara ett annat än det namn du anger för först **namn** i den här listan. Om du väljer att skapa både en IPv4 och IPv6-adress, skapar portalen två separata offentliga IP-adress resurserna, en för varje IP-adress-version som tilldelats.|
    |IP-adresstilldelning (endast synliga när du har markerat den **skapar en IPv6 (eller IPv4) adress** kryssrutan)|Ja, om du väljer den **skapa en IPv6** (eller IPv4) kryssrutan.|Om kryssrutan säger **skapar en IPv4-adress**, kan du välja en tilldelningsmetod. Om kryssrutan säger **skapar en IPv6-adress**, du kan inte välja en tilldelningsmetod eftersom det måste vara **dynamiska**.|
    |Prenumeration|Ja|Det måste finnas i samma [prenumeration](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) som resursen som du vill associera offentliga IP-adressen.|
    |Resursgrupp|Ja|Det kan finnas i samma eller olika, [resursgruppen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) som resursen som du vill associera offentliga IP-adressen.|
    |Plats|Ja|Det måste finnas i samma [plats](https://azure.microsoft.com/regions), även avses som region som resursen som du vill associera offentlig IP adress till.|

**Kommandon**

Även om portalen ger möjlighet att skapa två offentliga IP-adressresurser (en IPv4- och en IPv6), skapa en resurs med en adress för en IP-version eller den andra följande CLI och PowerShell-kommandon. Om du vill att två offentliga IP-adressresurser, ett för varje IP-version kör du kommandot två gånger för att ange olika namn och versioner för de offentliga IP-adress-resurserna. 

|Verktyget|Kommando|
|---|---|
|CLI|[Skapa AZ nätverket offentliga-ip](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)|

## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>Visa, ändra inställningar för eller ta bort en offentlig IP-adress

1. Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är tilldelade (minst) behörigheter för rollen Network-deltagare för din prenumeration. Läs den [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikeln om du vill veta mer om hur du tilldelar roller och behörigheter till konton.
2. I rutan som innehåller texten *söka resurser* längst upp i Azure-portalen, Skriv *offentliga ip-adressen*. När **offentliga IP-adresser** visas i sökresultaten klickar du på den.
3. I den **offentliga IP-adresser** bladet som visas, klicka på namnet på den offentliga IP-adressen som du vill visa, ändra inställningar för eller ta bort.
4. Gör något av följande alternativ beroende på om du vill visa, ta bort eller ändra den offentliga IP-adressen i bladet som visas för den offentliga IP-adressen.
    - **Visa**: den **översikt** på bladet visar inställningar för offentliga IP-adress, till exempel nätverksgränssnitt som den är kopplad till (om adressen är kopplad till ett nätverksgränssnitt). Portalen visar inte versionen av adress (IPv4 eller IPv6). Om du vill visa versionsinformationen kommandot PowerShell eller CLI för att visa den offentliga IP-adressen. Om IP-adressversion är IPv6, visas inte den tilldelade adressen av portalen, PowerShell eller CLI. 
    - **Ta bort**: ta bort den offentliga IP-adressen, klicka på **ta bort** i den **översikt** på bladet. Om adressen är för närvarande är kopplad till en IP-konfiguration kan inte tas bort. Om adressen är för närvarande associerad med en konfiguration, klickar du på **ta bort kopplingen** att koppla bort adress från IP-konfigurationen.
    - **Ändra**: Klicka på **Configuration**. Ändra inställningar med hjälp av informationen i steg 4 i den [skapa en offentlig IP-adress](#create-a-public-ip-address) i den här artikeln. Om du vill ändra tilldelningen för en IPv4-adress från statisk till dynamisk, måste du först koppla bort offentliga IPv4-adress från IP-konfiguration som den är kopplad till. Du kan ändra metoden tilldelning till en dynamisk och klicka på **associera** för att associera IP-adress till samma IP-konfiguration, en annan konfiguration eller lämna den kopplats bort. Koppla bort en offentlig IP-adress i den **översikt** klickar du på **ta bort kopplingen**.

>[!WARNING]
>När du ändrar metoden tilldelning från statisk till dynamisk förlorar du IP-adressen som har tilldelats till den offentliga IP-adressen. Medan Azure offentliga DNS-servrar som underhåller mappningen mellan statisk eller dynamisk adresser och eventuella DNS-namnetikett (om du har definierat ett), kan en dynamisk IP-adress ändra när den virtuella datorn startas efter att ha varit i tillståndet Stoppad (frigjord). Tilldela en statisk IP-adress för att förhindra att adressen ändras.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|CLI|[AZ nätverk offentliga IP-adresser](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#list) lista offentliga IP-adresser, [az nätverk offentlig ip visa](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#show) att visa inställningar. [az nätverket offentliga ip-uppdatering](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#update) att uppdatera; [az nätverket offentliga IP-ta bort](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#delete) att ta bort|
|PowerShell|[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) att hämta en offentlig IP-adress-objekt och visa dess inställningar [Set AzureRmPublicIpAddress](/powershell/resourcemanager/azurerm.network/set-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) att uppdatera inställningarna för; [Ta bort AzureRmPublicIpAddress](/powershell/module/azurerm.network/remove-azurermpublicipaddress) att ta bort|

## <a name="next-steps"></a>Nästa steg
Tilldela offentliga IP-adresser när du skapar följande Azure-resurser:

- [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eller [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuella datorer
- [Internet-riktade Azure belastningsutjämnare](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Programgateway](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Plats-till-plats-anslutning med en Azure VPN-Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Skaluppsättning för virtuell Azure-dator](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
