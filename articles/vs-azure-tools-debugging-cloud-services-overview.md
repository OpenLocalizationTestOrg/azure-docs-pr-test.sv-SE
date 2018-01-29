---
title: "Alternativ för felsökning av Azure-molntjänster | Microsoft Docs"
description: "Felsöka Azure-molntjänster"
services: visual-studio-online
documentationcenter: n/a
author: mikejo
manager: ghogen
editor: 
ms.assetid: 80755da7-8350-4f5c-97ce-2962beabb36d
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/18/2017
ms.author: mikejo
ms.openlocfilehash: 196a39a75f8a3151c277f0b2a7944e6886192171
ms.sourcegitcommit: b83781292640e82b5c172210c7190cf97fabb704
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/27/2017
---
# <a name="learn-the-various-ways-to-debug-an-azure-cloud-service"></a>Lär dig mer om olika sätt att felsöka en Azure-molntjänst
Den här artikeln innehåller länkar till de olika sätt att felsöka en Azure-molntjänst. 

## <a name="debugging-an-azure-cloud-service-in-visual-studio"></a>Felsökning av en Azure-molntjänst i Visual Studio
Du kan spara tid och pengar med hjälp av Azure compute emulator felsöka din molntjänst på en lokal dator. Du kan felsöka en tjänst lokalt innan du distribuerar det för att förbättra pålitligheten och prestandan utan att betala för beräkning tid. Vissa fel inträffa dock bara när du kör en molnbaserad tjänst i Azure. Fel som inträffar bara när du kör en molnbaserad tjänst i Azure kan felsökas genom att aktivera fjärrfelsökning när du publicerar din tjänst och sedan koppla felsökaren till en rollinstans. Mer information finns i [felsöka din molntjänst på den lokala datorn](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

## <a name="using-azure-diagnostics"></a>Med hjälp av Azure-diagnostik 
Du kan använda Azure-diagnostik för att logga in detaljerad information från kod som körs i roller, oavsett om rollerna som körs i en utvecklingsmiljö eller i Azure. Mer information finns i [aktiverar Azure-diagnostik i Azure Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=400450).

## <a name="using-intellitrace"></a>Med hjälp av IntelliTrace 
Om du använder Visual Studio Enterprise för att skriva roller riktade .NET Framework 4.5, kan du aktivera IntelliTrace när du distribuerar en Azure-molntjänst från Visual Studio. IntelliTrace innehåller en logg som du kan använda med Visual Studio för att felsöka programmet som om den kördes i Azure. Mer information finns i [felsökning en publicerade molntjänst med IntelliTrace och Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623016).

## <a name="remote-debugging"></a>Fjärrfelsökning 
Du kan aktivera fjärrfelsökning i dina molntjänster vid tidpunkten när du distribuerar Molntjänsten från Visual Studio. Om du väljer att aktivera fjärrfelsökning för en distribution är felsökning Fjärrinstallationstjänster installerade på de virtuella datorer som kör varje rollinstans. Dessa tjänster – till exempel `msvsmon.exe` – inte påverkar prestanda eller leder till extra kostnader. Mer information finns i [felsöka en molnbaserad tjänst i Azure](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).

## <a name="next-steps"></a>Nästa steg
- [Felsökning av en Azure-molntjänst eller en VM i Visual Studio](./vs-azure-tools-debug-cloud-services-virtual-machines.md) -mer information om hur du felsöker Azure-molntjänster.