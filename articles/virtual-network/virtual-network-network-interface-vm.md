---
title: "aaaAdd nätverk gränssnitt tooor ta bort från virtuella datorer i Azure | Microsoft Docs"
description: "Lär dig hur tooadd nätverk gränssnitt tooor bort nätverksgränssnitt i virtuella datorer."
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
ms.date: 07/25/2017
ms.author: jdial
ms.openlocfilehash: eb564b94932b9242f29bc23234b08b0c76ac23a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-network-interfaces-tooor-remove-from-virtual-machines"></a>Lägg till nätverksgränssnitt tooor ta bort från virtuella datorer

Lär dig hur tooadd ett befintligt nätverk gränssnitt när du skapar en virtuell dator och lägga till eller ta bort nätverksgränssnitt från en befintlig virtuell dator i hello stoppats (frigjorts) tillstånd. Ett nätverksgränssnitt kan en toocommunicate Azure virtuell dator (VM) med Internet, Azure och lokala resurser. En virtuell dator kan ha en eller flera nätverksgränssnitt. 

Om du behöver tooadd, ändra eller ta bort IP-adresser för ett nätverksgränssnitt läsa hello [hantera IP-adresser i nätverket gränssnittet](virtual-network-network-interface-addresses.md) artikel. Om du behöver toocreate, ändra eller ta bort nätverksgränssnitt, läsa hello [hantera nätverksgränssnitt](virtual-network-network-interface.md) artikel.

## <a name="before"></a>Innan du börjar

Fullständig hello följande uppgifter innan du slutför alla steg i alla avsnitt i den här artikeln:

- Lär dig mer om hur många nätverkskort stöder varje Linux och Windows VM-storlek genom att granska hello [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eller [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-storlekar artiklar.
- Logga in toohello Azure [portal](https://portal.azure.com), Azure-kommandoradsgränssnittet (CLI) eller Azure PowerShell med ett Azure-konto. Om du inte redan har ett Azure-konto, registrera dig för en [ledigt utvärderingskonto](https://azure.microsoft.com/free).
- Om du använder PowerShell-kommandon toocomplete uppgifter i den här artikeln [installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har hello senaste versionen av hello Azure PowerShell-kommandon som installeras. tooget hjälp för PowerShell-kommandon med exempel på, skriver `get-help <command> -full`.
- Om du använder Azure-kommandoradsgränssnittet (CLI) kommandon toocomplete uppgifter i den här artikeln [installera och konfigurera hello Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Se till att du har hello senaste versionen av hello Azure CLI är installerad. tooget hjälp för CLI-kommandona skriver `az <command> --help`. Du kan använda hello Azure Cloud-gränssnittet i stället för att installera hello CLI och dess krav. hello Azure Cloud-gränssnittet är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i hello Azure-portalen. Den har hello Azure CLI förinstallerat och konfigurerats toouse med ditt konto. toouse hello molnet Shell, klicka på hello molnet Shell **> _** knappen hello överst i hello [portal](https://portal.azure.com).

## <a name="about"></a>Om nätverksgränssnitt och virtuella datorer

Du kan lägga till (bifoga) en befintlig network interface tooa VM när du skapar hello VM, under förutsättning att hello nätverksgränssnittet inte redan ansluten tooanother VM. Du kan lägga till ett nätverksgränssnitt till eller ta bort (frånkoppla) ett nätverk gränssnitt toofrom en befintlig virtuell dator, förutsatt att hello VM har hello stoppats (frigjorts) tillstånd. Om du skapar en virtuell dator med hjälp av hello Azure-portalen skapas hello portal ett nätverksgränssnitt med standardinställningar. hello-portalen tillåter inte att du:

- Ange ett befintligt nätverk gränssnittet tooadd när du skapar hello VM
- Skapa en virtuell dator med flera nätverksgränssnitt
- Ange ett namn för nätverksgränssnittet hello (hello portal skapar hello nätverksgränssnitt med ett standardnamn som)

Du kan använda Azure PowerShell eller hello CLI toocreate ett nätverksgränssnitt eller virtuell dator med alla tidigare hello-attribut som du inte kan använda hello portal för. Innan du slutför hello uppgifterna i följande avsnitt hello Tänk hello följande begränsningar och beteenden:

- Alla VM-storlekar stöd minst två nätverkskort, men vissa VM-storlekar stöd för fler än två nätverksgränssnitt. Storlekar stöds bara ett nätverksgränssnitt i hello tidigare vissa VM. toolearn hur många nätverkskort som har stöd för varje VM-storlek kan läsa hello [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eller [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-storlekar artiklar. 
- I tidigare hello kunde endast nätverksgränssnitt tillagda tooVMs som stöd för flera nätverksgränssnitt och har skapats med minst två nätverksgränssnitt. Du kunde inte lägga till en virtuell dator som har skapats med ett nätverksgränssnitt med nätverket gränssnittet tooa även om hello VM-storlek stöds flera nätverksgränssnitt. Omvänt kan du kan bara ta bort nätverksgränssnitt från en virtuell dator med minst tre nätverksgränssnitt, eftersom virtuella datorer som skapades med minst två nätverksgränssnitt alltid hade toohave minst två nätverksgränssnitt. Ingen av dessa villkor gäller längre. Du kan nu skapa en virtuell dator med valfritt antal nätverksgränssnitt (upp toohello antal som stöds av hello VM-storlek) och lägga till eller ta bort alla antalet nätverksgränssnitt (från virtuella datorer i hello Stoppad (frigjord) tillstånd), så länge hello VM har alltid minst ett nätverksgränssnitt.
- Som standard definieras hello första nätverksgränssnitt i en virtuell dator som hello *primära* nätverksgränssnitt. Alla andra nätverksgränssnitt i hello VM är *sekundära* nätverksgränssnitt.
- Primära nätverksgränssnitt som är tilldelade en standard-gateway som hello Azure DHCP-servrar, men sekundära nätverksgränssnitt är inte. Eftersom sekundära nätverksgränssnitt har inte tilldelats en standard-gateway, inte kan de kommunicera med resurser utanför deras undernät, som standard. tooenable sekundära nätverksgränssnitt i en virtuell Windows-dator toocommunicate med resurser utanför deras undernät, lägga till vägar toohello operativsystem använder hello `route add` kommandot från en kommandorad för Windows. Eftersom hello standardbeteendet använder svaga värden routning, för virtuella Linux-datorer rekommenderar vi att begränsa trafik för sekundära gränssnitt tooa enda undernät. Om du behöver anslutningar utanför hello undernät för sekundära nätverksgränssnitt, aktivera principbaserad routning tooensure som ingång och utgång trafik använder hello nätverksgränssnittet samma.
- Som standard tilldelas all utgående trafik från VM skickas ut hello IP-adress hello toohello primära IP-adresskonfigurationen för hello primära nätverksgränssnittet. Du kan styra vilken IP-adress används för utgående trafik i hello VM operativsystem, men som standard är via hello primära nätverksgränssnittet.
- I hello efter alla virtuella datorer i hello har samma tillgänglighetsuppsättning krävs toohave som en enda eller flera nätverksgränssnitt. Virtuella datorer med valfritt antal gränssnitt kan nu finns i nätverket hello samma tillgänglighetsuppsättning in toohello antal som stöds av hello VM-storlek. Du kan bara lägga till en VM tooan tillgänglighetsuppsättning när den skapas om. Mer om tillgänglighetsuppsättningar läsa hello toolearn [hantera hello tillgängligheten för virtuella datorer i Azure](../virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-network%2ftoc.json#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) artikel.
- Medan nätverksgränssnitt i hello samma virtuella dator kan vara ansluten toodifferent undernät i ett VNet, hello nätverksgränssnitt måste vara anslutna toohello samma virtuella nätverk.
- Du kan lägga till IP-adresser för alla IP-adresskonfigurationen för någon primär eller sekundär network interface tooan Azure belastningsutjämnare backend-adresspool. Senaste hello, kunde bara hello primära IP-adressen för hello primära nätverksgränssnittet läggas tooa backend-adresspool. Mer om IP-adresser och konfigurationer för läsa hello toolearn [Lägg till, ändra eller ta bort IP-adresser](virtual-network-network-interface-addresses.md) artikel.
- Om du tar bort en virtuell dator tar inte bort hello nätverksgränssnitt som är bifogade tooit. När en virtuell dator tas bort frånkoppla hello nätverksgränssnitt från hello VM. Du kan lägga till hello nätverk gränssnitt toodifferent virtuella datorer eller ta bort dem.
- Om ett nätverksgränssnitt har en privat IPv6-adress som tilldelats tooit kan koppla du den tooa VM när du skapar hello VM. Du kan inte koppla ett nätverksgränssnitt med en tilldelad IPv6-adress tooa VM efter hello VM skapas. Om du bifogar ett nätverksgränssnitt med en tilldelad privata IPv6-adress när du skapar en virtuell dator kan bara bifoga att network interface toohello den virtuella datorn, oavsett hur många nätverkskort som har stöd för hello VM-storlek. Se [Network interface IP-adresser](virtual-network-network-interface-addresses.md) toolearn mer om att tilldela IP-adresser toonetwork gränssnitt.

## <a name="vm-create"></a>Lägg till befintliga nätverk gränssnitt tooa ny virtuell dator

När du skapar en virtuell dator via hello portal hello portalen skapar ett nätverksgränssnitt med standardinställningar och bifogar toohello VM för dig. Du kan inte lägga till befintliga nätverk gränssnitt tooa ny virtuell dator, eller skapa en virtuell dator med flera nätverksgränssnitt med hello Azure-portalen. Båda använder hello CLI eller PowerShell. Du kan lägga till så många nätverk gränssnitt tooa VM som hello VM-storlek som du skapar stöder. toolearn mer information om hur många nätverk gränssnitt varje VM storlek har stöd för, läsa hello [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eller [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-storlekar artiklar. hello nätverksgränssnitt som du lägger till tooa VM kan för närvarande inte anslutna tooanother VM. Mer om hur du skapar nätverksgränssnitt, läsa hello toolearn [hantera nätverksgränssnitt](virtual-network-network-interface.md#create-a-network-interface) artikel.

> [!WARNING]
> Om ett nätverksgränssnitt har en privat IPv6-adress som tilldelats tooit, kan du bara lägga hello network interface toohello virtuella datorn när du skapar hello virtuell dator. Du bifoga inte flera nätverk gränssnittet toohello virtuella datorer när du skapar hello virtuell dator eller efter hello virtuell dator skapas som en IPv6-adress tilldelas tooa nätverk gränssnittet kopplade tooa virtuella datorn. Se [Network interface IP-adresser](virtual-network-network-interface-addresses.md) toolearn mer om att tilldela IP-adresser toonetwork gränssnitt.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|CLI|[Skapa AZ vm](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[Ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-add-nic"></a>Lägg till en befintlig network interface tooan befintliga VM

Du kan lägga till så många nätverk gränssnitt tooa VM som hello VM-storlek som du vill lägga till nätverket gränssnitt toosupports. toolearn hur många nätverkskort som har stöd för varje VM-storlek kan läsa hello [Linux](../virtual-machines/linux/sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) eller [Windows](../virtual-machines/virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-network%2ftoc.json) VM-storlekar artiklar. hello VM som du vill tooadd en network interface toomust stöder hello antalet nätverksgränssnitt tooadd och måste vara i hello stoppad frigjord () tillstånd. hello nätverksgränssnitt som du vill tooadd kan för närvarande inte anslutna tooanother VM. Du kan inte lägga till nätverket gränssnitt tooan befintliga virtuella datorn med hello Azure-portalen. tooadd nätverksgränssnitt tooan befintlig virtuell dator måste du använda hello CLI eller PowerShell. 

> [!WARNING]
> Om ett nätverksgränssnitt har en privat IPv6-adress som tilldelats tooit kan läggas inte tooan befintlig virtuell dator. Du kan bara lägga till ett nätverksgränssnitt med en tilldelad privata IPv6-adress tooa virtuell dator när du skapar en virtuell dator. Se [Network interface IP-adresser](virtual-network-network-interface-addresses.md) toolearn mer om att tilldela IP-adresser toonetwork gränssnitt.

|Verktyget|Kommando|
|---|---|
|CLI|[Lägg till AZ vm nic](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#add) (referens) eller [detaljerade steg](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-a-vm)|
|PowerShell|[Lägg till AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (referens) eller [detaljerade steg](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#add-a-nic-to-an-existing-vm)|

## <a name="vm-view-nic"></a>Visa nätverksgränssnitt för en virtuell dator

Du kan visa hello nätverk gränssnitt kopplade tooa VM toolearn om konfigurationen för varje nätverksgränssnitt och hello IP-adresser tilldelas tooeach nätverksgränssnitt. 

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som har tilldelats rollen för hello ägare, deltagare eller Network-deltagare för din prenumeration. toolearn mer information om hur du tilldelar roller tooaccounts finns [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *virtuella datorer*. När **virtuella datorer** visas i sökresultaten hello klickar du på den.
3. I hello **virtuella datorer** bladet som visas, klicka på hello VM som du vill tooview nätverksgränssnitt för hello namn.
4. I hello **inställningar** avsnitt i hello virtuella bladet som visas för hello VM som du har valt, klickar du på **nätverksgränssnitt**. toolearn om inställningar för nätverksgränssnitt och hur toochange dem, skrivskyddade hello [hantera nätverksgränssnitt](virtual-network-network-interface.md) artikel. toolearn om att lägga till, ändrar eller tar bort IP-adresser som tilldelats tooa nätverksgränssnitt, se [hantera IP-adresser](virtual-network-network-interface-addresses.md).

**Kommandon**

|Verktyget|Kommando|
|---|---|
|CLI|[AZ vm visa](/cli/azure/vm?toc=%2fazure%2fvirtual-network%2ftoc.json#show)|
|PowerShell|[Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm?toc=%2fazure%2fvirtual-network%2ftoc.json)|

## <a name="vm-remove-nic"></a>Ta bort ett nätverksgränssnitt från en virtuell dator

hello måste VM som du vill tooremove (eller koppla från) ett nätverksgränssnitt från vara i hello stoppats (frigjorts) och måste ha minst två nätverksgränssnitt ansluten tooit. Du kan ta bort alla nätverksgränssnitt, men hello VM alltid måste ha minst ett nätverk gränssnittet kopplade tooit. Om du tar bort ett primärt nätverksgränssnitt tilldelar Azure hello primära attributet toohello gränssnitt som har varit ansluten toohello VM hello längsta. 

1. Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som har tilldelats rollen för hello ägare, deltagare eller Network-deltagare för din prenumeration. toolearn mer information om hur du tilldelar roller tooaccounts finns [inbyggda roller för rollbaserad åtkomstkontroll i Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. I hello som innehåller hello text *söka resurser* överst hello i hello Azure-portalen, Skriv *virtuella datorer*. När **virtuella datorer** visas i sökresultaten hello klickar du på den.
3. I hello **virtuella datorer** bladet som visas, klicka på hello VM som du vill tooremove ett nätverksgränssnitt för hello namn.
4. I hello **inställningar** avsnitt i hello virtuella bladet som visas för hello VM som du har valt, klickar du på **nätverksgränssnitt**. toolearn om inställningar för nätverksgränssnitt och hur toochange dem, skrivskyddade hello [hantera nätverksgränssnitt](virtual-network-network-interface.md) artikel. toolearn om att lägga till, ändrar eller tar bort IP-adresser som tilldelats tooa nätverksgränssnitt, se [hantera IP-adresser](virtual-network-network-interface-addresses.md).
5. I hello nätverk gränssnitt-bladet som visas, klickar du på hello **...**  toohello höger i hello nätverksgränssnittet som du vill toodetach.
6. Klicka på **frånkoppling**. Om det finns endast en virtuell dator för nätverket gränssnittet kopplade toohello, hello **Detach** alternativet inte är tillgängligt. Klicka på **Ja** i hello bekräftelserutan som visas.

**Kommandon**

|Verktyget|Kommando|
|---|---|
|CLI|[ta bort AZ vm nic](/cli/azure/vm/nic?toc=%2fazure%2fvirtual-network%2ftoc.json#remove) (referens) eller [detaljerade steg](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-a-vm)|
|PowerShell|[Ta bort AzureRMVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface?toc=%2fazure%2fvirtual-network%2ftoc.json) (referens) eller [detaljerade steg](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json#remove-a-nic-from-an-existing-vm)|

## <a name="next-steps"></a>Nästa steg
toocreate en virtuell dator med flera nätverksgränssnitt eller IP-adresser, läsa hello följande artiklar:

**Kommandon**

|Aktivitet|Verktyget|
|---|---|
|Skapa en virtuell dator med flera nätverkskort|[CLI](../virtual-machines/linux/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../virtual-machines/windows/multiple-nics.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
|Skapa en enda NIC VM med flera IPv4-adresser|[CLI](virtual-network-multiple-ip-addresses-cli.md), [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)|
|Skapa en enda NIC VM med en privat IPv6-adress (bakom en belastningsutjämnare i Azure)|[CLI](../load-balancer/load-balancer-ipv6-internet-cli.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [PowerShell](../load-balancer/load-balancer-ipv6-internet-ps.md?toc=%2fazure%2fvirtual-network%2ftoc.json), [Azure Resource Manager-mall](../load-balancer/load-balancer-ipv6-internet-template.md?toc=%2fazure%2fvirtual-network%2ftoc.json)|
