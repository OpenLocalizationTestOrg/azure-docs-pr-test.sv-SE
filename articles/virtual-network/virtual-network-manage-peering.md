---
title: "aaaCreate, ändra eller ta bort ett virtuellt Azure-nätverk som peering | Microsoft Docs"
description: "Lär dig hur toocreate, ändra eller ta bort ett virtuellt nätverk-peering."
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
ms.date: 07/26/2017
ms.author: jdial
ms.openlocfilehash: 70f85364ab23e23533ad276aeec0156cebe89be5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-change-or-delete-a-virtual-network-peering"></a>Skapa, ändra eller ta bort ett virtuellt nätverk-peering

Lär dig hur toocreate, ändra eller ta bort ett virtuellt nätverk-peering. Virtuellt nätverk peering aktiverar du tooconnect två virtuella nätverk i hello samma Azure-plats via hello Azure stamnät nätverk. När peerkoppla, hanteras fortfarande hello två virtuella nätverk som separata resurser. Resurser i antingen virtuella nätverket kommunicerar med hello samma svarstid och bandbredd som om hello resurser som fanns i hello samma virtuella nätverk. Om du inte är bekant med virtuella nätverk peering rekommenderar vi att läsa hello [peering översikt över virtuella nätverk](virtual-network-peering-overview.md) och slutföra hello [skapa virtuellt nätverk peering självstudiekursen](virtual-network-create-peering.md), innan du slutför hello aktiviteter i den här artikeln.

## <a name="before-you-begin"></a>Innan du börjar

Utför följande uppgifter innan du slutför stegen i alla avsnitt i den här artikeln hello:

- Granska hello [Azure begränsar](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) artikel toolearn om gränser för peering.
- Logga in toohello Azure-portalen, Azure-kommandoradsgränssnittet (CLI) eller Azure PowerShell med ett Azure-konto. Om du inte redan har ett Azure-konto, registrera dig för en [ledigt utvärderingskonto](https://azure.microsoft.com/free).
- Om du använder PowerShell-kommandon toocomplete uppgifter i den här artikeln [installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har hello senaste version av installerat hello Azure PowerShell-cmdlets. tooget hjälp för PowerShell-kommandon med exempel på, skriver `get-help <command> -full`.
- Om du använder Azure-kommandoradsgränssnittet (CLI) kommandon toocomplete uppgifter i den här artikeln [installera och konfigurera hello Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har hello senaste versionen av hello Azure CLI är installerad. tooget hjälp för CLI-kommandona skriver `az <command> --help`. Du kan använda hello Azure Cloud-gränssnittet i stället för att installera hello CLI och dess krav. hello Azure Cloud-gränssnittet är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i hello Azure-portalen. hello molnet Shell har hello Azure CLI förinstallerat och konfigurerats toouse med ditt konto. toouse hello molnet Shell, klicka på hello molnet Shell **> _** knappen hello överst i hello [portal](https://portal.azure.com). 

## <a name="create-a-peering"></a>Skapa en peering

>[!NOTE]
>Det finns flera krav, villkor och överväganden toosuccessfully skapa ett virtuellt nätverk som peering. Innan du skapar en peering, se till att du har bekantat dig med hello [kraven och begränsningarna](#requirements-and-constraints) och [nödvändiga behörigheter](#permissions).
>

1. Logga in toohello [portal](https://portal.azure.com) med ett konto som har tilldelats hello behov [eller behörigheter](#permissions).
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *virtuella nätverk*. När **virtuella nätverken** visas i sökresultaten hello klickar du på den. Välj inte **virtuella nätverk (klassiskt)** om det visas i hello listan som du inte kan skapa en peering från ett virtuellt nätverk som distribuerats via hello klassiska distributionsmodellen.
3. I hello **virtuella nätverken** bladet som visas, klicka på hello virtuella nätverk som du vill toocreate en peering för.
4. Klicka i hello-fönstret som visas för hello virtuella nätverk som du har valt **Peerkopplingar** i hello **inställningar** avsnitt.
5. Klicka på **+ Lägg till**. 
6. <a name="add-peering"></a>I hello **lägga till peering** bladet ange eller Välj värden för hello följande inställningar:
    - **Namn:** hello namnet för hello peering måste vara unikt inom hello virtuellt nätverk.
    - **Distributionsmodell för virtuellt nätverk:** Markera vilka distribution modellen hello virtuella nätverk som du vill toopeer med har distribuerats via.
    - **Jag vet mitt resurs-ID:** om du har läsbehörighet toohello virtuellt nätverk du vill toopeer med lämnar den här kryssrutan avmarkerad. Om du inte har läsbehörighet toohello virtuellt nätverk eller prenumeration som du vill toopeer med kan den här kryssrutan. Ange hello fullständiga resurs-ID för hello virtuella nätverk som du vill toopeer med i hello **resurs-ID** som visades när du har markerat kryssrutan hello. hello-resurs-ID du anger måste vara för ett virtuellt nätverk som finns i hello samma Azure [plats](https://azure.microsoft.com/regions) som den här virtuella nätverket. hello fullständiga resurs-ID ser ut ungefärför/prenumerationer/<Id>/resourceGroups/ < resursgruppnamn > /providers/Microsoft.Network/virtualNetworks/ <-namn virtuellt nätverk->. Du kan hämta hello resurs-ID för ett virtuellt nätverk genom att visa hello egenskaperna för ett virtuellt nätverk. hur tooview hello egenskaper för ett virtuellt nätverk, se toolearn [hantera virtuella nätverk](virtual-network-manage-network.md#view-vnet).
    - **Prenumerationen:** väljer hello [prenumeration](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) hello virtuella nätverk som du vill toopeer med. En eller flera prenumerationer visas, beroende på hur många prenumerationer ditt konto har läsbehörighet till. När du har markerat hello **resurs-ID** och den här inställningen är inte tillgänglig. Du kan peer-virtuella nätverk för olika prenumerationer så länge båda virtuella nätverk har skapats via Resource Manager. hello möjlighet toopeer alla prenumerationer som skapats via olika distributionsmodeller är i förhandsversionen. Registrera dig för förhandsgranskning hello innan du skapar en peering mellan virtuella nätverk som distribuerats via olika distributionsmodeller som finns i olika prenumerationer. Läs mer om hur tooregister för hello förhandsgranskning och [peer-virtuella nätverk som skapats via olika distributionsmodeller för olika prenumerationer](create-peering-different-deployment-models-subscriptions.md).
    - **Virtuellt nätverk:** Välj hello virtuella nätverk som du vill toopeer med. Du kan välja ett virtuellt nätverk som skapats via antingen Azure distributionsmodell men hello virtual network måste finnas i samma plats som hello virtuella nätverk som du initiera hello peering från hello. Du måste ha läs åtkomst toohello virtuellt nätverk för den toobe som visas i listan hello. Om ett virtuellt nätverk visas men nedtonad, kanske eftersom hello adressutrymmet för hello virtuella nätverket överlappar hello adressutrymmet för det här virtuella nätverket. Om virtuella adressutrymmen överlappar varandra, kan de inte peerkopplas. När du har markerat hello **resurs-ID** och den här inställningen är inte tillgänglig.
    - **Tillåt åtkomst till virtuella nätverk:** Välj **aktiverad** (standard) om du vill tooenable kommunikation mellan hello två virtuella nätverk. Om du aktiverar kommunikation mellan virtuella nätverk kan resurser som är anslutna tooeither virtuellt nätverk toocommunicate med varandra med hello samma bandbredd och svarstid som om de vore anslutna toohello samma virtuella nätverk. All kommunikation mellan resurser i hello två virtuella nätverk är över hello Azure-nätverk. Hej **VirtualNetwork** Standardtaggen för nätverkssäkerhetsgrupper omfattar hello virtuella nätverk och peerkoppla virtuella nätverk. Mer om toolearn network security grupp standardtaggar läsa hello [Network security groups översikt](virtual-networks-nsg.md#default-tags) artikel.  Välj **inaktiverad** om du inte vill trafik tooflow toohello peerkoppla virtuellt nätverk. Du kan välja **inaktiverad** om du har ett virtuellt nätverk med ett annat virtuellt nätverk är peerkopplat, men ibland vill toodisable trafikflödet mellan hello två virtuella nätverk. Du kan hitta aktivering/inaktivering av är mycket enklare än att ta bort och återskapa peerkopplingar. När den här inställningen inaktiveras inte trafiken mellan hello peerkoppla virtuella nätverk.
    - **Tillåt vidarebefordrad trafik:** Kontrollera virtuella nätverk (trafik som inte har sitt ursprung i hello peerkoppla virtuella nätverk) är den här rutan tooallow trafik vidarebefordras toohello peerkopplat tooflow toothis virtuellt nätverk. Trafik vidarebefordran är vanliga när du har distribuerat en virtuell nätverksenhet i hello virtuellt nätverk du är peering med och skapa användardefinierade vägar tooforward trafik via hello virtuell nätverksenhet. Om du lämnar den här kryssrutan avmarkerad (standard), kan inte vidarebefordras från hello peerkoppla virtuellt nätverk trafiken toothis virtuellt nätverk. Med den här funktionen gör kan hello vidarebefordrad trafik via hello peering, det inte skapar någon användardefinierade vägar och nätverks-virtuella installationer. Användardefinierade vägar och virtuella nätverksenheter skapas separat. Lär dig mer om [användardefinierade vägar](virtual-networks-udr-overview.md).
    - **Tillåt gateway överföring:** den här kryssrutan om du har ett virtuellt nätverk för virtuella nätverk gateway bifogade toothis och vill tooallow trafik från hello peerkoppla virtuellt nätverk tooflow via hello gateway. Det här virtuella nätverket kanske till exempel bifogade tooan lokalt nätverk via en virtuell nätverksgateway. Kontroll av den här rutan tillåter trafik från hello peerkoppla virtuellt nätverk tooflow via hello gateway bifogade toothis virtuellt nätverk. Om du markerar kryssrutan får hello peerkoppla virtuellt nätverk ha en gateway som konfigurerats. Hej peerkoppla virtuella nätverket måste ha hello **Använd fjärrgatewayen** markerad när konfigurerar hello peering från hello andra virtuella nätverket toothis virtuellt nätverk. Om du lämnar den här kryssrutan avmarkerad (standard), trafik från hello peerkoppla virtuella nätverk fortfarande flödar toothis virtuellt nätverk, men det går inte att flöda genom ett virtuellt nätverk för virtuella nätverk gateway bifogade toothis. Lär dig mer om [virtuella nätverksgatewayerna](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#s2smulti). 
    
    Du kan inte aktivera det här alternativet om du peering ett virtuellt nätverk (Resource Manager) med ett virtuellt nätverk (klassiskt). Även om hello trafiken flödar mellan hello två virtuella nätverk, kan inte hello virtuella nätverk (klassiskt) trafiken via nätverket gateway bifogade toohello virtuella nätverk (Resource Manager).
    - **Använd remote gateways:** Kontrollera den här rutan tooallow trafik från den här virtuella nätverket tooflow via ett virtuellt nätverk gateway bifogade toohello virtuellt nätverk du peering med. Har till exempel hello virtuella nätverk som du peering med en VPN-gateway som är ansluten som möjliggör kommunikation tooan lokalt nätverk.  Markera den här rutan tillåter trafik från den här virtuella nätverket tooflow via hello VPN-gateway bifogade toohello peerkoppla virtuellt nätverk. Om du markerar kryssrutan hello peerkoppla virtuella nätverk måste ha ett virtuellt nätverk gateway kopplade tooit och måste ha hello **Tillåt gateway överföring** kryssrutan är markerad. Om du lämnar den här kryssrutan avmarkerad (standard), trafik från hello peerkoppla virtuella nätverk fortfarande kan flöda toothis virtuella nätverk, men det går inte att flöda genom ett virtuellt nätverk för virtuella nätverk gateway bifogade toothis. 
    
    Du kan inte aktivera det här alternativet om du peering ett virtuellt nätverk (Resource Manager) med ett virtuellt nätverk (klassiskt). Även om hello trafiken flödar mellan hello två virtuella nätverk, kan inte hello virtuella nätverk (Resource Manager) trafiken via nätverket gateway bifogade toohello virtuella nätverk (klassiskt).
7. Klicka på hello **OK** knappen tooadd hello toohello virtuella undernätverk du valt.

### <a name="commands"></a>Kommandon

|Verktyget|Kommando|
|---|---|
|CLI|[Skapa AZ network vnet-peering](/cli/azure/network/vnet/peering#create?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Lägg till AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/add-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json)|


### <a name="scenarios"></a>Scenarier

Ett virtuellt nätverk som peering skapas mellan virtuella nätverk som skapats via hello samma eller olika distributionsmodeller som finns i hello samma eller olika prenumerationer. Utför en stegvis självstudiekurs för en hello följande scenarier:
 
|Azure-distributionsmodell  | Prenumeration  |
|---------|---------|
|Båda Resource Manager |[Samma](virtual-network-create-peering.md)|
| |[Olika](create-peering-different-subscriptions.md)|
|En Resource Manager, en klassisk     |[Samma](create-peering-different-deployment-models.md)|
| |[Olika](create-peering-different-deployment-models-subscriptions.md)|

## <a name="view-or-change-peering-settings"></a>Visa eller ändra peering inställningar

1. Logga in toohello [portal](https://portal.azure.com) med ett konto som har tilldelats hello behov [eller behörigheter](#permissions).
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *virtuella nätverk*. När **virtuella nätverken** visas i sökresultaten hello klickar du på den.
3. I hello **virtuella nätverken** bladet som visas, klicka på hello virtuella nätverk som du vill toocreate en peering för.
4. Klicka i hello-fönstret som visas för hello virtuella nätverk som du har valt **Peerkopplingar** i hello **inställningar** avsnitt.
5. Klicka på hello peering du vill tooview eller ändra inställningarna för.
6. Ändra hello lämplig inställning. Läs mer om hello alternativ för varje inställning i [steg 6](#add-peering) av hello skapar du en peering avsnitt i den här artikeln. 

    >[!NOTE]
    >Det finns flera krav, villkor och överväganden toosuccessfully skapa ett virtuellt nätverk som peering. Innan du skapar en peering, se till att du har bekantat dig med hello [kraven och begränsningarna](#requirements-and-constraints) och [nödvändiga behörigheter](#permissions).
    >

7. Klicka på **Spara**.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|CLI|[AZ vnet peering nätverkslistan](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#list) toolist peerkopplingar för ett virtuellt nätverk [az network vnet-peering visa](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#show) tooshow inställningar för en specifik peering och [az network vnet-peering uppdatering](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#update) toochange peering inställningar.|
|PowerShell|[Get-AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/get-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json) tooretrieve visa inställningarna för peering och [Set AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/set-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json) toochange inställningar.|

## <a name="delete-a-peering"></a>Ta bort en peering
När en peering tas bort flödar trafik från ett virtuellt nätverk längre toohello peerkoppla virtuellt nätverk. När virtuella nätverk som distribuerats via Resource Manager är peerkopplat har varje virtuellt nätverk en peering toohello andra virtuella nätverket. Om du tar bort hello peering från ett virtuellt nätverk inaktiveras hello kommunikation mellan hello virtuella nätverk, tas inte bort hello peering från hello andra virtuella nätverket. Hej peering status för hello peering som finns i hello andra virtuella nätverket är **frånkopplad**. Du kan inte återskapa hello peering tills du återskapa hello peering i hello första virtuella nätverket och hello peering status för både virtuella nätverk ändringar för*ansluten*. 

Om du vill virtuella nätverk toocommunicate ibland, men inte alltid, i stället för att ta bort en peering, du kan ange hello **Tillåt åtkomst till virtuella nätverk** inställning för**inaktiverad** i stället. toolearn hur, läsa steg 6 i hello [skapa en peering](#create-peering) i den här artikeln. Du kanske inaktivera och aktivera nätverksåtkomst som är enklare än tas bort och återskapas peerkopplingar.

1. Logga in toohello [portal](https://portal.azure.com) med ett konto som har tilldelats hello behov [eller behörigheter](#permissions).
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *virtuella nätverk*. När **virtuella nätverken** visas i sökresultaten hello klickar du på den.
3. I hello **virtuella nätverken** bladet som visas, klicka på hello virtuella nätverk som du vill toodelete en peering från.
4. I hello bladet som visas för hello virtuella nätverk som du har valt klickar du på **Peerkopplingar** under **inställningar**.
5. På hello listan över peerkopplingar som visas i hello peerkopplingar bladet, högerklicka på hello peering du vill toodelete **ta bort**, sedan **Ja** toodelete hello peering från hello första virtuella nätverket.
6. Fullständig hello föregående steg toodelete hello peering från hello andra virtuella nätverket i hello peering.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|CLI|[AZ network vnet peering ta bort](/cli/azure/network/vnet/peering?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Ta bort AzureRmVirtualNetworkPeering](/powershell/module/azurerm.network/remove-azurermvirtualnetworkpeering?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="requirements-and-constraints"></a>Krav och begränsningar 

- hello virtuella nätverk som peer-du måste ha icke-överlappande IP-adressutrymmen.
- Du kan inte lägga till adressutrymmen till eller ta bort adressutrymmen från ett virtuellt nätverk när ett virtuellt nätverk är peerkopplat med ett annat virtuellt nätverk. tooadd eller ta bort adressutrymmen, ta bort hello peering, lägga till eller ta bort hello adressutrymmen och återskapa hello peering. tooadd adressens utrymmen till eller ta bort adressutrymmen från virtuella nätverk, läsa hello [skapa, ändra eller ta bort virtuella nätverk](virtual-network-manage-network.md#add-address-spaces) artikel. 
- Du kan peer-två virtuella nätverk som distribuerats via Hanteraren för filserverresurser eller ett virtuellt nätverk som distribuerats via Resource Manager med ett virtuellt nätverk som distribuerats via hello klassiska distributionsmodellen. Du kan inte peer-två virtuella nätverk som skapats via hello klassiska distributionsmodellen. Om du inte är bekant med Azure distributionsmodeller läsa hello [förstå Azure distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel. Du kan använda en [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) tooconnect två virtuella nätverk som skapats via hello klassiska distributionsmodellen.
- När peering två virtuella nätverk som skapats via Resource Manager måste en peering konfigureras för varje virtuellt nätverk i hello peering. När du skapar hello peering toohello andra virtuella nätverket från hello första virtuella nätverket hello peering status är *initierade*.  När du skapar hello peering från hello andra virtuella nätverket toohello första virtuella nätverket peering statusen är *ansluten*. Om du visar hello peering status för hello första virtuella nätverket kan du se statusen har ändrats från *initierade* för*ansluten*. Hej peering har upprättas inte förrän hello peering status för båda peerkopplingar mellan virtuella nätverk är *ansluten*. 
- När peering ett virtuellt nätverk som skapats via Resource Manager med ett virtuellt nätverk som skapats via hello klassiska distributionsmodellen konfigurera du bara en peering för hello virtuella nätverk som distribuerats via Resource Manager. Du kan inte konfigurera peering för ett virtuellt nätverk (klassiskt) eller mellan två virtuella nätverk som distribuerats via hello klassiska distributionsmodellen. När du skapar hello peering från hello virtuella nätverk (Resource Manager) toohello virtuella nätverk (klassiska) hello peering status är *uppdatering*, ändras sedan inom kort för*ansluten*.   
- En peering upprättas mellan två virtuella nätverk. Peerkopplingar är inte transitiva. Om du skapar peerkopplingar mellan:
    - VirtualNetwork1 & VirtualNetwork2
    - VirtualNetwork2 & VirtualNetwork3

  Det finns ingen peering mellan VirtualNetwork1 och VirtualNetwork3 via VirtualNetwork2. Om du vill toocreate ett virtuellt nätverk peering mellan VirtualNetwork1 och VirtualNetwork3 måste ha du toocreate en peering mellan VirtualNetwork1 och VirtualNetwork3.
- Du kan inte matcha namn i peerkoppla virtuella nätverk med hjälp av standard-Azure namnmatchning. tooresolve namn i andra virtuella nätverk, måste du använda en anpassad DNS-server. toolearn hur tooset upp din DNS-server för att läsa hello [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server) artikel.
- Resurser i båda virtuella nätverken i hello peering kan kommunicera med varandra med hello samma bandbredd och fördröjning som om de befann sig i hello samma virtuella nätverk. Storlek på varje virtuell dator har sin egen maximal nätverksbandbredd men. Mer om maximal nätverksbandbredd för olika virtuella datorstorlekar, läsa hello toolearn [Windows](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eller [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuella storlekar artiklar.
- -Peer-virtuella nätverk som distribuerats via Resource Manager som är i hello samma eller olika prenumerationer.
- -Peer-virtuella nätverk som distribuerats via olika distributionsmodeller som är i hello samma eller olika prenumerationer (förhandsversion). 
- hello prenumerationer som båda virtuella nätverken i måste vara associerad toohello samma Azure Active Directory-klient. Om du inte redan har en AD-klient, kan du snabbt [skapar du en](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). Du kan använda en [VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#V2V) tooconnect två virtuella nätverk som finns i olika prenumerationer som är associerade med toodifferent Active Directory-klienter.
- Ett virtuellt nätverk kan peerkoppla tooanother virtuella nätverk och även vara anslutna tooanother virtuellt nätverk med en gateway för virtuella Azure-nätverket. När virtuella nätverk är anslutna via peering och en gateway, flödar trafik mellan virtuella nätverk för hello via hello peering konfiguration i stället för hello gateway.
- En nominell avgift tas ut för ingående och utgående trafik som använder virtuell nätverks-peering. Mer information finns i hello [sida med priser](https://azure.microsoft.com/pricing/details/virtual-network).


## <a name="permissions"></a>Behörigheter

hello-konton som du använder ett virtuellt nätverk som peering toocreate måste ha hello behövs eller behörigheter. Till exempel om du peering två virtuella nätverk som heter myVnetA och myVnetB, måste ditt konto tilldelas hello följande minsta eller behörigheter för varje virtuellt nätverk:
    
|Virtuellt nätverk|Distributionsmodell|Roll|Behörigheter|
|---|---|---|---|
|myVnetA|Resource Manager|[Nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
| |Klassisk|[Klassisk nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Saknas|
|myVnetB|Resource Manager|[Nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|
||Klassisk|[Klassisk nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#classic-network-contributor)|Microsoft.ClassicNetwork/virtualNetworks/peer|

Lär dig mer om [inbyggda roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) och tilldela särskilda behörigheter för[anpassade roller](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (enbart Resource Manager).

## <a name="next-steps"></a>Nästa steg

Lär dig hur toocreate en [NAV och ekrar nätverkstopologi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) 
