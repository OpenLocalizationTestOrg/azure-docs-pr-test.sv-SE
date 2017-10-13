---
title: "Felsökning av en publicerad en Azure cloud service med Visual Studio och IntelliTrace | Microsoft Docs"
description: "Lär dig att felsöka en molntjänst med Visual Studio och IntelliTrace"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5e6662fc-b917-43ea-bf2b-4f2fc3d213dc
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 7905dfb97cbd7578a8422567fe674839d00c21ef
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="debugging-a-published-azure-cloud-service-with-visual-studio-and-intellitrace"></a>Felsökning av en publicerad Azure cloud service med Visual Studio och IntelliTrace
Med IntelliTrace, kan du logga omfattande felsökningsinformation för en rollinstans när den körs i Azure. Om du behöver ta reda på orsaken till ett problem kan du använda IntelliTrace-loggar för att stega igenom koden från Visual Studio som om den kördes i Azure. I praktiken key IntelliTrace poster kodkörning och miljödata när ditt Azure-program körs som en tjänst i molnet i Azure och du kan upprepa inspelade data från Visual Studio. 

Du kan använda IntelliTrace om du har Visual Studio Enterprise installerad och din Azure-program mål .NET Framework 4 eller senare. IntelliTrace samlar in information för din Azure-roller. De virtuella datorerna för dessa roller körs alltid 64-bitars operativsystem.

Alternativt kan du använda [fjärrfelsökning](http://go.microsoft.com/fwlink/p/?LinkId=623041) att ansluta direkt till en molnbaserad tjänst som körs i Azure.

> [!IMPORTANT]
> IntelliTrace är avsedd för debug-scenarier och ska inte användas för en Produktionsdistribution.
> 

## <a name="configure-an-azure-application-for-intellitrace"></a>Konfigurera ett Azure-program för IntelliTrace
Du måste skapa och publicera programmet från Visual Studio Azure-projekt för att aktivera IntelliTrace för ett Azure-program. Du måste konfigurera IntelliTrace för ditt Azure-program innan du publicerar den till Azure. Om du publicerar ditt program utan att konfigurera IntelliTrace måste du publicera projektet. Mer information finns i [publicera ett Azure cloud services-projekt med hjälp av Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623012).

1. När du är redo att distribuera din Azure-program, kontrollera att bygga projektet målen är **felsöka**.

1. I **Solution Explorer**, högerklicka på projektet och, från snabbmenyn, Välj **publicera**.
   
1. I den **publicera Azure-programmet** dialogrutan, Välj den Azure-prenumerationen och välj **nästa**.

1. I den **inställningar** väljer den **avancerade inställningar** fliken.

1. Aktivera den **aktivera IntelliTrace** alternativet för att samla in IntelliTrace-loggar för ditt program när den publiceras i molnet.
   
1. Om du vill anpassa den grundläggande konfigurationen för IntelliTrace Välj **inställningar** bredvid **aktivera IntelliTrace**.

    ![Länk för IntelliTrace-inställningar](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/intellitrace-settings-link.png)
   
1. I den **IntelliTrace inställningar** dialogrutan kan du ange vilka händelser som ska logga, om du vill samla in information om anropet, vilka moduler och processer för att samla in loggar för och hur mycket utrymme som ska allokeras till inspelningen. Mer information om IntelliTrace finns [felsökning med IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).
   
    ![IntelliTrace-inställningar](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

IntelliTrace-logg är en cirkulär loggfil med den maximala storleken som anges i inställningarna för IntelliTrace (standardstorleken är 250 MB). IntelliTrace-loggar samlas in till en fil i filsystemet på den virtuella datorn. När du begär loggarna en ögonblicksbild tas då i gång och hämtas till den lokala datorn.

När Azure-Molntjänsten har publicerats till Azure kan du bestämma om IntelliTrace har aktiverats på noden Azure i **Server Explorer**, enligt följande bild:

![Server Explorer - IntelliTrace aktiverad](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="download-intellitrace-logs-for-a-role-instance"></a>Hämta IntelliTrace-loggar för en rollinstans
Med Visual Studio kan hämta du IntelliTrace-loggar för en rollinstans genom att följa dessa steg:

1. I **Server Explorer**, expandera den **molntjänster** nod, och leta upp rollinstansen vars loggar som du vill hämta. 

1. Högerklicka på instansen och på snabbmenyn s, Välj **visa IntelliTrace-loggar**. 

    ![Visa menyalternativet för IntelliTrace-loggar](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/view-intellitrace-logs.png)

1. IntelliTrace-loggar hämtas till en fil i en katalog på den lokala datorn. Varje gång du begär IntelliTrace-loggar, skapas en ny ögonblicksbild. När loggarna hämtas Visual Studio visar förloppet för åtgärden i den **Azure-aktivitetsloggen** fönster. Du kan expandera radobjekt för åtgärden att se mer information som visas i följande bild.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

Du kan fortsätta att arbeta i Visual Studio när hämtar IntelliTrace-loggar. När loggen har hämtats, öppnas i Visual Studio.

> [!NOTE]
> IntelliTrace-loggar kan innehålla undantag som ramen genererar och hanterar efteråt. Internt framework kod genereras undantagen som för att starta en roll, så du kan ignorera dem..
> 
> 

## <a name="next-steps"></a>Nästa steg
- [Alternativ för felsökning av Azure-molntjänster](vs-azure-tools-debugging-cloud-services-overview.md)
- [Publicera ett Azure cloud service med Visual Studio](vs-azure-tools-publishing-a-cloud-service.md)