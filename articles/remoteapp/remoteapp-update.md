---
title: aaaUpdate Azure RemoteApp-samlingen | Microsoft Docs
description: "Lär dig hur tooupdate Azure RemoteApp-samling"
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
ms.openlocfilehash: 849d7abfdfad4dbe6a235d2a28c71f7943eb812c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-a-collection-in-azure-remoteapp"></a>Uppdatera samlingen i Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Det kommer en tid, ändras när du behöver tooupdate hello appar eller avbildning i Azure RemoteApp-samlingen. Om du använder en hello avbildningarna som ingår i din Azure RemoteApp-prenumeration i ett moln eller hybrid-samling hanteras alla uppdateringar av Azure RemoteApp, så du kan vara enkel.

Om du använder en anpassad avbildning (du skapat från grunden eller som du skapade genom att ändra någon av våra bilderna) kan emellertid du ansvar för underhållet hello avbildningen och programmen. Om du behöver tooupdate avbildningen eller någon av hello apparna inuti den, måste toocreate en nya, uppdaterade version hello-avbildningen och Ersätt hello befintlig avbildning i samlingen med den här nya uppdaterade avbildningen.

Så, hur skaffar du uppdaterar din samling? Det är ganska enkelt:

1. Uppdatera hello-avbildning som du använde i samlingen. Tillämpa alla korrigeringsfiler eller uppdateringar som krävs och sedan spara det med ett nytt namn.
2. [Överför](remoteapp-uploadimage.md) eller [importera](remoteapp-image-on-azurevm.md) tooRemoteApp det bild.
3. Nu på hello samlingen klickar du på **uppdatering**.
4. Välj ny avbildning för hello hello **Mallavbildningen** lista.
5. Här ingår hello komplicerade – du behöver toodecide hur toodeal med alla användare som använder en app i hello samling. Du har hello följande alternativ:
   
   * **Ge användarna 60 minuter efter uppdateringen hello**. Så snart hello uppdateringen har slutförts visas ett meddelande tooany aktiva användare talar toosave arbetets och logg inaktiverat och logga in igen med Azure RemoteApp. Efter 60 minuter, aktiva användare som inte har loggat ut automatiskt att loggas ut. Användare kan omedelbart logga in igen.
   * **Logga ut användarna omedelbart**. Så snart hello uppdateringen har slutförts kan du logga ut alla användare automatiskt utan varning. Om du väljer det här alternativet kan användare förlora data. De kan dock återansluta toohello app omedelbart.
6. Klicka på hello markerat toostart hello uppdatera.

