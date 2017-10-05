---
title: Uppdatera Azure RemoteApp-samlingen | Microsoft Docs
description: "Lär dig hur du uppdaterar din Azure RemoteApp-samling"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
editor: 
ms.assetid: e553d432-e581-48fe-b996-c432357eb64a
ms.service: remoteapp
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 454d78445d6092aec9eaa383e4c50cf15195848c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="update-a-collection-in-azure-remoteapp"></a>Uppdatera samlingen i Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

Det kommer en tid, ändras när du behöver uppdatera appar eller avbildning i Azure RemoteApp-samlingen. Om du använder en av avbildningarna som ingår i din Azure RemoteApp-prenumeration i ett moln eller hybrid-samling hanteras alla uppdateringar av Azure RemoteApp, så du kan vara enkel.

Om du använder en anpassad avbildning (du skapat från grunden eller som du skapade genom att ändra någon av våra bilderna) kan emellertid du ansvar för underhållet av avbildningen och programmen. Om du behöver uppdatera avbildningen eller apparna inuti måste du skapa en ny, uppdaterad version av bilden och Ersätt den befintliga avbildningen i samlingen med den här nya uppdaterade avbildningen.

Så, hur skaffar du uppdaterar din samling? Det är ganska enkelt:

1. Uppdatera den bild som du använde i samlingen. Tillämpa alla korrigeringsfiler eller uppdateringar som krävs och sedan spara det med ett nytt namn.
2. [Överför](remoteapp-uploadimage.md) eller [importera](remoteapp-image-on-azurevm.md) avbildningen till RemoteApp.
3. På sidan samling som du **uppdatering**.
4. Välj den nya avbildningen från den **Mallavbildningen** lista.
5. Här ingår komplicerade - måste du bestämma hur du arbetar med alla användare som använder en app i samlingen. Du har följande alternativ:
   
   * **Ge användarna 60 minuter efter uppdateringen**. När uppdateringen är klar visas ett meddelande i Azure RemoteApp till alla aktiva användare som talar om för att spara arbetet och logga ut och logga in igen. Efter 60 minuter, aktiva användare som inte har loggat ut automatiskt att loggas ut. Användare kan omedelbart logga in igen.
   * **Logga ut användarna omedelbart**. När uppdateringen är klar, logga ut alla användare automatiskt utan varning. Om du väljer det här alternativet kan användare förlora data. Men kan de återansluta till appen omedelbart.
6. Klicka på kryssmarkeringen för att starta uppdateringen.

