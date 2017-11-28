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
# <a name="use-emulator-express-toodebug-cloud-services-application-in-vs-2017"></a><span data-ttu-id="17a8f-103">Använda emulatorn Express toodebug molntjänster programmet i VS 2017</span><span class="sxs-lookup"><span data-stu-id="17a8f-103">Use Emulator Express toodebug Cloud Services application in VS 2017</span></span>
<span data-ttu-id="17a8f-104">Den här artikeln förklarar hur toouse emulatorn Express toodebug molntjänster program i VS 2017.</span><span class="sxs-lookup"><span data-stu-id="17a8f-104">This article explains how toouse Emulator Express toodebug Cloud Services applications in VS 2017.</span></span>

## <a name="background-context"></a><span data-ttu-id="17a8f-105">Bakgrund kontext</span><span class="sxs-lookup"><span data-stu-id="17a8f-105">Background context</span></span>
<span data-ttu-id="17a8f-106">Emulatorn Express används som standard för felsökning Cloud Services webb- och arbetsroller roller i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="17a8f-106">Emulator Express is used by default for debugging Cloud Services Web and Worker roles in Visual Studio.</span></span> <span data-ttu-id="17a8f-107">Den här inställningen anges i egenskapssidan för hello Cloud Services-projekt.</span><span class="sxs-lookup"><span data-stu-id="17a8f-107">This setting is specified in hello Cloud Services project properties page.</span></span>

![Öppna projektegenskaperna][0]

![Emulatorn express väljs som standard][1]

<span data-ttu-id="17a8f-110">Hej [Visual C++ Redistributable] [ Visual C++ Redistributable] för Visual Studio krävs av emulatorn express.</span><span class="sxs-lookup"><span data-stu-id="17a8f-110">hello [Visual C++ Redistributable][Visual C++ Redistributable] for Visual Studio is required by Emulator express.</span></span> <span data-ttu-id="17a8f-111">Det är inte installerad med hello arbetsbelastning i Azure.</span><span class="sxs-lookup"><span data-stu-id="17a8f-111">Currently it is not installed with hello Azure workload.</span></span> <span data-ttu-id="17a8f-112">Vid F5 rörelser toodebug Cloud Services-program, Visual Studio skulle fråga tooinstall den här komponenten och fortsätta med felsökning.</span><span class="sxs-lookup"><span data-stu-id="17a8f-112">Upon F5 gesture toodebug a Cloud Services applications, Visual Studio would prompt tooinstall this component and proceed with debugging.</span></span>

![Fråga efter installationen C++ Redistributable][2]

<span data-ttu-id="17a8f-114">Klicka på Ja tooinstall C++ Redistributable.</span><span class="sxs-lookup"><span data-stu-id="17a8f-114">Click Yes tooinstall C++ Redistributable.</span></span>

![Installera C++ Redistributable][3]

<span data-ttu-id="17a8f-116">Tryck på F5 igen toolaunch felsökning sessioner.</span><span class="sxs-lookup"><span data-stu-id="17a8f-116">Press F5 again toolaunch debugging sessions.</span></span>

![Starta felsökning][4]

![Felsökning lyckades][5]

> <span data-ttu-id="17a8f-119">Obs: Installera Visual C++ Redistributable är en görs ansträngning.</span><span class="sxs-lookup"><span data-stu-id="17a8f-119">Note: Installing Visual C++ Redistributable is a onetime effort.</span></span> <span data-ttu-id="17a8f-120">Om du uppgradering från en äldre version av Azure SDK och har installerat emulatorn Express, får inte du det här problemet.</span><span class="sxs-lookup"><span data-stu-id="17a8f-120">If you were upgrading from an older version of Azure SDK and have installed Emulator Express, then you won’t encounter this problem.</span></span>
> 
> 

## <a name="manual-workaround"></a><span data-ttu-id="17a8f-121">Manuell lösning</span><span class="sxs-lookup"><span data-stu-id="17a8f-121">Manual workaround</span></span>
<span data-ttu-id="17a8f-122">Du kan också installera hello [Visual C++ Redistributable] [ Visual C++ Redistributable] manuellt och kommer att tillämpas samma effekt som hur Visual Studio installerat på datorn.</span><span class="sxs-lookup"><span data-stu-id="17a8f-122">You can also install hello [Visual C++ Redistributable][Visual C++ Redistributable] manually and same effect will be applied as how Visual Studio installed it on your system.</span></span>

<span data-ttu-id="17a8f-123">[vcredist_x86.exe][vcredist_x86.exe]</span><span class="sxs-lookup"><span data-stu-id="17a8f-123">[vcredist_x86.exe][vcredist_x86.exe]</span></span>

<span data-ttu-id="17a8f-124">[vcredist_x64.exe][vcredist_x64.exe]</span><span class="sxs-lookup"><span data-stu-id="17a8f-124">[vcredist_x64.exe][vcredist_x64.exe]</span></span>

## <a name="next-steps"></a><span data-ttu-id="17a8f-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="17a8f-125">Next Steps</span></span>
<span data-ttu-id="17a8f-126">Mer information om hur du använder Azure datorn Emulator toodebug dina molntjänster program i Visual Studio: [med emulatorn Express toorun och felsöka en molnbaserad tjänst på en lokal dator][Using Emulator Express toorun and debug a cloud service on a local machine]</span><span class="sxs-lookup"><span data-stu-id="17a8f-126">Learn more about using Azure Computer Emulator toodebug your Cloud Services applications in Visual Studio: [Using Emulator Express toorun and debug a cloud service on a local machine][Using Emulator Express toorun and debug a cloud service on a local machine]</span></span>

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
