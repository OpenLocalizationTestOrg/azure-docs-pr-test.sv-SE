---
title: "aaaCreate, ändra eller ta bort en Azure offentliga IP-adress | Microsoft Docs"
description: "Lär dig hur toocreate, ändra eller ta bort en offentlig IP-adress."
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
ms.openlocfilehash: 0f2578e8661c8f33419b896debcfa0ba1b41fc5c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-change-or-delete-a-public-ip-address"></a>Skapa, ändra eller ta bort en offentlig IP-adress

Läs mer om en offentlig IP-adress och hur toocreate, ändra och ta bort ett. En offentlig IP-adress är en resurs med en egen konfigurerbara inställningar. Tilldela en offentlig IP-adress tooother Azure-resurser kan:
- Inkommande tooresources för Internet-anslutning, till exempel Azure virtuella datorer (VM), Azure Virtual Machine-Skalningsuppsättningar, Azure VPN-Gateway, programgatewayer och Internet-riktade Azure belastningsutjämnare. Azure-resurser kan inte ta emot inkommande kommunikation från hello Internet utan en tilldelad offentliga IP-adress. Medan vissa Azure-resurser är allmänt tillgängligt via offentliga IP-adresser, måste andra resurser ha den offentliga IP-adresser tilldelas toothem toobe tillgänglig från hello Internet.
- Utgående anslutning toohello Internet med en förutsägbar IP-adress. Till exempel en virtuell dator kan kommunicera utgående toohello Internet utan en offentlig IP-adress som tilldelats tooit, men dess adress är nätverksadress översättas av Azure tooan oförutsägbart offentlig adress. Tilldela en offentlig IP-adress tooa resurs kan du tooknow vilken IP-adress används för hello utgående anslutning. Även om förutsägbar, kan hello-adress ändras beroende på hello tilldelningsmetod valt. Mer information finns i [skapa en offentlig IP-adress](#create-a-public-ip-address). Mer om utgående anslutningar från Azure-resurser, läsa hello toolearn [förstå utgående anslutningar](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.

## <a name="before-you-begin"></a>Innan du börjar

Fullständig hello följande uppgifter innan du slutför alla steg i alla avsnitt i den här artikeln:

- Granska hello [Azure begränsar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikel toolearn om gränser för offentliga IP-adresser.
- Logga in toohello Azure [portal](https://portal.azure.com), Azure-kommandoradsgränssnittet (CLI) eller Azure PowerShell med ett Azure-konto. Om du inte redan har ett Azure-konto, registrera dig för en [ledigt utvärderingskonto](https://azure.microsoft.com/free).
- Om du använder PowerShell-kommandon toocomplete uppgifter i den här artikeln [installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har hello senaste versionen av hello Azure PowerShell-kommandon som installeras. tooget hjälp för PowerShell-kommandon med exempel på, skriver `get-help <command> -full`.
- Om du använder Azure-kommandoradsgränssnittet (CLI) kommandon toocomplete uppgifter i den här artikeln [installera och konfigurera hello Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har hello senaste versionen av hello Azure CLI är installerad. tooget hjälp för CLI-kommandona skriver `az <command> --help`. Du kan använda hello Azure Cloud-gränssnittet i stället för att installera hello CLI och dess krav. hello Azure Cloud-gränssnittet är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i hello Azure-portalen. Den har hello Azure CLI förinstallerat och konfigurerats toouse med ditt konto. toouse hello molnet Shell, klicka på hello molnet Shell **> _** knappen hello överst i hello [portal](https://portal.azure.com).

Offentliga IP-adresser har en nominell kostnad. tooview hello prissättning, läsa hello [IP-adress priser](https://azure.microsoft.com/pricing/details/ip-addresses) sidan. 

## <a name="create-a-public-ip-address"></a>Skapa en offentlig IP-adress

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är tilldelade (minst) behörigheter för hello nätverket deltagarrollen för din prenumeration. Läs hello [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel toolearn mer om hur du tilldelar roller och behörigheter tooaccounts.
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *offentliga ip-adressen*. När **offentliga IP-adresser** visas i sökresultaten hello klickar du på den.
3. Klicka på **+ Lägg till** i hello **offentliga IP-adressen** bladet som visas.
4. Ange eller Välj värden för följande inställningar i hello hello **skapa offentlig IP-adress** bladet som visas, klicka sedan på **skapa**:

    |Inställning|Krävs?|Information|
    |---|---|---|
    |Namn|Ja|hello-namnet måste vara unikt inom hello resursgrupp som du väljer.|
    |IP-Version|Ja| Välj IPv4 eller IPv6. Medan offentliga IPv4-adresserna kan vara tilldelad tooseveral Azure-resurser, kan en IPv6-offentliga IP-adress endast tilldelas tooan Internetriktade belastningsutjämnare. hello belastningsutjämnare kan läsa in belastningsutjämna IPv6-trafik tooAzure virtuella datorer. Lär dig mer om [IPv6-trafik toovirtual datorer för belastningsutjämning](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
    |IP-adresstilldelning|Ja|**Dynamiska:** dynamiska adresser tilldelas endast när hello offentliga IP-adressen är kopplad tooa NIC kopplade tooa VM och hello VM startas för hello första gången. Dynamiska adresser kan ändras om hello VM hello nätverkskort är anslutna toois Stoppad (frigjord). hello adressen förblir hello samma om hello VM har startats om eller stoppats (men inte frigöra). **Statiskt:** statiska adresser tilldelas när hello offentlig IP-adress har skapats. Statiska adresser gör inte ändra även om hello VM placeras i hello Stoppad (frigjord) tillstånd. hello adress släpps endast när hello NIC tas bort. Du kan ändra hello tilldelningsmetod efter hello NIC skapas. Om du väljer IPv6 för hello **IP version**, hello endast tillgängligt tilldelningsmetod är **dynamiska**.|
    |Tidsgränsen för inaktivitet (minuter)|Nej|Hur många minuter tookeep en TCP eller HTTP-anslutning öppen utan klienter toosend keep-alive-meddelanden. Om du väljer IPv6 för **IP Version**, det här värdet kan inte ändras. |
    |DNS-namnetikett|Nej|Måste vara unika inom hello Azure-plats som du skapar hello namn i (över alla prenumerationer och alla kunder). hello Azure offentliga DNS-tjänsten registrerar automatiskt hello namn och IP-adress så att du kan ansluta tooa resursen med hello namn. Lägger till Azure *location.cloudapp.azure.com* (där platsen är hello platsen du väljer) toohello namnet du anger toocreate hello fullständigt kvalificerat DNS-namn. Om du väljer toocreate båda versionerna av adress är hello samma DNS-namn tilldelad tooboth hello IPv4 och IPv6-adresser. hello Azure DNS-tjänsten innehåller poster för både IPv4-A- och IPv6-AAAA namn och svarar med båda posterna när hello DNS-namnet slås upp. hello klienten väljer vilka adress (IPv4 eller IPv6) toocommunicate med.|
    |Skapa en IPv6 (eller IPv4) adress|Nej| Om IPv6 och IPv4 visas beror på vad du väljer för **IP Version**. Om du väljer exempelvis **IPv4** för **IP Version**, **IPv6** visas här.
    |Namnet (endast synliga när du har markerat hello **skapar en IPv6 (eller IPv4) adress** kryssrutan)|Ja, om du väljer hello **skapa en IPv6** (eller IPv4) kryssrutan.|hello namn måste vara ett annat än hello-namn som du anger för hello först **namn** i den här listan. Om du väljer toocreate både en IPv4 och IPv6-adress, skapar två separata offentliga IP-adress resurserna, en för varje IP-adressversion tilldelad tooit hello-portalen.|
    |IP-adresstilldelning (endast synliga när du har markerat hello **skapar en IPv6 (eller IPv4) adress** kryssrutan)|Ja, om du väljer hello **skapa en IPv6** (eller IPv4) kryssrutan.|Om kryssrutan hello säger **skapar en IPv4-adress**, kan du välja en tilldelningsmetod. Om kryssrutan hello säger **skapar en IPv6-adress**, du kan inte välja en tilldelningsmetod eftersom det måste vara **dynamiska**.|
    |Prenumeration|Ja|Det måste finnas i hello samma [prenumeration](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) som hello resurs du vill tooassociate hello offentliga IP-adress.|
    |Resursgrupp|Ja|Det kan finnas i hello samma eller olika, [resursgruppen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) som hello resurs du vill tooassociate hello offentliga IP-adress.|
    |Plats|Ja|Det måste finnas i hello samma [plats](https://azure.microsoft.com/regions), kallas även tooas region hello-resurs som du vill tooassociate hello offentliga IP-adress.|

**Kommandon**

Även om hello portal innehåller hello alternativet toocreate två offentliga IP-adress-resurser (en IPv4- och en IPv6), skapa hello följande CLI PowerShell kommandona och en resurs med en adress för en IP-version eller hello andra. Om du vill att två offentliga IP-adressresurser, ett för varje IP-version måste du köra kommandot hello två gånger, ange olika namn och versioner för hello offentliga IP-adress-resurser. 

|Verktyget|Kommando|
|---|---|
|CLI|[Skapa AZ nätverket offentliga-ip](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Ny AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)|

## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>Visa, ändra inställningar för eller ta bort en offentlig IP-adress

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är tilldelade (minst) behörigheter för hello nätverket deltagarrollen för din prenumeration. Läs hello [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel toolearn mer om hur du tilldelar roller och behörigheter tooaccounts.
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *offentliga ip-adressen*. När **offentliga IP-adresser** visas i sökresultaten hello klickar du på den.
3. I hello **offentliga IP-adresser** bladet som visas, klicka på hello offentlig IP-adress du vill tooview, ändra inställningar för eller ta bort hello namn.
4. Ta bort eller ändra hello offentliga IP-adressen i hello bladet som visas för hello offentlig IP-adress, någon av följande alternativ beroende på om du vill att tooview, hello.
    - **Visa**: hello **översikt** avsnitt av hello bladet visar viktiga inställningar för hello offentlig IP-adress, till exempel hello nätverksgränssnitt det associeras för (om hello adress är associerade tooa nätverksgränssnittet). hello portal visas inte hello version av hello-adress (IPv4 eller IPv6). tooview hello versionsinformation, Använd hello PowerShell eller CLI kommandot tooview hello offentlig IP-adress. Om hello IP-adressversion är IPv6, visas inte hello som tilldelats adress av hello-portalen, PowerShell eller hello CLI. 
    - **Ta bort**: toodelete hello offentlig IP-adress, klickar du på **ta bort** i hello **översikt** avsnitt av hello-bladet. Om hello-adressen är för närvarande associerad tooan IP-konfiguration, kan inte tas bort. Om hello-adressen är för närvarande associerad med en konfiguration, klickar du på **ta bort kopplingen** toodissociate hello-adress från hello IP-konfiguration.
    - **Ändra**: Klicka på **Configuration**. Ändra inställningar med hjälp av hello information i steg 4 i hello [skapa en offentlig IP-adress](#create-a-public-ip-address) i den här artikeln. toochange hello tilldelningen för en IPv4-adress från statisk toodynamic, du måste först koppla bort hello offentliga IPv4-adress från hello IP-konfiguration som den är kopplad till. Du kan ändra hello tilldelning metoden toodynamic och klicka på **associera** tooassociate hello IP-adress toohello samma IP-konfiguration, en annan konfiguration eller du kan lämna den kopplats bort. toodissociate en offentlig IP-adress i hello **översikt** klickar du på **ta bort kopplingen**.

>[!WARNING]
>När du ändrar hello tilldelningsmetod från statisk toodynamic förlorar du hello IP-adress som har tilldelats toohello offentlig IP-adress. Medan hello Azure offentliga DNS-servrar underhåller mappningen mellan statisk eller dynamisk adresser och eventuella DNS-namnetikett (om du har definierat ett), kan en dynamisk IP-adress ändra när hello VM startas efter att ha i hello stoppats (frigjorts) tillstånd. tooprevent hello adressen ändrar, tilldela en statisk IP-adress.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|CLI|[AZ nätverk offentliga IP-adresser](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#list) toolist offentliga IP-adresser, [az nätverk offentlig ip visa](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#show) tooshow inställningar. [az nätverket offentliga ip-uppdatering](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#update) tooupdate; [az nätverket offentliga IP-ta bort](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#delete) toodelete|
|PowerShell|[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) tooretrieve en offentlig IP adress objekt och visa dess inställningar [Set AzureRmPublicIpAddress](/powershell/resourcemanager/azurerm.network/set-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) tooupdate inställningar. [Ta bort AzureRmPublicIpAddress](/powershell/module/azurerm.network/remove-azurermpublicipaddress) toodelete|

## <a name="next-steps"></a>Nästa steg
Tilldela offentliga IP-adresser när du skapar hello följande Azure-resurser:

- [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eller [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuella datorer
- [Internet-riktade Azure belastningsutjämnare](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Programgateway](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Plats-till-plats-anslutning med en Azure VPN-Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Skaluppsättning för virtuell Azure-dator](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
