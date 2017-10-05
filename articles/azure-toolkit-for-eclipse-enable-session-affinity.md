---
title: "Aktivera Session tillhörighet med verktyget Azure för Eclipse"
description: "Lär dig mer om att aktivera session tillhörighet med verktyget Azure för Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 88c595ec-7d85-40bd-9078-8d6be7b3f0fa
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: ab8623d6f9751ed6d71d9a5b1c0d5e939c442862
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="enable-session-affinity"></a>Aktivera Session tillhörighet
Du kan aktivera HTTP-session tillhörighet eller ”Fäst sessioner”, för din roller i Azure-verktygen för Eclipse. Följande bild visar den **belastningsutjämning** egenskapsdialogrutan som används för att aktivera funktionen för mappning mellan sessionen:

![][ic719492]

## <a name="to-enable-session-affinity-for-your-role"></a>Så här aktiverar du session tillhörighet för din roll
1. Högerklicka på rollen i Eclipses Projektutforskaren, klicka på **Azure**, och klicka sedan på **belastningsutjämning**.

2. I den **egenskaper för belastningsutjämning för WorkerRole1** dialogrutan:

   a. Kontrollera **Aktivera HTTP-session tillhörighet (Fäst sessioner) för den här rollen.**

   b. För **indata slutpunkten för att använda**, Välj en slutpunkt för indata ska användas, till exempel **http (offentliga: 80, privat: 8080)**. Programmet måste använda den här slutpunkten som sin HTTP-slutpunkt. Du kan aktivera flera slutpunkter för din roll, men du kan bara markera ett av dem för att stödja Fäst sessioner.

   c. Återskapa ditt program.

När du har aktiverat, om du har mer än en rollinstans fortsätter HTTP-begäranden som kommer från en klient som hanteras av samma rollinstans.

Eclipse Toolkit gör detta genom att installera en IIS specialmoduler kallas routning av programbegäran (ARR) till var och en av dina rollinstanser. ARR dras om HTTP-begäranden till rätt roll-instans. Verktyget konfigurerar automatiskt den valda slutpunkten så att inkommande HTTP-trafik dirigeras först för ARR-programvaran. Verktyget skapar också en ny intern slutpunkt som din Java-server är konfigurerad för att lyssna på. Det är den slutpunkt som används av ARR för att dirigera om HTTP-trafik till rätt roll-instans. På så sätt kan varje instans av serverroll i flera instanser distributionen fungerar som en omvänd proxy för alla andra instanser, aktivera Fäst sessioner.

## <a name="notes-about-session-affinity"></a>Information om mappning mellan session
* Sessionen tillhörighet fungerar inte i beräkningsemulatorn. Inställningarna kan användas i beräkningsemulatorn utan att störa build-processen eller compute emulator körning, men själva funktionen fungerar inte i beräkningsemulatorn.

* Aktivera session tillhörighet resulterar i en ökning i mängden diskutrymme som används i din distribution i Azure som ytterligare programvara ska hämtas och installeras i dina rollinstanser när tjänsten startas i Azure-molnet.

* Tid att initiera varje roll tar längre tid.

* En intern slutpunkt att fungera som en trafik rerouter som nämns ovan, läggs.


## <a name="see-also"></a>Se även
[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]

[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Installera Azure Toolkit för Eclipse][Installing the Azure Toolkit for Eclipse] 

Mer information om hur du använder Azure med Java finns det [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[How to Maintain Session Data with Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->
