---
title: "Använda App-V-appar med Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur du använder App-V-appar i Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: ericorman
manager: mbaldwin
ms.assetid: e2292cb2-5c89-4b2b-ab11-74dbacd07c31
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: e55bb8db83c04025c46b383a9ebbef4399178116
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-app-v-apps-in-azure-remoteapp"></a>Använda App-V-appar i Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Du kan använda App-V-program i en Azure RemoteApp-hybridsamling som kräver domänanslutning.

Innan du börjar måste du se till att installera App-V 5.1-klienten med de senaste uppdateringarna. Du behöver skapa en [anpassad avbildning](remoteapp-create-custom-image.md) som innehåller App-V-klienten.  

Det är enkelt att använda din befintliga App-V-infrastruktur med Azure RemoteApp. Eftersom en hybridsamling distribueras till ett virtuellt Azure-nätverk som har åtkomst till domänkontrollanten och de virtuella datorerna är domänansluten, kan du utnyttja din befintliga App-v-infrastrukturen och distribution av metoder för att easyily värden App-V-program i Azure RemoteApp. Här följer ett par saker som du bör vara medveten om baserat på typen av App-V-distribution som du har för tillfället:

| Konfigurationsalternativ |  | Positivt | Negativt |
| --- | --- | --- | --- |
| Leveransmetod |Streaming (på begäran) |Appen är alltid senast och nytt fönster |Första tidsfördröjningen |
| Montera |Snabbaste; appen finns redan på den virtuella datorn |Överdriven storlek - tar upp avbildningen utrymme (gräns 127 GB) | |
| Appen plats lagring |Delat innehåll |Appen körs i minnet för Azure RemoteApp-instans |Eats minne och bra anslutning till streaming (filserver) där appen finns |
| Disken (Zeroed) |Snabb körning. Appen är inte beroende av tillgängligheten för innehållskälla |Överdriven storlek - tar upp avbildningen utrymme (gräns 127 GB) | |
| Mål |Användare |Kräver fullständig fristående App-V-infrastruktur | |
| Global (dator) |Publicera före eller mål med publicering server |Om du behöver uppdatera din Azure-avbildning om du vill uppdatera appen (stora). Tar upp utrymme på avbildningen. | |

 När du har skapat den anpassade avbildningen och hybridsamlingen publicera programmet, tilldela användare och få dina befintliga App-V-program finns i Azure RemoteApp som levereras till vilken enhet som helst var som helst.

