---
title: "aaaCreate, ändra eller ta bort en Azure-nätverket | Microsoft Docs"
description: "Lär dig hur toocreate, ändra eller ta bort ett virtuellt nätverk i Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/10/2017
ms.author: jdial
ms.openlocfilehash: 7dfe6632753182eae2a13bb0327f03f75e03d057
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-change-or-delete-a-virtual-network"></a>Skapa, ändra eller ta bort ett virtuellt nätverk

Lär dig hur toocreate och ta bort virtuella nätverk och ändra inställningar, t.ex DNS-servrar och IP-adressutrymmen för ett befintligt virtuellt nätverk.

Ett virtuellt nätverk är en representation av ditt eget nätverk i hello molnet. Ett virtuellt nätverk är en logisk isolering av hello Azure-molnet som är dedikerad tooyour Azure-prenumeration. För varje virtuellt nätverk som du skapar, kan du:
- Välj en adressutrymme tooassign. Ett adressutrymme består av en eller flera adressintervall som definierats med Classless Inter-Domain Routing CIDR-notation som 10.0.0.0/16.
- Välj toouse hello Azure-tillhandahållna DNS-servern eller Använd DNS-servern. Alla resurser som är anslutna toohello virtuellt nätverk har tilldelats den här DNS-server tooresolve namn inom hello virtuellt nätverk.
- Segment hello virtuellt nätverk i undernät, var och en med sin egen adressintervall inom hello adressutrymme hello virtuellt nätverk. hur toocreate, ändra och ta bort undernät, se toolearn [Lägg till, ändra eller ta bort undernät](virtual-network-manage-subnet.md).

Den här artikeln förklarar hur toocreate, ändra och ta bort virtuella nätverk med hjälp av hello Azure Resource Manager-distributionsmodellen.

## <a name="before"></a>Innan du börjar

Slutför hello följande krav innan du börjar hello uppgifter som beskrivs i den här artikeln:

- Om du är ny tooworking med virtuella nätverk, rekommenderar vi att du läser hello Övning i [skapa ditt första Azure-nätverk](virtual-network-get-started-vnet-subnet.md). Den här övningen hjälper dig att bekanta dig med virtuella nätverk.
- toolearn om gränser för virtuella nätverk, granska [Azure gränser](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Logga in toohello Azure-portalen, hello Azure-kommandoradsverktyget (Azure CLI) eller Azure PowerShell med hjälp av Azure-konto. Om du inte har ett Azure-konto kan du registrera dig för en [ledigt utvärderingskonto](https://azure.microsoft.com/free).
- Om du planerar toouse PowerShell-kommandon till toocomplete hello uppgifter som beskrivs i den här artikeln, måste du först [installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har hello senaste versionen av hello Azure PowerShell-cmdlets som är installerad. Ange tooget hjälp för PowerShell-kommandon i hello exempel `get-help <command> -full`.
- Om du planerar toouse Azure CLI-kommandon toocomplete hello uppgifter som beskrivs i den här artikeln, måste du först [installera och konfigurera Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har hello senaste versionen av Azure CLI är installerad. Ange tooget hjälp med Azure CLI-kommandona `az <command> --help`.


## <a name="create-vnet"></a>Skapa ett virtuellt nätverk

toocreate ett virtuellt nätverk:

1. Logga in toohello [portal](https://portal.azure.com) med ett konto som har tilldelats behörigheter för hello nätverket deltagarrollen (minst) för din prenumeration. toolearn mer information om hur du tilldelar roller och behörigheter tooaccounts finns [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Klicka på **ny** > **nätverk** > **för virtuella nätverk**.
3. På hello **för virtuella nätverk** bladet i hello **Välj en distributionsmodell** och lämna **Resource Manager** markerad och klicka sedan på **skapa**.
4. På hello **skapa virtuellt nätverk** bladet ange eller Välj värden för hello följande inställningar och sedan klickar du på **skapa**:
    - **Namnet**: hello-namnet måste vara unikt i hello [resursgruppen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) att du väljer toocreate hello virtuellt nätverk i. Du kan inte ändra hello namn efter hello virtuella nätverket har skapats. Du kan skapa flera virtuella nätverk över tid. Namnge förslag finns [namngivningskonventioner](/azure/architecture/best-practices/naming-conventions.md?toc=%2fazure%2fvirtual-network%2ftoc.json#naming-rules-and-restrictions). Följa en namngivningskonvention kan hjälpa dig att göra det enklare toomanage flera virtuella nätverk.
    - **Adressutrymmet**: Ange hello-adressutrymme i CIDR-notering. hello-adressutrymmet som du definierar kan vara public eller private (RFC 1918). Om du definierar hello-adressutrymmet som offentligt eller privat hello-adressutrymmet kan nås från hello virtuellt nätverk, virtuella nätverk och från alla lokala nätverk som du har anslutit toohello virtuellt nätverk. Du kan inte lägga till följande adressutrymmen hello:
        - 224.0.0.0/4 (Multicast)
        - 255.255.255.255/32 (Broadcast)
        - 127.0.0.0/8 (Loopback)
        - 169.254.0.0/16 (Link-local)
        - 168.63.129.16/32 (interna DNS)

      Även om du kan definiera en enda adressutrymme när du skapar hello virtuella nätverk, du kan lägga till flera adressutrymmen efter hello virtuella nätverket har skapats. toolearn hur tooadd en adress utrymme tooan befintligt virtuellt nätverk finns i [Lägg till eller ta bort ett adressutrymme](#add-address-spaces) i den här artikeln.

      >[!WARNING]
      >Om ett virtuellt nätverk innehåller adressutrymmen som överlappar ett annat virtuellt nätverk eller lokala nätverk, går inte att ansluta hello två nätverk. Överväg att om du kanske vill tooconnect hello virtuellt nätverk tooother virtuella nätverk eller lokala nätverk i hello framtida innan du definierar ett adressutrymme.
      >
      >

    - **Undernätnamnet**: hello undernätsnamn måste vara unika inom hello virtuellt nätverk. Du kan inte ändra hello undernätsnamn efter hello-undernät skapades. hello-portalen måste du definiera ett undernät när du skapar ett virtuellt nätverk, även om ett virtuellt nätverk är inte obligatoriska toohave några undernät. Du kan definiera bara ett undernät när du skapar ett virtuellt nätverk i hello-portalen. Du kan lägga till flera undernät toohello virtuella nätverk senare, när hello virtuella nätverket har skapats. tooadd en tooa virtuella undernätverk, se [skapar du ett undernät](virtual-network-manage-subnet.md#create-subnet) i [skapa, ändra eller ta bort undernät](virtual-network-manage-subnet.md). Du kan skapa ett virtuellt nätverk som har flera undernät med hjälp av Azure CLI eller PowerShell.

      >[!TIP]
      >Ibland skapa administratörer olika undernät toofilter eller kontroll trafik routning mellan undernät hello. Innan du definierar undernät, fundera på hur du kanske vill toofilter och vidarebefordra trafik mellan dina undernät. toolearn mer information om hur du filtrerar trafik mellan undernät, se [Nätverkssäkerhetsgrupper](virtual-networks-nsg.md). Azure automatiskt vägar trafik mellan undernät, men du kan åsidosätta Azure standardvägar. toolearn hur toooverride Azure standard undernätet routning av nätverkstrafik, finns i [användardefinierade vägar](virtual-networks-udr-overview.md).
      >

    - **Adressintervall för gatewayundernät**: hello intervallet måste vara inom hello adressutrymme som du angav för hello virtuellt nätverk. hello minsta intervall som du kan ange är /29, vilket möjliggör åtta IP-adresser för hello undernät. Azure reserver hello första och sista adressen i varje undernät för protokollet överensstämmelse. Tre ytterligare adresser är reserverade för användning i Azure-tjänsten. Därför har ett virtuellt nätverk med ett adressintervall för undernätet för /29 endast tre användbara IP-adresser. Om du planerar tooconnect ett virtuellt nätverk tooa VPN-gateway, måste du skapa en gateway-undernätet. Lär dig mer om [specifik adressintervallet överväganden för gateway-undernät](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Du kan ändra hello-adressintervall efter hello undernät skapas vissa villkor. hur toochange ett undernät-adressintervall, se toolearn [ändra undernätsinställningar](#change-subnet) i [Lägg till, ändra eller ta bort undernät](virtual-network-manage-subnet.md).
    - **Prenumerationen**: Välj en [prenumeration](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription). Du kan inte använda hello samma virtuella nätverk i mer än en Azure-prenumeration. Du kan dock ansluta ett virtuellt nätverk i en prenumeration toovirtual nätverk i andra prenumerationer. tooconnect virtuella nätverk för olika prenumerationer använder Azure VPN-Gateway eller virtuella nätverk peering. Azure-resurs som du ansluter toohello virtuella nätverket måste finnas i hello samma prenumeration som hello virtuellt nätverk.
    - **Resursgruppen**: Välj en befintlig [resursgruppen](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-groups) eller skapa en ny. En Azure-resurs som du ansluter toohello virtuella nätverk kan vara i hello samma resursgrupp som hello virtuellt nätverk eller i en annan resursgrupp.
    - **Plats**: Välj en Azure [plats](https://azure.microsoft.com/regions/), även kallad en region. Ett virtuellt nätverk kan vara i endast en Azure-plats. Du kan dock ansluta ett virtuellt nätverk i en plats tooa virtuella nätverk på en annan plats med hjälp av en VPN-gateway. Azure-resurs som du ansluter toohello virtuella nätverket måste finnas i hello samma plats som hello virtuellt nätverk.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|Azure CLI|[Skapa AZ network vnet](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Ny AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name = "view-vnet"></a>Visa virtuella nätverk och inställningar

tooview virtuella nätverk och inställningar:

1. Logga in toohello [portal](https://portal.azure.com) med ett konto som har tilldelats behörigheter för hello nätverket deltagarrollen (minst) för din prenumeration. toolearn mer information om hur du tilldelar roller och behörigheter tooaccounts finns [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Ange i hello portal sökrutan **virtuella nätverk**. I hello sökresultaten klickar du på **virtuella nätverken**.
3. På hello **virtuella nätverken** bladet, klickar du på hello virtuellt nätverk som du vill att tooview inställningar för.
4. hello följande inställningar finns i listan över hello bladet för hello virtuella nätverk som du har valt:
    - **Översikt över**: innehåller information om hello virtuellt nätverk, inklusive adressutrymme och DNS-servrar. hello följande skärmbild visar hello översikt över inställningar för ett virtuellt nätverk med namnet **MyVNet**:

        ![Översikt över nätverk-gränssnitt](./media/virtual-network-manage-network/vnet-overview.png)

      På hello **översikt** bladet kan du flytta en virtuell tooa olika prenumerationen eller resursen nätverksgrupp. hur toomove ett virtuellt nätverk, se toolearn [flytta resurser tooa annan resursgrupp eller prenumeration](../azure-resource-manager/resource-group-move-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json). hello artikeln förutsättningar och hur toomove resurser med hjälp av hello Azure-portalen, PowerShell och Azure CLI. Alla resurser som är anslutna toohello virtuella nätverk måste flytta till hello virtuellt nätverk.
    - **Adressutrymmet**: hello-adressutrymmen som tilldelas toohello virtuella nätverk visas. toolearn hur tooadd och tar bort ett adressutrymme slutföra hello stegen i [Lägg till eller ta bort ett adressutrymme](#address-spaces) i den här artikeln.
    - **Anslutna enheter**: visas alla resurser som är anslutna toohello virtuella nätverk. I föregående skärmbild hello, är tre nätverksgränssnitt och en belastningsutjämnare ansluten toohello virtuellt nätverk. Nya resurser som du skapar och ansluta toohello virtuella nätverk visas. Om du tar bort en resurs som var anslutna toohello virtuella nätverk visas den inte längre i hello-listan.
    - **Undernät**: visas en lista över undernät som finns inom hello virtuellt nätverk. hur tooadd och tar bort ett undernät, se toolearn [skapar du ett undernät](virtual-network-manage-subnet.md#create-subnet) och [tar bort ett undernät](virtual-network-manage-subnet.md#delete-subnet) i [Lägg till, ändra eller ta bort undernät](virtual-network-manage-subnet.md).
    - **DNS-servrar**: du kan ange om hello Azure intern DNS-server eller en anpassad DNS-server ger namnmatchning för enheter som är anslutna toohello virtuellt nätverk. När du skapar ett virtuellt nätverk med hjälp av hello Azure-portalen används Azure DNS-servrar för namnmatchning i ett virtuellt nätverk som standard. toomodify hello DNS-servrar, fullständig hello stegen i [Lägg till, ändra eller ta bort en DNS-server](#dns-servers) i den här artikeln.
    - **Peerkopplingar**: om det finns befintliga peerkopplingar hello prenumeration, anges här. Du kan visa inställningarna för befintliga peerkopplingar eller skapa, ändra eller ta bort peerkopplingar. toolearn mer om peerkopplingar, se [virtuella nätverk peering](virtual-network-peering-overview.md).
    - **Egenskaper för**: Visar inställningar om hello virtuellt nätverk, inklusive resurs-ID för hello virtuella nätverk och hello Azure-prenumeration i.
    - **Diagram över**: hello diagrammet innehåller en bild av alla enheter som är anslutna toohello virtuellt nätverk. hello diagram har vissa viktig information om hello-enheter. toomanage en enhet i hello diagram i den här vyn klickar du på hello enheten.
    - **Gemensamma inställningar för Azure**: toolearn mer om Azure standardinställningarna finns hello följande information:
        *   [Aktivitetslogg](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#activity-logs)
        *   [Åtkomstkontroll (IAM)](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#access-control)
        *   [Taggar](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#tags)
        *   [Lås](../azure-resource-manager/resource-group-lock-resources.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
        *   [Automation-skript](../azure-resource-manager/resource-manager-export-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json#export-the-template-from-resource-group)

**Kommandon**

|Verktyget|Kommando|
|---|---|
|Azure CLI|[AZ network vnet show](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVirtualNetwork](/powershell/module/azurerm.network/get-azurermvirtualnetwork/?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="add-address-spaces"></a>Lägg till eller ta bort ett adressutrymme

Du kan lägga till och ta bort adressutrymmen för ett virtuellt nätverk. Ett adressutrymme måste anges i CIDR-notation och kan inte överlappa andra adressutrymmen inom hello samma virtuella nätverk. hello-adressutrymmen som du definierar kan vara public eller private (RFC 1918). Om du definierar hello-adressutrymmet som offentligt eller privat hello-adressutrymmet kan nås från hello virtuellt nätverk, virtuella nätverk och från alla lokala nätverk som du har anslutit toohello virtuellt nätverk. Du kan inte lägga till följande adressutrymmen hello:

- 224.0.0.0/4 (Multicast)
- 255.255.255.255/32 (Broadcast)
- 127.0.0.0/8 (Loopback)
- 169.254.0.0/16 (Link-local)
- 168.63.129.16/32 (interna DNS)

tooadd eller ta bort ett adressutrymme:

1. Logga in toohello [portal](https://portal.azure.com) med ett konto som har tilldelats behörigheter för hello nätverket deltagarrollen (minst) för din prenumeration. toolearn mer information om hur du tilldelar roller och behörigheter tooaccounts finns [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Ange i hello portal sökrutan **virtuella nätverk**. I sökresultaten hello väljer **virtuella nätverken**.
3. På hello **virtuella nätverken** bladet, klickar du på hello virtuella nätverk som du vill tooadd eller ta bort ett adressutrymme.
4. På hello virtuella nätverk bladet under **inställningar**, klickar du på **adressutrymmet**.
5. Gör något av följande alternativ för hello på hello bladet för hello-adressutrymmet:
    - **Lägg till ett adressutrymme**: Ange hello nytt adressutrymme. hello-adressutrymmet kan inte överlappa ett befintligt adressutrymme som definieras för hello virtuellt nätverk.
    - **Ta bort ett adressutrymme**: Högerklicka på ett adressutrymme och klicka sedan på **ta bort**. Om ett undernät som finns i hello adressutrymme, kan du inte ta bort hello-adressutrymme. tooremove ett adressutrymme, måste du först ta bort alla undernät (och alla resurser som är anslutna toohello undernät) som finns i hello-adressutrymme.
6. Klicka på **Spara**.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|Azure CLI|Endast Resource Manager|[AZ network vnet uppdatering](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="dns-servers"></a>Lägga till, ändra eller ta bort en DNS-server

Alla virtuella datorer som är anslutna toohello virtuella nätverk registrera med hello DNS-servrar som du anger för hello virtuella nätverk. De även använda hello angivna DNS-servern för namnmatchning. Varje nätverksgränssnitt (NIC) på en virtuell dator kan ha sin egen DNS-serverinställningarna. Om ett nätverkskort har sin egen DNS-serverinställningarna, åsidosätta de hello DNS-serverinställningarna för hello virtuellt nätverk. toolearn mer information om NIC DNS-inställningar, se [Network interface-aktiviteter och inställningar](virtual-network-network-interface.md#change-dns-servers). toolearn mer information om namnmatchning för virtuella datorer och rollinstanser i Azure Cloud Services, se [namnmatchning för virtuella datorer och rollinstanser](virtual-networks-name-resolution-for-vms-and-role-instances.md). tooadd, ändra eller ta bort en DNS-server:

1. Logga in toohello [portal](https://portal.azure.com) med ett konto som har tilldelats behörigheter för hello nätverket deltagarrollen (minst) för din prenumeration. toolearn mer information om hur du tilldelar roller och behörigheter tooaccounts finns [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Skriv i hello portal-sökning **virtuella nätverk**. I sökresultaten hello väljer **virtuella nätverken**.
3. På hello **virtuella nätverken** bladet, klickar du på hello virtuella nätverk som du vill toochange DNS-inställningarna för.
4. På hello virtuella nätverk bladet under **inställningar**, klickar du på **DNS-servrar**.
5. Välj något av följande alternativ på hello blad som visar DNS-servrar hello:
    - **Standard (Azure-tillhandahållna)**: alla resursnamn och privata IP-adresser är automatiskt registrerad toohello Azure DNS-servrar. Du kan lösa namn mellan alla resurser som är anslutna toohello samma virtuella nätverk. Du kan inte använda det här alternativet tooresolve namn mellan virtuella nätverk. tooresolve namn mellan virtuella nätverk, måste du använda en anpassad DNS-server.
    - **Anpassad**: du kan lägga till en eller flera servrar, in toohello Azure begränsa för ett virtuellt nätverk. toolearn mer om DNS-serverns gränser, se [Azure gränser](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#virtual-networking-limits-classic). Du har hello följande alternativ:
        - **Lägga till en adress**: lägger till hello tooyour virtuellt nätverk DNS-servrar i listan över. Det här alternativet registrerar också hello DNS-server med Azure. Om du redan har registrerat en DNS-server med Azure väljer du DNS-servern i hello-listan.
        - **Ta bort en adress**: Klicka på nästa toohello server som du vill tooremove, **X**. Ta bort hello-server tar du bort hello server endast från den här listan över virtuella nätverk. hello DNS-server är registrerad i Azure för andra virtuella nätverk-toouse.
        - **Ändra ordning på DNS-serveradresser**: det är viktigt att du anger DNS-servrarna i hello tooverify korrigera för din miljö. DNS-serverlistor används i hello ordning som de anges. De fungerar inte som en inställning för resursallokering. Om hello första DNS-servern i listan hello kan nås, använder hello klienten den DNS-servern, oavsett hello DNS-servern fungerar korrekt. Ta bort alla hello DNS-servrar som listas och Lägg sedan till dem i hello ordning du vill ha.
        - **Ändra en adress**: Markera hello DNS-servern i hello lista och sedan ange hello nytt namn.
6. Klicka på **Spara**.
7. Starta om hello virtuella datorer som är anslutna toohello virtuellt nätverk, så tilldelas de hello nya DNS-serverinställningarna. Virtuella datorer fortsätter toouse deras aktuella DNS-inställningar förrän de startas om.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|Azure CLI|[AZ network vnet uppdatering](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Set-AzureRmVirtualNetwork](/powershell/module/azurerm.network/set-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="delete-vnet"></a>Ta bort ett virtuellt nätverk

Du kan ta bort ett virtuellt nätverk om det finns inga anslutna tooit resurser. Om det finns resurser anslutna tooany undernät inom hello virtuellt nätverk, måste du först radera hello-resurser som är anslutna tooall undernät i hello virtuellt nätverk. hello steg du vidta toodelete en resurs varierar beroende på hello resurs. toolearn hur toodelete resurser som är anslutna toosubnets läsa hello dokumentationen för varje resurstyp av du vill toodelete. toodelete ett virtuellt nätverk:

1. Logga in toohello [portal](https://portal.azure.com) med ett konto som är tilldelade (minst) behörigheter för hello nätverket deltagarrollen för din prenumeration. toolearn mer information om hur du tilldelar roller och behörigheter tooaccounts finns [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Ange i hello portal sökrutan **virtuella nätverk**. I hello sökresultaten klickar du på **virtuella nätverken**.
3. På hello **virtuella nätverken** bladet, Välj hello virtuella nätverk som du vill toodelete.
4. På hello virtuellt nätverksblad tooconfirm att det finns inga enheter anslutna toohello virtuellt nätverk under **inställningar**, klickar du på **anslutna enheter**. Om det finns anslutna enheter, måste du ta bort dem innan du kan ta bort hello virtuellt nätverk. Om det finns inga anslutna enheter, klickar du på **översikt**.
5. Hello överkant hello-bladet, klickar du på hello **ta bort** ikon.
6. tooconfirm hello borttagning av hello virtuellt nätverk, klickar du på **Ja**.


**Kommandon**

|Verktyget|Kommando|
|---|---|
|Azure CLI|[ta bort Azure-nätverk-vnet](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Ta bort-AzureRmVirtualNetwork](/powershell/module/azurerm.network/remove-azurermvirtualnetwork?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="next-steps"></a>Nästa steg

- toocreate en virtuell dator och anslut sedan tooa virtuellt nätverk, se [skapa ett virtuellt nätverk och ansluta virtuella datorer](virtual-network-get-started-vnet-subnet.md#create-vms).
- toofilter nätverkstrafik mellan undernät i ett virtuellt nätverk finns [skapa nätverkssäkerhetsgrupper](virtual-networks-create-nsg-arm-pportal.md).
- toopeer ett virtuellt nätverk tooanother virtuellt nätverk, se [skapa ett virtuellt nätverk som peering](virtual-network-create-peering.md#portal).
- toolearn om alternativ för att ansluta ett virtuellt nätverk tooan lokalt nätverk, se [om VPN-Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json#diagrams).
