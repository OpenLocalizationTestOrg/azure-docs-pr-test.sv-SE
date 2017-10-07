---
Rubrik: aaa ”Azure Analysis Services självstudiekursen lektionen 11: skapa roller | Microsoft Docs ”beskrivning: Beskriver hur toocreate roller i hello självstudiekursen Azure Analysis Services-projekt. tjänster: analysis services dokumentationcenter: '' författare: minewiskan manager: erikre editor: '' taggar: ''

MS.AssetID: ms.service: analysis services ms.devlang: NA ms.topic: get-started-article ms.tgt_pltfrm: NA ms.workload: na ms.date: 2017-05/26 ms.author: owend
---
# <a name="lesson-11-create-roles"></a>Lektion 11: Skapa roller

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

Under den här lektionen skapar du roller. Roller skyddar modellen databasen objekt och data genom att begränsa åtkomst tooonly användare som är medlemmar i rollen. Varje roll definieras med en enskild behörighet: Ingen, Läsa, Läsa och bearbeta, Bearbeta eller Administratör. Roller kan definieras vid modellredigering med hjälp av rollhanteraren. När en modell har distribuerats kan du hantera roller med SQL Server Management Studio (SSMS). Det finns fler toolearn [roller](https://docs.microsoft.com/sql/analysis-services/tabular-models/roles-ssas-tabular).
  
> [!NOTE]  
> Skapa roller är inte nödvändigt toocomplete den här kursen. Som standard har hello konto du är inloggad med administratörsbehörighet på hello modellen. För andra användare i din organisation toobrowse med hjälp av en reporting klient måste du dock skapa minst en roll med Läs behörigheter och lägga till de användarna som medlemmar.  
  
Du kan skapa tre roller:  
  
-   **Försäljningschef** – den här rollen kan inkludera användare i organisationen som du vill toohave Läs behörighet tooall modellobjekt och data.  
  
-   **Försäljning analytiker USA** – den här rollen kan inkludera användare i organisationen som du vill endast toobe kan toobrowse data relaterade toosales i hello USA. För den här rollen kan du använda en DAX-formeln toodefine en *radfilter*, vilket begränsar medlemmar toobrowse endast data för hello USA.  
  
-   **Administratören** – den här rollen kan innehålla användare som du vill toohave administratörsbehörigheter, vilket gör obegränsad åtkomst och behörighet tooperform administrativa uppgifter på hello modelldatabasen.  
  
Du kan lägga till konton från din organisation särskilda toomembers eftersom Windows-användarkonton och gruppkonton i din organisation är unika. Men för den här självstudiekursen kommer kan du även lämna hello medlemmar tomt. Du testar hello effekten av varje roll senare i lektionen 12: Analysera i Excel.  
  
Uppskattad tid toocomplete lektionen: **15 minuter**  
  
## <a name="prerequisites"></a>Krav  
Det här avsnittet ingår i självstudiekursen för tabellmodellering som bör slutföras i rätt ordning. Innan du utför hello uppgifter i den här lektionen bör du slutfört hello föregående lektionen: [lektionen 10: skapa partitioner](../tutorials/aas-lesson-10-create-partitions.md).  
  
## <a name="create-roles"></a>Skapa roller  
  
#### <a name="toocreate-a-sales-manager-user-role"></a>toocreate en försäljningschef användarroll  
  
1.  Högerklicka på **Roller** > **Roller** i tabellmodellutforskaren.  
  
2.  Klicka på **Ny** i rollhanteraren.  
  
3.  Klicka på ny hello-roll, och klicka sedan på hello **namn** kolumn, byta namn på hello rollen för**försäljningschef**.  
  
4.  I hello **behörigheter** kolumnen, klickar du på hello listrutan och välj sedan hello **Läs** behörighet. 

    ![aas-lesson11-new-role](../tutorials/media/aas-lesson11-new-role.png) 
  
5.  Valfritt: Klicka på hello **medlemmar** fliken och klicka sedan på **Lägg till**. I hello **Välj användare eller grupper** dialogrutan Ange hello Windows-användare eller grupper du vill tooinclude i hello rollen från din organisation.  
  
#### <a name="toocreate-a-sales-analyst-us-user-role"></a>toocreate en användarroll för försäljning analytiker USA  
  
1.  Klicka på **Ny** i rollhanteraren.    
  
2.  Byta namn på hello rollen för**försäljning analytiker USA**.  
  
3.  Ge rollen **läsbehörighet**.  
  
4.  Fliken hello radfilter, och sedan efter Hej **DimGeography** i hello DAX-Filter kolumn, tabell typen hello följande formel:  
  
    ```Administrator
    =DimGeography[CountryRegionCode] = "US" 
    ```
    
    Ett radfilter formeln måste matcha värdet för tooa boolesk (SANT/FALSKT). Med den här formeln anger du att endast rader med hello landskoden värdet ”USA” är synlig toohello användare.  
    ![aas-lesson11-role-filter](../tutorials/media/aas-lesson11-role-filter.png) 
  
6.  Valfritt: Klicka på hello **medlemmar** fliken och klicka sedan på **Lägg till**. I hello **Välj användare eller grupper** dialogrutan Ange hello Windows-användare eller grupper du vill tooinclude i hello rollen från din organisation.  
  
#### <a name="toocreate-an-administrator-user-role"></a>toocreate administratörsanvändarrollen  
  
1.  Klicka på **Ny**.  
  
2.  Byta namn på hello rollen för**administratör**.  
  
3.  Ge den här rollen **administratörsbehörighet**.  
  
4.  Valfritt: Klicka på hello **medlemmar** fliken och klicka sedan på **Lägg till**. I hello **Välj användare eller grupper** dialogrutan Ange hello Windows-användare eller grupper du vill tooinclude i hello rollen från din organisation. 
  
  
## <a name="whats-next"></a>Nästa steg
[Lektion 12: Analysera i Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).

  
  
