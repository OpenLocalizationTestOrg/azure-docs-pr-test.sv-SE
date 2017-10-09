---
title: aaaValidate hello Azure VNET toouse med Azure RemoteApp | Microsoft Docs
description: "Lär dig hur toomake till ditt Azure VNET är klar toouse med Azure RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: b573ba02-4587-4be5-9821-27bd891a73b2
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/23/2016
ms.author: mbaldwin
ms.openlocfilehash: 5587556c264356e6ab6039b983a38cb2b95ed268
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="validate-hello-azure-vnet-toouse-with-azure-remoteapp"></a>Validera hello Azure VNET toouse med Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Du kanske vill toovalidate hello VNET innan du använder ett virtuellt Azure-nätverk med Azure RemoteApp. Detta förhindrar att problem med anslutningen.

toovalidate ditt Azure VNET hello följande:

1. Skapa en virtuell Azure-dator i hello undernätet för hello Azure VNET som du vill toouse med Azure RemoteApp.
2. Ansluta toothat VM med hjälp av hello **Anslut** alternativet i hello-hanteringsportalen.
3. Anslut hello virtuella toohello samma domän som du vill toouse med Azure RemoteApp. Om du skapar en hybridsamling som ansluter tooyour lokalt nätverk att ansluta hello virtuella tooyour lokala domänen.

Om detta lyckas är hello Azure VNET klar toouse med RemoteApp.

Mer information om hello slutpunkt till slutpunkt hybrid samling arbetsflöde finns hello följande artiklar:

* [Hur tooplan ditt virtuella nätverk för Azure RemoteApp](remoteapp-planvnet.md)
* [Skapa en hybridsamling](remoteapp-create-hybrid-deployment.md)
* [Distribuera Azure RemoteApp-samlingen tooyour Azure Virtual Network (med stöd för ExpressRoute)](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)

