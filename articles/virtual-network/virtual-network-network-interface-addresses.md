---
title: "aaaConfigure IP-adresser för en Azure nätverksgränssnittet | Microsoft Docs"
description: "Lär dig hur tooadd, ändra och ta bort privata och offentliga IP-adresser för ett nätverksgränssnitt."
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
ms.openlocfilehash: 1e5ea6c65d93be9b1fda5d807500a0823c94c89c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-change-or-remove-ip-addresses-for-an-azure-network-interface"></a>Lägga till, ändra eller ta bort IP-adresser för ett Azure-nätverk-gränssnitt

Lär dig hur tooadd, ändra och ta bort offentliga och privata IP-adresser för ett nätverksgränssnitt. Privata IP-adresser som tilldelats tooa nätverksgränssnittet aktivera en virtuell dator toocommunicate med andra resurser i Azure-nätverk och anslutna nätverk. En privat IP-adress kan också utgående kommunikation toohello Internet med en oförutsägbart IP-adress. En [offentliga IP-adressen](virtual-network-public-ip-address.md) tilldelade tooa nätverksgränssnittet gör att inkommande kommunikation tooa virtuell dator från hello Internet. hello-adressen kan även utgående kommunikation från hello virtuella toohello Internet med en förutsägbar IP-adress. Mer information finns i [förstå utgående anslutningar i Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Om du behöver toocreate, ändra eller ta bort ett nätverksgränssnitt läsa hello [hantera ett nätverksgränssnitt](virtual-network-network-interface.md) artikel. Om du behöver tooadd nätverksgränssnitt tooor ta bort nätverksgränssnitt från en virtuell dator kan läsa hello [Lägg till eller ta bort nätverksgränssnitt](virtual-network-network-interface-vm.md) artikel. 


## <a name="before-you-begin"></a>Innan du börjar

Fullständig hello följande uppgifter innan du slutför alla steg i alla avsnitt i den här artikeln:

- Granska hello [Azure begränsar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikel toolearn om gränser för offentliga och privata IP-adresser.
- Logga in toohello Azure [portal](https://portal.azure.com), Azure-kommandoradsgränssnittet (CLI) eller Azure PowerShell med ett Azure-konto. Om du inte redan har ett Azure-konto, registrera dig för en [ledigt utvärderingskonto](https://azure.microsoft.com/free).
- Om du använder PowerShell-kommandon toocomplete uppgifter i den här artikeln [installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har hello senaste versionen av hello Azure PowerShell-kommandon som installeras. tooget hjälp för PowerShell-kommandon med exempel på, skriver `get-help <command> -full`.
- Om du använder Azure-kommandoradsgränssnittet (CLI) kommandon toocomplete uppgifter i den här artikeln [installera och konfigurera hello Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har hello senaste versionen av hello Azure CLI är installerad. tooget hjälp för CLI-kommandona skriver `az <command> --help`. Du kan använda hello Azure Cloud-gränssnittet i stället för att installera hello CLI och dess krav. hello Azure Cloud-gränssnittet är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i hello Azure-portalen. Den har hello Azure CLI förinstallerat och konfigurerats toouse med ditt konto. toouse hello molnet Shell, klicka på hello molnet Shell **> _** knappen hello överst i hello [portal](https://portal.azure.com).

## <a name="add-ip-addresses"></a>Lägg till IP-adresser

Du kan lägga till så många [privata](#private) och [offentliga](#public) [IPv4](#ipv4) adresser som behövs tooa nätverksgränssnitt inom hello gränser som anges i hello [Azure gränser ](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikel. Du kan inte använda hello portal tooadd en IPv6-adress tooan befintliga nätverksgränssnittet (även om du kan använda hello portal tooadd ett privat nätverksgränssnitt för IPv6-adress tooa när du skapar hello nätverksgränssnittet). Du kan använda PowerShell eller CLI tooadd privata IPv6-adress tooone hello [sekundära IP-konfiguration](#secondary) (så länge det finns inga befintliga sekundär IP-konfigurationer) gränssnitt som inte är ansluten tooa virtuella för ett befintligt nätverk datorn. Du kan inte använda några verktyget tooadd en offentlig nätverksgränssnittet för IPv6-adress tooa. Se [IPv6](#ipv6) mer information om hur du använder IPv6-adresser. 

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är tilldelade (minst) behörigheter för hello nätverket deltagarrollen för din prenumeration. Läs hello [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel toolearn mer om hur du tilldelar roller och behörigheter tooaccounts.
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *nätverksgränssnitt*. När **nätverksgränssnitt** visas i sökresultaten hello klickar du på den.
3. I hello **nätverksgränssnitt** bladet som visas, klicka på hello nätverksgränssnittet som du vill tooadd en IPv4-adress för.
4. Klicka på **IP-konfigurationer** i hello **inställningar** avsnitt i hello bladet för hello nätverksgränssnitt som du har valt.
5. Klicka på **+ Lägg till** i hello bladet som öppnas för IP-konfigurationer.
6. Anger hello följande och klicka sedan på **OK** tooclose hello **lägga till IP-konfiguration** bladet:

    |Inställning|Krävs?|Information|
    |---|---|---|
    |Namn|Ja|Måste vara unikt för hello nätverksgränssnittet|
    |Typ|Ja|Eftersom du vill lägga till en IP-konfiguration tooan befintliga nätverksgränssnittet och varje nätverksgränssnitt måste ha en [primära](#primary) IP-konfiguration, det enda alternativet är **sekundära**.|
    |Tilldelningsmetod för privat IP-adress|Ja|[**Dynamiska** ](#dynamic) kan ändras om hello virtuella datorn startas om efter att ha i hello stoppats (frigjorts) tillstånd. Azure tilldelas en tillgänglig adress från hello adressutrymme hello undernät hello nätverksgränssnittet är anslutet till. [**Statisk** ](#static) adresser inte är publicerat tills hello nätverksgränssnittet tas bort. Ange en IP-adress från hello undernät utrymme adressintervallet som för inte närvarande används av en annan IP-konfiguration.|
    |Offentlig IP-adress|Nej|**Inaktiverat:** ingen offentlig IP-adressresurs är för närvarande associerad toohello IP-konfiguration. **Aktiverad:** Välj en befintlig offentlig IPv4-IP-adress eller skapa en ny. toolearn hur toocreate en offentlig IP-adress för att läsa hello [offentliga IP-adresser](virtual-network-public-ip-address.md#create-a-public-ip-address) artikel.|
7. Lägg till sekundära privata IP-adresser toohello virtuella operativsystemets manuellt genom att slutföra hello instruktionerna i hello [tilldela flera IP-adresser toovirtual datorn operativsystem](virtual-network-multiple-ip-addresses-portal.md#os-config) artikel. Se [privata](#private) IP-adresser för speciella överväganden innan du lägger till IP-adresser tooa virtuella operativsystemets manuellt. Lägg inte till några offentliga IP-adresser toohello virtuella operativsystemets.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|CLI|[Skapa AZ nätverket nic ip-konfiguration](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Lägg till AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/add-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-ip-address-settings"></a>Ändra inställningar för IP-adress

Du kan behöva toochange hello tilldelningsmetod för en IPv4-adress, ändra hello statisk IPv4-adress eller ändra hello offentlig IP-adress tilldelas tooa nätverksgränssnitt. Om du ändrar hello privata IPv4-adressen för en sekundär IP-konfiguration som är kopplad till ett sekundärt nätverksgränssnitt i en virtuell dator (Läs mer om [primära och sekundära nätverksgränssnitt](virtual-network-network-interface-vm.md#about)), placera hello virtuella datorn till hello stoppats (frigjorts) tillstånd innan du slutför hello följande steg: 

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är tilldelade (minst) behörigheter för hello nätverket deltagarrollen för din prenumeration. Läs hello [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel toolearn mer om hur du tilldelar roller och behörigheter tooaccounts.
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *nätverksgränssnitt*. När **nätverksgränssnitt** visas i sökresultaten hello klickar du på den.
3. I hello **nätverksgränssnitt** bladet som visas, klicka på hello nätverksgränssnittet du vill tooview eller ändra inställningar för IP-adresser.
4. Klicka på **IP-konfigurationer** i hello **inställningar** avsnitt i hello bladet för hello nätverksgränssnitt som du har valt.
5. Klicka på hello IP-konfiguration som du vill toomodify hello listan i hello bladet som öppnas för IP-konfigurationer.
6. Ändra hello-inställningarna efter behov, med hjälp av hello information om hello inställningar i steg 6 i hello [lägga till en IP-konfiguration](#create-ip-config) i den här artikeln. Klicka på **spara** tooclose hello bladet för hello IP-konfiguration som du har ändrat.

>[!NOTE]
>Om hello primära nätverksgränssnittet har flera IP-konfigurationer och ändrar hello privata IP-adressen för hello primära IP-konfigurationen, måste du manuellt tilldela hello primära och sekundära IP-adresser toohello nätverksgränssnittet i Windows (inte krävs för Linux). toomanually tilldela IP-adresser tooa nätverksgränssnittet inom ett operativsystem, läsa hello [tilldela flera IP-adresser toovirtual datorer](virtual-network-multiple-ip-addresses-portal.md#os-config) artikel. Se [privata](#private) IP-adresser för speciella överväganden innan du lägger till IP-adresser tooa virtuella operativsystemets manuellt. Lägg inte till några offentliga IP-adresser toohello virtuella operativsystemets.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|CLI|[AZ nätverket nic ip-config uppdatering](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Ange AzureRMNetworkInterfaceIpConfig](/powershell/module/azurerm.network/set-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="remove-ip-addresses"></a>Ta bort IP-adresser

Du kan ta bort [privata](#private) och [offentliga](#public) IP-adresser från ett nätverksgränssnitt, men ett nätverksgränssnitt måste alltid ha minst en privat IPv4-adress som tilldelats tooit.

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är tilldelade (minst) behörigheter för hello nätverket deltagarrollen för din prenumeration. Läs hello [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) artikel toolearn mer om hur du tilldelar roller och behörigheter tooaccounts.
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *nätverksgränssnitt*. När **nätverksgränssnitt** visas i sökresultaten hello klickar du på den.
3. I hello **nätverksgränssnitt** bladet som visas, klicka på hello nätverksgränssnittet som du vill tooremove IP-adresser från.
4. Klicka på **IP-konfigurationer** i hello **inställningar** avsnitt i hello bladet för hello nätverksgränssnitt som du har valt.
5. Högerklicka på en [sekundära](#secondary) IP-konfiguration (du kan inte ta bort hello [primära](#primary) configuration) du vill toodelete, klickar på **ta bort**, klicka sedan på **Ja**  tooconfirm hello borttagning. Om hello konfiguration har en offentlig IP-adressresurs associerad tooit hello resurs har kopplats bort från hello IP-konfiguration, men hello resurs inte tas bort.
6. Stäng hello **IP-konfigurationer** bladet.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|CLI|[ta bort AZ nätverket nic ip-konfiguration](/cli/azure/network/nic/ip-config?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Ta bort AzureRmNetworkInterfaceIpConfig](/powershell/module/azurerm.network/remove-azurermnetworkinterfaceipconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="ip-configurations"></a>IP-konfigurationer

[Privata](#private) och (valfritt) [offentliga](#public) IP-adresser tilldelas tooone eller flera IP-konfigurationer tilldelade tooa nätverksgränssnitt. Det finns två typer av IP-konfigurationer:

### <a name="primary"></a>Primär

Varje nätverksgränssnitt tilldelas en primär IP-konfiguration. En primär IP-konfiguration:

- Har en [privata](#private) [IPv4](#ipv4) adress som tilldelats tooit. Du kan inte tilldela en privat [IPv6](#ipv6) tooa primära IP-adresskonfiguration.
- Kan också ha en [offentliga](#public) IPv4-adress som tilldelats tooit. Du kan inte tilldela en offentlig IPv6 tooa primär eller sekundär IP-adresskonfiguration. Du kan dock, tilldela en offentlig IPv6-adress tooan Azure belastningsutjämnare, som kan läsa in Utjämna trafiken tooa virtuella datorns privata IPv6-adress. Mer information finns i [information och begränsningar för IPv6](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations).

### <a name="secondary"></a>Sekundär

Dessutom tooa primära IP-konfiguration, ett nätverksgränssnitt kan ha noll eller flera sekundära IP-konfigurationer som tilldelats tooit. En sekundär IP-konfiguration:

- Måste ha en privat IPv4- eller IPv6-adress som tilldelats tooit. Om hello-adress IPv6 kan hello nätverksgränssnitt bara ha en sekundär IP-konfiguration. Om hello-adress IPv4 kan hello nätverksgränssnitt ha flera sekundära IP-konfigurationer som tilldelats tooit. toolearn mer information om hur många privata och offentliga IPv4-adresser kan tilldelas tooa nätverksgränssnitt, se hello [Azure begränsar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikel.  
- Kan också ha en offentlig IPv4-adress som tilldelats tooit om hello privata IP-adressen är IPv4. Om hello privata IP-adressen är IPv6, kan du inte tilldela en offentlig IPv4- eller IPv6-toohello IP-adresskonfiguration. Tilldela flera IP-adresser tooa nätverksgränssnittet är användbart i scenarier som:
    - Du kan hantera flera webbplatser eller tjänster med olika IP-adresser och SSL-certifikat på en enda server.
    - En virtuell dator som fungerar som en virtuell nätverksenhet, till exempel en brandvägg eller läsa in belastningsutjämning.
    - Hej möjlighet tooadd någon hello privata IPv4-adresser för alla gränssnitt tooan Azure belastningsutjämnare backend-adresspool för hello nätverk. Senaste hello, kunde bara hello primära IPv4-adress för hello primära nätverksgränssnittet läggas tooa backend-adresspool. toolearn mer information om hur tooload Utjämna flera IPv4-konfigurationer finns hello [flera IP-konfigurationer för belastningsutjämning](../load-balancer/load-balancer-multiple-ip.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel. 
    - Hej möjlighet tooload belastningsutjämna ett IPv6-adress som tilldelats tooa nätverksgränssnitt. toolearn mer information om hur tooload balansera tooa privata IPv6-adress, finns hello [belastningsutjämna IPv6-adresser](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel.


## <a name="address-types"></a>Adresstyper

Du kan tilldela hello följande typer av IP-adresser tooan [IP-konfiguration](#ip-configurations):

### <a name="private"></a>Privat

Privata [IPv4](#ipv4) adresser aktivera en virtuell dator toocommunicate med andra resurser i ett virtuellt nätverk eller andra anslutna nätverk. En virtuell dator kan inte lämnas inkommande, och inte heller hello virtuella kommunicera utgående med en privat [IPv6](#ipv6) adress med ett undantag. En virtuell dator kan kommunicera med hello Azure belastningsutjämnare använder en IPv6-adress. Mer information finns i [information och begränsningar för IPv6](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#details-and-limitations). 

Som standard tilldelar hello Azure DHCP-servrar hello privat IPv4-adress för hello [primära IP-konfigurationen](#primary) för nätverksgränssnittet för hello network interface toohello inom hello virtuella operativsystemets. Om inte behövs bör du aldrig manuellt ange hello IP-adressen för ett nätverksgränssnitt i hello virtuella datorns operativsystem. 

> [!WARNING]
> Om hello IPv4-adress har angetts som skiljer hello primära IP-adressen för ett nätverksgränssnitt i en virtuell dators operativsystem någonsin hello privat IPv4-adress som tilldelats toohello primära IP-adresskonfigurationen för hello primära nätverksgränssnittet kopplade tooa virtuell dator i Azure, förlora anslutningen toohello virtuella datorn.

Det finns scenarier där det är nödvändigt toomanually set hello IP-adressen för ett nätverksgränssnitt i hello virtuella datorns operativsystem. Du måste till exempel manuellt ange hello primära och sekundära IP-adresser för en Windows-operativsystemet när du lägger till flera IP-adresser tooan virtuella Azure-datorn. För en virtuell Linux-dator, kanske du bara behöver toomanually set hello sekundära IP-adresser. Se [lägga till IP-adresser tooa VM operativsystemet](virtual-network-multiple-ip-addresses-portal.md#os-config) mer information. När du manuellt ange hello IP-adress inom hello operativsystemet rekommenderas att du alltid tilldela hello adresser toohello IP-konfiguration för ett nätverksgränssnitt med hello statiska (i stället för dynamiska) tilldelningsmetod. Tilldela hello-adressen med hello statisk metod säkerställer hello adress inte ändras i Azure. Om du skulle toochange hello-adress som tilldelats tooan IP-konfiguration, rekommenderar vi som du:

1. tooensure hello virtuell dator tar emot en adress från hello Azure DHCP-servrar, ändra hello tilldelning av hello IP-adress tillbaka tooDHCP inom hello operativsystemet och starta om hello virtuell dator.
2. Stoppa (frigöra) hello virtuell dator.
3. Ändra hello IP-adress för hello IP-konfiguration i Azure.
4. Starta hello virtuell dator.
5. [Konfigurera manuellt](virtual-network-multiple-ip-addresses-portal.md#os-config) hello sekundära IP-adresser inom hello operativsystem (och även hello primära IP-adress inom Windows) toomatch du anger i Azure.
 
Genom att följa stegen i hello Hej privata IP-adress som tilldelats toohello-nätverksgränssnitt i Azure, och inom en virtuell dators operativsystem, förblir hello samma. tookeep spåra som virtuella datorer i din prenumeration som du angett manuellt IP-adresser inom ett operativsystem, Överväg att lägga till en Azure [taggen](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags) toohello virtuella datorer. Du kan använda ”IP-adresstilldelning: statisk”, till exempel. På så sätt kan du lätt kan hitta hello virtuella datorer i din prenumeration som du har angett hello IP-adress för inom hello operativsystemet manuellt.

Dessutom kan tooenabling en virtuell dator toocommunicate med andra resurser inom hello samma eller anslutna virtuella nätverk, ett privat IP-adress även en virtuell dator toocommunicate utgående toohello Internet. Utgående anslutningar är källan nätverksadress översättas av Azure tooan oförutsägbart offentlig IP-adress. Mer om Azure utgående Internetanslutning kan läsa hello toolearn [Azure utgående Internetanslutning](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel. Du kan inte kommunicera inkommande tooa virtuella datorns privata IP-adress från hello Internet.

### <a name="public"></a>Offentligt

Offentliga IP-adresser aktivera inkommande anslutning tooa virtuell dator från hello Internet. Utgående anslutningar toohello Internet använder en förutsägbar IP-adress. Se [förstå utgående anslutningar i Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json) mer information. Du kan tilldela en offentlig IP-adress tooan IP-konfiguration, men inte behöver. Om du inte tilldelar en offentlig IP-adress tooa virtuell dator kan den fortfarande kommunicera utgående toohello Internet med sin privata IP-adress. Mer om den offentliga IP-adresser, läsa hello toolearn [offentliga IP-adressen](virtual-network-public-ip-address.md) artikel.

Det finns begränsningar toohello antalet privata och offentliga IP-adresser som du kan tilldela tooa nätverksgränssnittet. Mer information läser du hello [Azure begränsar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikel.

> [!NOTE]
> Azure omvandlar en virtuell dators privat IP-adress tooa offentliga IP-adress. Därför hello operativsystem är inte medveten om eventuella offentliga IP-adresser som tilldelats tooit, så det finns inget behov av tooever manuellt tilldela en offentlig IP-adress inom hello-operativsystemet.

## <a name="assignment-methods"></a>Metoder

Offentliga och privata IP-adresser tilldelas med hjälp av hello följande metoder:

### <a name="dynamic"></a>Dynamisk

Dynamisk privata IPv4 och IPv6 tilldelas adresser (valfritt) som standard. Dynamisk kan ändras om hello virtuella hello stoppats (frigjorts) tillstånd och igång. Om du inte vill IPv4-adresser toochange för hello hello virtuella datorn, tilldela hello adresser med hjälp av hello statisk metod. Du kan bara tilldela en privat IPv6-adress med hjälp av hello dynamiska tilldelningsmetod. Du kan inte tilldela en offentlig IPv6 tooan IP-adresskonfiguration med någon av metoderna.

### <a name="static"></a>Statisk

Adresser som tilldelats med hjälp av hello statisk metod ändras inte förrän en virtuell dator tas bort. Du tilldelar manuellt en statisk privat IPv4-tooan IP-adresskonfiguration från hello-adressutrymmet för hello undernät hello nätverksgränssnitt. (Valfritt) kan du tilldela ett offentligt eller privat statisk IPv4-tooan IP-adresskonfiguration. Du kan inte tilldela en statisk offentlig eller privat IPv6 tooan IP-adresskonfiguration. toolearn mer information om hur Azure tilldelar statiska offentliga IPv4-adresser finns hello [offentliga IP-adressen](virtual-network-public-ip-address.md) artikel.

## <a name="ip-address-versions"></a>IP-adress versioner

Du kan ange hello följande versioner när du tilldelar adresser:

### <a name="ipv4"></a>IPv4

Varje nätverksgränssnitt måste ha ett [primära](#primary) IP-konfiguration med en tilldelad [privata](#private) [IPv4](#ipv4) adress. Du kan lägga till en eller flera [sekundära](#secondary) IP-konfigurationer som har en privat IPv4 och (valfritt) en IPv4 [offentliga](#public) IP-adress.

### <a name="ipv6"></a>IPv6

Du kan tilldela noll eller ett privat [IPv6](#ipv6) -tooone sekundära IP-adresskonfigurationen för ett nätverksgränssnitt. hello nätverksgränssnitt kan inte ha några befintliga sekundär IP-konfigurationer. Du kan inte lägga till en IP-konfiguration med en IPv6-adress med hjälp av hello portal. Använd PowerShell eller hello CLI tooadd en IP-konfiguration med en privat IPv6-adress tooan befintliga nätverksgränssnittet. hello nätverksgränssnitt får inte vara anslutna tooan befintliga VM.

> [!NOTE]
> Om du kan skapa ett nätverksgränssnitt med en IPv6-adress med hjälp av hello portal, du kan inte lägga till en befintlig network interface tooa ny eller befintlig virtuell dator, med hjälp av hello portal. Använd PowerShell eller hello Azure CLI 2.0 toocreate ett nätverksgränssnitt med en privat IPv6-adress och bifogar hello nätverksgränssnittet när du skapar en virtuell dator. Du kan inte koppla ett nätverksgränssnitt med en privat IPv6-adress som tilldelats tooit tooan befintlig virtuell dator. Du kan inte lägga till en privat IPv6 tooan IP-adresskonfiguration för alla nätverk gränssnittet kopplade tooa virtuella datorer med hjälp av alla verktyg (portal, CLI eller PowerShell).

Du kan inte tilldela en offentlig IPv6 tooa primär eller sekundär IP-adresskonfiguration.

## <a name="next-steps"></a>Nästa steg
toocreate en virtuell dator med olika IP-konfigurationer, läsa hello följande artiklar:

|Aktivitet|Verktyget|
|---|---|
|Skapa en virtuell dator med flera nätverkskort|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Skapa en enda NIC VM med flera IPv4-adresser|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Skapa en enda NIC VM med en privat IPv6-adress (bakom en belastningsutjämnare i Azure)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-mall](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
