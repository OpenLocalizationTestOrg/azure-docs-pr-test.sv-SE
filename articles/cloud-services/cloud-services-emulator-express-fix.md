---
title: "aaaSetup emulatorn express toodebug molntjänster program i Visual Studio | Microsoft Docs"
description: "Förklarar hur tooinstall hello C++ redistributable tooenable emulatorn Express i Visual Studio"
services: cloud-services
documentationcenter: 
author: cawa
manager: paulyuk
editor: 
ms.assetid: 22b20f7a-23f4-4f7f-b536-3bf1e01adcd1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/02/2016
ms.author: cawa
ms.openlocfilehash: 6fb506f0b1384f2e52310799eb5ae2a102d777bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-emulator-express-toodebug-cloud-services-application-in-vs-2017"></a>Använda emulatorn Express toodebug molntjänster programmet i VS 2017
Den här artikeln förklarar hur toouse emulatorn Express toodebug molntjänster program i VS 2017.

## <a name="background-context"></a>Bakgrund kontext
Emulatorn Express används som standard för felsökning Cloud Services webb- och arbetsroller roller i Visual Studio. Den här inställningen anges i egenskapssidan för hello Cloud Services-projekt.

![Öppna projektegenskaperna][0]

![Emulatorn express väljs som standard][1]

Hej [Visual C++ Redistributable] [ Visual C++ Redistributable] för Visual Studio krävs av emulatorn express. Det är inte installerad med hello arbetsbelastning i Azure. Vid F5 rörelser toodebug Cloud Services-program, Visual Studio skulle fråga tooinstall den här komponenten och fortsätta med felsökning.

![Fråga efter installationen C++ Redistributable][2]

Klicka på Ja tooinstall C++ Redistributable.

![Installera C++ Redistributable][3]

Tryck på F5 igen toolaunch felsökning sessioner.

![Starta felsökning][4]

![Felsökning lyckades][5]

> Obs: Installera Visual C++ Redistributable är en görs ansträngning. Om du uppgradering från en äldre version av Azure SDK och har installerat emulatorn Express, får inte du det här problemet.
> 
> 

## <a name="manual-workaround"></a>Manuell lösning
Du kan också installera hello [Visual C++ Redistributable] [ Visual C++ Redistributable] manuellt och kommer att tillämpas samma effekt som hur Visual Studio installerat på datorn.

[vcredist_x86.exe][vcredist_x86.exe]

[vcredist_x64.exe][vcredist_x64.exe]

## <a name="next-steps"></a>Nästa steg
Mer information om hur du använder Azure datorn Emulator toodebug dina molntjänster program i Visual Studio: [med emulatorn Express toorun och felsöka en molnbaserad tjänst på en lokal dator][Using Emulator Express toorun and debug a cloud service on a local machine]

[Visual C++ Redistributable]:https://www.microsoft.com/en-us/download/details.aspx?id=30679
[vcredist_x86.exe]:https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x86.exe
[vcredist_x64.exe]:https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x64.exe
[Using Emulator Express toorun and debug a cloud service on a local machine]:https://azure.microsoft.com/en-us/documentation/articles/vs-azure-tools-emulator-express-debug-run/

[0]: ./media/cloud-services-emulator-express-fix/vs-05.png
[1]: ./media/cloud-services-emulator-express-fix/vs-06.png
[2]: ./media/cloud-services-emulator-express-fix/vs-01.png
[3]: ./media/cloud-services-emulator-express-fix/vs-02.png
[4]: ./media/cloud-services-emulator-express-fix/vs-03.png
[5]: ./media/cloud-services-emulator-express-fix/vs-04.png
