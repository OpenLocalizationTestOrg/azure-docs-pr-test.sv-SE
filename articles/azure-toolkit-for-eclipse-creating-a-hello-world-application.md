---
title: "Skapa en Hello World molntjänst för Azure i Eclipse"
description: "Lär dig hur du skapar ett enkelt Hello World-program med hjälp av Azure-verktygen för Eclipse."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 7262e705-59d6-43ce-b888-29a21c8e0cb7
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 9b31f0faeb6ee7b5e7b8fe3a1f2827133d6188e6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>Skapa en Hello World molntjänst för Azure i Eclipse
Följande steg visar hur du skapar och distribuerar en grundläggande JSP-appen till Azure med Azure-verktygen för Eclipse. Ett exempel på JSP visas för enkelhetens skull, men hög liknande steg skulle vara lämplig för ett Java-servlet som Azure-distribution är.

Programmet ser ut ungefär så här:

![][ic600360]

## <a name="prerequisites"></a>Krav
* En Java Developer Kit (JDK), v 1.7 eller senare.
* Solförmörkelse IDE för Java EE-utvecklare Indigo eller senare. Detta kan hämtas från <http://www.eclipse.org/downloads/>.
* En distribution av en Java-baserad webbserver eller programservern, till exempel Apache Tomcat GlassFish, JBoss-programservern, Jetty eller IBM® WebSphere® programmet Server Liberty Core.
* En Azure-prenumeration som kan erhållas från <http://azure.microsoft.com/pricing/purchase-options/>.
* Azure Toolkit för Eclipse. Mer information finns i [installera Azure Toolkit för Eclipse][Installing the Azure Toolkit for Eclipse].

## <a name="to-create-a-hello-world-application"></a>Skapa ett Hello World-program
Först ska vi börja med att skapa ett Java-projekt.

1. Starta Eclipse och menyn klickar du på **filen**, klickar du på **ny**, och klicka sedan på **dynamiskt webbprojekt**. (Om du inte ser **dynamiskt webbprojekt** visas som tillgängliga projekt när du klickar på **filen**, **ny**, gör du följande: Klicka på **filen**, klickar du på **ny**, klickar du på **projekt...** , expandera **Web**, klickar du på **dynamiskt webbprojekt**, och klicka på **nästa**.)

1. Namnge projektet för den här kursen är **MyHelloWorld**. (Se till att du använder det här namnet, efterföljande steg i den här självstudiekursen räknar WAR-filen namnet MyHelloWorld). Skärmen visas som liknar följande:

   ![][ic589576]

1. Klicka på **Slutför**.

1. I Eclipses Projektutforskaren vy Expandera **MyHelloWorld**. Högerklicka på **Webbinnehåll**, klicka på **Ny** och sedan på **JSP-fil**.

1. I den **ny JSP-fil** dialogrutan, namnge filen **index.jsp**. Behåll den överordnade mappen som **MyHelloWorld/webbinnehåll**, enligt följande:

   ![][ic659262]

1. I den **Välj JSP-mall** dialogrutan för den här självstudiekursen väljer **ny JSP-fil (html)** och på **Slutför**.

1. När index.jsp-filen öppnas i Eclipse, lägga till i text som ska visas dynamiskt **Hello World!** i det befintliga `<body>`-elementet när index.jsp-filen öppnas i Eclipse. Uppdaterade `<body>` innehåll ska visas som följande:
   ```
   <body>
   <b><% out.println("Hello World!"); %></b>
   </body>
   ```
1. Spara index.jsp.

## <a name="to-deploy-your-application-to-azure-the-quick-and-simple-way"></a>Distribuera programmet till Azure, snabbt och enkelt sätt
Du kan använda följande genväg för att testa direkt på Azure-molnet så snart du har ett Java webbprogram som är redo att testa.

1. Klicka i Eclipses Projektutforskaren **MyHelloWorld**.

2. I verktygsfältet i Eclipse klickar du på den **publicera** nedrullningsbar knapp och klicka sedan på **Publicera som Azure Cloud Service**

   ![][publishDropdownButton]

3. Om du publicerar programmet till Azure för första gången och du inte har skapat ett Azure-distribution projekt för det här programmet innan du kan skapa ett Azure-distribution-projekt du automatiskt. Du bör se följande fråga som visar också JDK-paketet och programservern som ska distribueras automatiskt för att köra programmet.

   ![][ic789598]
   
   Genväg på så sätt kan ett snabbt och enkelt sätt att testa ditt program i Azure utan att behöva konfigurera en viss server eller JDK som skiljer sig från standardvärdena. Om du är nöjd med standardinställningarna kan du klicka på **OK** att fortsätta med följande steg.
   Men om du vill ändra JDK eller programserver för ditt program kan du göra det senare genom att redigera projektet Azure-distribution som har skapats automatiskt åt dig, eller klicka **Avbryt** nu och läsa  **Om Azure-distribution projekt** i den här kursen.

4. I den **publicera till Azure** dialogrutan:

   1. Om det inte finns några prenumerationer att välja i den **prenumeration** listan ännu, Följ dessa steg om du vill importera din prenumerationsinformation:
      1. Klicka på **Import från PUBLICERINGSINSTÄLLNINGARNA filen**.
      2. I den **importera prenumerationsinformation** dialogrutan klickar du på **hämta PUBLICERINGSINSTÄLLNINGARNA filen**. Om du inte ännu loggat in på ditt Azure-konto, uppmanas du att logga in. Du uppmanas sedan att spara en Azure inställningsfilen för publicering. Spara den på din lokala dator.
      3. Fortfarande i den **importera prenumerationsinformation** dialogrutan klickar du på den **Bläddra** knappen Välj publicera inställningsfilen som du sparade lokalt i föregående steg och klicka sedan på **öppna**. Skärmen bör se ut ungefär så här:![][ic644267]
      4. Klicka på **OK**.
   2. För **prenumeration**, Välj den prenumeration som du vill använda för din distribution.
   3. För **lagringskonto**, Välj lagringskonto som du vill använda, eller klicka på **ny** att skapa ett nytt lagringskonto.
   4. För **tjänstnamnet**, Välj den molntjänst som du vill använda, eller klicka på **ny** att skapa en ny molntjänst.
   5. För **mål OS**, Välj versionen av operativsystemet som du vill använda för din distribution.
   6. För **målmiljön**, för den här kursen väljer **mellanlagring**. (När du är redo att distribuera till din plats för produktion, ska du ändra den till **produktion**.)
   7. Valfritt: Se till att **skriva över tidigare distribution** är markerad om du vill att nya distributionen för att automatiskt skriva över den tidigare distribueringen. När du aktiverar det här alternativet kommer inte experience ”409 konflikt” problem vid publicering till samma plats.
      Observera att den **publicera till Azure** dialogrutan innehåller ett avsnitt för **fjärråtkomst**. Fjärråtkomst är inte aktiverat som standard och vi kommer inte att aktivera den för det här exemplet. Om du vill aktivera fjärråtkomst, skulle du ange ett användarnamn och lösenord som ska användas vid inloggning via fjärranslutning. Mer information om fjärråtkomst finns [att aktivera fjärråtkomst för Azure-distributioner i Eclipse][Enabling Remote Access for Azure Deployments in Eclipse].
      Din **publicera till Azure** dialogrutan visas som liknar följande:![][ic719488]

5. Klicka på **publicera** att publicera till mellanlagringsmiljön.

   När du uppmanas att utföra en fullständig version, klickar du på **Ja**. Det kan ta flera minuter innan den första versionen.
   En **Azure-aktivitetsloggen** visas i avsnittet Eclipse flikar vyer.
   ![][ic719489]Du kan använda den här loggfilen samt de **konsolen** vy, för att se förloppet för distributionen. Ett alternativ är att logga in på den [Azure-hanteringsportalen][Azure Management Portal], och använda den **molntjänster** avsnittet för att övervaka status.

6. När distributionen har distribuerats på **Azure-aktivitetsloggen** visar statusen **publicerade**. Klicka på **publicerade**, enligt följande bild och webbläsaren öppnas en instans av distributionen.

   ![][ic719490]

Eftersom det var en distribution till en fristående miljö kan DNS-namnet kommer att vara i formatet http://&lt;*guid*&gt;. cloudapp.net och URL: en får innehålla DNS-namn plus ett suffix för ditt program. Till exempel http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld. (Den **MyHelloWorld** delen är skiftlägeskänsligt.) Du kan också se DNS-namn om du klickar på distributionens namn i hanteringsportalen för Azure-plattformen (inom den molntjänster delen av hanteringsportalen).

Även om den här genomgången var för en distribution till mellanlagringsmiljön, en distribution till tillverkning följer samma steg, förutom i den **publicera till Azure** markerar **produktion** i stället för **Mellanlagring** för den **målmiljön**. En distribution till tillverkning resulterar i en URL som baseras på DNS-namnet på ditt val i stället för ett GUID som används för mellanlagring.

> [!WARNING]
> Nu har du distribuerat ditt Azure-program till molnet. Men innan du fortsätter, Tänk på att ett distribuerat program, även om den inte körs, kommer att fortsätta att tillkomma fakturerbar tid för din prenumeration. Därför är det mycket viktigt att du tar bort oönskade distributioner från din Azure-prenumeration.
> 
> 

## <a name="about-azure-deployment-projects"></a>Om av Azure-distributionsprojekt
Ett projekt för distribution av Azure krävs för att distribuera en eller flera Java-program till Azure. Det spelar roll ”paketets” som dina program behöver som ska omslutas i för att kunna publiceras på Azure.

Förutom information om dina program, ett projekt för Azure-distribution innehåller även information om andra viktiga komponenter av distributionen viktigast av allt: server-behållare för program att köra ditt webbprogram i och Java runtime ska köras på. Azure stöder ett stort urval av Java-körningar och Java-programservrar som du kan välja mellan.

Även om det här exemplet förenklas för utbildning, kan ett projekt för Azure-distribution även innehålla andra viktig konfigurationsinformation som låter dig skapa nästan godtyckligt komplexa, skalbar, hög tillgänglighet molntjänster med flera nivåer med dina program. Du kan aktivera **session tillhörighet (”Fäst sessioner”)**, **snabb cachelagring**, **SSL genom att avlasta**, **brandvägg/port routning**, **fjärråtkomst**, och ett antal andra kraftfulla funktioner.

Om du har fyllt i föregående avsnitt i den här kursen (”för att distribuera programmet till Azure, snabbt och enkelt sätt”), visas nu ett nytt Azure-distribution-projekt i Projektutforskaren genereras automatiskt och med namnet ” **MyHelloWorld_onAzure**”.

Du kan också startat den här självstudiekursen först skapa ett tomt Azure distributionsprojekt själv och sedan lägga till dina program till den. Det är en längre process, men ger dig större kontroll över den inledande konfigurationen från början.

Klicka för att skapa ett nytt Azure-distribution-projekt från grunden i **nya projekt för distribution av Azure** knappen ![][ic710876].

Oavsett om du arbetar med ett redan befintligt projekt för Azure-distribution, eller skapar en helt ny, du kan ändra konfigurationsinställningarna och komponenter, till exempel JDK eller programservern lika enkelt när som helst.

Ändra JDK, eller programservern eller listan över i ett befintligt projekt för Azure-distribution:

1. Expandera projektnoden (t.ex. **MyHelloWorld_onAzure**) i Projektutforskaren

2. Högerklicka på **WorkerRole1**

3. Expandera den **Azure** undermenyn i snabbmenyn

4. Klicka på **serverkonfiguration**

Oavsett om du har skapat de här stegen för konfiguration av servern genom att redigera ett befintligt projekt för Azure-distribution som visas ovan eller skapar en ny från början, ser samma typ av dialogrutor så att du kan konfigurera JDK, server och program komponenter. Om du vill lära dig mer om hur du ändrar inställningarna i dessa dialogrutor, till exempel för att ändra JDK, programservern och lägga till eller ta bort program i en distribution finns i [Server konfigurationsegenskaper] [ Server configuration properties] artikel.

## <a name="windows-only-to-deploy-your-application-to-the-compute-emulator"></a>Endast Windows: att distribuera programmet till beräkningsemulatorn

> [!NOTE]
> Azure-emulatorn är endast tillgängligt i Windows. Hoppa över det här avsnittet om du använder ett annat operativsystem än Windows.
> 
> 

Om du har skapat ett nytt Azure distributionsprojekt följa stegen ovan, d.v.s. implicit genom att publicera programmet till Azure, JDK och programmet har servrar konfigurerats för molnet, men inte för lokala emulering. Följ dessa steg för att förbereda ditt projekt för att testa i lokala-emulatorn:

1. Klicka i Eclipses Projektutforskaren **MyHelloWorld_onAzure**.

2. Högerklicka på **WorkerRole1**.

3. Expandera den **Azure** undermeny på snabbmenyn.

4. Klicka på **serverkonfiguration**.

5. På den **JDK** kontrollerar om verktyget har förkonfigurerat standard lokala JDK för dig. Om inte, eller om du vill ändra antagen standardvärdena, se till att den **använder JDK från den här sökvägen för lokal testning** är markerad och JDK-installationsplatsen som du vill använda har angetts. Om du vill ändra det, klickar du på den **Bläddra** och använder kontrollen Bläddra, Välj katalogplatsen av JDK ska användas.

6. Klicka på den **Server** fliken.

7. I den **sökväg till lokal server** textrutan längst ned i dialogrutan Ange sökvägen till en server som matchar typen och högre versionsnumret för den server som valts överst i dialogrutan under lokalt installerade på  **Distribuera en server på en sådan** kryssrutan. Om du vill använda en annan typ eller högre version av application server ändra under kryssrutan först.

8. Klicka på **OK**.

9. I verktygsfältet i Eclipse klickar du på den **körs i Azure-emulatorn** knappen ![][ic710879]. Om den **körs i Azure-emulatorn** knappen inte är aktiverad, se till att **MyHelloWorld_onAzure** väljs i Eclipses Projektutforskaren och se till att Eclipses Projektutforskaren har fokus som aktuellt fönstret. Detta först starta en fullständig version av projektet och starta sedan webbprogrammet Java i beräkningsemulatorn. (Observera att beroende på datorns prestandaegenskaper, den första versionen kan det ta mellan några sekunder att några minuter, men efterföljande versioner får snabbare.) Det första steget i Skapa har slutförts uppmanas du av Windows UAC User Account Control () så att det här kommandot för att göra ändringar på datorn. Klicka på **Ja**.

> [!IMPORTANT]
> Om du inte ser UAC-fråga, kontrollera Aktivitetsfältet för UAC-ikonen och klicka först. Ibland UAC uppmaning visas inte som det översta fönstret, men visas endast som en ikon i Aktivitetsfältet.
> 
> 

1. Granska resultatet av compute emulator UI för att avgöra om det uppstår några problem med ditt projekt. Det kan ta några minuter innan programmet startas helt inom beräkningsemulatorn beroende på innehållet i distributionen.

2. Starta din webbläsare och använda URL: en `http://localhost:8080/MyHelloWorld` som adressen (den `MyHelloWorld` delen i Webbadressen är skiftlägeskänsligt). Du bör se MyHelloWorld programmet (utdata i index.jsp) liknar följande bild:

   ![][ic589579]

När du är redo att stoppa programmet från att köras i beräkningsemulatorn i Eclipse-verktygsfältet klickar du på den **återställa Azure-emulatorn** knappen ![][ic710880].

## <a name="to-delete-your-deployment"></a>Ta bort distributionen
Se till att ta bort distributionen i Azure-verktygen för Eclipse **MyHelloWorld_onAzure** är valt i Eclipses Projektutforskaren, kontrollera Projektutforskaren Eclipse har det aktuella fönstret fokusera och klicka sedan på  **Avpublicera** knappen ![][ic710883], i Eclipse-verktygsfältet. (Du kan göra samma åtgärd genom att högerklicka på **MyHelloWorld_onAzure** i Eclipses Projektutforskaren, klicka på **Azure** och sedan klicka på **Undeploy från Azure-molnet**.) Detta visas den **avpublicera Azure-projekt** dialogrutan.

![][ic719491]

Välj tjänsten prenumerationen och i molnet som innehåller din distribution, väljer du den distribution som du vill ta bort och klicka sedan på **Upphäv publicering**.

(Ett alternativ till att använda verktyget för att ta bort distributionen är att använda den **molntjänster** avsnitt i Azure-hanteringsportalen: Gå till din distribution markerar du den och klicka sedan på den **ta bort** knappen. Detta stoppar och sedan ta bort distributionen. Om du bara vill att stoppa distributionen och ta inte bort den, klickar du på den **stoppa** knappen i stället för den **ta bort** knappen, men som nämns ovan, om du inte ta bort distributionen fakturerbar avgifter fortsätter att påförs för din distribution även om den är stoppad).

## <a name="see-also"></a>Se även
[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]

[Installera Azure Toolkit för Eclipse][Installing the Azure Toolkit for Eclipse] 

[Vad är nytt i Azure Toolkit för Eclipse][What's New in the Azure Toolkit for Eclipse]

Mer information om hur du använder Azure med Java finns det [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Enabling Remote Access for Azure Deployments in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699538
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Server configuration properties]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic589576]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589576.png
[ic589579]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589579.png
[ic600360]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic600360.png
[ic644267]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic644267.png
[ic659262]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic659262.png
[ic710876]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710876.png
[ic710879]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710880.png
[ic710882]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710883.png
[ic719488]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719488.png
[ic719489]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719489.png
[ic719490]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719490.png
[ic719491]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719491.png
[ic789598]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic789598.png
[publishDropdownButton]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/publishDropdownButton.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690944.aspx -->
