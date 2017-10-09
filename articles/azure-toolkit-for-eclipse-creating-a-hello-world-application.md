---
title: "aaaCreate en Hello World molntjänst för Azure i Eclipse"
description: "Lär dig hur toocreate en enkel Hello World program med hjälp av hello Azure Toolkit för Eclipse."
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
ms.openlocfilehash: dfb81374aaf78e933c0bf83a1dbd98023801491a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>Skapa en Hello World molntjänst för Azure i Eclipse
hello följande steg visar hur toocreate och distribuera en grundläggande JSP programmet tooAzure med hello Azure Toolkit för Eclipse. Ett exempel på JSP visas för enkelhetens skull, men hög liknande steg skulle vara lämplig för ett Java-servlet som Azure-distribution är.

programmet hello ser liknande toohello följande:

![][ic600360]

## <a name="prerequisites"></a>Krav
* En Java Developer Kit (JDK), v 1.7 eller senare.
* Solförmörkelse IDE för Java EE-utvecklare Indigo eller senare. Detta kan hämtas från <http://www.eclipse.org/downloads/>.
* En distribution av en Java-baserad webbserver eller programservern, till exempel Apache Tomcat GlassFish, JBoss-programservern, Jetty eller IBM® WebSphere® programmet Server Liberty Core.
* En Azure-prenumeration som kan erhållas från <http://azure.microsoft.com/pricing/purchase-options/>.
* hello Azure Toolkit för Eclipse. Mer information finns i [installerar hello Azure Toolkit för Eclipse][Installing hello Azure Toolkit for Eclipse].

## <a name="toocreate-a-hello-world-application"></a>toocreate ett Hello World-program
Först ska vi börja med att skapa ett Java-projekt.

1. Starta Eclipse och hello-menyn klickar du på **filen**, klickar du på **ny**, och klicka sedan på **dynamiskt webbprojekt**. (Om du inte ser **dynamiskt webbprojekt** visas som tillgängliga projekt när du klickar på **filen**, **ny**, sedan hello följande: Klicka på **filen**, klickar du på **ny**, klickar du på **projekt...** , expandera **Web**, klickar du på **dynamiskt webbprojekt**, och klicka på **nästa**.)

1. Namn för den här kursen är hello projektet **MyHelloWorld**. (Se till att du använder det här namnet, efterföljande steg i den här självstudiekursen räknar din WAR-filen toobe med namnet MyHelloWorld). Skärmen visas liknande toohello följande:

   ![][ic589576]

1. Klicka på **Slutför**.

1. I Eclipses Projektutforskaren vy Expandera **MyHelloWorld**. Högerklicka på **Webbinnehåll**, klicka på **Ny** och sedan på **JSP-fil**.

1. I hello **ny JSP-fil** dialogrutan, namnet hello fil **index.jsp**. Behåll hello överordnade mappen som **MyHelloWorld/webbinnehåll**som visas i hello följande:

   ![][ic659262]

1. I hello **Välj JSP-mall** dialogrutan för den här självstudiekursen väljer **ny JSP-fil (html)** och på **Slutför**.

1. När hello index.jsp-filen öppnas i Eclipse, lägga till texten toodynamically visas på **Hello World!** inom hello befintliga `<body>` element. Uppdaterade `<body>` innehåll ska visas som hello följande:
   ```
   <body>
   <b><% out.println("Hello World!"); %></b>
   </body>
   ```
1. Spara index.jsp.

## <a name="toodeploy-your-application-tooazure-hello-quick-and-simple-way"></a>toodeploy ditt program tooAzure, hello snabbt och enkelt sätt
Du kan använda hello följande genväg tootry den ut direkt på hello Azure cloud så snart du har en klar tootest för Java web application.

1. Klicka i Eclipses Projektutforskaren **MyHelloWorld**.

2. I hello Eclipse i verktygsfältet klickar du på hello **publicera** nedrullningsbar knapp och klicka sedan på **Publicera som Azure Cloud Service**

   ![][publishDropdownButton]

3. Om du publicerar det här programmet tooAzure för hello första gången och du inte har skapat ett Azure-distribution projekt för det här programmet innan du kan skapa ett Azure-distribution-projekt du automatiskt. Du bör se hello följande fråga, som också visar hello JDK paket och program server som ska automatiskt distribueras toorun ditt program.

   ![][ic789598]
   
   Genväg på så sätt kan ett snabbt och enkelt sätt tootest ditt program i Azure utan tooconfigure en viss server eller JDK som skiljer sig från hello standardvärden. Om du är nöjd med hello standardinställningar kan du klicka på **OK** toocontinue med hello följande steg.
   Men om du vill toochange hello JDK eller programmet server toouse för ditt program, kan du göra det senare genom att redigera hello Azure distributionsprojekt som skapades automatiskt av du eller klicka **Avbryt** nu och läsa Hej **om Azure-distribution projekt avsnittet** i den här kursen.

4. I hello **publicera tooAzure** dialogrutan:

   1. Om det finns inga prenumerationer tooselect i hello **prenumeration** listan ännu, Följ dessa steg tooimport din prenumerationsinformation:
      1. Klicka på **Import från PUBLICERINGSINSTÄLLNINGARNA filen**.
      2. I hello **importera prenumerationsinformation** dialogrutan klickar du på **hämta PUBLICERINGSINSTÄLLNINGARNA filen**. Om du inte ännu loggat in på ditt Azure-konto, kommer du att ange toolog i. Sedan uppmanas du att toosave en Azure inställningsfilen för publicering. Spara den tooyour lokal dator.
      3. Fortfarande i hello **importera prenumerationsinformation** dialogrutan klickar du på hello **Bläddra** knappen, Välj hello Publicera fil med inställningar som du sparade lokalt i hello föregående steg och klicka sedan på  **Öppna**. Skärmen bör se ut ungefär toohello följande:![][ic644267]
      4. Klicka på **OK**.
   2. För **prenumeration**, Välj hello prenumeration som du vill använda för din distribution.
   3. För **lagringskonto**, Välj hello storage-konto som du vill toouse eller klicka på **ny** toocreate ett nytt lagringskonto.
   4. För **tjänstnamnet**, Välj hello molntjänst som du vill toouse eller klicka på **ny** toocreate en ny molntjänst.
   5. För **mål OS**väljer hello version av operativsystemet för hello som du vill toouse för din distribution.
   6. För **målmiljön**, för den här kursen väljer **mellanlagring**. (När du är klar toodeploy tooyour produktionsplatsen ska du ändra detta för**produktion**.)
   7. Valfritt: Se till att **skriva över tidigare distribution** är markerad om du vill att din nya distribution tooautomatically överskrivning hello tidigare distribution. När du aktiverar det här alternativet kommer inte experience ”409 konflikt” problem när du publicerar toohello samma plats.
      Observera att hello **publicera tooAzure** dialogrutan innehåller ett avsnitt för **fjärråtkomst**. Fjärråtkomst är inte aktiverat som standard och vi kommer inte att aktivera den för det här exemplet. tooenable fjärråtkomst, anger du en användare och lösenord toouse vid inloggning via fjärranslutning. Mer information om fjärråtkomst finns [att aktivera fjärråtkomst för Azure-distributioner i Eclipse][Enabling Remote Access for Azure Deployments in Eclipse].
      Din **publicera tooAzure** öppnas och liknande toohello följande:![][ic719488]

5. Klicka på **publicera** toopublish toohello mellanlagringsmiljön.

   När begärd tooperform en fullständig version, klickar på **Ja**. Det kan ta flera minuter för hello första version.
   En **Azure-aktivitetsloggen** visas i avsnittet Eclipse flikar vyer.
   ![][ic719489]Du kan använda den här loggfilen samt hello **konsolen** visa toosee hello förloppet för distributionen. Ett alternativ är toolog i toohello [Azure-hanteringsportalen][Azure Management Portal], och använda hello **molntjänster** avsnittet toomonitor hello status.

6. När distributionen har distribuerats, hello **Azure-aktivitetsloggen** visar statusen **publicerade**. Klicka på **publicerade**, enligt följande hello avbildningen och webbläsaren öppnas en instans av distributionen.

   ![][ic719490]

Eftersom det var en distribution tooa mellanlagring miljö hello DNS-namn ska vara av hello formuläret http://&lt;*guid*&gt;. cloudapp.net och hello URL innehåller hello DNS-namn plus ett suffix för ditt program. Till exempel http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld. (hello **MyHelloWorld** delen är skiftlägeskänsligt.) Du kan också se hello DNS namn om du klickar på hello distributionens namn i hello hanteringsportalen för Azure-plattformen (inom hello molntjänster delen av hello management portal).

Även om den här genomgången var för en distribution toohello mellanlagring miljö, en distribution tooproduction följer hello samma steg utom inom hello **publicera tooAzure** markerar **produktion** i stället för **mellanlagring** för hello **målmiljön**. En distribution tooproduction resulterar i en URL som baseras på hello DNS-namn du själv väljer, i stället för ett GUID som används för mellanlagring.

> [!WARNING]
> Nu har du distribuerat ditt Azure-program toohello moln. Men innan du fortsätter, Tänk på att ett distribuerat program, även om den inte körs, kommer att fortsätta tooaccrue fakturerbar tid för din prenumeration. Därför är det mycket viktigt att du tar bort oönskade distributioner från din Azure-prenumeration.
> 
> 

## <a name="about-azure-deployment-projects"></a>Om av Azure-distributionsprojekt
Order toodeploy en eller flera Java-program tooAzure, ett projekt för distribution av Azure krävs. Det spelar hello roll hello ”package” som dina program behöver toobe kapslas in i i ordning toobe publiceras på Azure.

Förutom hello information om dina program, ett projekt för Azure-distribution innehåller även information om andra viktiga komponenter av distributionen viktigast av allt: hello programmet server behållaren toorun webbappen i och hello Java runtime toorun den på. Azure stöder ett stort urval av Java-körningar och Java-programservrar som du kan välja mellan.

Även om hello exempel används här förenklas för utbildning, kan ett projekt för Azure-distribution även innehålla andra viktig konfigurationsinformation som du kan använda toocreate nästan godtyckligt komplexa, skalbar, hög tillgänglighet molntjänster med flera nivåer med dina program. Du kan aktivera **session tillhörighet (”Fäst sessioner”)**, **snabb cachelagring**, **SSL genom att avlasta**, **brandvägg/port routning**, **fjärråtkomst**, och ett antal andra kraftfulla funktioner.

Om du har slutfört hello föregående avsnitt i den här kursen (”toodeploy ditt program tooAzure, hello snabbt och enkelt sätt”), visas nu ett nytt projekt i Azure-distribution i hello Projektutforskaren genereras automatiskt och med namnet ” **MyHelloWorld_onAzure**”.

Du kan också startat den här självstudiekursen först skapa ett tomt Azure distributionsprojekt själv och sedan lägga till ditt program tooit. Det är en längre process, men ger dig större kontroll över hello inledande konfiguration från hello början.

toocreate ett nytt projekt i Azure-distribution från början, klicka på hello **nya projekt för distribution av Azure** knappen ![][ic710876].

Oavsett om du arbetar med ett redan befintligt projekt för Azure-distribution, eller skapar en helt ny, du kan toochange dess konfigurationsinställningar och komponenter, till exempel hello JDK eller hello programserver lika enkelt när som helst.

toochange hello JDK, eller hello programserver eller hello programlistan i ett befintligt projekt för Azure-distribution:

1. Expandera hello projektnoden (t.ex. **MyHelloWorld_onAzure**) i hello Projektutforskaren

2. Högerklicka på **WorkerRole1**

3. Expandera hello **Azure** undermenyn i hello snabbmenyn

4. Klicka på **serverkonfiguration**

Oavsett om du startade dessa konfigurationssteg för servern genom att redigera ett befintligt projekt för Azure-distribution som visas ovan eller skapar en ny från början, ser du hello samma typ av dialogrutor så att du tooconfigure din JDK och programinställningar komponenter. toolearn mer hur toochange hello inställningar i dessa dialogrutor, till exempel toochange hello JDK, hello programserver och lägga till eller ta bort program i en distribution finns hello [Server konfigurationsegenskaper] [ Server configuration properties] artikel.

## <a name="windows-only-toodeploy-your-application-toohello-compute-emulator"></a>Endast Windows: toodeploy program-toohello beräkningsemulatorn

> [!NOTE]
> hello Azure-emulatorn är endast tillgängligt i Windows. Hoppa över det här avsnittet om du använder ett annat operativsystem än Windows.
> 
> 

Om du har skapat ett nytt Azure distributionsprojekt följande hello stegen som beskrivs ovan, d.v.s. implicit genom att publicera dina program tooAzure hello JDK och programservrar har konfigurerats för hello moln, men inte för lokala emulering. tooprepare ditt projekt för att testa i lokala hello-emulatorn gör så här:

1. Klicka i Eclipses Projektutforskaren **MyHelloWorld_onAzure**.

2. Högerklicka på **WorkerRole1**.

3. Expandera hello **Azure** undermenyn i hello snabbmenyn.

4. Klicka på **serverkonfiguration**.

5. På hello **JDK** kontrollerar om hello toolkit har förkonfigurerat standard lokala JDK för dig. Om inte, eller om du vill toochange hello antas standarder, kontrollera att hello **Använd hello JDK från den här sökvägen för lokal testning** är markerad och hello JDK installationsplats som du vill toouse har angetts. Om du vill toochange, klicka på hello **Bläddra** och använder hello Bläddra kontroll, Välj hello katalogplatsen för hello JDK toouse.

6. Klicka på hello **Server** fliken.

7. I hello **sökväg till lokal server** textrutan längst ned hello hello dialogrutan Ange hello sökväg för en server som matchar hello typ och högre versionsnummer för hello-server som valts hello överst i dialogrutan för hello under lokalt installerade Hej **distribuera en server på en sådan** kryssrutan. Om du vill toouse en annan typ eller högre version av hello-programservern kan du ändra hello valet under kryssrutan först.

8. Klicka på **OK**.

9. I hello Eclipse i verktygsfältet klickar du på hello **körs i Azure-emulatorn** knappen ![][ic710879]. Om hello **körs i Azure-emulatorn** knappen inte är aktiverad, se till att **MyHelloWorld_onAzure** väljs i Eclipses Projektutforskaren och se till att Eclipses Projektutforskaren har fokus som hello aktuellt fönster. Detta startar en fullständig version av projektet och starta din Java-webbprogram i hello beräkningsemulatorn. (Observera att beroende på datorns prestandaegenskaper hello första build kan ta mellan några sekunder tooa några minuter, men efterföljande versioner får snabbare.) Hello första build steget har slutförts uppmanas du av Windows UAC User Account Control () tooallow toomake för det här kommandot ändrar tooyour dator. Klicka på **Ja**.

> [!IMPORTANT]
> Om du inte ser hello UAC kommandotolk, kontrollera hello Aktivitetsfältet för hello UAC-ikonen och klickar på den först. Ibland hello UAC-fråga visas inte som det översta fönstret, men visas endast som en ikon i Aktivitetsfältet.
> 
> 

1. Granska hello utdata från hello compute emulator UI toodetermine om det uppstår några problem med ditt projekt. Det kan ta ett par minuter för ditt program toobe startats helt inom hello beräkningsemulatorn hello innehållet i distributionen.

2. Starta din webbläsare och använda hello URL `http://localhost:8080/MyHelloWorld` som hello-adress (hello `MyHelloWorld` del av hello URL är skiftlägeskänsligt). Du bör se din MyHelloWorld program (index.jsp hello utdata), liknande toohello följande bild:

   ![][ic589579]

När du är klar toostop ditt program körs i hello beräkningsemulatorn i hello Eclipse i verktygsfältet klickar du på hello **återställa Azure-emulatorn** knappen ![][ic710880].

## <a name="toodelete-your-deployment"></a>toodelete distributionen
toodelete distributionen inom hello Azure Toolkit för Eclipse, se till att **MyHelloWorld_onAzure** är valt i Eclipses Projektutforskaren, kontrollera hello Eclipse Projektutforskaren har hello aktuella fönstret fokusera och klicka sedan på Hej **Upphäv publicering** knappen ![][ic710883], i hello Eclipse-verktygsfältet. (Du kan göra hello samma åtgärd genom att högerklicka på **MyHelloWorld_onAzure** i Eclipses Projektutforskaren, klicka på **Azure** och sedan klicka på **Undeploy från Azure-molnet**.) Detta visar hello **avpublicera Azure-projekt** dialogrutan.

![][ic719491]

Välj hello prenumerationen och i molnet tjänsten som innehåller din distribution, väljer hello distribution du vill toodelete och klicka sedan på **Upphäv publicering**.

(En alternativ toousing hello toolkit toodelete hello distribution är toouse hello **molntjänster** avsnitt i hello Azure-hanteringsportalen: navigera tooyour distribution, markerar du den och klicka sedan på hello **ta bort** knappen. Detta stoppar och sedan ta bort hello distributionen. Om du bara vill toostop hello distribution och inte ta bort, klickar du på hello **stoppa** knappen i stället för hello **ta bort** knappen om hello distribution, fakturerbar avgifter tas inte bort men som nämns ovan, Fortsätt tooaccrue för din distribution även om den är stoppad).

## <a name="see-also"></a>Se även
[Azure Toolkit för Eclipse][Azure Toolkit for Eclipse]

[Installera hello Azure Toolkit för Eclipse][Installing hello Azure Toolkit for Eclipse] 

[Vad är nytt i hello Azure Toolkit för Eclipse][What's New in hello Azure Toolkit for Eclipse]

Mer information om hur du använder Azure med Java finns hello [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Enabling Remote Access for Azure Deployments in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699538
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Server configuration properties]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[What's New in hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

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
