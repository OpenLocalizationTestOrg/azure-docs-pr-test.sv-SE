---
title: "aaaCreate, ändra eller ta bort en Azure nätverksgränssnittet | Microsoft Docs"
description: "Lär dig vad en nätverksgränssnittet är och hur toocreate, ändra inställningar för och ta bort ett."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/24/2017
ms.author: jdial
ms.openlocfilehash: dcb6cdbd73bc0d0ca4efb9d972d370cf2d54abb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-change-or-delete-a-network-interface"></a>Skapa, ändra eller ta bort ett nätverksgränssnitt

Lär dig hur toocreate, ändra inställningar för och ta bort ett nätverksgränssnitt. Ett nätverksgränssnitt kan en virtuell dator i Azure-toocommunicate med Internet, Azure och lokala resurser. När du skapar en virtuell dator med hello Azure-portalen skapar hello portal ett nätverksgränssnitt med standardinställningar för dig. Du kan i stället väljer toocreate nätverksgränssnitt med anpassade inställningar och Lägg till en eller flera nätverk gränssnitt tooa virtuell dator när du skapar den. Du kan också toochange standard gränssnitt för nätverksinställningar för en befintlig nätverksgränssnitt. Den här artikeln förklarar hur toocreate ett nätverksgränssnitt med anpassade inställningar, ändra befintliga inställningar, till exempel tilldelning av nätverket filter (nätverkssäkerhetsgrupp), undernättilldelning, DNS-serverinställningarna och IP-vidarebefordring och ta bort ett nätverksgränssnitt.

Om du behöver tooadd, ändra eller ta bort IP-adresser för ett nätverksgränssnitt läsa hello [hantera IP-adresser](virtual-network-network-interface-addresses.md) artikel. Om du behöver tooadd nätverksgränssnitt till eller ta bort nätverksgränssnitt från virtuella datorer, läsa hello [Lägg till eller ta bort nätverksgränssnitt](virtual-network-network-interface-vm.md) artikel.


## <a name="before-you-begin"></a>Innan du börjar

Fullständig hello följande uppgifter innan du slutför alla steg i alla avsnitt i den här artikeln:

- Granska hello [Azure begränsar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikel toolearn om gränser för nätverksgränssnitt.
- Logga in toohello Azure [portal](https://portal.azure.com), Azure-kommandoradsgränssnittet (CLI) eller Azure PowerShell med ett Azure-konto. Om du inte redan har ett Azure-konto, registrera dig för en [ledigt utvärderingskonto](https://azure.microsoft.com/free).
- Om du använder PowerShell-kommandon toocomplete uppgifter i den här artikeln [installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har hello senaste versionen av hello Azure PowerShell-kommandon som installeras. tooget hjälp för PowerShell-kommandon med exempel på, skriver `get-help <command> -full`.
- Om du använder Azure-kommandoradsgränssnittet (CLI) kommandon toocomplete uppgifter i den här artikeln [installera och konfigurera hello Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har hello senaste versionen av hello Azure CLI är installerad. tooget hjälp för CLI-kommandona skriver `az <command> --help`. Du kan använda hello Azure Cloud-gränssnittet i stället för att installera hello CLI och dess krav. hello Azure Cloud-gränssnittet är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i hello Azure-portalen. Den har hello Azure CLI förinstallerat och konfigurerats toouse med ditt konto. toouse hello molnet Shell, klicka på hello molnet Shell **> _** knappen hello överst i hello [portal](https://portal.azure.com).

## <a name="create-a-network-interface"></a>Skapa ett nätverksgränssnitt

När du skapar en virtuell dator med hello Azure-portalen skapar hello portal ett nätverksgränssnitt med standardinställningar för dig. Om du hellre vill ange alla gränssnitt nätverksinställningarna kan du skapa ett nätverksgränssnitt med anpassade inställningar och bifoga hello network interface tooa virtuella datorn när du skapar hello virtuella datorn (med PowerShell eller hello Azure CLI). Du kan också skapa ett nätverksgränssnitt och lägga till den tooan befintlig virtuell dator (med PowerShell eller hello Azure CLI). toolearn hur toocreate en virtuell dator med en befintlig gränssnittet eller tooadd till eller ta bort nätverksgränssnitt från den befintliga virtuella datorer, läsa hello [Lägg till eller ta bort nätverksgränssnitt](virtual-network-network-interface-vm.md) artikel. Innan du skapar ett nätverksgränssnitt måste du ha en befintlig [virtuellt nätverk](virtual-networks-create-vnet-arm-pportal.md) i hello samma plats och prenumeration du skapar ett nätverksgränssnitt i.

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är tilldelade (minst) behörigheter för hello nätverket deltagarrollen för din prenumeration. Läs hello [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel toolearn mer om hur du tilldelar roller och behörigheter tooaccounts.
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *nätverksgränssnitt*. När **nätverksgränssnitt** visas i sökresultaten hello klickar du på den.
3. I hello **nätverksgränssnitt** bladet som visas, klickar du på **+ Lägg till**.
4. I hello **skapa nätverksgränssnittet** bladet som visas anger, eller Välj värden för hello följande inställningar och sedan klickar du på **skapa**:

    |Inställning|Krävs?|Information|
    |---|---|---|
    |Namn|Ja|hello-namnet måste vara unikt inom hello resursgrupp som du väljer. Över tiden har du förmodligen flera nätverksgränssnitt i din Azure-prenumeration. Läs hello [namngivningskonventioner](/azure/architecture/best-practices/naming-conventions?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions) artikel för förslag när du skapar en naming convention toomake hantera flera nätverksgränssnitt enklare. hello namn kan inte ändras när hello nätverksgränssnittet har skapats.|
    |Virtuellt nätverk|Ja|Välj hello virtuellt nätverk för hello nätverksgränssnitt. Du kan bara tilldela network interface tooa virtuella nätverk som finns i hello samma prenumeration och plats som hello nätverksgränssnitt. När ett nätverksgränssnitt har skapats kan ändra du inte hello virtuella nätverk som den är tilldelad till. hello virtuell dator som du lägger till hello network interface toomust även finnas i hello samma plats och prenumerationen som hello nätverksgränssnitt.|
    |Undernät|Ja|Välj ett undernät hello virtuellt nätverk som du har valt. Du kan ändra hello undernät hello nätverksgränssnittet är tilldelad tooafter som den har skapats.|
    |Privata IP-adresstilldelning|Ja| I den här inställningen väljer du hello tilldelningsmetod för hello IPv4-adress. Välj hello följande metoder: **dynamiska:** när du väljer det här alternativet kan tilldelas en tillgänglig adress Azure automatiskt från hello adressutrymme hello undernät som du har valt. Azure kan tilldela en annan adress tooa nätverksgränssnittet när hello virtuell dator i startas efter att ha i hello stoppats (frigjorts) tillstånd. hello adressen förblir hello samma om hello virtuella datorn har startats om utan att ha i hello stoppats (frigjorts) tillstånd. **Statiskt:** när du väljer det här alternativet måste du manuellt tilldelar en tillgänglig IP-adress från hello adressutrymme hello undernät som du har valt. Statiska adresser ändras inte tills du ändrar dem eller hello nätverksgränssnittet har tagits bort. Du kan ändra hello tilldelningsmetod när hello nätverksgränssnittet har skapats. hello Azure DHCP-server tilldelar nätverksgränssnittet adress toohello inom hello hello virtuella datorns operativsystem.|
    |Nätverkssäkerhetsgrupp|Nej| Lämna ställa in också**ingen**, Välj en befintlig [nätverkssäkerhetsgruppen](virtual-networks-nsg.md), eller [skapar en nätverkssäkerhetsgrupp](virtual-networks-create-nsg-arm-pportal.md). Nätverkssäkerhetsgrupper aktivera toofilter nätverkstrafik till och från ett nätverksgränssnitt. Du kan använda noll eller ett säkerhet grupp tooa nätverksgränssnittet för nätverket. Noll eller en säkerhetsgrupp för nätverk kan också användas toohello undernät hello nätverksgränssnittet har tilldelats. När en nätverkssäkerhetsgrupp är tillämpade tooa nätverksgränssnitt och hello undernät hello nätverksgränssnittet har tilldelats, ibland oväntade resultat. tootroubleshoot nätverkssäkerhet grupper tillämpade toonetwork gränssnitt och undernät, läsa hello [felsöka nätverkssäkerhetsgrupper](virtual-network-nsg-troubleshoot-portal.md#nsg) artikel.|
    |Prenumeration|Ja|Välj en av dina Azure [prenumerationer](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). hello virtuella du bifogar ett nätverk gränssnittet tooand hello virtuellt nätverk ansluts toomust finns i hello samma prenumeration.|
    |Privat IP-adress (IPv6)|Nej| Om du markerar den här kryssrutan, en IPv6-adress är tilldelade toohello nätverksgränssnitt, dessutom toohello IPv4-adress tilldelas toohello nätverksgränssnittet. Se hello [IPv6](#IPv6) i den här artikeln viktig information om du använder IPv6 med nätverksgränssnitt. Du kan inte välja en tilldelningsmetod för hello IPv6-adress. Om du väljer tooassign en IPv6-adress tilldelas med dynamiska hello-metoden.
    |IPv6-namn (visas bara när hello **privat IP-adress (IPv6)** är markerad) |Ja, om hello **privat IP-adress (IPv6)** är markerad.| Det här namnet är tilldelad tooa sekundära IP-konfiguration för hello nätverksgränssnitt. Mer information om IP-konfigurationer i hello [Visa inställningar för nätverksgränssnittet](#view-network-interface-settings) i den här artikeln.|
    |Resursgrupp|Ja|Välj en befintlig [resursgruppen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) eller skapa en. Ett nätverksgränssnitt kan finnas i hello samma eller annan resursgrupp än hello virtuell dator som du kopplar den till, eller hello virtuella nätverk du koppla den till.|
    |Plats|Ja|hello virtuella du bifogar ett nätverk gränssnittet tooand hello virtuellt nätverk ansluts toomust finns i hello samma [plats](https://azure.microsoft.com/regions), kallas även tooas en region.|

hello portal ger inte hello alternativet tooassign en offentlig IP-adress toohello gränssnitt när du skapar det, även om hello-portalen skapa offentlig IP-adress och tilldela den tooa nätverksgränssnittet när du skapar en virtuell dator med hello-portalen. toolearn hur tooadd toohello nätverket för offentlig IP-adress gränssnitt när du har skapat den, läsa hello [hantera IP-adresser](virtual-network-network-interface-addresses.md) artikel. Om du vill toocreate ett nätverksgränssnitt med en offentlig IP-adress, måste du använda hello CLI eller PowerShell toocreate hello nätverksgränssnitt.

>[!Note]
> Azure tilldelas ett MAC-adress toohello nätverksgränssnitt förrän hello nätverksgränssnittet är anslutna tooa virtuella datorn och hello virtuella datorn startas hello första gången. Du kan inte ange att Azure ska tilldelas toohello nätverksgränssnittet hello MAC-adress. hello nätverksgränssnittet för MAC-adressen förblir tilldelade toohello tills hello nätverksgränssnittet tas bort eller ändras hello privat IP-adress som tilldelats toohello primära IP-adresskonfigurationen för hello primära nätverksgränssnittet. Mer om IP-adresser och IP-konfigurationer, läsa hello toolearn [hantera IP-adresser](virtual-network-network-interface-addresses.md) artikel.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|CLI|[Skapa AZ nätverket nic](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|

## <a name="view-network-interface-settings"></a>Visa inställningar för nätverksgränssnitt

Du kan visa och ändra de flesta inställningar för ett nätverksgränssnitt när den har skapats.

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är tilldelade (minst) behörigheter för hello nätverket deltagarrollen för din prenumeration. Läs hello [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel toolearn mer om hur du tilldelar roller och behörigheter tooaccounts.
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *nätverksgränssnitt*. När **nätverksgränssnitt** visas i sökresultaten hello klickar du på den.
3. I hello **nätverksgränssnitt** bladet som visas, klicka på hello nätverksgränssnittet du vill tooview eller ändra inställningarna för.
4. hello finns följande inställningar i hello bladet som visas för hello nätverksgränssnitt som du har valt:
    - **Översikt:** innehåller information om hello nätverksgränssnitt, t.ex. hello IP-adresser tilldelas tooit hello virtuella undernät hello nätverksgränssnittet har tilldelats och hello virtuella hello nätverksgränssnittet är kopplat för (om den är ansluten tooone). hello följande bild visar hello översikt över inställningar för ett nätverksgränssnitt med namnet **mywebserver256**: ![översikt över gränssnittet](./media/virtual-network-network-interface/nic-overview.png) du kan flytta en network interface tooa annan resursgrupp eller prenumerationen genom att klicka på (**ändra**) nästa toohello **resursgruppen** eller **prenumerationsnamn**. Om du flyttar hello nätverksgränssnitt, måste du flytta alla resurser relaterade toohello nätverksgränssnitt med den. Om hello nätverksgränssnittet är anslutna tooa virtuell dator, till exempel måste du även flytta hello virtuella datorn och andra virtuella-relaterade resurser. toomove ett nätverksgränssnitt läsa hello [flytta resursen tooa ny resursgrupp eller prenumeration](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json#use-portal) artikel. hello artikeln förutsättningar och hur toomove resurser med hjälp av hello Azure-portalen, PowerShell och hello Azure CLI.
    - **IP-konfigurationer:** offentliga och privata IPv4 och IPv6-adresser tilldelas tooIP konfigurationer i den här listan. Om en IPv6-adress har tilldelats tooan IP-konfiguration, visas inte hello-adress. Mer information om IP-konfigurationer och hur tooadd och tar bort IP-adresser i hello [konfigurera IP-adresser för en Azure nätverksgränssnittet](virtual-network-network-interface-addresses.md) artikel. IP-vidarebefordring och undernättilldelning konfigureras i det här avsnittet. Mer om dessa inställningar läsa hello toolearn [aktivera eller inaktivera IP-vidarebefordring](#enable-or-disable-ip-forwarding) och [ändra undernättilldelning](#change-subnet-assignment) avsnitt i den här artikeln.
    - **DNS-servrar:** kan du ange vilken DNS-server ett nätverksgränssnitt tilldelas av hello Azure DHCP-servrar. hello nätverksgränssnitt kan ärva hello-inställningen från hello nätverksgränssnitt för virtuella nätverk hello har tilldelats eller har en anpassad inställning som åsidosätter hello inställning för hello virtuella nätverk som den är tilldelad till. toomodify vad som visas, fullständig hello stegen i hello [ändra DNS-servrar](#change-dns-servers) i den här artikeln.
    - **Nätverkssäkerhetsgrupp (NSG):** visar vilket NSG är associerad toohello nätverksgränssnitt (eventuella). En NSG innehåller regler för inkommande och utgående toofilter nätverkstrafik för hello nätverksgränssnitt. Om en NSG är associerad toohello nätverksgränssnitt, hello namnet på hello associerade NSG visas. toomodify vad som visas, fullständig hello stegen i hello [hantera nätverket säkerhetsassociationer för gruppen](virtual-network-manage-nsg-arm-portal.md#manage-associations) artikel.
    - **Egenskaper:** visar nyckeln inställningar om hello nätverksgränssnitt, inklusive dess MAC-adress (tomt om hello nätverksgränssnittet är anslutna tooa virtuella datorn) och hello prenumeration som den finns i.
    - **Effektiva säkerhetsregler:** säkerhetsregler visas om hello nätverksgränssnittet är anslutna tooa som kör virtuella datorn och en NSG är associerad toohello nätverksgränssnittet eller den hello undernät som den är tilldelad till. toolearn mer information om vad som visas, läsa hello [felsöka nätverkssäkerhetsgrupper](virtual-network-nsg-troubleshoot-portal.md#nsg) artikel. Mer om NSG: er, läsa hello toolearn [Nätverkssäkerhetsgrupper](virtual-networks-nsg.md) artikel.
    - **Effektiva vägar:** vägar finns i listan om hello nätverksgränssnittet är anslutna tooa som kör virtuella datorn. hello flöden är en kombination av hello Azure standardvägar några användardefinierade vägar (UDR) och BGP-vägar som kan finnas för hello undernät hello nätverksgränssnittet har tilldelats. toolearn mer information om vad som visas, läsa hello [felsöka vägar](virtual-network-routes-troubleshoot-portal.md#view-effective-routes-for-a-network-interface) artikel. Mer om Azure standard och udr: er, läsa hello toolearn [användardefinierade vägar](virtual-networks-udr-overview.md) artikel.
    - **Gemensamma inställningar för Azure Resource Manager:** toolearn mer om vanliga inställningar för Azure Resource Manager, läsa hello [aktivitetsloggen](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs), [åtkomstkontroll (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control), [taggar ](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags), [Låser](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json), och [automatiseringsskriptet](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group) artiklar.

**Kommandon**

Om en IPv6-adress har tilldelats tooa nätverksgränssnitt, returnerar hello faktum att hello-adress tilldelas, men det inte avsändaradress hello tilldelade hello PowerShell-utdata. På liknande sätt hello CLI returnerar hello faktum att hello adress tilldelas, men returnerar *null* i utdata för hello-adress.

|Verktyget|Kommando|
|---|---|
|CLI|[listan över AZ nätverk nic](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#list) tooview nätverksgränssnitt i hello prenumerationen; [az nätverket nic visa](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#show) tooview inställningar för ett nätverksgränssnitt|
|PowerShell|[Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) tooview nätverksgränssnitt i hello prenumeration eller visa inställningarna för ett nätverksgränssnitt|

## <a name="change-dns-servers"></a>Ändra DNS-servrar

hello DNS-server är tilldelad som nätverksgränssnittet för hello Azure DHCP server toohello inom hello virtuella operativsystemets. hello DNS-server som tilldelats är oavsett hello DNS-Serverinställningen för ett nätverksgränssnitt. toolearn mer information om inställningarna för matchning av namn för ett nätverksgränssnitt finns [namnmatchning för virtuella datorer](virtual-networks-name-resolution-for-vms-and-role-instances.md). hello nätverksgränssnitt kan ärva hello inställningar från hello virtuellt nätverk eller använda sin egen unika inställningar som åsidosätter hello hello virtuellt nätverk.

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är tilldelade (minst) behörigheter för hello nätverket deltagarrollen för din prenumeration. Läs hello [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel toolearn mer om hur du tilldelar roller och behörigheter tooaccounts.
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *nätverksgränssnitt*. När **nätverksgränssnitt** visas i sökresultaten hello klickar du på den.
3. I hello **nätverksgränssnitt** bladet som visas, klicka på hello nätverksgränssnittet du vill tooview eller ändra inställningarna för.
4. I hello bladet för hello nätverksgränssnitt som du har valt klickar du på **DNS-servrar** under **inställningar**.
5. Klicka på antingen:
    - **Ärv virtuella nätverk (standard)**: Välj alternativet tooinherit hello DNS Serverinställningen definierats för hello virtuellt nätverk hello nätverksgränssnittet har tilldelats. En anpassad DNS-server eller hello Azure-tillhandahållna DNS-server har definierats på hello virtuellt nätverksnivå. hello Azure-tillhandahållna DNS-servern kan matcha värdnamn för resurser som tilldelats toohello samma virtuella nätverk. FQDN måste vara används tooresolve för resurser som tilldelats toodifferent virtuella nätverk.
    - **Anpassad**: du kan konfigurera egna namn på DNS-server tooresolve över flera virtuella nätverk. Ange IP-adress för hello hello-server som du vill toouse som en DNS-server. hello DNS-serveradress du anger tilldelas endast toothis gränssnitt och åsidosättningar DNS-inställningar för nätverksgränssnittet för hello virtuellt nätverk hello har tilldelats.
6. Klicka på **Spara**.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|CLI|[AZ nätverket nic uppdatering](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Ange AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="enable-or-disable-ip-forwarding"></a>Aktivera eller inaktivera IP-vidarebefordring

IP-vidarebefordring kan hello virtuella ett nätverksgränssnitt som är kopplad till:
- Tar emot nätverkstrafik som inte är avsedda för en hello IP-adresser som är tilldelade tooany hello IP-konfigurationer som tilldelats toohello nätverksgränssnitt.
- Skicka trafik med en annan IP-källadress än hello en tilldelad tooone IP-konfigurationer för ett nätverksgränssnitt.

hello inställningen måste vara aktiverat för varje nätverksgränssnitt som är bifogade toohello virtuell dator som tar emot trafik som hello virtuella behov tooforward. En virtuell dator kan vidarebefordra trafik om den har flera nätverksgränssnitt eller till ett enda nätverk gränssnittet kopplade tooit. IP-vidarebefordring är en Azure-inställning, måste hello virtuella datorn också köra ett program kan tooforward hello trafik, till exempel brandvägg, WAN-optimering och program för belastningsutjämning. När en virtuell dator körs nätverksprogram, hello virtuella datorn är ofta hänvisas tooas en virtuell nätverksenhet. Du kan visa en lista över virtuella redo toodeploy nätverksinstallationer i hello [Azure Marketplace](https://azuremarketplace.microsoft.com/marketplace/apps/category/networking?page=1&subcategories=appliances). IP-vidarebefordring är vanligt med användardefinierade vägar. Mer om användardefinierade vägar, läsa hello toolearn [användardefinierade vägar](virtual-networks-udr-overview.md) artikel.

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är tilldelade (minst) behörigheter för hello nätverket deltagarrollen för din prenumeration. Läs hello [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel toolearn mer om hur du tilldelar roller och behörigheter tooaccounts.
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *nätverksgränssnitt*. När **nätverksgränssnitt** visas i sökresultaten hello klickar du på den.
3. I hello **nätverksgränssnitt** bladet som visas, klicka på hello nätverksgränssnittet du vill tooenable eller inaktivera IP-vidarebefordring för.
4. I hello bladet för hello nätverksgränssnitt som du har valt klickar du på **IP-konfigurationer** i hello **inställningar** avsnitt.
5. Klicka på **aktiverad** eller **inaktiverade** (standardinställningen) toochange hello inställningen.
6. Klicka på **Spara**.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|CLI|[AZ nätverket nic uppdatering](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Ange AzureRmNetworkInterface](/powershell/module/azurerm.network/set-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-subnet-assignment"></a>Ändra undernättilldelning

Du kan ändra hello undernät, men inte hello virtuellt nätverk som har tilldelats ett nätverksgränssnitt.

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är tilldelade (minst) behörigheter för hello nätverket deltagarrollen för din prenumeration. Läs hello [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel toolearn mer om hur du tilldelar roller och behörigheter tooaccounts.
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *nätverksgränssnitt*. När **nätverksgränssnitt** visas i sökresultaten hello klickar du på den.
3. I hello **nätverksgränssnitt** bladet som visas, klicka på hello nätverksgränssnittet du vill tooview eller ändra inställningarna för.
4. Klicka på **IP-konfigurationer** under **inställningar** hello-bladet för hello nätverksgränssnitt som du har valt. Om alla privata IP-adresser för alla IP-konfigurationer i listan har **(statisk)** nästa toothem måste du ändra hello IP-adresstilldelning metoden toodynamic genom att slutföra hello steg som följer. Alla privata IP-adresser måste tilldelas med hello dynamisk tilldelning metoden toochange hello undernättilldelning för hello nätverksgränssnitt. Om hello-adresser tilldelas med hello dynamisk metod, Fortsätt toostep fem. Om IPv4-adresser tilldelas med statisk tilldelningsmetod för hello, utför följande steg toochange hello tilldelning metoden toodynamic hello:
    - Klicka på hello IP-konfiguration som du vill toochange hello IPv4-adress tilldelningsmetod för hello listan över IP-konfigurationer.
    - I hello bladet som visas för hello IP-konfiguration klickar du på **dynamiska** för hello **tilldelning** metod. Du kan inte tilldela en IPv6-adress med statisk tilldelningsmetod för hello.
    - Klicka på **Spara**.
5. Välj hello undernät som du vill tooconnect hello network interface toofrom hello **undernät** listrutan.
6. Klicka på **Spara**. Nya dynamiska adresser tilldelas från hello undernät adressintervall för hello Nytt undernät. Efter att hello gränssnittet tooa nya undernät tilldela du en statisk IPv4-adress från hello nya undernät-adressintervall om du väljer. Mer om att lägga till, ändra och ta bort IP-adresser för ett nätverksgränssnitt läsa hello toolearn [hantera IP-adresser](virtual-network-network-interface-addresses.md) artikel.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|CLI|[AZ nätverket nic ip-config uppdatering](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Ange AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-a-network-interface"></a>Ta bort ett nätverksgränssnitt

Du kan ta bort ett nätverksgränssnitt som den inte är anslutna tooa virtuell dator. Om den är ansluten tooa virtuell dator måste du första plats hello virtuell dator i hello Stoppad (frigjord) tillstånd, koppla från hello nätverksgränssnittet från hello virtuell dator innan du kan ta bort hello nätverksgränssnitt. toodetach ett nätverksgränssnitt från en virtuell dator, fullständig hello stegen i hello [ta bort en nätverksgränssnittet från en virtuell dator](virtual-network-network-interface-vm.md#vm-remove-nic) avsnitt i hello [Lägg till eller ta bort nätverksgränssnitt](virtual-network-network-interface-vm.md) artikel. Om du tar bort en virtuell dator tar du bort alla nätverk gränssnitt bifogade tooit, men tar inte bort hello nätverksgränssnitt.

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är tilldelade (minst) behörigheter för hello nätverket deltagarrollen för din prenumeration. Läs hello [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel toolearn mer om hur du tilldelar roller och behörigheter tooaccounts.
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *nätverksgränssnitt*. När **nätverksgränssnitt** visas i sökresultaten hello klickar du på den.
3. Högerklicka på hello nätverksgränssnittet toodelete och på **ta bort**.
4. Klicka på **Ja** tooconfirm borttagning av hello nätverksgränssnitt.

När du tar bort ett nätverksgränssnitt, alla MAC eller tilldela IP-adresser tooit släpps.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|CLI|[AZ datornätverket nic ta bort](/cli/azure/network/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Ta bort AzureRmNetworkInterface](/powershell/module/azurerm.network/remove-azurermnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Nästa steg
toocreate en virtuell dator med flera nätverksgränssnitt eller IP-adresser, läsa hello följande artiklar:

**Kommandon**

|Aktivitet|Verktyget|
|---|---|
|Skapa en virtuell dator med flera nätverkskort|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Skapa en enda NIC VM med flera IPv4-adresser|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Skapa en enda NIC VM med en privat IPv6-adress (bakom en belastningsutjämnare i Azure)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-mall](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
