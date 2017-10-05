---
title: "Lista över URL: er och portar som godkända för Azure RemoteApp distribueras i kundens virtuella nätverk | Microsoft Docs"
description: "Lär dig mer om vilka portar och URL: er måste du konfigurera för kommunikation via Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: mghosh1616
manager: mbaldwin
ms.assetid: 5a001ff7-14c9-47fa-9b39-78fd5a5f0250
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: c17ff8d5441ca92f7b893edb541a1e9730c2a847
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="list-of-ports-and-urls-to-permit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Lista över portar och URL: er för att tillåta åtkomst för Azure RemoteApp distribueras i kundens virtuella nätverk
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Om du distribuerar en Azure RemoteApp-moln eller hybrid samling i ett virtuellt nätverk (VNET), kan du granska följande portinformationen. Mer information om virtuella nätverk [översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md). Om du har skapat en nätverkssäkerhetsgrupp (NSG) som begränsar trafiken till virtuella resurser i samlingen, se till att följande portar är tillgänglig och tillåts via säkerhetsprinciper på det virtuella nätverket. Mer information om nätverkssäkerhetsgrupper [vad är en Nätverkssäkerhetsgrupp? (NSG) ](../virtual-network/virtual-networks-nsg.md).

## <a name="azure-remoteapp-subnet-needs-access-to-these-endpoints-and-urls"></a>Azure RemoteApp-undernätet måste ha åtkomst till dessa slutpunkter och URL: er:
* *. servicebus.windows.net
* *. servicebus.net
* https://*.RemoteApp.windowsazure.com  
* https://www.RemoteApp.windowsazure.com 
* https://*RemoteApp.windowsazure.com  
* https://*.Core.Windows.NET  
* Utgående: TCP: TCP: 443, 9351, 9352, 10101 10175 
* Valfri – UDP: 10201 10275  

## <a name="azure-remoteapp-clients-need-access-to-these-endpoints-and-urls"></a>Azure RemoteApp-klienter behöver åtkomst till dessa slutpunkter och URL: er:
Av klienter avses stationära datorer, enheter, etc. som användare använder för att ansluta till appar som distribuerats i Azure RemoteApp-samling.

* https://telemetry.RemoteApp.windowsazure.com  
* https://*.RemoteApp.windowsazure.com (är ett valfritt UDP-portar för den här adressen) 
* https://login.windows.net  
* https://login.microsoftonline.com  
* https://www.RemoteApp.windowsazure.com 
* https://*.Core.Windows.NET  
* Utgående: TCP: 443  
* Valfritt - UDP: 3391 

