---
title: "aaaList för URL: er och portar toowhitelist för Azure RemoteApp distribueras i kundens virtuella nätverk | Microsoft Docs"
description: "Lär dig vilka portar och URL: er som du behöver tooconfigure för kommunikation via Azure RemoteApp."
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
ms.openlocfilehash: 039866f7b64ac763ca833d66031ade3def1d3543
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="list-of-ports-and-urls-toopermit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Listan över URL: er och portar toopermit åtkomst för Azure RemoteApp distribueras i kundens virtuella nätverk
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Om du distribuerar en Azure RemoteApp-moln eller hybrid samling i ett virtuellt nätverk (VNET), port granska hello följande information. Mer information om virtuella nätverk [översikt över virtuella nätverk](../virtual-network/virtual-networks-overview.md). Om du har skapat en nätverkssäkerhetsgrupp (NSG) att begränsa trafik toohello virtuella resurser i samlingen, kontrollera hello följande portar är tillgänglig och tillåts via hello säkerhetsprinciper på hello virtuellt nätverk. Mer information om nätverkssäkerhetsgrupper [vad är en Nätverkssäkerhetsgrupp? (NSG) ](../virtual-network/virtual-networks-nsg.md).

## <a name="azure-remoteapp-subnet-needs-access-toothese-endpoints-and-urls"></a>Azure undernät RemoteApp åtkomst toothese slutpunkter och URL: er:
* *. servicebus.windows.net
* *. servicebus.net
* https://*.RemoteApp.windowsazure.com  
* https://www.RemoteApp.windowsazure.com 
* https://*RemoteApp.windowsazure.com  
* https://*.Core.Windows.NET  
* Utgående: TCP: TCP: 443, 9351, 9352, 10101 10175 
* Valfri – UDP: 10201 10275  

## <a name="azure-remoteapp-clients-need-access-toothese-endpoints-and-urls"></a>Azure RemoteApp-klienter behöver komma åt toothese slutpunkter och URL: er:
Av klienter som avses hello stationära datorer, enheter osv som personer detta Använd tooconnect toohello appar som har distribuerats i hello-Azure RemoteApp-samling.

* https://telemetry.RemoteApp.windowsazure.com  
* https://*.RemoteApp.windowsazure.com (hello valfria UDP-portarna är för den här adressen) 
* https://login.windows.net  
* https://login.microsoftonline.com  
* https://www.RemoteApp.windowsazure.com 
* https://*.Core.Windows.NET  
* Utgående: TCP: 443  
* Valfritt - UDP: 3391 

