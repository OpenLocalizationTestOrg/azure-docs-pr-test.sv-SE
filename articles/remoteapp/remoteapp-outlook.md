---
title: aaaUsing Outlook i Azure RemoteApp | Microsoft Docs
description: "Lär dig hur tooconfigure och använder Outlook i Azure RemoteApp | Microsoft Azure"
services: remoteapp
documentationcenter: 
author: pavithir
manager: mbaldwin
ms.assetid: cb2a498f-9539-4522-a874-542114926a61
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 119d2629ac47bd8d20d617985a9b488068aa959f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-microsoft-outlook-in-azure-remoteapp"></a>Använda Microsoft Outlook i Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Azure RemoteApp stöder Microsoft Outlook O365. Läs mer om hur [Office fungerar i Azure RemoteApp](remoteapp-officesubscription.md). Det finns några rekommenderade inställningar för Outlook när det används i Azure RemoteApp.

## <a name="cached-mode"></a>Cachelagrat läge
Cachelagrat läge är en rekommenderad konfiguration när du använder Outlook i Azure RemoteApp. När du konfigurerar ett Outlook 2013 konto toouse fungerar cachelagrat Exchange-läge, Outlook 2013 från en lokal kopia av hello användarens Microsoft Exchange-postlåda som är lagrad i en offline-datafil (OST-fil) på hello användarens dator, tillsammans med hello offlineadressboken. (OAB). hello cachelagrade postlådan och OAB uppdateras regelbundet från hello O365-tjänsten. Läs mer om [hello skillnaderna mellan cachelagrat läge och onlineläge](https://technet.microsoft.com/library/jj683103.aspx).

hello användaren kan välja **cachelagrat Exchange-läge** eller **onlineläge** under kontoinstallationen eller genom att ändra inställningarna för hello-konto. Du kan också distribuera en läge eller hello andra med hjälp av hello verktyg för anpassning av Office (dessa) eller en Grupprincip.  

Läs [steg för steg-instruktionerna om hur du aktiverar cachelagrat läge](https://technet.microsoft.com/library/c6f4cad9-c918-420e-bab3-8b49e1885034#proc).

## <a name="search"></a>Söka
Sökningen i Outlook har begränsningar i Azure RemoteApp. Azure RemoteApp använder delade virtuella datorer tooaccommodate användarsessioner. Sökindexeringen beror på hello dator-ID som skiljer sig mellan olika virtuella datorer. Det är möjligt att de är riktade tooa varje gång en användare loggar in på Azure RemoteApp, ny virtuell dator. Det betyder att om vi aktiverar lokal sökning hello indexeraren körs varje gång hello datorns ID ändras (när hello användaren finns på en annan virtuell dator). Beroende på hello hello storlek. OST-fil hello indexeraren kan ta en lång tid toocomplete och använder upp resurser som krävs för andra appar. Sökningen blir inte bara långsam, den kanske inte heller ger resultat. Med hjälp av en kontoprofil onlineläge skulle komma runt problemet, men prestandan bli lidande på grund av toohello bristen på en lokal cache (se hello länken ovan för mer information om hello skillnaden mellan cachelagrat läge och onlineläge). Tyvärr går det inte att inaktivera indexerad/lokal sökning, och onlinesökning kan inte aktiveras som standard i Outlook 2013.

