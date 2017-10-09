## <a name="log-in-toohello-azure-portal"></a>Logga in toohello Azure-portalen

Logga in toohello [Azure-portalen](https://portal.azure.com/).

## <a name="create-a-blank-sql-database-using-hello-azure-portal"></a>Skapa en tom SQL-databas med hjälp av hello Azure-portalen

Azure SQL-databasen skapas med en definierad uppsättning [beräknings-och lagringsresurser](../articles/sql-database/sql-database-service-tiers.md). hello-databas skapas inom en [Azure-resursgrupp](../articles/azure-resource-manager/resource-group-overview.md) och i en [logisk Azure SQL Database-server](../articles/sql-database/sql-database-features.md). 

Följ dessa steg toocreate en tom SQL-databas. 

1. Klicka på hello **ny** knappen hittades på hello övre vänstra hörnet av hello Azure-portalen.

2. Välj **databaser** från hello **ny** och väljer **SQL-databas** från hello **databaser** sidan. 

   ![Skapa en tom databas](../articles/sql-database/media/sql-database-design-first-database/create-empty-database.png)

3. Fyll i formuläret om hello SQL-databas med hello följande information som visas i föregående bild hello:   

   | Inställning | Föreslaget värde | Beskrivning |
   | --------| --------------- | ----------- | 
   | **Databasnamn** | mySampleDatabase | För giltiga databasnamn, se [databasidentifierare](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers). | 
   | **Prenumeration** | Din prenumeration  | Mer information om dina prenumerationer finns i [Prenumerationer](https://account.windowsazure.com/Subscriptions). |
   | **Resursgrupp** | myResourceGroup | Giltiga resursgruppnamn finns i [Namngivningsregler och begränsningar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). |
   | **Välj källa** | Tom databas | Anger att en tom databas ska skapas. |
   ||||

4. Klicka på **Server** toocreate och konfigurera en ny server för den nya databasen. Fyll i hello **nytt serverformulär** med hello följande information: 

   | Inställning | Föreslaget värde | Beskrivning |
   | --------| --------------- | ----------- | 
   | **Servernamn** | Ett globalt unikt namn. | Giltiga servernamn finns i [Namngivningsregler och begränsningar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions). | 
   | **Inloggning för serveradministratör** | Ett giltigt namn. | För giltiga inloggningsnamn, se [Databasidentifierare](https://docs.microsoft.com/sql/relational-databases/databases/database-identifiers).|
   | **Lösenord** | Alla giltiga lösenord. | Lösenordet måste innehålla minst åtta tecken och måste innehålla tecken från tre av hello följande kategorier: versaler, gemener, siffror och specialtecken. |
   | **Plats** | Någon giltig plats. | För information om regioner, se [Azure-regioner](https://azure.microsoft.com/regions/). |
   ||||

   ![skapa databas-server](../articles/sql-database/media/sql-database-design-first-database/create-database-server.png)

5. Klicka på **Välj**.

6. Klicka på **prisnivå** toospecify hello tjänstnivå och prestandanivå servicenivå för den nya databasen. Den här kursen väljer **20 dtu: er** och **250** GB lagringsutrymme.

   ![skapa databas-s1](../articles/sql-database/media/sql-database-design-first-database/create-empty-database-pricing-tier.png)

7. Klicka på **Använd**.  

8. Välj en **sorteringen** för hello tom databas (för den här kursen används hello standardvärdet). Mer information om sorteringar finns [sorteringar](https://docs.microsoft.com/sql/t-sql/statements/collations)

9. Klicka på **skapa** tooprovision hello-databasen. Etablering tar om en minut och en halv toocomplete. 

10. På verktygsfältet hello **meddelanden** toomonitor hello distributionsprocessen.

   ![avisering](../articles/sql-database/media/sql-database-get-started-portal/notification.png)

## <a name="create-a-server-level-firewall-rule-using-hello-azure-portal"></a>Skapa en brandväggsregel på servernivå med hjälp av hello Azure-portalen

hello SQL Database-tjänsten skapar en brandvägg på servernivå för hello. Hello brandvägg förhindrar ursprungligen externa verktyg och program från att ansluta toohello server eller tooany databaser på hello-servern. Anslutningar tillåts efter en brandväggsregel skapas tooopen specifika IP-adresser. Följ dessa steg toocreate en [SQL-databas brandväggsregel på servernivå](../articles/sql-database/sql-database-firewall-configure.md) för din klients IP-adress och tooenable extern anslutning via hello SQL Database-Brandvägg för din IP-adress. 


> [!NOTE]
> Azure SQL Database kommunicerar via port 1433. Du kan ansluta tooSQL databasen förrän hello brandväggen i ditt nätverk tillåter att utgående trafik via port 1433.


1. När hello distributionen är klar klickar du på **SQL-databaser** från hello vänstra menyn och klicka sedan på **mySampleDatabase** på hello **SQL-databaser** sidan. hello översiktssidan för din databas öppnas som visar du hello fullständigt kvalificerade servernamnet (exempelvis **mynewserver20170313.database.windows.net**) och innehåller alternativ för ytterligare konfiguration. Kopiera det fullständiga servernamnet för senare användning.

   > [!IMPORTANT]
   > Du behöver den här fullständigt kvalificerade namnet tooconnect tooyour-server och dess databaser i efterföljande snabbstarter.
   > 

   ![servernamn](../articles/sql-database/media/sql-database-get-started-portal/server-name.png) 

2. Klicka på **ange serverbrandvägg** hello verktygsfältet enligt hello föregående bild. Hej **brandväggsinställningar** öppnas sidan för hello SQL Database-server. 

   ![brandväggsregler för server](../articles/sql-database/media/sql-database-get-started-portal/server-firewall-rule.png) 


3. Klicka på **lägga till klientens IP-Adressen** på hello verktygsfältet tooadd din aktuella IP-adressen tooa ny brandväggsregel. Med en brandväggsregel kan du öppna port 1433 för en enskild IP-adress eller för IP-adressintervall.

4. Klicka på **Spara**. En brandväggsregel på servernivå har skapats för din aktuella IP-adress som är öppna port 1433 på hello logisk server.

   ![ange brandväggsregel för server](../articles/sql-database/media/sql-database-get-started-portal/server-firewall-rule-set.png) 

4. Klicka på **OK** och stäng sedan hello **brandväggsinställningar** sidan.

Du kan ansluta toohello Azure SQL Database-server och dess databaser genom att använda ett verktyg som SQL Server Management Studio (SSMS). hello anslutningen är från den här IP-adressen och den använder hello server administratörskonto som skapats tidigare.


> [!IMPORTANT]
> Som standard är åtkomst via hello SQL Database-brandvägg aktiverad för alla Azure-tjänster. Klicka på **OFF** på den här sidan toodisable för alla Azure-tjänster.


## <a name="get-connection-string-values-using-hello-azure-portal"></a>Hämta anslutningssträngarnas värden med hjälp av hello Azure-portalen

Hämta hello fullständigt kvalificerade servernamnet för din Azure SQL Database-server i hello Azure-portalen. Du kan använda hello fullständiga server name tooconnect tooyour servern med hjälp av SQL Server Management Studio.

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).

2. Välj **SQL-databaser** vänstra hello-menyn och klicka på din databas på hello **SQL-databaser** sidan. 

3. I hello **Essentials** rutan i hello Azure Portalsida för databasen, leta upp och sedan kopiera hello **servernamn**.

   ![anslutningsinformation](../articles/sql-database/media/sql-database-get-started-portal/server-name.png) 
