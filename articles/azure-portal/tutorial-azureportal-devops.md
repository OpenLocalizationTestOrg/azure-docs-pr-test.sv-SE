---
title: "Självstudier: DevOps med hello Azure Portal | Microsoft Docs"
description: "Läs hello olika DevOps arbetsflöden i hello Azure-portalen."
services: azure-portal
documentationcenter: 
author: mlearned
manager: douge
editor: mlearned
ms.assetid: 4f1c5bc1-c732-4d35-b5df-0fd68e547d38
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2016
ms.author: mlearned
ms.openlocfilehash: 4c32dbbd4e4b1c3809ef4b01e1496e350183ebde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-devops-with-hello-azure-portal"></a>Självstudier: DevOps med hello Azure-portalen
hello Azure-plattformen är full med flexibla DevOps-arbetsflöden. I den här kursen lär du dig hur tooleverage hello funktionerna i hello Azure Portal toodevelop, testa, distribuera, felsöka, övervaka och hantera program som körs. Den här självstudiekursen fokuserar på hello följande:

1. Skapa en webbapp och aktivera kontinuerlig distribution
2. Utveckla och testa en app
3. Övervaka och felsöka en app
4. Allmänna programhanteringsaktiviteter

## <a name="creating-a-web-app-and-enabling-continuous-deployment"></a>Skapa en webbapp och aktivera kontinuerlig distribution
Skapa en webbapp med [Azure App Service](https://azure.microsoft.com/services/app-service/), som du ska använda i hello resten av den här kursen. Du ska börja med att aktivera kontinuerlig distribution från lagringsplatsen för din källkod till vår aktiva Azure-miljö.

1. Logga in på hello Azure-portalen
2. Välj **Apptjänster** &gt; **Lägg till ikonen** och ange ett namn, väljer din prenumeration och skapa en ny resurs grupp tooserve som hello behållare för hello-tjänsten.
   
   Resursgrupper kan du toomanage olika aspekter av hello lösning, till exempel fakturering, distributioner och övervakning av alla som en enda grupp via [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
   
   ![image1][image1]
3. Din apptjänst skapas efter en liten stund. Ta ett par minuter tooexplore hello olika menyalternativen för hello-tjänsten i hello-portalen.
   
   ![image2][image2]    
4. Klicka på hello-URL. Observera hello mängd tillgängliga alternativ för verktyg och databaser. Du kan också använda hello språk och ramverk önskat inklusive .NET, Java och Ruby.
   
   ![image3][image3]    
5. hello Azure-portalen gör kontinuerlig distribution med en enkel process som inbegriper några enkla steg. I hello Azure-portalen, väljer du inställningar från hello ikonen för hello apptjänst som du nyss skapade.
   
   ![image4][image4]
   
   Rulla toohello publicering avsnitt från hello bladet som öppnas på höger hello.
   
   ![image5][image5]
6. Konfigurera vissa inställningar tooenable kontinuerlig distribution för hello app. Klicka på Distributionskälla och sedan på Välj källa. Observera hello mängd olika alternativ som du har för databasen källor.
   
   ![image6][image6]
7. Välj GitHub i det här exemplet. Du kan också välja valfri hello lagringsplats och Ställ in hello-autentiseringsuppgifter.
   
   ![image7][image7]
8. Efter tillstånd tooyour databasen kan du sedan välja ett projekt och grenen som du vill toodeploy. Det finns flera fiktiva exempel nedan.
   
   ![image8][image8]
9. Klicka på OK när du har valt projekt och gren. Du bör starta toosee meddelanden för en distribution.
   
   ![image9][image9]
10. Gå tillbaka tooGitHub toosee hello webhook toointegrate hello källa kontrollen lagringsplatsen har skapats med Azure. hello Azure Portal möjliggör integrering med GitHub med några enkla steg.
    
    ![image10][image10]
11. toodemonstrate kontinuerlig distribution, du snabbt lägga till vissa innehåll toohello databas. Lägg till ett exempel text filen tooa GitHub-repo för ett enkelt exempel. Du är ledigt toouse .NET, Ruby, Python eller någon annan typ av program med App Service. Känna sig fria tooadd en textfil ASP.NET MVC, Java eller Ruby programmet toohello lagringsplatsen önskat.
    
    ![image11][image11]
12. Genomför ändringar tooyour databasen du se när en ny distribution initiera i hello portal meddelandefältet. Klicka på Synkronisera om snabbt inte ser ändringar efter genomför tooyour databasen.
    
    ![image12][image12]
13. Nu om du försöker och läsa hello sidan för hello apptjänst får ett 403-fel. I det här exemplet är det eftersom det inte finns några vanliga standardinställning dokument för hello-sidan, till exempel en fil som index.htm eller default.html. Snabbt lösa med hello tooling i hello Azure-portalen.  Välj inställningar i hello Azure Portal &gt; programinställningar.
    
     ![image13][image13]
14. Ett blad öppnas för programinställningar. Ange hello hello sidan ”SamplePage.html” och klicka på Spara. Ta ett par minuter tooexplore hello andra inställningar.
    
    ![image14][image14]
15. Du kan också uppdatera din webbläsare URL tooensure visas hello förväntade ändringar. Det finns i det här fallet enkel text nu fylla hello-sidan. Varje ytterligare ändra toohello databasen skulle resultera i en ny automatisk distribution.
    
    ![image15][image15]
    
    Aktiverar kontinuerlig distribution med hello Azure Portal är en enkel upplevelse. Du kan också skapa mer komplexa versionen pipelines och använda andra metoder med befintliga källkontrollen och kontinuerlig integration system toodeploy tooAzure, till exempel utnyttja automatiserade bygg- och versionen hanteringssystem.

## <a name="develop-and-test-an-app"></a>Utveckla och testa en app
Sedan gör några ändringar toohello kod grundläggande och snabbt distribuera ändringarna. Du kan också ställa in vissa prestandatestning för hello webbprogrammet.

1. Välj Apptjänster hello navigeringsfönstret i hello Azure-portalen och leta upp din Apptjänst.
   
   ![image16][image16]
2. Klicka på Verktyg
   
   ![image17][image17]
3. Observera hello utveckla kategori under Verktyg. Det finns flera användbara verktyg här vilket innebär att vi toowork med appar utan att lämna hello Azure-portalen. Klicka på Konsol.
   
   ![image18][image18]
4. Du kan utfärda live kommandon för din app i hello konsolfönstret. Typen hello dir kommando och trycker RETUR. Observera att kommandon som kräver utökade privilegier inte fungerar.
   
   ![image19][image19]
5. Flytta tillbaka toohello utveckla kategori och välj Visual Studio Online. Obs! Visual Studio Online heter Visual Studio Team Services nu.
   
   ![image20][image20]
6. Växla hello i webbläsaren redigera upplevelse för din App.
   
   ![image21][image21]
7. Ett webbtillägg installerar för din app. Tillägg snabbt och enkelt lägga till funktioner tooapps i Azure. Lägg märke till några av hello andra Tilläggstyper som är tillgängliga i hello skärmbilden nedan.
   
   ![image22][image22]
8. Klicka på Gå när hello Visual Studio Online-tillägget har installerats.
   
   ![image23][image23]
9. En webbläsare fliken öppnas där du ser en utveckling IDE direkt i hello webbläsare. Meddelande hello upplevelse nedan är i Chrome.
   
   ![image24][image24]
10. Du kan utföra flera aktiviteter, till exempel redigera filer, lägga till filer och mappar och ladda ned innehåll från hello liveplatsen. Göra en snabb redigera toohello SamplePage.html fil.
    
    ![image25][image25]
11. Hello ändringar sparas automatiskt i en liten stund. Om du navigerar bakåt toohello sidan kan du se hello ändringar. Tänk på att live-redigeringar som dessa sällan lämpar sig för produktionsmiljöer. Hello verktyg gör det mycket enkelt toomake snabb ändringar för utveckling och test miljöer.
    
    ![image26][image26]
    
    ![image27][image27]
12. Flytta tillbaka toohello verktyg bladet och under hello utveckla kategori, klicka på prestandatest.
    
    ![image28][image28]
13. Du måste tooset ett team services-konto. Mer information finns här: [Skapa ett Team Services-konto](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services)
14. Klicka på ny toocreate en prestandatest.
    
    ![image29][image29]
    
    Konfigurera hello olika värden och klicka på Kör Test längst hello hello dialog tooinitiate en prestandatest.
    
    ![image30][image30]
    
    ![image31][image31]
15. Du kan övervaka hello tillstånd när hello test startar körs.
    
    ![image32][image32]
    
    När hello test har slutförts visas mer information om du klickar på hello resultat.
    
    ![image33][image33]
16. I det här exemplet skapas en liten testkörning, så det finns data är begränsad tooanalyze, men du kan se olika mått samt kör testet från den här vyn. hello Azure Portal gör att skapa, köra och analysera web prestandatesterna en enkel process. hello skärmbilderna nedan visar hello prestandadata.
    
    ![image34][image34]
    
    ![image35][image35]
    
    ![image36][image36]

## <a name="monitoring-and-troubleshooting-an-app"></a>Övervaka och felsöka en app
Azure tillhandahåller många funktioner för övervakning och felsökning av program som körs.

1. Välj Verktyg i hello Azure-portalen för våra webbprogrammet.
   
   ![image37][image37]
2. Observera hello olika alternativ för att använda verktyg tootroubleshoot potentiella problem med en app som körs under hello felsöka kategori. Du kan till exempel övervaka Live HTTP-trafik, aktivera självåterställning, visa loggar osv.
   
   ![image38][image38]
3. Välj plats mått tooquickly get en vy av vissa HTTP-koder.
   
   ![image39][image39]
4. Välj Diagnostik som tjänst. Välj din programtyp och välj Kör.
   
   ![image40][image40]
   
   En insamling börjar.
   
   ![image41][image41]
5. Du kan välja hello relevant logg toodiagnose potentiella problem. Du måste tooenable loggning toosee alla tillgängliga data för hello alternativ, till exempel http-loggar.
   
   ![image42][image42]
   
   Genom att klicka på hello minnesdump filen du kan ladda ned och analysera en DebugDiag analys rapporten toohelp hitta potentiella problem.
   
   ![image43][image43]
6. tooview mer data du behöver tooenable ytterligare loggning. Navigera toohello webbprogram i hello Azure-portalen, och välja inställningar.
   
   ![image44][image44]
7. Bläddra nedåt toohello funktioner kategori och välj diagnostikloggar.
   
      ![image45][image45]
8. Observera hello olika alternativ för loggning. Aktivera webbserverloggning och klicka på Spara.
   
   ![image46][image46]
9. Flytta tillbaka toohello verktyg område för hello app väljer du diagnostik som tjänst och klicka på Kör toorerun hello datainsamling.
   
   ![image47][image47]
10. Data som samlas in för HTTP-loggar visas nu hello HTTP-loggning inställningen är aktiverad.
    
    ![image48][image48]
11. Genom att klicka på logg för hello HTML-fil, kan du skapa en omfattande webbläsarbaserad rapport för ytterligare undersökning.
    
    ![image49][image49]
12. Flytta tillbaka toohello tools-avsnittet i hello Azure-portalen för hello app. Rulla toohello verktyg avsnittet och välja Processutforskaren.
    
    ![image50][image50]
13. Genom att välja Processutforskaren kan du visa information om processer som körs. Meddelande nedan du kan detaljerat processer och även avsluta processer från hello Azure-portalen.
    
    ![image51][image51]
    
    ![image52][image52]
14. Flytta tillbaka toohello inställningsbladet hello vänster. Klicka på Ny supportförfrågan.
    
    ![image53][image53]
15. Från hello bladet på hello rätt du fyller i information om hello problem, ange kontaktinformation och även överföra diagnostikdata. hello Azure-portalen kan arbeta med Microsoft-supporten smidigt.
    
    ![image54][image54]
    
    ![image55][image55]
    
    hello Azure Portal tillhandahåller kraftfulla och välbekanta tooling upplevelser toohelp övervaka och felsöka våra program som körs. Du kan också kan tootake åtgärd snabbt genom att utföra uppgifter som återvinning processer, aktivera och inaktivera olika datasamlingar och även integrera med Microsoft professionell support.

## <a name="general-application-management"></a>Allmän programhantering
När du hanterar program behöver ofta tooperform ett stort antal aktiviteter, till exempel konfigurera säkerhetskopieringsstrategier, implementera och hantera identitetsleverantörer och konfigurera rollbaserad åtkomstkontroll. Som med hello andra DevOps-upplevelser, integrerar hello Azure-plattformen dessa uppgifter direkt i hello-portalen.

1. tooensure du hålla hello webbprogrammet från förlust av data du behöver tooconfigure säkerhetskopior. Navigera toohello inställningar för din webbapp.
   
   ![image56][image56]
2. Rulla ned toohello funktioner kategori i hello bladet på hello rätt.
   
    ![image57][image57]
3. Välj säkerhetskopiering. Då öppnas ett blad på hello rätt.
   
   ![image58][image58]
4. Klicka på Konfigurera, Välj ett lagringskonto från hello bladet på hello rätt.
   
   ![image59][image59]
5. Nu skapa och välja en lagring behållaren toohold säkerhetskopiorna. Klicka på Skapa längst hello hello-bladet. Välj sedan hello-behållare.
   
   ![image60][image60]
6. När du har valt hello behållare, kan du konfigurera scheman, samt installationsprogrammet säkerhetskopieringar för dina databaser. Klicka på hello spara ikon för det här scenariot.
   
    ![image61][image61]
7. När du har sparat Rulla tillbaka toohello bladet hello vänster för säkerhetskopiering. Klicka på Säkerhetskopiera nu tooback hello-program.
   
    ![image62][image62]
8. En säkerhetskopia skapas efter en liten stund. Meddelande hello Återställ nu alternativet på hello skärmbilden nedan.
   
    ![image63][image63]
9. Klicka på Återställ nu och undersöka hello alternativ toohello bladet på hello rätt. Du kan välja en lämplig säkerhetskopierings- och lätt återställning tooan tidigare tillstånd som krävs. hello Azure-portalen har hjälpt oss enkelt aktivera en enkel haveriberedskapsstrategi för hello app.
   
    ![image64][image64]
10. Flytta tillbaka toohello inställningsbladet hello vänster och under funktioner och välj autentisering/auktorisering.
    
     ![image65][image65]
11. Välj App Service-autentisering i hello bladet på hello rätt. Observera hello mängd olika alternativ som du kan konfigurera med populära providers.
    
     ![image66][image66]
12. Välj önskat hello-providern och Lägg märke hello alternativ för hello omfång. Du kan ange App-ID och App hemlighet och snabbt aktivera Facebook-autentisering för hello app. hello Azure-portalen aktiverar autentisering som en helhetslösning för appar.
    
     ![image67][image67]
13. Flytta tillbaka toohello inställningsbladet och Välj användare under hello resurshantering kategori.
    
     ![image68][image68]
14. Hello-bladet på hello rätt undersöka hello olika alternativ för att lägga till roller och användare. hello Azure-portalen kan du enkelt styra RBAC (rollbaserad åtkomstkontroll) för hello program.
    
     ![image69][image69]

## <a name="summary"></a>Sammanfattning
Den här självstudiekursen visas några hello ström med hello Azure-plattformen genom att snabbt aktivera kontinuerlig distribution för en webbapp, utför olika utveckling och testning aktiviteter, övervaka och felsöka en live-app och slutligen hantera nyckel strategier som katastrofåterställning, identitet och rollbaserad åtkomstkontroll. hello Azure-plattformen gör det möjligt för en integrerad upplevelse för dessa DevOps-arbetsflöden och du kan arbeta effektivt genom att hålla sig i kontexten för hello uppgiften.

## <a name="next-steps"></a>Nästa steg
* Azure Resource Manager är viktigt för att aktivera DevOps på hello Azure-plattformen.  toolearn finns mer [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
* Mer om Azure App Service-distributionen finns toolearn [distribuera din app tooAzure Apptjänst](../app-service-web/web-sites-deploy.md)

[image1]: ./media/tutorial-azureportal-devops/image1.png
[image2]: ./media/tutorial-azureportal-devops/image2.png
[image3]: ./media/tutorial-azureportal-devops/image3.png
[image4]: ./media/tutorial-azureportal-devops/image4.png
[image5]: ./media/tutorial-azureportal-devops/image5.png
[image6]: ./media/tutorial-azureportal-devops/image6.png
[image7]: ./media/tutorial-azureportal-devops/image7.png
[image8]: ./media/tutorial-azureportal-devops/image8.png
[image9]: ./media/tutorial-azureportal-devops/image9.png
[image10]: ./media/tutorial-azureportal-devops/image10.png
[image11]: ./media/tutorial-azureportal-devops/image11.png
[image12]: ./media/tutorial-azureportal-devops/image12.png
[image13]: ./media/tutorial-azureportal-devops/image13.png
[image14]: ./media/tutorial-azureportal-devops/image14.png
[image15]: ./media/tutorial-azureportal-devops/image15.png
[image16]: ./media/tutorial-azureportal-devops/image16.png
[image17]: ./media/tutorial-azureportal-devops/image17.png
[image18]: ./media/tutorial-azureportal-devops/image18.png
[image19]: ./media/tutorial-azureportal-devops/image19.png
[image20]: ./media/tutorial-azureportal-devops/image20.png
[image21]: ./media/tutorial-azureportal-devops/image21.png
[image22]: ./media/tutorial-azureportal-devops/image22.png
[image23]: ./media/tutorial-azureportal-devops/image23.png
[image24]: ./media/tutorial-azureportal-devops/image24.png
[image25]: ./media/tutorial-azureportal-devops/image25.png
[image26]: ./media/tutorial-azureportal-devops/image26.png
[image27]: ./media/tutorial-azureportal-devops/image27.png
[image28]: ./media/tutorial-azureportal-devops/image28.png
[image29]: ./media/tutorial-azureportal-devops/image29.png
[image30]: ./media/tutorial-azureportal-devops/image30.png
[image31]: ./media/tutorial-azureportal-devops/image31.png
[image32]: ./media/tutorial-azureportal-devops/image32.png
[image33]: ./media/tutorial-azureportal-devops/image33.png
[image34]: ./media/tutorial-azureportal-devops/image34.png
[image35]: ./media/tutorial-azureportal-devops/image35.png
[image36]: ./media/tutorial-azureportal-devops/image36.png
[image37]: ./media/tutorial-azureportal-devops/image37.png
[image38]: ./media/tutorial-azureportal-devops/image38.png
[image39]: ./media/tutorial-azureportal-devops/image39.png
[image40]: ./media/tutorial-azureportal-devops/image40.png
[image41]: ./media/tutorial-azureportal-devops/image41.png
[image42]: ./media/tutorial-azureportal-devops/image42.png
[image43]: ./media/tutorial-azureportal-devops/image43.png
[image44]: ./media/tutorial-azureportal-devops/image44.png
[image45]: ./media/tutorial-azureportal-devops/image45.png
[image46]: ./media/tutorial-azureportal-devops/image46.png
[image47]: ./media/tutorial-azureportal-devops/image47.png
[image48]: ./media/tutorial-azureportal-devops/image48.png
[image49]: ./media/tutorial-azureportal-devops/image49.png
[image50]: ./media/tutorial-azureportal-devops/image50.png
[image51]: ./media/tutorial-azureportal-devops/image51.png
[image52]: ./media/tutorial-azureportal-devops/image52.png
[image53]: ./media/tutorial-azureportal-devops/image53.png
[image54]: ./media/tutorial-azureportal-devops/image54.png
[image55]: ./media/tutorial-azureportal-devops/image55.png
[image56]: ./media/tutorial-azureportal-devops/image56.png
[image57]: ./media/tutorial-azureportal-devops/image57.png
[image58]: ./media/tutorial-azureportal-devops/image58.png
[image59]: ./media/tutorial-azureportal-devops/image59.png
[image60]: ./media/tutorial-azureportal-devops/image60.png
[image61]: ./media/tutorial-azureportal-devops/image61.png
[image62]: ./media/tutorial-azureportal-devops/image62.png
[image63]: ./media/tutorial-azureportal-devops/image63.png
[image64]: ./media/tutorial-azureportal-devops/image64.png
[image65]: ./media/tutorial-azureportal-devops/image65.png
[image66]: ./media/tutorial-azureportal-devops/image66.png
[image67]: ./media/tutorial-azureportal-devops/image67.png
[image68]: ./media/tutorial-azureportal-devops/image68.png
[image69]: ./media/tutorial-azureportal-devops/image69.png
