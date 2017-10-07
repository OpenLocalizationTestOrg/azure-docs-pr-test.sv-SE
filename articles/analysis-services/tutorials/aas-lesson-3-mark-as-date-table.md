---
Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 3: Markera som Datumtabell | Microsoft Docs ”beskrivning: Beskriver hur toomark ett datum tabell i självstudiekursen hello Azure Analysis Services-projekt. tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''

MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 06/01/2017 ms.author: owend
---
# <a name="lesson-3-mark-as-date-table"></a>Lektion 3: Markera som datumtabell

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

I ”lektion 2: Hämta data” importerade du en dimensionstabell med namnet DimDate. I din modell heter den här tabellen DimDate, och den kan också kallas *datumtabell* eftersom den innehåller information om datum och tid.  
  
När du använder funktioner för DAX-tidsinformation, till exempel när du skapar mått senare, måste du ange egenskaper som innehåller en *datumtabell* och en unik *datumkolumn*-identifierare i tabellen.
  
Nu bör du markera hello DimDate tabellen som hello *datumtabell* och hello datumkolumnen (i hello datumtabell) som hello *datumkolumnen* (unik identifierare).  

Innan du markera hello tabell och datum datumkolumnen är ett bra tillfälle toodo lite housekeeping toomake din modell enklare toounderstand. I hello DimDate tabellen ser du en kolumn med namnet **FullDateAlternateKey**. Den här kolumnen innehåller en rad för varje dag i varje kalenderår som ingår i hello tabell. Du använder ofta den här kolumnen i måttformler och i rapporter. Men FullDateAlternateKey är inte så bra identifierare för den här kolumnen. Du byter namn på den för**datum**, vilket gör det enklare tooidentify och inkludera i formler. När det är möjligt, är det en bra idé toorename objekt som tabeller och kolumner toomake dem lättare tooidentify i SSDT och rapportering programmen Power BI och Excel-klienten. 
  
Uppskattad tid toocomplete lektionen: **tre minuter**  
  
## <a name="prerequisites"></a>Krav  
Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning. Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 2: hämta data](../tutorials/aas-lesson-2-get-data.md). 

### <a name="toorename-hello-fulldatealternatekey-column"></a>toorename hello FullDateAlternateKey kolumn

1.  Klicka på hello i hello modellen designer **DimDate** tabell.

2.  Dubbelklicka på hello-huvud för hello **FullDateAlternateKey** kolumn, och Byt sedan för**datum**.

  
### <a name="tooset-mark-as-date-table"></a>tooset Markera som Datumtabell  
  
1.  Välj hello **datum** kolumn, och klicka sedan på hello **egenskaper** fönstret under **datatyp**, kontrollera **datum** är markerad.  
  
2.  Klicka på hello **tabell** menyn Klicka **datum**, och klicka sedan på **Markera som Datumtabell**.  
  
3.  I hello **Markera som Datumtabell** i dialogrutan hello **datum** listbox, Välj hello **datum** enligt hello Unik identifierare. Den är vanligtvis markerad som standard. Klicka på **OK**. 

    ![aas-lesson3-date-table](../tutorials/media/aas-lesson3-date-table.png)
  

## <a name="whats-next"></a>Nästa steg
[Lektion 4: Skapa relationer](../tutorials/aas-lesson-4-create-relationships.md).
  
