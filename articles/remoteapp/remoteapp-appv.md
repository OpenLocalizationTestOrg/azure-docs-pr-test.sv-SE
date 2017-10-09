---
title: aaaUsing App-V-appar med Azure RemoteApp | Microsoft Docs
description: "Lär dig hur toouse App-V-program i Azure RemoteApp."
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
ms.openlocfilehash: 9cf5c2eeee2a0ce2cf98e1cf6497dffbc6eff016
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-app-v-apps-in-azure-remoteapp"></a>Använda App-V-appar i Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Du kan använda App-V-program i en Azure RemoteApp-hybridsamling som kräver domänanslutning.

Innan du börjar, kontrollera att tooinstall hello App-V 5.1 klienten med hello senaste uppdateringarna. Du behöver toocreate en [anpassad avbildning](remoteapp-create-custom-image.md) som innehåller hello App-V-klienten.  

Det är enkelt toouse din befintliga App-V-infrastruktur med Azure RemoteApp. Eftersom en hybridsamling distribueras till ett virtuellt Azure-nätverk som har åtkomst tooyour domänkontrollant och hello är ansluten till en domän, kan du utnyttja befintliga App-v-infrastruktur och distribution metoder tooeasyily App-V värdprogrammet i Azure RemoteApp. Här följer ett par saker som du bör vara medveten om baserat på hello typen av App-V-distribution som du har för tillfället:

| Konfigurationsalternativ |  | Positivt | Negativt |
| --- | --- | --- | --- |
| Leveransmetod |Streaming (på begäran) |Appen är alltid hello senaste och uppdaterad |Första tidsfördröjningen |
| Montera |Snabbaste; appen finns redan på hello VM |Överdriven storlek - tar upp avbildningen utrymme (gräns 127 GB) | |
| Appen plats lagring |Delat innehåll |Appen körs i minnet för Azure RemoteApp-instans |Eats minne och bra anslutning toostreaming (fil) server där app hello finns |
| Disken (Zeroed) |Snabb körning. Appen är inte beroende av tillgängligheten för innehållskälla |Överdriven storlek - tar upp avbildningen utrymme (gräns 127 GB) | |
| Mål |Användare |Kräver fullständig fristående App-V-infrastruktur | |
| Global (dator) |Publicera före eller mål med publicering server |Måste tooupdate Azure bilden om du vill tooupdate hello app (stora). Tar upp utrymme på avbildningen. | |

 När du har skapat den anpassade avbildningen och hybridsamlingen publicera programmet, tilldela användare och få dina befintliga App-V-program finns i Azure RemoteApp levereras tooany enheten var som helst.

