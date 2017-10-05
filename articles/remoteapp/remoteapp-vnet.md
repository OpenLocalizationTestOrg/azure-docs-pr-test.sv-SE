---
title: "Verifiera Azure VNET att använda med Azure RemoteApp | Microsoft Docs"
description: "Lär dig att kontrollera att ditt Azure VNET är redo att användas med Azure RemoteApp"
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
ms.openlocfilehash: 05c8a0ff04293947cec391b6467cc4adddb6a7b0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="validate-the-azure-vnet-to-use-with-azure-remoteapp"></a>Verifiera Azure VNET att använda med Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Innan du använder ett virtuellt Azure-nätverk med Azure RemoteApp kan du vill validera VNET. Detta förhindrar att problem med anslutningen.

För att verifiera ditt Azure VNET, gör du följande:

1. Skapa en virtuell Azure-dator i undernätet i Azure-VNET som du vill använda med Azure RemoteApp.
2. Anslut till den virtuella datorn med hjälp av den **Anslut** alternativet i hanteringsportalen.
3. Anslut den virtuella datorn till samma domän som du vill använda med Azure RemoteApp. Om du skapar en hybridsamling som ansluter till ditt lokala nätverk, Anslut den virtuella datorn till den lokala domänen.

Om detta lyckas är Azure VNET redo att användas med RemoteApp.

Mer information om arbetsflödet för slutpunkt till slutpunkt hybrid-samling finns i följande artiklar:

* [Planera ditt virtuella nätverk för Azure RemoteApp](remoteapp-planvnet.md)
* [Skapa en hybridsamling](remoteapp-create-hybrid-deployment.md)
* [Distribuera Azure RemoteApp-samling till ditt Azure-nätverk (med stöd för ExpressRoute)](http://blogs.msdn.com/b/rds/archive/2015/04/23/deploy-azure-remoteapp-collection-to-your-azure-virtual-network-with-support-for-expressroute.aspx)

