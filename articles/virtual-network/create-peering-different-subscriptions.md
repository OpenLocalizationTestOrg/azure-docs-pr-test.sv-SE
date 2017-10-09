---
title: aaaCreate ett Azure virtual network peering - Resource Manager - olika prenumerationer | Microsoft Docs
description: "Lär dig hur toocreate ett virtuellt nätverk peering mellan virtuella nätverk skapas via Resource Manager som finns i olika Azure-prenumerationer."
services: virtual-network
documentationcenter: 
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
ms.date: 07/17/2017
ms.author: jdial;narayan;annahar
ms.openlocfilehash: c7983a86031e061c1155144e5c493ee9578fa583
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-virtual-network-peering---resource-manager-different-subscriptions"></a>Skapa ett virtuellt nätverk peering - hanteraren för filserverresurser, olika prenumerationer 

I kursen får du lära dig toocreate ett virtuellt nätverk peering mellan virtuella nätverk som skapats via Resource Manager. hello virtuella nätverk finns i olika prenumerationer. Peering två virtuella nätverk gör resurser i olika virtuella nätverk toocommunicate med varandra med hello samma bandbredd och svarstid som om hello resurserna var i hello samma virtuella nätverk. Lär dig mer om [virtuella nätverk peering](virtual-network-peering-overview.md). 

hello steg toocreate ett virtuellt nätverk som peering är olika, beroende på om hello virtuella nätverk är i hello samma eller olika prenumerationer och som [Azure distributionsmodell](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) hello virtuella nätverk skapas via. Lär dig hur toocreate ett virtuellt nätverk peering i andra scenarier genom att klicka på hello scenariot från hello följande tabell:

|Azure-distributionsmodell  | Azure-prenumeration  |
|--------- |---------|
|[Båda Resource Manager](virtual-network-create-peering.md) |samma|
|[En Resource Manager, en klassisk](create-peering-different-deployment-models.md) |samma|
|[En Resource Manager, en klassisk](create-peering-different-deployment-models-subscriptions.md) |Olika|

Att går inte skapa ett virtuellt nätverk som peering mellan två virtuella nätverk som distribuerats via hello klassiska distributionsmodellen. Ett virtuellt nätverk som peering kan bara skapas mellan två virtuella nätverk som finns i hello samma Azure-region. När du skapar ett virtuellt nätverk peering mellan virtuella nätverk som finns i olika prenumerationer, hello prenumerationer måste båda vara associerad toohello samma Azure Active Directory-klient. Om du inte redan har en Azure Active Directory-klient, kan du snabbt [skapar du en](../active-directory/develop/active-directory-howto-tenant.md?toc=%2fazure%2fvirtual-network%2ftoc.json#start-from-scratch). Om du behöver tooconnect virtuella nätverk som skapats både via hello klassiska distributionsmodellen, som finns i olika Azure-regioner eller som finns i prenumerationer som är associerade med toodifferent Azure Active Directory-klienter kan du använda en Azure [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) tooconnect hello virtuella nätverk. 

Du kan använda hello [Azure-portalen](#portal), hello Azure [kommandoradsgränssnittet](#cli) (CLI) eller Azure [PowerShell](#powershell) toocreate peering ett virtuellt nätverk. Klicka på någon av hello tidigare verktyget länkar toogo direkt toohello steg för att skapa ett virtuellt nätverk peering verktyget dina val.

## <a name="portal"></a>Skapa peering - Azure-portalen

Den här kursen använder olika konton för varje prenumeration. Om du använder ett konto som har behörigheter tooboth prenumerationer kan du använda hello samma konto för alla åtgärder, hoppa över hello steg för att logga utanför hello portal och hoppa över hello steg för att tilldela en annan användare behörighet toohello virtuella nätverk.

1. Logga in toohello [Azure-portalen](https://portal.azure.com) som UserA. hello-konto som du loggar in med måste ha hello behörighet toocreate peering ett virtuellt nätverk. Se hello [behörigheter](#permissions) i den här artikeln för information.
2. Klicka på **+ ny**, klickar du på **nätverk**, klicka på **för virtuella nätverk**.
3. I hello **skapa virtuellt nätverk** bladet anger, eller Välj värden för hello följande inställningar och sedan klickar du på **skapa**:
    - **Namnet**: *myVnetA*
    - **Adressutrymmet**: *10.0.0.0/16*
    - **Undernätnamnet**: *standard*
    - **Adressintervall för gatewayundernät**: *10.0.0.0/24*
    - **Prenumerationen**: Välj prenumeration A.
    - **Resursgruppen**: Välj **Skapa nytt** och ange *myResourceGroupA*
    - **Plats**: *östra USA*
4. I hello **söka resurser** rutan hello överst i hello portal, typen *myVnetA*. Klicka på **myVnetA** när den visas i sökresultaten hello. Ett blad som visas för hello **myVnetA** virtuellt nätverk.
5. I hello **myVnetA** bladet som visas, klickar du på **åtkomstkontroll (IAM)** hello lodräta listan med alternativ på hello vänster sida av hello-bladet.
6. I hello **myVnetA - åtkomstkontroll (IAM)** bladet som visas, klickar du på **+ Lägg till**.
7. I hello **lägga till behörigheter** bladet som visas, väljer **Network-deltagare** i hello **rollen** rutan.
8. I hello **Välj** markerar ett b, eller ange användare BS e-postadress toosearch för den. hello listan över användare som visas är från hello samma Azure Active Directory-klient som hello virtuellt nätverk du ställer in hello-peering för.
9. Klicka på **Spara**.
10. I hello **myVnetA - åtkomstkontroll (IAM)** bladet, klickar du på **egenskaper** hello lodräta listan med alternativ på hello vänster sida av hello-bladet. Kopiera hello **resurs-ID**, som används i ett senare steg. hello resurs-ID är liknande toohello följande exempel: /subscriptions/<Subscription Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/virtualNetworks/myVnetA.
11. Logga ut från hello portal som UserA och sedan logga in som användare b.
12. Slutför steg 2 – 3, att ange eller markera hello följande värden i steg 3:

    - **Namnet**: *myVnetB*
    - **Adressutrymmet**: *10.1.0.0/16*
    - **Undernätnamnet**: *standard*
    - **Adressintervall för gatewayundernät**: *10.1.0.0/24*
    - **Prenumerationen**: Välj prenumeration B.
    - **Resursgruppen**: Välj **Skapa nytt** och ange *myResourceGroupB*
    - **Plats**: *östra USA*

13. I hello **söka resurser** rutan hello överst i hello portal, typen *myVnetB*. Klicka på **myVnetB** när den visas i sökresultaten hello. Ett blad som visas för hello **myVnetB** virtuellt nätverk.
14. I hello **myVnetB** bladet som visas, klickar du på **egenskaper** hello lodräta listan med alternativ på hello vänster sida av hello-bladet. Kopiera hello **resurs-ID**, som används i ett senare steg. hello resurs-ID är liknande toohello följande exempel: /subscriptions/<Susbscription ID>/resourceGroups/myResoureGroupB/providers/Microsoft.ClassicNetwork/virtualNetworks/myVnetB.
15. Klicka på **åtkomstkontroll (IAM)** i hello **myVnetB** bladet och sedan utför steg 5 – 10 för myVnetB, ange **UserA** i steg 8.
16. Logga ut från hello-portalen som användare b och logga in som UserA.
17. I hello **söka resurser** rutan hello överst i hello portal, typen *myVnetA*. Klicka på **myVnetA** när den visas i sökresultaten hello. Ett blad som visas för hello **myVnet** virtuellt nätverk.
18. Klicka på **myVnetA**.
19. I hello **myVnetA** bladet som visas, klickar du på **Peerkopplingar** hello lodräta listan med alternativ på hello vänster sida av hello-bladet.
20. I hello **myVnetA - Peerkopplingar** bladet som visas, klickar du på **+ Lägg till**
21. I hello **Lägg till peering** bladet som visas anger, eller välj hello följande alternativ och klicka på **OK**:
     - **Namnet**: *myVnetAToMyVnetB*
     - **Virtuellt nätverk distributionsmodell**: Välj **Resource Manager**.
     - **Jag vet mitt resurs-ID**: den här kryssrutan.
     - **Resurs-ID**: Ange hello resurs-ID från steg 14.
     - **Tillåt åtkomst till virtuella nätverk:** se till att **aktiverad** är markerad.
    Inga andra inställningar används i den här kursen. Läs toolearn om inställningar för alla peering [hantera peerkopplingar mellan virtuella nätverk](virtual-network-manage-peering.md#create-a-peering).
22. När du klickar på **OK** i föregående steg hello hello **Lägg till peering** blad stängs och du ser hello **myVnetA - Peerkopplingar** bladet igen. Efter några sekunder visas hello peering du skapade i hello-bladet. **Initierade** visas i hello **PEERING STATUS** för hello **myVnetAToMyVnetB** peering du skapat. Du har peerkoppla myVnetA toomyVnetB, men du måste nu peer-myVnetB toomyVnetA. Hej peering måste skapas i båda riktningarna tooenable resurser i hello virtuella nätverk toocommunicate med varandra.
23. Logga ut från hello portal som UserA och logga in som användare b.
24. Slutför steg 17-21 igen för myVnetB. Namn i steget 21 hello peering *myVnetBToMyVnetA*väljer *myVnetA* för **för virtuella nätverk**, och ange hello-ID från steg 10 i hello **resurs-ID** rutan.
25. Några sekunder när du klickar på **OK** toocreate hello-peering för myVnetB, hello **myVnetBToMyVnetA** peering du precis har skapat visas med **ansluten** i hello  **PEERING STATUS** kolumn.
26. Logga ut från hello-portalen som användare b och logga in som UserA.
27. Slutför steg 17-19 igen. Hej **PEERING STATUS** för hello **myVnetAToVNetB** peering är nu också **ansluten**. Hej peering etablerats efter att du ser **ansluten** i hello **PEERING STATUS** för både virtuella nätverk i hello peering. Azure-resurser som du skapar i antingen virtuellt nätverk är nu kan toocommunicate med varandra via sina IP-adresser. Om du använder standard-Azure namnmatchning för virtuella nätverk för hello hello resurser i hello virtuella nätverk är inte kan tooresolve namn över hello virtuella nätverk. Om du vill tooresolve namn över virtuella nätverk i en peering måste du skapa DNS-servern. Lär dig hur tooset in [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
28. **Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.
29. **Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i hello [bort resurser](#delete-portal) i den här artikeln.

## <a name="cli"></a>Skapa peering - Azure CLI

Den här kursen använder olika konton för varje prenumeration. Om du använder ett konto som har behörigheter tooboth prenumerationer kan du använda hello samma konto för alla steg hoppa över hello steg för att logga ut ur Azure och ta bort hello rader i skriptet som skapar användaren rolltilldelningar. Ersätt UserA@azure.com och UserB@azure.com i alla hello följande skript med hello användarnamn som du använder för användare a och b.

Hej följande skript:

- Kräver hello Azure CLI version 2.0.4 eller senare. toofind hello version, kör `az --version`. Om du behöver tooupgrade finns [installera Azure CLI 2.0](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Fungerar i ett Bash-gränssnitt. Alternativen på Azure CLI skriptkörning på Windows-klient kan se [kör Windows hello Azure CLI](../virtual-machines/windows/cli-options.md?toc=%2fazure%2fvirtual-network%2ftoc.json). 

Du kan använda hello Azure Cloud Shell istället för att installera hello CLI och dess beroenden. hello Azure Cloud-gränssnittet är ett kostnadsfritt Bash-gränssnitt som du kan köra direkt i hello Azure-portalen. Den har hello Azure CLI förinstallerat och konfigurerats toouse med ditt konto. Klicka på hello **prova** knappen i hello skript som följer, som anropar ett moln-gränssnitt som du kan logga in tooyour Azure-konto med. 

1. Öppna en CLI-session och logga in tooAzure som UserA med hello `azure login` kommando. hello-konto som du loggar in med måste ha hello behörighet toocreate peering ett virtuellt nätverk. Se hello [behörigheter](#permissions) i den här artikeln för information.
2. Kopiera hello följande skript tooa textredigerare på datorn genom att ersätta `<SubscriptionA-Id>` med hello ID SubscriptionA sedan kopiera hello ändrade skript, klistra in den i din CLI-sessionen och tryck på `Enter`. Om du inte vet ditt prenumerations-Id, ange hello 'az konto visa'-kommandot. Hej värde för **id** i hello utdata är ditt prenumerations-Id.

    ```azurecli-interactive
    # Create a resource group.
    az group create \
      --name myResourceGroupA \
      --location eastus

    # Create virtual network A.
    az network vnet create \
      --name myVnetA \
      --resource-group myResourceGroupA \
      --location eastus \
      --address-prefix 10.0.0.0/16

    # Assign UserB permissions toovirtual network A.
    az role assignment create \
      --assignee UserB@azure.com \
      --role "Network Contributor" \
      --scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```
    
     Hej behörighetstilldelningen för b är inte ett krav. Peering kan etableras även om användarna individuellt höja peering begäranden för sina respektive virtuella nätverk, så länge hello begär matchning. Lägga till en privilegierad användare av myVNetB som network-deltagare i hello lokala virtuella nätverk gör det enklare toodo hello konfiguration.
3. Logga ut från Azure som UserA med hello `az logout` kommandot och sedan logga in tooAzure som b. hello-konto som du loggar in med måste ha hello behörighet toocreate peering ett virtuellt nätverk. Se hello [behörigheter](#permissions) i den här artikeln för information.
4. Skapa myVnetB. Kopiera hello skriptinnehåll i steg 2 tooa textredigerare på datorn. Ersätt `<SubscriptionA-Id>` med hello ID SubscriptionB. Ändra 10.0.0.0/16 too10.1.0.0/16, ändra alla som tooB och alla Bs tooA. Kopiera hello ändrade skript, klistra in den i tooyour CLI sessionen och tryck på `Enter`. 
5. Logga ut från Azure som b och logga in tooAzure som UserA.
6. Skapa ett virtuellt nätverk från myVnetA toomyVnetB-peering. Kopiera hello följande skript innehållet tooa textredigerare på datorn. Ersätt `<SubscriptionB-Id>` med hello ID SubscriptionB. Kopiera hello ändrade skript tooexecute hello skript, klistrar in den i sessionen CLI och tryck på RETUR.
 
    ```azurecli-interactive
        # Get hello id for myVnetA.
        vnetAId=$(az network vnet show \
          --resource-group myResourceGroupA \
          --name myVnetA \
          --query id --out tsv)
    
        # Peer myVNetA toomyVNetB.
        az network vnet peering create \
          --name myVnetAToMyVnetB \
          --resource-group myResourceGroupA \
          --vnet-name myVnetA \
          --remote-vnet-id /subscriptions/<SubscriptionB-Id>/resourceGroups/myResourceGroupB/providers/Microsoft.Network/VirtualNetworks/myVnetB \
          --allow-vnet-access
    ```

7. Visa hello peering status för myVnetA.

    ```azurecli-interactive
    az network vnet peering list \
      --resource-group myResourceGroupA \
      --vnet-name myVnetA \
      --output table
    ```

    hello tillstånd är **initierade**. Ändras för**ansluten** när du har skapat hello peering toomyVnetA från myVnetB.

8. Logga ut UserA från Azure och logga in tooAzure som b.
9. Skapa hello peering från myVnetB toomyVnetA. Kopiera hello skriptinnehåll i steg 6 tooa textredigerare på datorn. Ersätt `<SubscriptionB-Id>` med hello-ID för SubscriptionA och ändra alla som tooB och alla Bs tooA. När du har ändrat hello, kopiera hello ändrad skript, klistrar in den i din CLI-sessionen och tryck på `Enter`.
10. Visa hello peering status för myVnetB. Kopiera hello skriptinnehåll i steg 7 tooa textredigerare på datorn. Ändra en tooB för hello resursgrupp och virtuella nätverksnamn, kopiera hello skript, klistra in hello ändrade skript i tooyour CLI-sessionen och tryck sedan på `Enter`. Hej peering tillstånd är **ansluten**. Hej peering tillståndet för myVnetA ändringar för**ansluten** när du har skapat hello peering från myVnetB toomyVnetA. Du kan logga UserA in tooAzure och går igenom steg 7 igen tooverify hello peering tillståndet för myVnetA. 

    > [!NOTE]
    > Hej peering upprättas inte förrän hello peering tillstånd är **ansluten** för både virtuella nätverk.

11. **Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.
12. **Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i [bort resurser](#delete-cli) i den här artikeln.

Azure-resurser som du skapar i antingen virtuellt nätverk är nu kan toocommunicate med varandra via sina IP-adresser. Om du använder standard-Azure namnmatchning för virtuella nätverk för hello hello resurser i hello virtuella nätverk är inte kan tooresolve namn över hello virtuella nätverk. Om du vill tooresolve namn över virtuella nätverk i en peering måste du skapa DNS-servern. Lär dig hur tooset in [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).
 
## <a name="powershell"></a>Skapa peering - PowerShell

Den här kursen använder olika konton för varje prenumeration. Om du använder ett konto som har behörigheter tooboth prenumerationer kan du använda hello samma konto för alla steg hoppa över hello steg för att logga ut ur Azure och ta bort hello rader i skriptet som skapar användaren rolltilldelningar. Ersätt UserA@azure.com och UserB@azure.com i alla hello följande skript med hello användarnamn som du använder för användare a och b.

1. Installera hello senaste versionen av hello PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) modul. Om du är ny tooAzure PowerShell Se [översikt över Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Starta en PowerShell-session.
3. I PowerShell, logga in tooAzure som UserA genom att ange hello `login-azurermaccount` kommando. hello-konto som du loggar in med måste ha hello behörighet toocreate peering ett virtuellt nätverk. Se hello [behörigheter](#permissions) i den här artikeln för information.
4. Skapa en resursgrupp och virtuella nätverk A. kopiera hello följande tooa skripttext editor på din dator. Ersätt `<SubscriptionA-Id>` med hello ID SubscriptionA. Om du inte vet ditt prenumerations-Id, ange hello `Get-AzureRmSubscription` kommandot tooview den. Hej värde för **Id** hello returnerade utdata är ditt prenumerations-ID. tooexecute hello skriptet kopiera hello ändrade skript, klistra in den i tooPowerShell och tryck sedan på `Enter`.

    ```powershell
    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name MyResourceGroupA `
      -Location eastus

    # Create virtual network A.
    $vNetA = New-AzureRmVirtualNetwork `
      -ResourceGroupName MyResourceGroupA `
      -Name 'myVnetA' `
      -AddressPrefix '10.0.0.0/16' `
      -Location eastus

    # Assign UserB permissions toomyVnetA.
    New-AzureRmRoleAssignment `
      -SignInName UserB@azure.com `
      -RoleDefinitionName "Network Contributor" `
      -Scope /subscriptions/<SubscriptionA-Id>/resourceGroups/myResourceGroupA/providers/Microsoft.Network/VirtualNetworks/myVnetA
    ```

    Hej behörighetstilldelningen för b är inte ett krav. Peering kan etableras även om användarna individuellt höja peering begäranden för sina respektive virtuella nätverk, så länge hello begär matchning. Lägger till en privilegierad användare av hello gör andra virtuella nätverket som en användare i hello lokala virtuella nätverk det enklare toodo hello konfiguration.
5. Logga ut UserA från Azure och logga in användare b. hello-konto som du loggar in med måste ha hello behörighet toocreate peering ett virtuellt nätverk. Se hello [behörigheter](#permissions) i den här artikeln för information.
6. Kopiera hello skriptinnehåll i steg 4 tooa textredigerare på datorn. Ersätt `<SubscriptionA-Id>` med hello-ID för prenumerationen B. Ändra 10.0.0.0/16 too10.1.0.0/16. Ändra alla som tooB och alla Bs tooA. tooexecute hello skriptet kopiera hello ändrade skript, klistra in i PowerShell och tryck sedan på `Enter`.
7. Logga ut användare b från Azure och logga in UserA.
8. Skapa hello peering från myVnetA toomyVnetB. Kopiera hello följande skript tooa textredigerare på datorn. Ersätt `<SubscriptionB-Id>` med hello-ID för prenumerationen B. tooexecute hello skript, kopiera hello ändrade skript, klistra in i tooPowerShell och tryck sedan på `Enter`.
 
    ```powershell
    # Peer myVnetA toomyVnetB.
    $vNetA=Get-AzureRmVirtualNetwork -Name myVnetA -ResourceGroupName myResourceGroupA
    Add-AzureRmVirtualNetworkPeering `
      -Name 'myVnetAToMyVnetB' `
      -VirtualNetwork $vNetA `
      -RemoteVirtualNetworkId "/subscriptions/<SubscriptionB-Id>/resourceGroups/myResourceGroupB/providers/Microsoft.Network/virtualNetworks/myVnetB"
    ```

9. Visa hello peering status för myVnetA.

    ```powershell
    Get-AzureRmVirtualNetworkPeering `
      -ResourceGroupName myResourceGroupA `
      -VirtualNetworkName myVnetA `
      | Format-Table VirtualNetworkName, PeeringState
    ```

    hello tillstånd är **initierade**. Ändras för**ansluten** när du konfigurera hello peering toomyVnetA från myVnetB.

10. Logga ut UserA från Azure och logga in användare b.
11. Skapa hello peering från myVnetB toomyVnetA. Kopiera hello skriptinnehåll i steg 8 tooa textredigerare på datorn. Ersätt `<SubscriptionB-Id>` med hello-ID för prenumerationen A och ändra alla A tooB och alla B tooA. tooexecute hello skriptet kopiera hello ändrade skript, klistra in den i tooPowerShell och tryck sedan på `Enter`.
12. Visa hello peering status för myVnetB. Kopiera hello skriptinnehåll i steg 9 tooa textredigerare på datorn. Ändra en tooB för hello resursgrupp och virtuella nätverksnamn. tooexecute hello skript, klistra in hello ändrade skriptet i PowerShell och tryck sedan på `Enter`. hello tillstånd är **ansluten**. Hej peering tillståndet för **myVnetA** ändras för**ansluten** när du har skapat hello peering från **myVnetB** för**myVnetA**. Du kan logga UserA in tooAzure och går igenom steg 9 igen tooverify hello peering tillståndet för myVnetA. 

    > [!NOTE]
    > Hej peering upprättas inte förrän hello peering tillstånd är **ansluten** för både virtuella nätverk.

    Azure-resurser som du skapar i antingen virtuellt nätverk är nu kan toocommunicate med varandra via sina IP-adresser. Om du använder standard-Azure namnmatchning för virtuella nätverk för hello hello resurser i hello virtuella nätverk är inte kan tooresolve namn över hello virtuella nätverk. Om du vill tooresolve namn över virtuella nätverk i en peering måste du skapa DNS-servern. Lär dig hur tooset in [namnmatchning med hjälp av DNS-servern](virtual-networks-name-resolution-for-vms-and-role-instances.md#name-resolution-using-your-own-dns-server).

13. **Valfria**: även om att skapa virtuella datorer inte ingår i den här självstudiekursen, du kan skapa en virtuell dator i varje virtuellt nätverk och ansluta från en virtuell dator toohello andra toovalidate anslutning.
14. **Valfria**: toodelete hello resurser som du skapar i den här självstudiekursen, fullständig hello stegen i [bort resurser](#delete-powershell) i den här artikeln.

## <a name="permissions"></a>Behörigheter

hello-konton som du använder ett virtuellt nätverk som peering toocreate måste ha hello behövs eller behörigheter. Om du peering två virtuella nätverk med namnet till exempel **myVnetA** och **myVnetB**, ditt konto ha tilldelats hello följande minsta eller behörigheter för varje virtuellt nätverk:
    
|Virtuellt nätverk|Roll|Behörigheter|
|---|---|---|
|myVnetA|[Nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/virtualNetworkPeerings/write|
|myVnetB|[Nätverksdeltagare](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor)|Microsoft.Network/virtualNetworks/peer|

Lär dig mer om [inbyggda roller](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) och tilldela särskilda behörigheter för[anpassade roller](../active-directory/role-based-access-control-custom-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (enbart Resource Manager).

## <a name="delete"></a>Ta bort resurser
När du är klar med den här kursen kan kanske du vill toodelete hello resurser som du skapade i hello självstudier, så att du inte betalar användningsavgifter. En resursgrupp också tar du bort alla resurser som finns i hello resursgruppen.

### <a name="delete-portal"></a>Azure-portalen

1. Logga in toohello Azure-portalen som UserA.
2. Ange i hello portal sökrutan **myResourceGroupA**. I hello sökresultaten klickar du på **myResourceGroupA**.
3. På hello **myResourceGroupA** bladet, klickar du på hello **ta bort** ikon.
4. tooconfirm hello borttagning, i hello **typen hello RESURSGRUPPENS namn** ange **myResourceGroupA**, och klicka sedan på **ta bort**.
5. Logga ut från hello portal som UserA och logga in som användare b.
6. Slutför steg 2-4 för myResourceGroupB.

### <a name="delete-cli"></a>Azure CLI

1. Logga in tooAzure som UserA och kör följande kommando hello:

    ```azurecli-interactive
    az group delete --name myResourceGroupA --yes
    ```
2. Logga ut från Azure som UserA och logga in som användare b.
3. Kör följande kommando hello:

    ```azurecli-interactive
    az group delete --name myResourceGroupB --yes
    ```

### <a name="delete-powershell"></a>PowerShell

1. Logga in tooAzure som UserA och kör följande kommando hello:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupA -force
    ```

2. Logga ut från Azure som UserA och logga in som användare b.
3. Kör följande kommando hello:

    ```powershell
    Remove-AzureRmResourceGroup -Name myResourceGroupB -force
    ```

## <a name="next-steps"></a>Nästa steg

- Väl bekanta dig med viktiga [peering begränsningar för virtuellt nätverk och beteenden](virtual-network-manage-peering.md#requirements-and-constraints) innan du skapar ett virtuellt nätverk peering för produktion använder.
- Lär dig mer om alla [peering inställningar för virtuella nätverk](virtual-network-manage-peering.md#create-a-peering).
- Lär dig hur för[skapa ett nav och eker nätverkstopologi](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke?toc=%2fazure%2fvirtual-network%2ftoc.json#vnet-peering) med virtuella nätverk peering.
