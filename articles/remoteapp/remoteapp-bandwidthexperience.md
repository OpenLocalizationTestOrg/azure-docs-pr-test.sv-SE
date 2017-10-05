---
title: "Azure RemoteApp - hur får nätverkets bandbredd och kvaliteten på några fungerar tillsammans? | Microsoft Docs"
description: "Lär dig hur nätverkets bandbredd i Azure RemoteApp kan påverka dina användares kvalitet."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 74ebc1fb-5187-4056-b08c-0e03b5ecaca6
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: 74116902e973fba440b3c662fdf76202d052b4c8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Azure RemoteApp - hur får nätverkets bandbredd och kvaliteten på några fungerar tillsammans?
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.
> 
> 

När du tittar på det [övergripande nätverksbandbredd](remoteapp-bandwidth.md) krävs för Azure RemoteApp, Tänk på följande faktorer: dessa är en del av ett dynamiskt system som påverkar användarupplevelsen. 

* **Tillgänglig nätverksbandbredd och aktuella nätverksförhållanden** – en uppsättning parametrar (förlust, svarstid, jitter) i samma nätverk vid en given tidpunkt kan påverka programmet direktuppspelning, vilket innebär en nedsänkt användarupplevelsen. Bandbredden som är tillgängliga i ditt nätverk är en funktion av överbelastning, slumpmässiga förlust, svarstid eftersom dessa parametrar påverkar mekanismen för kontroll av överbelastning vilket i sin tur kontroller överföringshastigheten för att undvika konflikter.  Till exempel gör en lossy nätverk eller ett nätverk med hög latens att användaren ska få felaktiga även på ett nätverk med 1 000 MB bandbredd. Förluster eller fördröjningar variera beroende på antalet användare som finns i samma nätverk och vad användarna gör (till exempel titta på videor, hämtning eller överföringen av stora filer, skriva ut).
* **Användningsscenariot** -upplevelsen beror på vad användarna gör som enskilda användare och som en grupp i samma nätverk. Till exempel kräver läsa en bild bara en bildruta uppdateras. Om användaren skims och rullar innehållet i ett textdokument, måste de ett högre antal bildrutor uppdateras per sekund. Kommunikationen fram och tillbaka till servern i det här scenariot kommer slutligen att använda mer bandbredd i nätverket. Även överväga ett ytterst exempel: flera användare titta på videor för hög definition (till exempel 4K-matchning), innehåller HD konferenssamtal, 3D video spel eller arbetar på CAD-system. Alla dessa kan göra även ett nätverk med mycket hög bandbredd praktiskt taget oanvändbar.
* **Skärmen upplösning och antalet skärmbilder** -mer bandbredd i nätverket krävs för att en fullständig uppdatering större skärmar än mindre skärmar. Den underliggande tekniken är ganska bra på kodning och överföra endast områden av skärmarna som har uppdaterats, men en gång då och då, hela skärmen behöver uppdateras. När användaren har en högre upplösning (till exempel 4K-upplösning), kräver att update mer bandbredd i nätverket än en skärm med lägre upplösning (till exempel 1024x768px). Den här samma logik som gäller om du använder mer än en skärm för omdirigering. Bandbredden måste öka med antalet skärmar.
* **Omdirigering av Urklipp och enheten** – detta är ett mycket inte uppenbara problem, men i många fall om en användare lagrar en stor del av data till Urklipp, tar det lite tid att information ska överföras från fjärrskrivbordsklienten till servern. Underordnade upplevelsen kan påverkas av upplevelse av överföring av urklippsinnehåll uppströms. Detsamma gäller för enhetsomdirigering av – om en skanner eller webbkamera genererar mycket data som måste skickas överordnad till servern, eller en skrivare från att ta emot ett stort dokument eller lokal lagring måste vara tillgängliga för en app som körs i molnet för att kopiera en stor fil , användare märker uteslutna bildrutor eller tillfälligt ”frusen” video eftersom de data som krävs för omdirigering av enheter ökar nätverksbandbredden behöver. 

När du utvärderar din nätverksbandbredd måste du se till att fundera över alla dessa faktorer som fungerar som ett system.

Gå nu tillbaka till den [huvudnätverket bandbredd artikel](remoteapp-bandwidth.md), eller gå vidare till att testa din [nätverksbandbredd](remoteapp-bandwidthtests.md).

## <a name="learn-more"></a>Läs mer
* [Beräkna Azure RemoteApp användningen av nätverksbandbredd](remoteapp-bandwidth.md)
* [Azure RemoteApp - testa din användandet av nätverkets bandbredd med några vanliga scenarier](remoteapp-bandwidthtests.md)
* [Azure RemoteApp-nätverkets bandbredd – allmänna riktlinjer (om du kan testa din egen)](remoteapp-bandwidthguidelines.md)

