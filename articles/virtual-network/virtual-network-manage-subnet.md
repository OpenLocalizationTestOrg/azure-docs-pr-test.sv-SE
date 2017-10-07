---
title: "aaaAdd, ändra eller ta bort ett undernät för virtuella Azure-nätverket | Microsoft Docs"
description: "Lär dig hur tooadd, ändra eller ta bort ett undernät för virtuellt nätverk i Azure."
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
ms.openlocfilehash: 0d6d813b7e2fc52a00a87f6c6714ab5b7ca589ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-change-or-delete-a-virtual-network-subnet"></a>Lägga till, ändra eller ta bort ett undernät för virtuellt nätverk

Lär dig hur tooadd, ändra eller ta bort ett undernät för virtuellt nätverk. 

Om du inte är bekant med virtuella nätverk, innan du lägger till, ändra eller ta bort ett undernät, rekommenderar vi att du läser [översikt över Azure Virtual Network](virtual-networks-overview.md) och [skapa, ändra eller ta bort ett virtuellt nätverk](virtual-network-manage-network.md). Alla Azure-resurser som har distribuerats till ett virtuellt nätverk har distribuerats i ett undernät i ett virtuellt nätverk. Flera undernät skapas vanligtvis inom ett virtuellt nätverk till:
- **Filtrera trafik mellan undernät**. Du kan använda network security grupper toosubnets toofilter inkommande och utgående nätverkstrafik för alla resurser (t.ex. virtuella datorer) som finns i hello virtuella nätverk. toolearn mer om hur toocreate en nätverkssäkerhetsgrupp Se [skapa nätverkssäkerhetsgrupper](virtual-networks-create-nsg-arm-pportal.md).
- **Kontrollera routning mellan undernät**. Azure skapar standardvägar så att trafik dirigeras automatiskt mellan undernät. Du kan åsidosätta Azure standardvägar genom att skapa användardefinierade vägar. toolearn mer information om användardefinierade vägar finns [skapar användardefinierade vägar](virtual-network-create-udr-arm-ps.md). 

Den här artikeln förklarar hur tooadd, ändra och ta bort ett undernät för virtuella nätverk som har skapats med hjälp av hello Azure Resource Manager-distributionsmodellen.
 
## <a name="before"></a>Innan du börjar

Slutför hello följande krav innan du börjar hello uppgifter som beskrivs i den här artikeln:

- Om du är ny tooworking med virtuella nätverk, rekommenderar vi att du läser hello Övning i [skapa ditt första Azure-nätverk](virtual-network-get-started-vnet-subnet.md). Den här övningen hjälper dig att bekanta dig med virtuella nätverk.
- toolearn om gränser för virtuella nätverk, granska [Azure gränser](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- Logga in toohello Azure-portalen, hello Azure-kommandoradsverktyget (Azure CLI) eller Azure PowerShell med hjälp av Azure-konto. Om du inte har ett Azure-konto kan du registrera dig för en [ledigt utvärderingskonto](https://azure.microsoft.com/free).
- Om du planerar toouse PowerShell-kommandon till toocomplete hello uppgifter som beskrivs i den här artikeln, måste du först [installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har hello senaste versionen av hello Azure PowerShell-cmdlets som är installerad. Ange tooget hjälp för PowerShell-kommandon i hello exempel `get-help <command> -full`.
- Om du planerar toouse Azure CLI-kommandon toocomplete hello uppgifter som beskrivs i den här artikeln, måste du:
    - [Installera och konfigurera hello Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har hello senaste versionen av Azure CLI är installerad.
    - Använd hello Azure Cloud-gränssnittet. Du kan använda hello Azure Cloud Shell istället för att installera hello CLI och dess beroenden. hello Azure Cloud-gränssnittet är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i hello Azure-portalen. Den har hello Azure CLI förinstallerat och konfigurerats toouse med ditt konto. toouse hello molnet Shell, klicka på hello molnet Shell (**> _**) ikonen hello överst i hello Azure-portalen. 

  Ange tooget hjälp med Azure CLI-kommandona `az <command> --help`.

## <a name="create-subnet"></a>Lägg till ett undernät

tooadd ett undernät:

1. Logga in toohello [portal](https://portal.azure.com) med ett konto som har tilldelats behörigheter för hello nätverket deltagarrollen (minst) för din prenumeration. toolearn mer information om hur du tilldelar roller och behörigheter tooaccounts finns [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Ange i hello portal sökrutan **virtuella nätverk**. I hello sökresultaten klickar du på **virtuella nätverken**.
3. På hello **virtuella nätverken** bladet, klickar du på hello virtuella nätverk som du vill tooadd ett undernät till.
4. På hello virtuellt nätverk-bladet, klickar du på **undernät**.
5. Klicka på **+ undernät**.
6. På hello **Lägg till undernät** bladet ange värden för hello följande parametrar:
    - **Namnet**: hello-namnet måste vara unikt inom hello virtuellt nätverk.
    - **Adressintervall**: hello intervallet måste vara unika inom hello adressutrymmet för hello virtuellt nätverk. hello-intervallet får inte överlappa med andra undernät adressintervall inom hello virtuellt nätverk. hello-adressutrymmet måste anges med hjälp av Classless Inter-Domain Routing CIDR-notering. Du kan till exempel definiera en undernätsadressutrymmet 10.0.0.0/24 i ett virtuellt nätverk med adressutrymme 10.0.0.0/16. hello minsta intervall som du kan ange är /29, vilket möjliggör åtta IP-adresser för hello undernät. Azure reserver hello första och sista adressen i varje undernät för protokollet överensstämmelse. Tre ytterligare adresser är reserverade för användning i Azure-tjänsten. Därför definiera ett undernät med en /29 resulterar i tre användbara IP-adresser i undernätet hello-adress. Om du planerar tooconnect ett virtuellt nätverk tooa VPN-gateway, måste du skapa en gateway-undernätet. Lär dig mer om [specifik adressintervallet överväganden för gateway-undernät](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Du kan ändra hello-adressintervall efter hello undernät läggs vissa villkor. hur toochange ett undernät-adressintervall, se toolearn [ändra undernätsinställningar](#change-subnet) i den här artikeln.
    - **Nätverkssäkerhetsgruppen**: Om du vill kan du kan associera en befintlig säkerhetsgrupp för nätverk med hello undernät toocontrol inkommande och utgående filtrering av nätverkstrafik för hello undernät. Hej nätverkssäkerhetsgruppen måste finnas i hello samma prenumeration och plats som hello virtuellt nätverk. Den måste också skapas med hjälp av hello Resource Manager-modellen. toolearn mer om hur toocreate en nätverkssäkerhetsgrupp Se [Nätverkssäkerhetsgrupper](virtual-networks-create-nsg-arm-pportal.md).
    - **Routningstabellen**: (valfritt) du kan koppla en befintlig routningstabellen med hello undernät toocontrol nätverkstrafik routning tooother nätverk. hello routningstabellen måste finnas i hello samma prenumeration och plats som hello virtuellt nätverk. Den måste också skapas med hjälp av hello Resource Manager-modellen. toolearn mer om hur toocreate vägtabeller Se [användardefinierade vägar](virtual-network-create-udr-arm-ps.md).
    - **Användare**: du kan styra åtkomst toohello undernätet med hjälp av inbyggda roller eller dina egna anpassade roller. toolearn mer information om hur du tilldelar roller och användare tooaccess hello undernät, se [använda rollen tilldelning toomanage åtkomst tooyour Azure resurser](../active-directory/role-based-access-control-configure.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-access).
7. tooadd hello toohello virtuella undernätverk som du har valt klickar du på **OK**.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|Azure CLI|[Skapa AZ undernät för virtuellt nätverk](/cli/azure/network/vnet/subnet?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Nya AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json), [lägga till AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/add-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="change-subnet"></a>Ändra inställningar för undernätet

Du kan ändra nätverkssäkerhetsgrupper och vägtabeller användaren åtkomst tooa undernät genom att hantera resurser som är i ett undernät. toolearn om dessa inställningar i [Lägg till ett undernät](#create-subnet), finns i steg 6. Om du vill toochange hello-adressutrymme i ett undernät måste du först bort alla resurser som finns i hello undernät. hello steg du vidta toodelete en resurs varierar beroende på hello resurs. toolearn hur toodelete resurser som finns i undernät, skrivskyddade hello dokumentationen för varje resurs typen som du vill toodelete. toochange hello-adressintervallet för ett undernät:

1. Logga in toohello [portal](https://portal.azure.com) med ett konto som har tilldelats behörigheter för hello nätverket deltagarrollen (minst) för din prenumeration. toolearn mer information om hur du tilldelar roller och behörigheter tooaccounts finns [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Ange i hello portal sökrutan **virtuella nätverk**. I hello sökresultaten klickar du på **virtuella nätverken**.
3. På hello **virtuella nätverken** bladet, klickar du på hello virtuella nätverk som du vill toochange ett adressintervall för undernätet.
4. Klicka på hello undernät som du vill toochange hello-adressintervall.
5. Hello undernät-bladet, i hello **adressintervall** ange hello nya adressintervall. hello-intervallet måste vara unika inom hello adressutrymmet för hello virtuellt nätverk. hello-intervallet får inte överlappa med andra undernät adressintervall inom hello virtuellt nätverk. hello-adressutrymmet måste anges med hjälp av CIDR-notering. Du kan till exempel definiera en undernätsadressutrymmet 10.0.0.0/24 i ett virtuellt nätverk med adressutrymme 10.0.0.0/16. hello minsta intervall som du kan ange är /29, vilket möjliggör åtta IP-adresser för hello undernät. Azure reserver hello första och sista adressen i varje undernät för protokollet överensstämmelse. Tre ytterligare adresser är reserverade för användning i Azure-tjänsten. Därför kan ett undernät med en /29 adressintervall har tre användbara IP-adresser. Om du planerar tooconnect ett virtuellt nätverk tooa VPN-gateway, måste du skapa en gateway-undernätet. Lär dig mer om [specifik adressintervallet överväganden för gateway-undernät](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md?toc=%2fazure%2fvirtual-network%2ftoc.json#gwsub). Du kan ändra hello-adressintervall efter hello undernät skapas vissa villkor. hur toochange ett undernät-adressintervall, se toolearn [ändra undernätsinställningar](#change-subnet) i den här artikeln.
6. Klicka på **Spara**.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|Azure CLI|[AZ network vnet undernät uppdatering](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#update)|
|PowerShell|[Ange AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/set-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|


## <a name="delete-subnet"></a>Ta bort ett undernät

Du kan ta bort ett undernät om det finns inga resurser i hello undernät. Om det finns resurser i hello undernät, måste du ta bort hello-resurser som ingår i hello undernät innan du kan ta bort hello undernät. hello steg du vidta toodelete en resurs varierar beroende på hello resurs. toolearn hur toodelete resurser som finns i undernät, skrivskyddade hello dokumentationen för varje resurs typen som du vill toodelete. toodelete ett undernät:

1. Logga in toohello [portal](https://portal.azure.com) med ett konto som har tilldelats behörigheter för hello nätverket deltagarrollen (minst) för din prenumeration. toolearn mer information om hur du tilldelar roller och behörigheter tooaccounts finns [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Ange i hello portal sökrutan **virtuella nätverk**. I hello sökresultaten klickar du på **virtuella nätverken**.
3. På hello **virtuella nätverken** bladet, klickar du på hello virtuella nätverk som du vill toodelete ett undernät.
4. På hello virtuella nätverk bladet under **inställningar**, klickar du på **undernät**.
5. Hello listan med undernät som visas på hello undernät-bladet, högerklicka på hello undernät som du vill toodelete klickar du på **ta bort**, och klicka sedan på **Ja** toodelete hello undernät.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|Azure CLI|[ta bort AZ nätverks-virtuella nätverk](/cli/azure/network/vnet?toc=%2fazure%2fvirtual-network%2ftoc.json#delete)|
|PowerShell|[Ta bort AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/remove-azurermvirtualnetworksubnetconfig?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="next-steps"></a>Nästa steg

toocreate en virtuell dator i ett undernät, se [skapa ett virtuellt nätverk och distribuera virtuella datorer i undernät hello](virtual-network-get-started-vnet-subnet.md#create-vms).
