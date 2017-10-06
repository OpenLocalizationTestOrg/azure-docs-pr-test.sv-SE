---
Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 1: skapa ett nytt projekt tabellmodell | Microsoft Docs ”beskrivning: Beskriver hur toocreate en ny Azure Analysis Services-självstudiekurs projektet. tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''

MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend
---
# <a name="lesson-1-create-a-tabular-model-project"></a>Lektion 1: Skapa ett projekt för tabellmodeller

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Nu bör använder du SQL Server Data Tools (SSDT) toocreate ett nytt projekt tabellmodell på hello 1400 kompatibilitetsnivå. När det nya projektet har skapats kan du börja lägga till data och redigera din modell. Den här lektionen ger dig också en kort introduktion toohello tabellmodell redigeringsmiljön i SSDT.  
  
Uppskattad tid toocomplete lektionen: **10 minuter**  
  
## <a name="prerequisites"></a>Krav  
Det här avsnittet är hello första lektionen i en tabellmodell redigering av kursen. toocomplete detta lektion, det finns flera krav som du behöver toohave på plats. Det finns fler toolearn [Azure Analysis Services - Adventure Works kursen](../tutorials/aas-adventure-works-tutorial.md).  
  
## <a name="create-a-new-tabular-model-project"></a>Skapa ett nytt projekt för tabellmodeller  
  
#### <a name="toocreate-a-new-tabular-model-project"></a>toocreate ett nytt tabellmodell-projekt  
  
1.  I SSDT på hello **filen** -menyn klickar du på **ny** > **projekt**.  
  
2.  I hello **nytt projekt** dialogrutan Expandera **installerad** > **Business Intelligence** > **Analysis Services**, och klicka sedan på **Analysis Services-Tabellprojekt**.  
  
3.  I **namn**, typen **AW Internet försäljning**, och sedan ange en plats för hello projektfiler.  
  
    Som standard **lösningsnamn** hello är samma som hello projektnamn; men du kan ange en annan lösningsnamn.  
  
4.  Klicka på **OK**.  
  
5.  I hello **tabellmodell designer** dialogrutan **integrerade arbetsytan**.  
  
    hello arbetsytan värd för en tabellmodelldatabas med hello samma namn som hello projekt under redigering av modellen. Integrerad arbetsytan innebär SSDT använder en inbyggd instans eliminera hello måste tooinstall en separat server-instans för Analysis Services för redigering av modellen.
      
6.  I **Kompatibilitetsnivå** väljer du **SQL Server 2017 / Azure Analysis Services (1400)**.   
 
    ![aas-lesson1-tmd](../tutorials/media/aas-lesson1-tmd.png)
      
    Om du inte ser SQL Server 2017 / Azure Analysis Services (1400) i hello kompatibilitet nivån listbox, använder inte hello senaste versionen av SQL Server Data Tools. tooget hello senaste versionen, se [installera SQL Server Data tools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).  
      
  
## <a name="understanding-hello-ssdt-tabular-model-authoring-environment"></a>Förstå hello SSDT tabellmodell redigeringsmiljön  
Nu när du har skapat ett nytt projekt tabellmodell kan ta en stund tooexplore hello tabellmodell redigeringsmiljön i SSDT.  
  
När projektet har skapats öppnas det i SSDT. På vänster sida i hello **Tabular modellen Explorer**, visas en trädvy över hello objekt i modellen. Eftersom du ännu inte har importerat data, är hello mappar tom. Du kan högerklicka på ett objekt mappen tooperform åtgärder, liknande toohello menyraden. Eftersom du gå igenom den här kursen kan du använda hello Tabular modellen Explorer toonavigate olika objekt i projektet modellen.

![aas-lesson1-tme](../tutorials/media/aas-lesson1-tme.png)

Klicka på hello **Solution Explorer** fliken. Här kan du se filen **Model.bim**. Om du inte ser hello designer-fönstret toohello vänster (hello tomt fönster med hello Model.bim fliken), i **Solution Explorer**under **AW Internet försäljnings projekt**, dubbelklicka på hello  **Model.bim** fil. Hej Model.bim filen innehåller hello metadata för ditt modellprojekt. 

![aas-lesson1-se](../tutorials/media/aas-lesson1-se.png)
  
Klicka på **Model.bim**. I hello **egenskaper** visas hello modellegenskaper, de viktigaste som är hello **DirectQuery-läge** egenskapen. Denna egenskap anger om hello modellen har distribuerats i InMemory-läge (av) eller DirectQuery-läge (på). I den här självstudien skapar och distribuerar du din modell i InMemory-läge.

![aas-lesson1-properties](../tutorials/media/aas-lesson1-properties.png)
  
När du skapar en modellprojekt vissa modellegenskaper konfigureras automatiskt enligt toohello Data Modeling inställningar som kan anges i hello **verktyg** menyn > **alternativ** dialogrutan. Egenskaper för säkerhetskopiering, bevarande av arbetsytan och arbetsyteserver ange hur och var hello arbetsytan databas (din modell redigering databasen) har säkerhetskopierats, bevaras i minnet och inbyggda. Du kan ändra de här inställningarna senare om det behövs, men lämna dem som de är för tillfället.  

Högerklicka på **AW Internet Sales** (projektet) i **Solution Explorer** och sedan på **Egenskaper**. Hej **AW Internet försäljning egenskapssidor** dialogrutan visas. Du ställer in några av de här egenskaperna senare när du distribuerar modellen.  
  
När du installerade SSDT har flera nya menyalternativ lagts toohello Visual Studio-miljön. Klicka på hello **modellen** menyn. Från den här du importerar data, uppdatera data i arbetsytan, bläddra din modell i Excel, skapa perspektiv och roller, Välj hello modellen visa och ange beräkningsalternativ. Klicka på hello **tabell** menyn. Härifrån kan du skapa och hantera relationer, ange inställningar för datumtabell, skapa partitioner och redigera tabellegenskaper. Om du klickar på hello **kolumnen** -menyn kan du lägga till och ta bort kolumner i en tabell, Lås kolumner och ange sorteringsordning. SSDT lägger också till vissa knappar toohello-fältet. Mest användbara är hello Autosumma funktionen toocreate standard aggregering mått för en markerad kolumn. Andra knappar ger snabb åtkomst toofrequently använda funktioner och kommandon.  
  
Utforska några av hello dialogrutor och platser för olika funktioner specifika tooauthoring tabellmodeller. När vissa objekt inte är aktiv ännu, får du en bättre uppfattning om hello tabellmodell redigeringsmiljön.  
  

## <a name="whats-next"></a>Nästa steg
[Lektion 2: Hämta data](../tutorials/aas-lesson-2-get-data.md).

  
  
  
