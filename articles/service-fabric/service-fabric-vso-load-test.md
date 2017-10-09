---
title: "aaaLoad testa ditt program med hjälp av Visual Studio Team Services | Microsoft Docs"
description: "Lär dig hur toostress testa Azure Service Fabric-program med hjälp av Visual Studio Team Services."
services: service-fabric
documentationcenter: na
author: cawams
manager: timlt
editor: 
ms.assetid: fc743585-0d1b-483f-981d-493f4552ac07
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 11/18/2016
ms.author: cawa
ms.openlocfilehash: 663cf8db5e8f0a4d0d7f27b585645d7f776392f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-test-your-application-by-using-visual-studio-team-services"></a>Läs in testa ditt program med hjälp av Visual Studio Team Services
Den här artikeln visar hur toouse Microsoft Visual Studio belastningen test funktioner toostress testar ett program. Den använder en serverdel för Azure Service Fabric-tillståndskänslig service och en frontwebb för tillståndslösa tjänsten. hello är exempelprogram som används här ett flygplan plats simulatorn. Du kan ange ett flygplan ID, avvikelse tid och mål. hello programmets serverdel bearbetar hello begäranden och hello klientdelen visas på en karta hello flygplan som matchar kriterierna för hello.

hello illustrerar följande diagram hello Service Fabric-program som du ska testa.

![Diagram över hello exempelprogram flygplan plats][0]

## <a name="prerequisites"></a>Krav
Innan du börjar behöver du toodo hello följande:

* Skaffa ett Visual Studio Team Services-konto. Du kan få ett kostnadsfritt [Visual Studio Team Services](https://www.visualstudio.com).
* Hämta och installera Visual Studio 2013 eller Visual Studio 2015. Den här artikeln används Visual Studio 2015 Enterprise edition, men Visual Studio 2013 och andra utgåvor ska fungera på samma sätt.
* Distribuera ditt program tooa mellanlagring miljö. Se [hur toodeploy program tooa remote kluster med hjälp av Visual Studio](service-fabric-publish-app-remote-cluster.md) information om detta.
* Förstå användningsmönster för ditt program. Den här informationen är används toosimulate hello belastningen mönster.
* Förstå hello målet för att testa din belastningen. Detta hjälper dig att tolka och analysera hello belastningen testresultaten.

## <a name="create-and-run-hello-web-performance-and-load-test-project"></a>Skapa och köra hello webbprestanda och belastningstest projekt
### <a name="create-a-web-performance-and-load-test-project"></a>Skapa ett webbprogram prestanda och belastningstest projekt
1. Öppna Visual Studio 2015. Välj **filen** > **ny** > **projekt** på hello menyraden tooopen hello **nytt projekt** dialogrutan.
2. Expandera hello **Visual C#** nod och välj **Test** > **project webbprestanda och belastningstest**. Namnge hello projektet och välj sedan hello **OK** knappen.

    ![Skärmbild av dialogrutan för hello nytt projekt][1]

    Du bör se ett nytt webbprogram prestanda och belastningstest projekt i Solution Explorer.

    ![Skärmdump av Solution Explorer visar hello nytt projekt][2]

### <a name="record-a-web-performance-test"></a>Registrera ett webbtest prestanda
1. Öppna hello .webtest projekt.
2. Välj hello **Lägg till registrering** ikonen toostart en inspelning session i webbläsaren.

    ![Skärmbild som visar hello Lägg till inspelning ikon i en webbläsare][3]

    ![Skärmbild som visar hello post-knappen i en webbläsare][4]
3. Bläddra toohello Service Fabric-programmet. hello inspelning panelen ska visa hello webbegäranden.

    ![Skärmbild av webbförfrågningar i hello inspelning panelen][5]
4. Utför en sekvens med åtgärder som du förväntar dig hello användare tooperform. Dessa åtgärder kan användas som en mönster toogenerate hello belastning.
5. När du är klar väljer du hello **stoppa** knappen toostop registrering.

    ![Skärmbild som visar hello stopp-knappen][6]

    Hej .webtest projekt i Visual Studio bör har tagit emot ett antal begäranden. Dynamiska parametrar ersätts automatiskt. Nu kan du ta bort några extra, upprepade beroende-begäranden som inte är en del av Testscenario.
6. Spara hello projektet och välj sedan hello **kör testa** kommandot toorun hello webbprestanda testa lokalt och kontrollera att allt fungerar korrekt.

    ![Skärmbild som visar hello kör Test-kommando][7]

### <a name="parameterize-hello-web-performance-test"></a>Parameterstyra hello webbtest prestanda
Du skapa en parameter utifrån hello webbtest prestanda genom att konvertera den tooa kodade webbtest prestanda och sedan redigera hello kod. Alternativt kan binda du hello prestanda test tooa datalistan så att hello test går igenom hello data. Se [generera och kör en kodad web prestandatest](https://msdn.microsoft.com/library/ms182552.aspx) mer information om hur tooconvert hello web prestanda test tooa kodats test. Se [lägga till en prestandatest för datakällan tooa web](https://msdn.microsoft.com/library/ms243142.aspx) information om hur toobind data tooa web prestanda testa.

I det här exemplet ska vi konvertera hello prestanda test tooa kodade webbtest så att du kan ersätta hello flygplan ID med en GUID som genererats och lägga till fler begäranden toosend flygplan toodifferent platser.

### <a name="create-a-load-test-project"></a>Skapa ett projekt för test av belastningen
Ett projekt för test av belastningen består av ett eller flera av de scenarier som beskrivs av hello webbtest prestanda och enhetstest, tillsammans med ytterligare angivna belastningen testa inställningar. hello följande steg visar hur toocreate en belastning Testa projektet:

1. Välj på snabbmenyn i projektets webbprestanda och belastningstest hello **Lägg till** > **belastningstest**. I hello **belastningen testa** guiden, Välj hello **nästa** knappen tooconfigure hello testa inställningar.
2. I hello **mönster för att läsa in** väljer du om du vill att en konstant användarbelastningen eller en steg-belastning som börjar med ett fåtal användare och ökar hello användare över tid.

    Om du har en bra uppskattning av hello mängden användarbelastningen och vill toosee hur hello aktuella utförs, välja **konstant ladda**. Om målet är toolearn om hello utförs konsekvent under olika belastningar väljer **steg belastningen**.
3. I hello **testa blandning** väljer hello **Lägg till** knappen och markera sedan hello test som du vill tooinclude i hello belastningstest. Du kan använda hello **Distribution** kolumnen toospecify hello procent av totalt tester som körs för varje test.
4. I hello **inställningar** ange hello belastningen testperioden.

   > [!NOTE]
   > Hej **testa iterationer** alternativet är tillgängligt endast när du kör ett belastningstest lokalt med hjälp av Visual Studio.
   >
   >
5. I hello **plats** avsnitt i **inställningar**, ange hello plats där begäranden om belastning test genereras. hello-guiden kan du uppmanas toolog i tooyour Team Services-konto. Logga in och sedan välja en geografisk plats. När du är klar väljer du hello **Slutför** knappen.
6. När du har skapat hello belastningstest öppna hello .loadtest projektet och välj hello aktuella köra inställning, till exempel **inställningar** > **kör Settings1 [Active]**. Då öppnas hello kör inställningar i hello **egenskaper** fönster.
7. I hello **resultat** avsnitt i hello **inställningar** egenskapsfönstret, hello **tidsinställning information lagring** inställningen ska ha **ingen** som Standardvärdet. Ändra värdet för**alla enskilda uppgifter** tooget mer information om hello belastningen testresultaten. Se [ladda testning](https://www.visualstudio.com/load-testing.aspx) mer information om hur tooconnect tooVisual Studio Team Services och kör ett test.

### <a name="run-hello-load-test-by-using-visual-studio-team-services"></a>Kör hello belastningstest med hjälp av Visual Studio Team Services
Välj hello **kör belastningen testa** kommandot toostart hello testkörning.

![Skärmbild som visar hello kör belastningstest kommando][8]

## <a name="view-and-analyze-hello-load-test-results"></a>Visa och analysera hello belastningen testresultat
Läser in test fortlöper som hello, hello prestandainformation läggs till i diagram. Du bör se något liknande toohello följande diagram.

![Skärmbild som visar prestandadiagram för testresultat][9]

1. Välj hello **hämta rapport** länk hello övre delen av hello-sidan. När hello rapporten har hämtats kan välja hello **visa rapporten** knappen.

    På hello **diagram** visas diagram för olika prestandaräknare. På hello **sammanfattning** fliken hello övergripande testresultaten visas. Hej **tabeller** visar hello Totalt antal skickade och misslyckade belastningstester.
2. Välj hello numret länkar på hello **Test** > **misslyckades** och hello **fel** > **antal** kolumner toosee felinformation.

    Hej **detaljer** fliken visas virtuella användar- och testa scenariot information för misslyckade begäranden. Data kan vara användbar om hello belastningstest innehåller flera scenarier.

Se [analysera läsa in Testa resultaten i hello diagram vy över hello ladda testa Analyzer](https://www.visualstudio.com/load-testing.aspx) testresultat för mer information om hur du visar belastning.

## <a name="automate-your-load-test"></a>Automatisera dina belastningstest
Visual Studio Team Services ladda Test innehåller API: er toohelp du hanterar belastningstester och analysera resultaten i ett Team Services-konto. Se [moln belastningen testning Rest-API: er](http://blogs.msdn.com/b/visualstudioalm/archive/2014/11/03/cloud-load-testing-rest-apis-are-here.aspx) för mer information.

## <a name="next-steps"></a>Nästa steg
* [Övervakning och diagnos av tjänster i en inställning för utveckling av lokal dator](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[0]: ./media/service-fabric-vso-load-test/OverviewDiagram.png
[1]: ./media/service-fabric-vso-load-test/NewProjectDialog.png
[2]: ./media/service-fabric-vso-load-test/Project.png
[3]: ./media/service-fabric-vso-load-test/AddRecording.png
[4]: ./media/service-fabric-vso-load-test/AddRecording2.png
[5]: ./media/service-fabric-vso-load-test/ActionSequence.png
[6]: ./media/service-fabric-vso-load-test/StopRecording.png
[7]: ./media/service-fabric-vso-load-test/RunTest.png
[8]: ./media/service-fabric-vso-load-test/RunTest2.png
[9]: ./media/service-fabric-vso-load-test/Graph.png
