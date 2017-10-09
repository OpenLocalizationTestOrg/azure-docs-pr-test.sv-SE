---
Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 13: distribuera | Microsoft Docs ”beskrivning: Beskriver hur toodeploy hello kursen projektet tooAzure Analysis Services.
tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''

MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 07/17/2017 ms.author: owend
---
# <a name="lesson-13-deploy"></a>Lektion 13: Distribuera

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Nu bör konfigurera du Distributionsegenskaper. Ange en Azure Analysis Services-servern toodeploy tooand ett namn för hello modellen. Därefter kan du distribuera hello modellen toothat instans. När modellen har distribuerats kan användarna ansluta tooit med hjälp av ett reporting klientprogram. Det finns fler toolearn [distribuera tooAzure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).  
  
Uppskattad tid toocomplete lektionen: **5 minuter**  
  
## <a name="prerequisites"></a>Krav  
Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning. Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 12: Analysera i Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).  

> [!IMPORTANT]  
> Du måste ha [administratörsbehörighet](../analysis-services-server-admins.md) på hello remote Analysis Services-servern i ordning toodeploy tooit.  

> [!IMPORTANT]  
> Om du har installerat hello AdventureWorksDW2014 exempeldatabasen på en lokal SQL Server och du distribuerar din modell tooan Azure Analysis Services-server, en [lokala datagateway](../analysis-services-gateway.md) krävs.
  
## <a name="deploy-hello-model"></a>Distribuera hello modellen  
  
#### <a name="tooconfigure-deployment-properties"></a>Egenskaper för distribution av tooconfigure  

  
1.  I **Solution Explorer**, högerklicka på hello **AW Internet försäljning** projektet och klicka sedan på **egenskaper**.  
  
2.  I hello **AW Internet försäljning egenskapssidor** dialogrutan under **Distributionsserver**, i hello **Server** egenskap, ange hello hela servern.  

    ![aas-lesson13-deploy-property](../tutorials/media/aas-lesson13-deploy-property.png)
  
3.  I hello **databasen** egenskapen, skriver **Adventure Works Internet försäljning**.  
  
4.  I hello **modellnamn** egenskapen, skriver **Försäljningsmodellen för Adventure Works Internet**.  
  
5.  Kontrollera dina val och klicka sedan på **OK**.  
  
#### <a name="toodeploy-hello-adventure-works-internet-sales"></a>toodeploy hello Adventure Works Internet försäljning
  
1.  I **Solution Explorer**, högerklicka på hello **AW Internet försäljning** project > **skapa**.  

2.  Högerklicka på hello **AW Internet försäljning** project > **distribuera**.

    När du distribuerar tooAzure Analysis Services, kan du att ange tooenter ditt konto. Ange ditt organisationskonto och lösenord, till exempel nancy@adventureworks.com. Det här kontot måste vara i administratörer på hello-servern.
  
    hello distribuera dialogrutan visas och visar hello Distributionsstatus för hello metadata och varje tabell som ingår i hello modellen.  
    
    ![aas-lesson13-deploy-status](../tutorials/media/aas-lesson13-deploy-status.png)
  
3. När distributionen är klar kan du klicka på **Stäng**.  
  
## <a name="conclusion"></a>Slutsats  
Grattis! Du är färdig med att redigera och distribuera din första Analysis Services Tabular-modell. Den här kursen har hjälpt hjälper dig att slutföra hello vanligaste uppgifterna för att skapa en tabellmodell. Nu när Adventure Works Internet försäljning modellen har distribuerats, kan du använda SQL Server Management Studio toomanage hello modellen. skapa processen skript och en säkerhetskopieringsplan. Användarna kan nu också ansluta toohello modellen med hjälp av ett reporting klientprogram, till exempel Microsoft Excel eller Power BI.  

![aas-lesson13-ssms](../tutorials/media/aas-lesson13-ssms.png)
  
  
  
## <a name="whats-next"></a>Nästa steg
[Anslut med Power BI Desktop](../analysis-services-connect-pbi.md)   
[Kompletterande lektion – Dynamisk säkerhet](../tutorials/aas-supplemental-lesson-dynamic-security.md)   
[Kompletterande lektion – Detaljrader](../tutorials/aas-supplemental-lesson-detail-rows.md)   
[Kompletterande lektion – Ojämna hierarkier](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)   
