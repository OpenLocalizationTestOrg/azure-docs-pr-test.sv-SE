---
title: "aaaAzure RemoteApp - hur får nätverkets bandbredd och kvaliteten på några fungerar tillsammans? | Microsoft Docs"
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
ms.openlocfilehash: 62b0caadf63359eceb63d27fae6ad289b682ff63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-remoteapp---how-do-network-bandwidth-and-quality-of-experience-work-together"></a>Azure RemoteApp - hur får nätverkets bandbredd och kvaliteten på några fungerar tillsammans?
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

När du tittar på hello [övergripande nätverksbandbredd](remoteapp-bandwidth.md) krävs för Azure RemoteApp, Kom ihåg hello följande faktorer – detta är en del av ett dynamiskt system som påverkar hello övergripande användarupplevelsen. 

* **Tillgänglig nätverksbandbredd och aktuella nätverksförhållanden** – en uppsättning parametrar (förlust, svarstid, jitter) på samma nätverk vid en given tidpunkt hello kan påverka hello programmet direktuppspelning, vilket innebär en nedsänkt övergripande användarupplevelsen. hello bandbredd som är tillgänglig i nätverket är en funktion av överbelastning, slumpmässiga förlust, latens eftersom dessa parametrar påverkar mekanism för kontroll av hello överbelastning, som i tur kontroller hello överföring hastighet tooavoid kollisioner.  Till exempel gör en lossy nätverk eller ett nätverk med hög latens hello användarmiljö felaktiga även på ett nätverk med 1 000 MB bandbredd. hello förluster eller fördröjningar variera beroende på hello antalet användare som finns på hello samma nätverk och vad användarna gör (till exempel titta på videor, hämtning eller överföringen av stora filer, skriva ut).
* **Användningsscenariot** -hello upplevelse beror på vilken hello användarna gör som enskilda användare och som en grupp på hello samma nätverk. Till exempel kräver läsa en bild bara en enda ram toobe som uppdateras. Om användaren hello skims och rullar över hello innehållet i ett textdokument, måste de ett högre antal bildrutor toobe uppdateras per sekund. Hej kommunikation tillbaka och tillbaka toohello servern i det här scenariot kommer slutligen att använda mer bandbredd i nätverket. Även överväga ett ytterst exempel: flera användare titta på videor för hög definition (till exempel 4K-matchning), innehåller HD konferenssamtal, 3D video spel eller arbetar på CAD-system. Alla dessa kan göra även ett nätverk med mycket hög bandbredd praktiskt taget oanvändbar.
* **Skärmen upplösning och hello antal skärmar** -mer bandbredd i nätverket är obligatoriska toofull uppdatering större skärmar än mindre skärmar. hello underliggande tekniken är ganska bra på kodning och överföra endast hello regionerna hello skärmar som har uppdaterats, men en gång då och då, hello hela skärmen måste toobe uppdateras. När hello användare har en högre upplösning (till exempel 4K-upplösning), kräver som uppdatering mer bandbredd i nätverket än en skärm med lägre upplösning (till exempel 1024x768px). Den här samma logik som gäller om du använder mer än en skärm för omdirigering. Bandbredden måste tooincrease med hello antal skärmar.
* **Omdirigering av Urklipp och enheten** – detta är ett mycket inte uppenbara problem, men i många fall om en användare lagrar en stor del av data toohello Urklipp, tar det lite tid för att information tootransfer från hello fjärrskrivbordsklienten toohello server. hello underordnade upplevelse kan påverkas av hello upplevelse av att skicka innehållet i Urklipp hello uppströms. hello samma sak gäller för enhetsomdirigering av – om en skanner webbkamera genererar mycket data som måste skickas toobe överordnade toohello server eller en skrivare måste tooreceive ett stort dokument eller lokal lagring måste toobe tillgängliga tooan app körs i hello molnet toocopy en stora filer, märker användarna uteslutna bildrutor eller tillfälligt ”frusen” video eftersom hello data som behövs för omdirigering av enheter hello ökar hello nätverksbandbredd behöver. 

När du utvärderar din nätverksbandbredd måste se till att tooconsider alla dessa faktorer som fungerar som ett system.

Gå tillbaka toohello [huvudnätverket bandbredd artikel](remoteapp-bandwidth.md), eller flytta på tootesting din [nätverksbandbredd](remoteapp-bandwidthtests.md).

## <a name="learn-more"></a>Läs mer
* [Beräkna Azure RemoteApp användningen av nätverksbandbredd](remoteapp-bandwidth.md)
* [Azure RemoteApp - testa din användandet av nätverkets bandbredd med några vanliga scenarier](remoteapp-bandwidthtests.md)
* [Azure RemoteApp-nätverkets bandbredd – allmänna riktlinjer (om du kan testa din egen)](remoteapp-bandwidthguidelines.md)

