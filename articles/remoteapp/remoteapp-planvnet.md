---
title: "aaaHow tooplan ditt virtuella nätverk för en Azure RemoteApp-samling | Microsoft Docs"
description: "Lär dig hur tooplan ditt virtuella nätverk för en Azure RemoteApp-samling."
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
ms.openlocfilehash: d7eeefc3c66815b18f9338e2e428585e6f81a12a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooplan-your-virtual-network-for-azure-remoteapp"></a>Hur tooplan ditt virtuella nätverk för Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Det här dokumentet beskriver hur tooset in din Azure-nätverk (VNET) och hello undernät för Azure RemoteApp. Om du inte känner till virtuella Azure-nätverk är en funktion som hjälper till att du toovirtualize din infrastruktur toohello moln och toocreate hybrid nätverkslösningar med Azure och lokala resurser. Du kan läsa mer om det [här](../virtual-network/virtual-networks-overview.md).

Om du vill toodefine säkerhetsprinciper för trafik (både inkommande och utgående) i det virtuella nätverket där du distribuerar Azure RemoteApp, rekommenderar vi att skapa ett separat undernät för Azure RemoteApp från hello resten av dina distributioner i hello Azure virtuella nätverk. Mer information om hur toodefine säkerhetsprinciper på din virtuella Azure-nätverk undernät Läs [vad är en Nätverkssäkerhetsgrupp (NSG)?](../virtual-network/virtual-networks-nsg.md).

## <a name="types-of-azure-remoteapp-collections-with-azure-virtual-networks"></a>Typer av Azure RemoteApp-samlingar med virtuella Azure-nätverk
hello visar följande bilder hello två olika samling alternativ när du vill toouse ett virtuellt nätverk.

### <a name="azure-remoteapp-cloud-collection-with-vnet"></a>Azure RemoteApp-molnsamling med VNET
 ![Azure RemoteApp - molnsamling med ett virtuellt nätverk](./media/remoteapp-planvpn/ra-cloudvpn.png)

Detta representerar en Azure RemoteApp-samling där alla hello resurser att hello RemoteApp-sessionen värdar måste tooaccess har distribuerats i Azure. De kan vara i hello samma virtuella nätverk som hello RemoteApp VNET eller ett annat VNET i Azure.

### <a name="azure-remoteapp-hybrid-collection-with-vnet"></a>Azure RemoteApp hybrid-samling med virtuella nätverk
![Azure RemoteApp - hybridsamling med ett virtuellt nätverk](./media/remoteapp-planvpn/ra-hybridvpn.png)

Detta representerar en Azure RemoteApp-samling där hello resurser att hello RemoteApp-sessionen värdar måste tooaccess är distribuerade lokalt. hello RemoteApp VNET är länkade toohello lokalt nätverk med hybridrapportering i Azure-tekniker som plats-till-plats-VPN eller Expressroute.

## <a name="how-hello-system-works"></a>Så här fungerar hello system
Under hello omfattar distribuerar Azure RemoteApp virtuella Azure-datorer (med överförda bilden) toohello undernät för virtuellt nätverk som du angav under etableringen. Om du valt för en hybridsamling försök vi tooresolve hello FQDN för hello-domänkontrollant som du angav i hello etablering arbetsflödet med hello DNS-server som finns i hello virtuellt nätverk.  
Om du ansluter tooan befintligt virtuellt nätverk gör att tooexpose hello nödvändiga portarna i ditt nätverkssäkerhetsgrupper i Azure RemoteApp-undernät. 

Vi rekommenderar att du använder en [tillräckligt stor undernät för Azure RemoteApp](remoteapp-vnetsizing.md). hello största stöds av Azure virtuella nätverk är/8 som (med CIDR-subnätsdefinitioner). Ditt undernät måste vara tillräckligt stor tooaccommodate alla hello Azure RemoteApp-virtuella datorer under skala upp när fler användare kommer åt hello appar. 

Följande är hello saker du behöver tooenable på undernät för virtuellt nätverk: 

1. Utgående trafik från hello undernät ska tillåtas på port intervallet 10101 10175 toocommunicate med någon av hello interna Azure RemoteApp-tjänster.
2. Utgående trafik ska tillåtas från din undernät tooconnect tooAzure lagring på port 443
3. Om du har Active Directory finns i Azure, kontrollera att alla VM inom hello undernät för virtuellt nätverk för Azure RemoteApp är kan tooconnect toothat-domänkontrollant. hello DNS i hello virtuella nätverk ska kunna tooresolve hello FQDN för den här domänkontrollanten.

## <a name="virtual-network-with-forced-tunneling"></a>Virtuellt nätverk med Tvingad tunneltrafik
[Tvingad tunneltrafik](../vpn-gateway/vpn-gateway-about-forced-tunneling.md) har nu stöd för alla nya Azure RemoteApp-samlingar. Vi stöder för närvarande inte hello migrering av en befintlig samling toosupport Tvingad tunneltrafik.  Du måste toodelete alla befintliga samlingar med hello VNET du länkar tooAzure RemoteApp och skapa en ny en tooget Tvingad tunneltrafik aktiveras samlingar. 

