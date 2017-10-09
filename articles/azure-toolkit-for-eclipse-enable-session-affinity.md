---
title: "aaaEnable Session tillhörighet med hello Azure Toolkit för Eclipse"
description: "Lär dig hur tooenable session tillhörighet med hello Azure Toolkit för Eclipse."
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
ms.openlocfilehash: 523e728c58bda95e7af4b242e831694eb6d75cb6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-session-affinity"></a>Aktivera Session tillhörighet
Du kan aktivera HTTP-session tillhörighet eller ”Fäst sessioner”, för rollerna inom hello Azure Toolkit för Eclipse. hello följande bild visar hello **belastningsutjämning** Egenskaper dialogrutan används tooenable hello session tillhörighet funktionen:

![][ic719492]

## <a name="tooenable-session-affinity-for-your-role"></a>tooenable session tillhörighet för din roll
1. Högerklicka på hello roll i Eclipses Projektutforskaren, klicka på **Azure**, och klicka sedan på **belastningsutjämning**.

2. I hello **egenskaper för belastningsutjämning för WorkerRole1** dialogrutan:

   a. Kontrollera **Aktivera HTTP-session tillhörighet (Fäst sessioner) för den här rollen.**

   b. För **inkommande slutpunkt toouse**, till exempel väljer en indataslutpunkten toouse **http (offentliga: 80, privat: 8080)**. Programmet måste använda den här slutpunkten som sin HTTP-slutpunkt. Du kan aktivera flera slutpunkter för din roll, men du kan välja endast en av dem toosupport Fäst sessioner.

   c. Återskapa ditt program.

När du har aktiverat, om du har mer än en rollinstans, HTTP-begäranden som kommer från en viss klient kommer att fortsätta hanteras av hello samma instans av serverroll.

hello Eclipse Toolkit gör detta genom att installera en IIS specialmoduler kallas routning av programbegäran (ARR) till var och en av dina rollinstanser. ARR dras om HTTP-begäranden toohello rätt rollinstans. hello toolkit konfigurerar automatiskt hello markerade slutpunkten så att hello inkommande HTTP-trafik är första routade toohello ARR-programvara. hello toolkit skapas också en ny intern slutpunkt som Java-server är konfigurerad toolisten till. Det är hello-slutpunkt som används av ARR tooreroute hello HTTP-trafik toohello rätt rollinstans. På så sätt kan fungerar varje instans av serverroll i flera instanser distributionen som en omvänd proxy för alla hello andra instanser aktiverar Fäst sessioner.

## <a name="notes-about-session-affinity"></a>Information om mappning mellan session
* Sessionen tillhörighet fungerar inte i hello beräkningsemulatorn. hello inställningar kan användas i hello beräkningsemulatorn utan att störa build-processen eller compute emulator körning, men själva hello-funktionen fungerar inte i hello beräkningsemulatorn.

* Aktivera session tillhörighet resulterar i ökad hello mängden diskutrymme som används i din distribution i Azure som ytterligare programvara ska hämtas och installeras i dina rollinstanser när tjänsten startas i hello Azure-molnet.

* hello tid tooinitialize varje roll tar längre tid.

* En intern slutpunkt toofunction som en trafik rerouter som nämns ovan, läggs.


## <a name="see-also"></a>Se även
[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]

[Skapa ett Hello World-program för Azure i Eclipse][Creating a Hello World Application for Azure in Eclipse]

[Installera hello Azure Toolkit för Eclipse][Installing hello Azure Toolkit for Eclipse] 

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[How tooMaintain Session Data with Session Affinity]: http://go.microsoft.com/fwlink/?LinkID=699539
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic719492]: ./media/azure-toolkit-for-eclipse-enable-session-affinity/ic719492.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690950.aspx -->
