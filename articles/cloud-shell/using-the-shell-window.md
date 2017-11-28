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
# <a name="using-hello-azure-cloud-shell-window"></a><span data-ttu-id="b5865-103">Med hjälp av hello Azure Cloud Shell</span><span class="sxs-lookup"><span data-stu-id="b5865-103">Using hello Azure Cloud Shell window</span></span>

<span data-ttu-id="b5865-104">Det här dokumentet förklarar hur toouse hello molnet Shell-fönstret.</span><span class="sxs-lookup"><span data-stu-id="b5865-104">This document explains how toouse hello Cloud Shell window.</span></span>

## <a name="concurrent-sessions"></a><span data-ttu-id="b5865-105">Samtidiga sessioner</span><span class="sxs-lookup"><span data-stu-id="b5865-105">Concurrent sessions</span></span>
<span data-ttu-id="b5865-106">Molnet Shell kan flera samtidiga sessioner mellan flikar i webbläsaren genom att låta varje session tooexist som en separat Bash-process.</span><span class="sxs-lookup"><span data-stu-id="b5865-106">Cloud Shell enables multiple concurrent sessions across browser tabs by allowing each session tooexist as a separate Bash process.</span></span>
<span data-ttu-id="b5865-107">Om du avslutar en session måste tooexit från varje session-fönster som varje process körs enskilt även om de körs på hello samma dator.</span><span class="sxs-lookup"><span data-stu-id="b5865-107">If exiting a session, be sure tooexit from each session window as each process runs independently although they run on hello same machine.</span></span>

## <a name="restart-cloud-shell"></a><span data-ttu-id="b5865-108">Starta om molnet Shell</span><span class="sxs-lookup"><span data-stu-id="b5865-108">Restart Cloud Shell</span></span>
![](media/recycle.png)
> [!WARNING]
> <span data-ttu-id="b5865-109">Starta om molnet Shell återställer tillståndet för datorn och alla filer inte bestående av filen resursen kommer att gå förlorade.</span><span class="sxs-lookup"><span data-stu-id="b5865-109">Restarting Cloud Shell will reset machine state and any files not persisted by your file share will be lost.</span></span>

* <span data-ttu-id="b5865-110">Klicka på ikonen för hello omstart på hello verktygsfältet tooreceive en ny molnet Shell-miljö.</span><span class="sxs-lookup"><span data-stu-id="b5865-110">Click hello restart icon on hello toolbar tooreceive a new Cloud Shell environment.</span></span>

## <a name="minimize--maximize-cloud-shell-window"></a><span data-ttu-id="b5865-111">Minimera & maximera fönster för molnet Shell</span><span class="sxs-lookup"><span data-stu-id="b5865-111">Minimize & maximize Cloud Shell window</span></span>
![](media/minmax.png)
* <span data-ttu-id="b5865-112">Klicka på hello minimera ikon på hello uppifrån höger om hello fönstret toohide den.</span><span class="sxs-lookup"><span data-stu-id="b5865-112">Click hello minimize icon on hello top right of hello window toohide it.</span></span> <span data-ttu-id="b5865-113">Klicka på hello molnet Shell ikonen igen toounhide.</span><span class="sxs-lookup"><span data-stu-id="b5865-113">Click hello Cloud Shell icon again toounhide.</span></span>
* <span data-ttu-id="b5865-114">Klicka på hello maximera ikonen tooset fönstret toomax höjd.</span><span class="sxs-lookup"><span data-stu-id="b5865-114">Click hello maximize icon tooset window toomax height.</span></span> <span data-ttu-id="b5865-115">toorestore fönstret tooprevious storlek, klicka på Återställ.</span><span class="sxs-lookup"><span data-stu-id="b5865-115">toorestore window tooprevious size, click restore.</span></span>

## <a name="copy-and-paste"></a><span data-ttu-id="b5865-116">Kopiera och klistra in</span><span class="sxs-lookup"><span data-stu-id="b5865-116">Copy and paste</span></span>
* <span data-ttu-id="b5865-117">Windows: `Ctrl-insert` toocopy och `Shift-insert` toopaste.</span><span class="sxs-lookup"><span data-stu-id="b5865-117">Windows: `Ctrl-insert` toocopy and `Shift-insert` toopaste.</span></span> <span data-ttu-id="b5865-118">Högerklicka på listrutan kan också aktivera kopiera och klistra in.</span><span class="sxs-lookup"><span data-stu-id="b5865-118">Right-click dropdown can also enable copy/paste.</span></span>
  * <span data-ttu-id="b5865-119">FireFox/IE kanske inte stöder Urklipp behörigheter korrekt.</span><span class="sxs-lookup"><span data-stu-id="b5865-119">FireFox/IE may not support clipboard permissions properly.</span></span>
* <span data-ttu-id="b5865-120">Mac OS: `Cmd-c` toocopy och `Cmd-v` toopaste.</span><span class="sxs-lookup"><span data-stu-id="b5865-120">Mac OS: `Cmd-c` toocopy and `Cmd-v` toopaste.</span></span> <span data-ttu-id="b5865-121">Högerklicka på listrutan kan också aktivera kopiera och klistra in.</span><span class="sxs-lookup"><span data-stu-id="b5865-121">Right-click dropdown can also enable copy/paste.</span></span>

## <a name="resize-cloud-shell-window"></a><span data-ttu-id="b5865-122">Ändra storlek på molnet Shell-fönster</span><span class="sxs-lookup"><span data-stu-id="b5865-122">Resize Cloud Shell window</span></span>
* <span data-ttu-id="b5865-123">Klicka och dra hello överkant hello verktygsfältet uppåt eller nedåt tooresize hello molnet Shell-fönstret.</span><span class="sxs-lookup"><span data-stu-id="b5865-123">Click and drag hello top edge of hello toolbar up or down tooresize hello Cloud Shell window.</span></span>

## <a name="scrolling-text-display"></a><span data-ttu-id="b5865-124">Visning av rullande text</span><span class="sxs-lookup"><span data-stu-id="b5865-124">Scrolling text display</span></span>
* <span data-ttu-id="b5865-125">Rulla med musen eller pekplatta toomove terminal texten.</span><span class="sxs-lookup"><span data-stu-id="b5865-125">Scroll with your mouse or touchpad toomove terminal text.</span></span>

## <a name="exit-command"></a><span data-ttu-id="b5865-126">Avslutningskommandot</span><span class="sxs-lookup"><span data-stu-id="b5865-126">Exit command</span></span>
<span data-ttu-id="b5865-127">Kör `exit` avslutar hello aktiv session.</span><span class="sxs-lookup"><span data-stu-id="b5865-127">Running `exit` terminates hello active session.</span></span> <span data-ttu-id="b5865-128">Detta inträffar som standard efter 20 minuter utan interaktion.</span><span class="sxs-lookup"><span data-stu-id="b5865-128">This behavior occurs by default after 20 minutes without interaction.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b5865-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b5865-129">Next steps</span></span>
[<span data-ttu-id="b5865-130">Molnet Shell Snabbstart</span><span class="sxs-lookup"><span data-stu-id="b5865-130">Cloud Shell Quickstart</span></span>](quickstart.md)
