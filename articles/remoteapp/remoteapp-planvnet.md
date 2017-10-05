---
title: "Planera ditt virtuella nätverk för en Azure RemoteApp-samling | Microsoft Docs"
description: "Lär dig hur du planerar din virtuellt nätverk för en Azure RemoteApp-samling."
services: remoteapp
documentationcenter: 
author: mghosh1616
manager: mbaldwin
ms.assetid: ad9aff0e-f374-49c0-951d-4a7be1c36de0
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 1eb8115b13fb18074b4c4726b69e3d9faf387c32
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-plan-your-virtual-network-for-azure-remoteapp"></a>Planera ditt virtuella nätverk för Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Det här dokumentet beskriver hur du ställer in din Azure-nätverk (VNET) och undernätet för Azure RemoteApp. Om du inte känner till virtuella Azure-nätverk är en funktion som hjälper dig att virtualisera nätverkets infrastruktur till molnet och för att skapa hybridlösningar med Azure och lokala resurser. Du kan läsa mer om det [här](../virtual-network/virtual-networks-overview.md).

Om du vill definiera principer för trafik (både inkommande och utgående) i det virtuella nätverket där du distribuerar Azure RemoteApp, rekommenderar vi att skapa ett separat undernät för Azure RemoteApp från resten av dina distributioner i Azure virtuella nätverk. Mer information om hur du definierar säkerhetsprinciper på din virtuella Azure-nätverket undernät Läs [vad är en Nätverkssäkerhetsgrupp (NSG)?](../virtual-network/virtual-networks-nsg.md).

## <a name="types-of-azure-remoteapp-collections-with-azure-virtual-networks"></a>Typer av Azure RemoteApp-samlingar med virtuella Azure-nätverk
Följande bild visar två olika samling alternativ när du vill använda ett virtuellt nätverk.

### <a name="azure-remoteapp-cloud-collection-with-vnet"></a>Azure RemoteApp-molnsamling med VNET
 ![Azure RemoteApp - molnsamling med ett virtuellt nätverk](./media/remoteapp-planvpn/ra-cloudvpn.png)

Detta representerar en Azure RemoteApp-samling där alla resurser som värdar behöver åtkomst till RemoteApp-sessionen har distribuerats i Azure. De kan vara i samma virtuella nätverk som RemoteApp VNET eller ett annat VNET i Azure.

### <a name="azure-remoteapp-hybrid-collection-with-vnet"></a>Azure RemoteApp hybrid-samling med virtuella nätverk
![Azure RemoteApp - hybridsamling med ett virtuellt nätverk](./media/remoteapp-planvpn/ra-hybridvpn.png)

Detta representerar en Azure RemoteApp-samling där vissa av de resurser som RemoteApp-sessionen värdar som behöver åtkomst till är distribuerade lokalt. RemoteApp VNET är länkad till det lokala nätverket med hybridrapportering i Azure-tekniker som plats-till-plats VPN eller Expressroute.

## <a name="how-the-system-works"></a>Så här fungerar systemet
Under försättsbladen distribuerar Azure RemoteApp virtuella Azure-datorer (med överförda bilden) till undernät för virtuellt nätverk som du angav under etableringen. Om du valt för en hybridsamling försöker vi matcha FQDN för den domänkontrollant som du angav i etablering arbetsflödet med DNS-servern i det virtuella nätverket.  
Om du ansluter till ett befintligt virtuellt nätverk, se till att visa de nödvändiga portarna i ditt nätverkssäkerhetsgrupper i Azure RemoteApp-undernät. 

Vi rekommenderar att du använder en [tillräckligt stor undernät för Azure RemoteApp](remoteapp-vnetsizing.md). Den största stöds av Azure virtuella nätverk är/8 som (med CIDR-subnätsdefinitioner). Ditt undernät bör vara tillräckligt stor för att hantera alla virtuella Azure RemoteApp-datorer under skala upp när fler användare har åtkomst till apparna. 

Följande är de saker som du behöver aktivera på undernät för virtuellt nätverk: 

1. Utgående trafik från undernätet som ska tillåtas på portintervall 10101 10175 att kommunicera med en av de interna Azure RemoteApp-tjänsterna.
2. Utgående trafik ska tillåtas från ditt undernät för att ansluta till Azure Storage på port 443
3. Om du har Active Directory finns i Azure, kontrollera att alla VM i undernät för virtuellt nätverk för Azure RemoteApp är ansluta till domänkontrollanten. DNS i det virtuella nätverket måste kunna matcha FQDN för den här domänkontrollanten.

## <a name="virtual-network-with-forced-tunneling"></a>Virtuellt nätverk med Tvingad tunneltrafik
[Tvingad tunneltrafik](../vpn-gateway/vpn-gateway-about-forced-tunneling.md) har nu stöd för alla nya Azure RemoteApp-samlingar. Vi stöder för närvarande inte migrering av en befintlig samling att stödja Tvingad tunneling.  Du måste ta bort alla befintliga samlingar med hjälp av det VNET som du länkar till Azure RemoteApp och skapa en ny som ska få Tvingad tunneltrafik aktiveras samlingar. 

