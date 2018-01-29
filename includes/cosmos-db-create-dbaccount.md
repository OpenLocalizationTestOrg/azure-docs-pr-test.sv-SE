1. Ett nytt fönster i webbläsaren, logga in på [Azure-portalen](https://portal.azure.com/).
2. Klicka på **nya** > **databaser** > **Cosmos Azure DB**.
   
   ![Azure Portal-databasfönstret](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. I den **nytt konto** anger du inställningarna för det nya kontot i Azure Cosmos DB. 
 
    Inställning|Föreslaget värde|Beskrivning
    ---|---|---
    ID|*Ange ett unikt namn*|Ange ett unikt namn som identifierar det här kontot i Azure Cosmos DB. Eftersom*documents.azure.com* läggs till det ID du anger för att skapa din URI ska du använda ett unikt men identifierbart ID.<br><br>Ditt id får bara innehålla gemener, siffror och bindestreck och måste innehålla mellan 3 och 50 tecken.
    API|SQL|API: et avgör vilken typ av konto för att skapa. Azure Cosmos-DB innehåller fem-API: er som passar bäst för ditt program: SQL (dokumentdatabasen), Gremlin (graph-databas), MongoDB (dokumentdatabasen), Azure Table och Cassandra, varje som för närvarande kräver ett särskilt konto. <br><br>Välj **SQL** eftersom den här snabbstarten du skapar en dokumentdatabasen frågbar med hjälp av SQL-syntax och är tillgänglig med SQL-API.<br><br>[Mer information om SQL-API](../articles/cosmos-db/documentdb-introduction.md)|
    Prenumeration|*Din prenumeration*|Välj Azure-prenumeration som du vill använda för det här kontot i Azure Cosmos DB. 
    Resursgrupp|Skapa nytt<br><br>*Ange samma unika namn som anges ovan i ID: T*|Välj **Skapa nytt**, ange ett nytt namn resursgrupp för ditt konto. För enkelhetens skull kan du använda samma namn som för ditt ID. 
    Plats|*Välj regionen som är närmast dina användare*|Välj geografisk plats som värd för ditt Azure DB som Cosmos-konto. Använd den plats som är närmast dina användare att ge dem snabbast åtkomst till data.
    Aktivera geo-redundans| Lämna tomt | Detta skapar en replikerad version av databasen i en region för andra (parad). Lämna tomt.  
    Fäst vid instrumentpanelen | Välj | Markera den här kryssrutan så att ditt nya konto har lagts till på portalen instrumentpanelen för enkel åtkomst.

    Klicka sedan på **Skapa**.

    ![Det nya kontobladet för Azure Cosmos DB](./media/cosmos-db-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)

4. Skapa konto tar några minuter. När kontot skapas i portalen visas den **distribuerar Azure Cosmos DB** panelen till höger kan du behöva rulla åt höger på instrumentpanelen för att se panelen. Det finns också en förloppsindikator som visas längst upp på skärmen. Du kan titta på antingen området för pågår. 

    ![Meddelandefönstret i Azure-portalen](./media/cosmos-db-create-dbaccount/deploying-cosmos-db.png)

    När kontot har skapats kan den **Grattis! Azure DB som Cosmos-kontot skapades** visas. 

