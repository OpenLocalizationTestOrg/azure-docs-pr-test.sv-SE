---
Rubrik: aaa ”Azure Analysis Services självstudiekursen kompletterande lektionen: ojämn hierarkier | Microsoft Docs ”beskrivning: Beskriver hur toofix ojämn hierarkier i hello Azure Analysis Services-kursen.
tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''

MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 2017-05/26 ms.author: owend
---
# <a name="supplemental-lesson---ragged-hierarchies"></a>Kompletterande lektion – Ojämna hierarkier

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

I den här kompletterande lektionen löser du ett vanligt problem vid pivotering i hierarkier som innehåller tomma värden (medlemmar) på olika nivåer. Det kan till exempel förekomma i en organisation där en chef på hög nivå har både avdelningschefer och personer som inte är chefer som direkt underställda. Eller i geografiska hierarkier som består av land-region-ort, där vissa orter saknar en överordnad delstat eller provins, till exempel Washington D.C. och Vatikanstaten. När en hierarki har tomt medlemmar, det ofta är underordnad toodifferent eller ojämn nivåer.

![aas-lesson-detail-ragged-hierarchies-table](../tutorials/media/aas-lesson-detail-ragged-hierarchies-table.png)

Tabellmodeller på hello 1400 kompatibilitetsnivå har ytterligare **dölja medlemmar** -egenskapen för hierarkier. Hej **standard** inställningen förutsätter att det finns inga tomma medlemmar på alla nivåer. Hej **dölja tomma medlemmar** undantar tomma medlemmar från hello hierarkin när de läggs tooa pivottabell eller rapporten.  
  
Uppskattad tid toocomplete lektionen: **20 minuter**  
  
## <a name="prerequisites"></a>Krav  
Den här kompletterande lektionen ingår i en självstudiekurs om tabellmodeller. Innan du utför hello uppgifter i den här kompletterande lektionen bör du slutfört alla tidigare erfarenheter eller har en slutförd Adventure Works Internet försäljning modellen exempelprojektet. 

Om du har skapat hello AW Internet försäljnings-projekt som en del av hello kursen, innehåller din modell ännu inte några data eller ojämna hierarkier. toocomplete kursen kompletterande du först ha toocreate Hej problemet genom att lägga till vissa ytterligare tabeller, skapa relationer, beräknade kolumner, ett mått och en ny organisationshierarki. Det tar cirka 15 minuter. Sedan kan du hämta toosolve i bara några minuter.  

## <a name="add-tables-and-objects"></a>Lägga till tabeller och objekt
  
### <a name="tooadd-new-tables-tooyour-model"></a>tooadd nya tabeller tooyour modellen
  
1.  I tabellmodellutforskaren expanderar du **Datakällor**. Sedan högerklickar du på din anslutning och väljer **Importera nya tabeller**.
  
2.  I navigatören väljer du **DimEmployee** och **FactResellerSales** och klickar sedan på **OK**.

3.  Klicka på **Importera** i frågeredigeraren

4.  Skapa följande hello [relationer](../tutorials/aas-lesson-4-create-relationships.md):

    | Tabell 1           | Kolumn       | Filterriktning   | Tabell 2     | Kolumn      | Active |
    |-------------------|--------------|--------------------|-------------|-------------|--------|
    | FactResellerSales | OrderDateKey | Standard            | DimDate     | Date        | Ja    |
    | FactResellerSales | DueDate      | Standard            | DimDate     | Date        | Nej     |
    | FactResellerSales | ShipDateKey  | Standard            | DimDate     | Date        | Nej     |
    | FactResellerSales | ProductKey   | Standard            | DimProduct  | ProductKey  | Ja    |
    | FactResellerSales | EmployeeKey  | tooBoth tabeller | DimEmployee | EmployeeKey | Ja    |

5. I hello **DimEmployee** tabell, skapar hello följande [beräknade kolumner](../tutorials/aas-lesson-5-create-calculated-columns.md): 

    **Sökväg** 
    ```
    =PATH([EmployeeKey],[ParentEmployeeKey])
    ```

    **FullName** 
    ```
    =[FirstName] & " " & [MiddleName] & " " & [LastName]
    ```

    **Level1** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,1)) 
    ```

    **Level2** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,2)) 
    ```

    **Level3** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,3)) 
    ```

    **Level4** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,4)) 
    ```

    **Level5** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,5)) 
    ```

6.  I hello **DimEmployee** tabell, skapa en [hierarkin](../tutorials/aas-lesson-9-create-hierarchies.md) med namnet **organisation**. Lägg till följande kolumner i ordning hello: **Level1**, **nivå 2**, **Level3**, **Level4**, **Level5**.

7.  I hello **FactResellerSales** tabell, skapar hello följande [mått](../tutorials/aas-lesson-6-create-measures.md):

    ```
    ResellerTotalSales:=SUM([SalesAmount])
    ```

8.  Använd [analysera i Excel](../tutorials/aas-lesson-12-analyze-in-excel.md) tooopen Excel och automatiskt skapa en pivottabell.

9.  I **PivotTable-Fields**, lägga till hello **organisation** hierarkin från hello **DimEmployee** tabell för**rader**, och hello **ResellerTotalSales** måttet från hello **FactResellerSales** tabell för**värden**.

    ![aas-lesson-detail-ragged-hierarchies-pivottable](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable.png)

    Som du ser i hello pivottabell visar rader som är ojämna hello-hierarkin. Det finns flera rader med tomma medlemmar.

## <a name="toofix-hello-ragged-hierarchy-by-setting-hello-hide-members-property"></a>toofix hello ojämn hierarki med egenskapen hello Dölj medlemmar

1.  I **tabellmodellutforskaren** expanderar du **Tabeller** > **DimEmployee** > **Hierarkier** > **Organisation**.

2.  I **Egenskaper** > **Dölj medlemmar** väljer du **Dölj tomma medlemmar**. 

    ![aas-lesson-detail-ragged-hierarchies-hidemembers](../tutorials/media/aas-lesson-detail-ragged-hierarchies-hidemembers.png)

3.  Uppdatera hello pivottabell i Excel. 

    ![aas-lesson-detail-ragged-hierarchies-pivottable-refresh](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable-refresh.png)

    Nu ser det betydligt bättre ut!

## <a name="see-also"></a>Se även   
[Lektion 9: Skapa hierarkier](../tutorials/aas-lesson-9-create-hierarchies.md)  
[Kompletterande lektion – Dynamisk säkerhet](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[Kompletterande lektion – Detaljrader](../tutorials/aas-supplemental-lesson-detail-rows.md)  