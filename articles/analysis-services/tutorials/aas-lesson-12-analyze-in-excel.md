---
Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 12: Analysera i Excel | Microsoft Docs ”beskrivning: Beskriver hur toouse analysera i Excel i hello Azure Analysis Services-självstudiekurs projektet. tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''

MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 2017-05/26 ms.author: owend
---
# <a name="lesson-12-analyze-in-excel"></a>Lektion 12: Analysera i Excel

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

I kursen använder hello analysera i Excel funktionen tooopen Microsoft Excel, skapa automatiskt en anslutning toohello modellen arbetsyta och lägga till automatiskt en pivottabell toohello kalkylblad. hello analysera i Excel-funktionen är avsedd tooprovide snabbt och enkelt sätt tootest hello effekt av modellen utforma tidigare toodeploying din modell. Du utför inte någon dataanalys under den här lektionen. hello syftet med den här lektionen är toofamiliarize, hello modellen skriva, med hello verktyg du kan använda tootest modelldesignen.   
  
toocomplete kursen Excel måste vara installerad på hello samma dator som SSDT.
  
Uppskattad tid toocomplete lektionen: **fem minuter**  
  
## <a name="prerequisites"></a>Krav  
Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning. Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 11: skapa roller](../tutorials/aas-lesson-11-create-roles.md).  
  
## <a name="browse-using-hello-default-and-internet-sales-perspectives"></a>Bläddra med hello standard och Internet försäljning perspektiv  
Dessa första uppgifter du bläddra i din modell med hjälp av både hello standard perspektiv, som innehåller alla modellobjekt, och med hjälp av hello Internet försäljning perspektiv du tidigare. hello Internet försäljning perspektiv undantar hello tabellobjekt för kunden.  
  
#### <a name="toobrowse-by-using-hello-default-perspective"></a>toobrowse med hjälp av hello standard perspektiv  
  
1.  Klicka på hello **modellen** menyn > **analysera i Excel**.  
  
2.  I hello **analysera i Excel** dialogrutan klickar du på **OK**.  
  
    Excel öppnas med en ny arbetsbok. Anslutning till en datakälla skapas med hjälp av hello det aktuella användarkontot och hello standardperspektivet är synlig används toodefine-fält. En pivottabell läggs automatiskt toohello kalkylblad.  
  
3.  I hello i Excel **Fältlista**, meddelande hello **DimDate** och **FactInternetSales** måttgrupper visas. Hej **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory**, och **FactInternetSales** tabeller med sina respektive kolumner visas också.  
  
4.  Stäng Excel utan att spara hello arbetsboken.  
  
#### <a name="toobrowse-by-using-hello-internet-sales-perspective"></a>toobrowse med hjälp av hello Internet försäljning perspektiv  
  
1.  Klicka på hello **modellen** -menyn och klicka sedan på **analysera i Excel**.  
  
2.  I hello **analysera i Excel** dialogrutan lämna **aktuella Windows-användaren** markerade, i hello **perspektiv** nedrullningsbara listrutan för väljer **Internet försäljning** , och klicka sedan på **OK**. 
    
    ![aas-lesson12-perspective](../tutorials/media/aas-lesson12-perspective.png)
    
3.  I Excel i **PivotTable-Fields**, Lägg märke till hello DimCustomer tabeller tas inte hello fältlistan.  
    
    ![aas-lesson12-fields](../tutorials/media/aas-lesson12-fields.png)
    
4.  Stäng Excel utan att spara hello arbetsboken.  
  
## <a name="browse-by-using-roles"></a>Bläddra med hjälp av roller  
Roller är en viktig del av alla tabellmodeller. Utan minst en roll läggs toowhich användare till som medlemmar och användare inte kan komma åt och analysera data med hjälp av din modell. hello analysera i Excel-funktionen kan du tootest hello roller som du har definierat.  
  
#### <a name="toobrowse-by-using-hello-sales-manager-user-role"></a>toobrowse med hjälp av hello försäljningschef användarroll  
  
1.  Klicka på hello i SSDT, **modellen** -menyn och klicka sedan på **analysera i Excel**.  
  
2.  I **ange hello namn eller roll toouse tooconnect toohello användarmodellen**väljer **rollen**, och välj sedan i den nedrullningsbara listrutan för hello **försäljningschef**, och klicka sedan på  **OK**.  
  
    Excel öppnas med en ny arbetsbok. En pivottabell skapas automatiskt. hello fältlistan för pivottabell tabellen innehåller alla hello data tillgängliga fält i din nya modell.  
      
3.  Stäng Excel utan att spara hello arbetsboken.  
  
## <a name="whats-next"></a>Nästa steg
Gå toohello kursen: [lektionen 13: distribuera](../tutorials/aas-lesson-13-deploy.md).

  
  
  
