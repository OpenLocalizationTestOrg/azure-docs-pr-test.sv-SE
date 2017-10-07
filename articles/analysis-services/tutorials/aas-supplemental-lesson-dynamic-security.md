---
Rubrik: aaa ”Azure Analysis Services självstudiekursen kompletterande lektionen: dynamiska säkerhet | Microsoft Docs ”beskrivning: Beskriver hur toouse dynamiska säkerhet med hjälp av raden filter i hello Azure Analysis Services-kursen.
tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''

MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 2017-05/26 ms.author: owend
---
# <a name="supplemental-lesson---dynamic-security"></a>Kompletterande lektion – Dynamisk säkerhet

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

I den här kompletterande lektionen skapar du en ytterligare roll som implementerar dynamisk säkerhet. Dynamisk säkerhet skyddar radnivå baserat på hello användar namn eller inloggnings-id för hello inloggade användaren. 
  
tooimplement dynamiska säkerhet kan du lägga en tabell tooyour modell som innehåller hello användarnamnen för de användare som kan ansluta toohello modell och bläddra modellobjekt och data. hello-modell som du skapar med den här kursen är i hello kontexten för Adventure Works; dock toocomplete detta lektion, måste du lägga till en tabell som innehåller användare från din egen domän. Du behöver inte hello lösenord för hello användarnamn som har lagts till. toocreate en EmployeeSecurity-tabell med ett litet antal användare från din egen domän du använda hello klistra in funktionen Klistra in data från ett Excel-kalkylblad. I ett verkligt scenario skulle hello tabell som innehåller användarnamn vanligtvis vara en tabell från en faktiska databasen som en datakälla. till exempel en verklig DimEmployee tabell.  
  
tooimplement dynamiska säkerhet kan du använda två DAX-funktioner: [användarnamn funktionen (DAX)](http://msdn.microsoft.com/22dddc4b-1648-4c89-8c93-f1151162b93f) och [LOOKUPVALUE funktionen (DAX)](http://msdn.microsoft.com/73a51c4d-131c-4c33-a139-b1342d10caab). Dessa funktioner, som tillämpas i en radfilterformel, definieras i en ny roll. Med funktionen hello LOOKUPVALUE anger hello formeln ett värde från hello EmployeeSecurity tabell. hello formeln skickar sedan att värdet toohello användarnamn funktion, vilket anger hello hello användares användarnamn inloggad tillhör toothis roll. hello-användare kan sedan bläddra endast de data som anges av hello rollen radfilter. I det här scenariot kan ange du att försäljning anställda endast kan bläddra Internet försäljningsdata för hello Försäljningsdistrikt som de är en medlem.  
  
De aktiviteter som är unikt toothis Adventure Works tabellmodell scenariot, men inte nödvändigtvis tillämpas tooa verkligt scenario identifieras som sådana. Varje aktivitet innehåller ytterligare information som beskriver hello syftet hello-aktivitet.  
  
Uppskattad tid toocomplete lektionen: **30 minuter**  
  
## <a name="prerequisites"></a>Krav  
Den här kompletterande lektionen ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning. Innan du utför hello uppgifter i den här kompletterande lektionen bör du har slutfört alla tidigare erfarenheter.  
  
## <a name="add-hello-dimsalesterritory-table-toohello-aw-internet-sales-tabular-model-project"></a>Lägg till hello DimSalesTerritory tabell toohello AW Internet försäljning modellen Tabellprojekt  
tooimplement dynamiska säkerhet för det här scenariot för Adventure Works, måste du lägga till två ytterligare tabeller tooyour modell. hello första tabellen som du lägger till är DimSalesTerritory (som Försäljningsdistrikt) från hello samma AdventureWorksDW-databasen. Du senare använda en rad filter toohello SalesTerritory tabell som definierar hello viss data hello inloggade användare kan bläddra.  
  
#### <a name="tooadd-hello-dimsalesterritory-table"></a>tooadd hello DimSalesTerritory tabell  
  
1.  I tabellmodellutforskaren > **Datakällor** högerklickar du på din anslutning och sedan på **Importera nya tabeller**.  

    Hello autentiseringsuppgifter för personifiering dialogrutan visas skriver du hello personifiering autentiseringsuppgifter som du använde i lektionen 2: Lägg till Data.
  
2.  Välj hello i Navigator **DimSalesTerritory** tabell och klicka sedan på **OK**.    
  
3.  I frågeredigeraren klickar du på hello **DimSalesTerritory** fråga och ta sedan bort **SalesTerritoryAlternateKey** kolumn.  
  
7.  Klicka på **Importera**.  
  
    hello ny tabell läggs toohello modellen arbetsytan. Objekt och data från hello källtabellen DimSalesTerritory importeras sedan till Tabellmodell din AW Internet försäljning.  
  
9. När hello tabellen har importerats, klickar du på **Stäng**.  

## <a name="add-a-table-with-user-name-data"></a>Lägga till en tabell med användarnamndata  
Hej DimEmployee tabellen i hello AdventureWorksDW som exempeldatabas innehåller användare från hello AdventureWorks domän. Dessa användarnamn finns inte i din egen miljö. Du måste skapa en tabell i din modell som innehåller ett litet antal (minst tre) faktiska användare från din organisation. Du lägger sedan till användarna som medlemmar toohello ny roll. Du behöver inte hello lösenord för hello exempel användarnamn, men du behöver faktiska Windows-användarnamn från en egen domän.  
  
#### <a name="tooadd-an-employeesecurity-table"></a>tooadd en EmployeeSecurity tabell  
  
1.  Öppna Microsoft Excel och skapa ett kalkylblad.  
  
2.  Kopiera hello i den följande tabellen, inklusive hello rubrikraden och klistra in den i hello kalkylblad.  

    ```
      |EmployeeId|SalesTerritoryId|FirstName|LastName|LoginId|  
      |---------------|----------------------|--------------|-------------|------------|  
      |1|2|<user first name>|<user last name>|\<domain\username>|  
      |1|3|<user first name>|<user last name>|\<domain\username>|  
      |2|4|<user first name>|<user last name>|\<domain\username>|  
      |3|5|<user first name>|<user last name>|\<domain\username>|  
    ```

3.  Ersätt hello förnamn, efternamn och domän\användarnamn med hello namn och inloggnings-ID för tre användare i din organisation. Placera hello samma användare på hello två första raderna för EmployeeId 1, visar den här användaren tillhör toomore än en försäljning territorium. Lämna hello EmployeeId och SalesTerritoryId som de är.  
  
4.  Spara hello kalkylblad som **SampleEmployee**.  
  
5.  Hello kalkylblad, markerar du alla hello-celler med data om anställda, inklusive hello sidhuvud, och sedan högerklicka hello markerade data och klicka på **kopiera**.  
  
6.  Klicka på hello i SSDT, **redigera** -menyn och klicka sedan på **klistra in**.  
  
    Om klistra in är nedtonad klickar du på någon kolumn i en tabell i hello modellen designer-fönstret och försök igen.  
  
7.  I hello **klistra in Preview** i dialogrutan **tabellnamn**, typen **EmployeeSecurity**.  
  
8.  I **Data toobe klistras in**, kontrollera hello data omfattar alla hello användardata och rubriker från hello SampleEmployee kalkylblad.  
  
9. Kontrollera att **Låt första raden utgöra kolumnrubriker** är markerat och klicka sedan på **Ok**.  
  
    En ny tabell med namnet EmployeeSecurity med medarbetardata som kopieras från hello SampleEmployee kalkylblad skapas.  
  
## <a name="create-relationships-between-factinternetsales-dimgeography-and-dimsalesterritory-table"></a>Skapa relationer mellan tabellerna FactInternetSales, DimGeography och DimSalesTerritory  
Hej FactInternetSales och DimGeography DimSalesTerritory tabellen alla innehålla en gemensam kolumn SalesTerritoryId. Hej SalesTerritoryId kolumnen i hello DimSalesTerritory tabellen innehåller värden med ett annat Id för alla områden.  
  
#### <a name="toocreate-relationships-between-hello-factinternetsales-dimgeography-and-hello-dimsalesterritory-table"></a>toocreate relationer mellan hello FactInternetSales, DimGeography och hello DimSalesTerritory tabell  
  
1.  I diagramvyn i hello **DimGeography** tabell, klicka och håll på hello **SalesTerritoryId** kolumn, och sedan dra hello markören toohello **SalesTerritoryId** kolumn i hello **DimSalesTerritory** tabell och släpp.  
  
2.  I hello **FactInternetSales** tabell, klicka och håll på hello **SalesTerritoryId** kolumn, och sedan dra hello markören toohello **SalesTerritoryId** kolumn i hello  **DimSalesTerritory** tabell och släpp.  
  
    Meddelande hello Active-egenskapen för den här relationen är False, vilket innebär att det är inaktivt. hello har redan tabellen FactInternetSales en annan aktiv relation.  
  
## <a name="hide-hello-employeesecurity-table-from-client-applications"></a>Dölj hello EmployeeSecurity tabell från klientprogram  
I det här steget dölja du hello EmployeeSecurity tabell, som visas i ett klientprogram fältlistan. Tänk på att du inte skyddar tabellen genom att dölja den. Användare kan fortfarande fråga på EmployeeSecurity-tabelldata om de vet hur man gör. toosecure Hej EmployeeSecurity tabelldata hindrar användare från att kunna tooquery datainnehåll, du använda ett filter för en aktivitet med senare.  
  
#### <a name="toohide-hello-employeesecurity-table-from-client-applications"></a>toohide hello EmployeeSecurity tabell från klientprogram  
  
-   Högerklicka i hello modellen designer i diagramvyn hello **medarbetare** tabell rubrik och klicka sedan på **dölja från klientverktyg**.  
  
## <a name="create-a-sales-employees-by-territory-user-role"></a>Skapa användarrollen Säljpersonal per område  
I den här aktiviteten skapar du en användarroll. Den här rollen omfattar ett radfilter som definierar vilka rader i hello DimSalesTerritory tabell är synliga toousers. hello filter tillämpas sedan i hello en-till-många-relation riktning tooall andra tabeller relaterade tooDimSalesTerritory. Du kan också använda ett filter som skyddar hello hela EmployeeSecurity tabellen från att frågbar av alla användare som är medlem i rollen hello.  
  
> [!NOTE]  
> hello försäljning anställda av territorium roll som du skapar i den här lektionen begränsar medlemmar toobrowse (eller fråga) endast försäljningsdata för hello försäljning territorium toowhich de tillhör. Om du lägger till en användare som en medlem toohello försäljning anställda av territorium roll som också finns en medlem i en roll som skapas i [lektionen 11: skapa roller](../tutorials/aas-lesson-11-create-roles.md), får du en kombination av behörigheter. När en användare är medlem i flera roller, hello behörigheter och radfilter som definierats för varje roll är kumulativa. Det vill säga har hello användare hello större behörigheter som anges av hello kombination av roller.  
  
#### <a name="toocreate-a-sales-employees-by-territory-user-role"></a>toocreate en försäljning anställda av territorium användarroll  
  
1.  Klicka på hello i SSDT, **modellen** -menyn och klicka sedan på **roller**.  
  
2.  Klicka på **Ny** i **rollhanteraren**.  
  
    En ny roll med hello ingen behörighet har lagts till toohello lista.  
  
3.  Klicka på hello ny roll, och klicka sedan på hello **namn** kolumn, byta namn på hello rollen för**anställda försäljning per område**.  
  
4.  I hello **behörigheter** kolumnen, klickar du på hello listrutan och välj sedan hello **Läs** behörighet.  
  
5.  Klicka på hello **medlemmar** fliken och klicka sedan på **Lägg till**.  
  
6.  I hello **Välj användare eller grupp** i dialogrutan **RETUR hello-objekt med namnet tooselect**, Skriv hello första exemplet användarnamn du använde när du skapar hello EmployeeSecurity tabell. Klicka på **Kontrollera namn** tooverify hello användarnamn är giltig och klicka sedan på **Ok**.  
  
    Upprepa det här steget lägger till hello andra Exempelnamn för användare som du använde när du skapar hello EmployeeSecurity tabell.  
  
7.  Klicka på hello **radfilter** fliken.  
  
8.  För hello **EmployeeSecurity** tabell i hello **DAX-Filter** kolumn, typen hello följande formel:  
  
    ```
      =FALSE()  
    ```
  
    Den här formeln anger att alla kolumner matcha toohello FALSKT booleskt villkor. Inga kolumner har valts för hello EmployeeSecurity tabellen kan efterfrågas av en medlem i hello försäljning anställda av territorium användarroll.  
  
9. För hello **DimSalesTerritory** tabell, typen hello följande formel:  

    ```  
    ='Sales Territory'[Sales Territory Id]=LOOKUPVALUE('Employee Security'[Sales Territory Id], 
      'Employee Security'[Login Id], USERNAME(), 
      'Employee Security'[Sales Territory Id], 
      'Sales Territory'[Sales Territory Id]) 
    ```
  
    I den här formeln returnerar hello funktionen LOOKUPVALUE alla värden för hello DimEmployeeSecurity [SalesTerritoryId]-kolumnen, där hello EmployeeSecurity [LoginId] är hello samma som hello aktuella inloggade Windows-användarnamn och EmployeeSecurity [ SalesTerritoryId] är hello samma som hello DimSalesTerritory [SalesTerritoryId].  
  
    hello är försäljning territorium-ID: N som returneras av LOOKUPVALUE och sedan använda toorestrict hello rader som visas i hello DimSalesTerritory tabell. Endast rader där hello SalesTerritoryID för hello rad är i hello uppsättning ID som returnerades av funktionen för hello LOOKUPVALUE visas.  
  
10. Klicka på **Ok** i rollhanteraren.  
  
## <a name="test-hello-sales-employees-by-territory-user-role"></a>Testa hello försäljning anställda av territorium användarroll  
I det här steget använder du hello analysera i Excel-funktionen i SSDT tootest hello effekt hello försäljning anställda av territorium användarroll. Du anger ett av hello användarnamn du har lagt till toohello EmployeeSecurity tabellen och som en medlem i rollen hello. Det här användarnamnet används sedan som hello gällande namnet i hello anslutningen mellan Excel och hello modellen.  
  
#### <a name="tootest-hello-sales-employees-by-territory-user-role"></a>tootest hello försäljning anställda av territorium användarroll  
  
1.  Klicka på hello i SSDT, **modellen** -menyn och klicka sedan på **analysera i Excel**.  
  
2.  I hello **analysera i Excel** i dialogrutan **ange hello namn eller roll toouse tooconnect toohello användarmodellen**väljer **andra Windows-användare**, och klicka sedan på **Bläddra**.  
  
3.  I hello **Välj användare eller grupp** i dialogrutan **ange hello objektet namnet tooselect**, ange ett användarnamn som du har inkluderat i hello EmployeeSecurity tabell och klicka sedan på **Kontrollera namn**.  
  
4.  Klicka på **Ok** tooclose hello **Välj användare eller grupp** dialogrutan och klicka sedan på **Ok** tooclose hello **analysera i Excel** dialogrutan.  
  
    Excel öppnas med en ny arbetsbok. En pivottabell skapas automatiskt. hello PivotTable-Fields listan innehåller de flesta av hello datafält som är tillgängliga i din nya modell.  
  
    Meddelande hello EmployeeSecurity tabellen visas inte i hello PivotTable-Fields lista. Du valde att dölja den här tabellen för klientverktyg i en föregående aktivitet.  
  
5.  I hello **fält** under **∑ Internet försäljning** (mått), Välj hello **InternetTotalSales** mått. hello mått har angetts i hello **värden** fält.  
  
6.  Välj hello **SalesTerritoryId** kolumn från hello **DimSalesTerritory** tabell. hello kolumn har angetts i hello **radetiketter** fält.  
  
    Meddelande Internet försäljningssiffrorna visas bara för hello en region toowhich hello gällande användarnamnet du använde tillhör. Om du väljer en annan kolumn, visas som stad från hello DimGeography tabellen som rad Etikettfält, endast städer i hello försäljning territorium toowhich hello effektiva användaren tillhör.  
  
    Den här användaren kan inte bläddra eller fråga alla Internet-försäljningsdata för områden än hello en de tillhör. Den här begränsningen beror på att hello radfilter som definierats för hello DimSalesTerritory tabell i hello försäljning anställda av territorium användarrollen skyddar data för alla data som rör tooother Försäljningsdistrikt.  
  
## <a name="see-also"></a>Se även  
[Funktionen USERNAME (DAX)](https://msdn.microsoft.com/library/hh230954.aspx)  
[Funktionen LOOKUPVALUE (DAX)](https://msdn.microsoft.com/library/gg492170.aspx)  
[Funktionen CUSTOMDATA (DAX)](https://msdn.microsoft.com/library/hh213140.aspx)  