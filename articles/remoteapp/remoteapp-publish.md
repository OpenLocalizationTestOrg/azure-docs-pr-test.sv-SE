---
title: aaaPublish en app i Azure RemoteApp | Microsoft Docs
description: "Lär dig hur toopublish program och resurser i Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: c7e1a2cd-8e1f-4a33-bd43-8032ec9ac952
ms.service: remoteapp
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d7d92187e9ed999ac79554c9bb61f56a8eceeb31
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toopublish-an-app-in-remoteapp"></a>Hur toopublish en app i RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

När du har skapat din RemoteApp-samling behöver du toopublish hello appar eller resurser som du vill toomake som är tillgängliga för användarna. Hej mallavbildningarna som följer med din prenumeration bara ha några appar publiceras som standard - tooshare hello andra appar måste du toopublish dem.

> [!NOTE]
> Behöver du tooupdate en app? Du behöver för[uppdatering hello avbildningen](remoteapp-update.md) första.
> 
> 

På hello **Publishing** hello-portalen klickar du på **publicera**. Du kan antingen lägga till en app från din mallavbildningen **starta** menyn eller ange hello sökvägen toowhere hello appen är installerad på hello mallavbildningen. Om du väljer tooadd från hello **starta** -menyn väljer hello app toopublish hello-listan. Om du väljer tooprovide hello sökvägen toohello app måste ange ett namn för hello app och hello sökvägen toohello app. Använda variabler i hello sökväg – till exempel ”% systemdrive %” i stället för ”c:\".

> [!NOTE]
> Om du vill tooadd appen från hello **starta** menyn måste toohave *lagts till som app-toohello **starta** menyn på mallavbildningen.* Annars RemoteApp visas bara vad *är* på hello **starta** -menyn och ska förväxlas. 
> 
> toomake att dina appar är i hello **starta** menyn placera genvägsfilen - **lnk** - inuti hello %systemdrive%\ProgramData\Microsoft\Windows\Start %ALLUSERSPROFILE%\Microsoft\Windows\Start-meny\Program.
> 
> Om du har glömt tooadd hello app toohello **starta** menyn när du skapade hello mall och välj tooadd hello sökvägen toohello app. (Eller återskapa din mallavbildningen, men det är ganska är lite mer arbete.)
> 
> 

