---
title: "aaaAzure RemoteApp nätverkets bandbredd – allmänna riktlinjer | Microsoft Docs"
description: "Förstå några grundläggande bandbredd riktlinjer för din Azure RemoteApp-samlingar och appar."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 529bf318-ae37-4c14-a11c-43fa703d68a3
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d3407eea71e2e8ac524787ee680cfd870c800864
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a>Azure RemoteApp-nätverkets bandbredd – allmänna riktlinjer (om du kan testa din egen)
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

Om du inte har hello tid eller kapaciteten toorun hello [nätverks bandbredd testerna](remoteapp-bandwidthtests.md) för Azure RemoteApp här följer några ganska allmänna riktlinjer som hjälper dig att beräkna bandbredd per användare.

Om du har en blandning av dessa scenarier kan vi rekommenderar inte något mindre än (eller lika med) 10 MB/s som hello minsta nätverkets bandbredd för moderna Internetanslutna appar i en fjärrmiljö. (Även om som diskuterats, inte kommer det garanterar bättre än genomsnittlig användarupplevelsen.)

## <a name="complex-powerpoint-simple-powerpoint"></a>Komplexa PowerPoint enkla PowerPoint
Azure RemoteApp är bäst på 100 MB LAN. Vid hello 10 MB/s nätverksprofil när jitter ovan 120 ms är mer än 5%, hello användaren ser en genomsnittlig upplevelse. Olika glaring - exempelvis i ett bildspel på 1 MB/s hello, hello användaren kanske inte se animerade övergångar alls eftersom ramar hoppas över.

## <a name="internet-explorer-mixed-pdf-pdf-text"></a>Internet Explorer, blandat PDF-filer, PDF, Text
10 MB/s nätverksprofil är nära tooLAN i de flesta aspekter. 1 MB/s ger en OK upplevelse, även om det kan finnas vissa jitter när en användare rullar när det finns avbildningar på hello-skärmen.

## <a name="flash-video-youtube"></a>Videon (YouTube)
100 MB/s LAN ger hello bästa möjliga upplevelse, medan 10 MB/s är acceptabel (betydelse vi Håll dig uppdaterad med hello bildfrekvens men jitter ökar). Vid 1 MB/s är mycket hög och märkbar jitter.

## <a name="word-typing-word-remote-input"></a>Word skriva (Word remote indata)
Det här är ett scenario med låg bandbredd användning. Vi ger lika bra i en upplevelse som LAN på 256 KB/sek.

## <a name="learn-more"></a>Läs mer
* [Beräkna Azure RemoteApp användningen av nätverksbandbredd](remoteapp-bandwidth.md)
* [Azure RemoteApp - hur får nätverkets bandbredd och kvaliteten på några fungerar tillsammans?](remoteapp-bandwidthexperience.md)
* [Azure RemoteApp - tseting din användandet av nätverkets bandbredd med några vanliga scenarier](remoteapp-bandwidthtests.md)

