---
title: "fönster för aaaUsing hello Azure Cloud Shell (förhandsversion) | Microsoft Docs"
description: "Genomgång hello Azure Cloud Shell fönster."
services: 
documentationcenter: 
author: jluk
manager: timlt
tags: azure-resource-manager
ms.assetid: 
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: juluk
ms.openlocfilehash: 571db3c8e177799a9e05f38a7cf8d2a4d5f8c8de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-azure-cloud-shell-window"></a>Med hjälp av hello Azure Cloud Shell

Det här dokumentet förklarar hur toouse hello molnet Shell-fönstret.

## <a name="concurrent-sessions"></a>Samtidiga sessioner
Molnet Shell kan flera samtidiga sessioner mellan flikar i webbläsaren genom att låta varje session tooexist som en separat Bash-process.
Om du avslutar en session måste tooexit från varje session-fönster som varje process körs enskilt även om de körs på hello samma dator.

## <a name="restart-cloud-shell"></a>Starta om molnet Shell
![](media/recycle.png)
> [!WARNING]
> Starta om molnet Shell återställer tillståndet för datorn och alla filer inte bestående av filen resursen kommer att gå förlorade.

* Klicka på ikonen för hello omstart på hello verktygsfältet tooreceive en ny molnet Shell-miljö.

## <a name="minimize--maximize-cloud-shell-window"></a>Minimera & maximera fönster för molnet Shell
![](media/minmax.png)
* Klicka på hello minimera ikon på hello uppifrån höger om hello fönstret toohide den. Klicka på hello molnet Shell ikonen igen toounhide.
* Klicka på hello maximera ikonen tooset fönstret toomax höjd. toorestore fönstret tooprevious storlek, klicka på Återställ.

## <a name="copy-and-paste"></a>Kopiera och klistra in
* Windows: `Ctrl-insert` toocopy och `Shift-insert` toopaste. Högerklicka på listrutan kan också aktivera kopiera och klistra in.
  * FireFox/IE kanske inte stöder Urklipp behörigheter korrekt.
* Mac OS: `Cmd-c` toocopy och `Cmd-v` toopaste. Högerklicka på listrutan kan också aktivera kopiera och klistra in.

## <a name="resize-cloud-shell-window"></a>Ändra storlek på molnet Shell-fönster
* Klicka och dra hello överkant hello verktygsfältet uppåt eller nedåt tooresize hello molnet Shell-fönstret.

## <a name="scrolling-text-display"></a>Visning av rullande text
* Rulla med musen eller pekplatta toomove terminal texten.

## <a name="exit-command"></a>Avslutningskommandot
Kör `exit` avslutar hello aktiv session. Detta inträffar som standard efter 20 minuter utan interaktion.

## <a name="next-steps"></a>Nästa steg
[Molnet Shell Snabbstart](quickstart.md)
