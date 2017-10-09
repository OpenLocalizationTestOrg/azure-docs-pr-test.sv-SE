---
title: aaaDebugging publicerade ett Azure cloud service med Visual Studio och IntelliTrace | Microsoft Docs
description: "Lär dig hur toodebug ett moln service med Visual Studio och IntelliTrace"
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
ms.openlocfilehash: 60734a71d5499de72e1ca81a3ab0ab9f34bc303a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="debugging-a-published-azure-cloud-service-with-visual-studio-and-intellitrace"></a>Felsökning av en publicerad Azure cloud service med Visual Studio och IntelliTrace
Med IntelliTrace, kan du logga omfattande felsökningsinformation för en rollinstans när den körs i Azure. Om du behöver toofind hello orsaken till ett problem kan använda du hello IntelliTrace-loggar toostep genom från Visual Studio-koden som om den kördes i Azure. I praktiken IntelliTrace viktiga kod körning och miljö postdata när ditt Azure-program körs som en tjänst i molnet i Azure och du kan spela upp hello registreras data från Visual Studio. 

Du kan använda IntelliTrace om du har Visual Studio Enterprise installerad och din Azure-program mål .NET Framework 4 eller senare. IntelliTrace samlar in information för din Azure-roller. hello virtuella datorer för dessa roller körs alltid 64-bitars operativsystem.

Alternativt kan du använda [fjärrfelsökning](http://go.microsoft.com/fwlink/p/?LinkId=623041) tooattach direkt tooa molntjänst som körs i Azure.

> [!IMPORTANT]
> IntelliTrace är avsedd för debug-scenarier och ska inte användas för en Produktionsdistribution.
> 

## <a name="configure-an-azure-application-for-intellitrace"></a>Konfigurera ett Azure-program för IntelliTrace
tooenable IntelliTrace för ett Azure-program måste du skapa och publicera programmet hello från ett Azure för Visual Studio-projekt. Du måste konfigurera IntelliTrace för ditt Azure-program innan du publicerar den tooAzure. Om du publicerar ditt program utan att konfigurera IntelliTrace måste toorepublish hello projektet. Mer information finns i [publicera ett Azure cloud services-projekt med hjälp av Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623012).

1. När du är klar toodeploy ditt Azure-program, kontrollerar du att projektet build målen är inställda för**felsöka**.

1. I **Solution Explorer**, högerklicka på projektet och, hello snabbmenyn väljer **publicera**.
   
1. I hello **publicera Azure-programmet** dialogrutan, Välj hello Azure-prenumeration och välj **nästa**.

1. I hello **inställningar** sidan, Välj hello **avancerade inställningar** fliken.

1. Aktivera hello **aktivera IntelliTrace** alternativet toocollect IntelliTrace-loggar för ditt program när den publiceras i hello molnet.
   
1. toocustomize hello IntelliTrace grundkonfiguration, Välj **inställningar** nästa för**aktivera IntelliTrace**.

    ![Länk för IntelliTrace-inställningar](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/intellitrace-settings-link.png)
   
1. I hello **IntelliTrace inställningar** dialogrutan, du kan ange vilka händelser toolog, om toocollect anropa information, vilka moduler och processer toocollect loggar för och hur mycket utrymme tooallocate toohello registrering. Mer information om IntelliTrace finns [felsökning med IntelliTrace](http://go.microsoft.com/fwlink/?LinkId=214468).
   
    ![IntelliTrace-inställningar](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

Hej IntelliTrace-logg är en cirkulär loggfil över hello maximala storleken som angavs i hello IntelliTrace-inställningar (hello standardstorlek är 250 MB). IntelliTrace-loggar är insamlade tooa filen i hello filsystem hello virtuell dator. När du begär hello loggar en ögonblicksbild tas då i gång och hämtas tooyour lokal dator.

När hello Azure-Molntjänsten har publicerade tooAzure kan du bestämma om IntelliTrace har aktiverats från hello Azure nod i **Server Explorer**som visas i följande bild hello:

![Server Explorer - IntelliTrace aktiverad](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="download-intellitrace-logs-for-a-role-instance"></a>Hämta IntelliTrace-loggar för en rollinstans
Med Visual Studio kan hämta du IntelliTrace-loggar för en rollinstans genom att följa dessa steg:

1. I **Server Explorer**, expandera hello **molntjänster** nod, och leta upp rollinstansen vars loggar som du vill toodownload. 

1. Högerklicka på hälsningspaket rollinstansen och snabbmenyn hello s, Välj **visa IntelliTrace-loggar**. 

    ![Visa menyalternativet för IntelliTrace-loggar](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/view-intellitrace-logs.png)

1. Hej IntelliTrace-loggar är hämtade tooa fil i en katalog på den lokala datorn. Varje gång du begär hello IntelliTrace-loggar, skapas en ny ögonblicksbild. Medan hello loggar hämtas, Visual Studio visar hello hello åtgärdens förlopp i hello **Azure-aktivitetsloggen** fönster. Som visas i följande bild hello, kan du expandera hello radobjekt för hello åtgärden toosee detalj.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

Du kan fortsätta toowork i Visual Studio medan hämtar hello IntelliTrace-loggar. När hello loggen har hämtats, öppnas i Visual Studio.

> [!NOTE]
> Hej IntelliTrace-loggar kan innehålla undantag hello ramverket genererar och hanterar efteråt. Internt framework kod genereras undantagen som för att starta en roll, så du kan ignorera dem..
> 
> 

## <a name="next-steps"></a>Nästa steg
- [Alternativ för felsökning av Azure-molntjänster](vs-azure-tools-debugging-cloud-services-overview.md)
- [Publicera ett Azure cloud service med Visual Studio](vs-azure-tools-publishing-a-cloud-service.md)