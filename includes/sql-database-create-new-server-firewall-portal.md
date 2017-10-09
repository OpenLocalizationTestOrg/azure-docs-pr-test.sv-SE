
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-11-28 , rickbyh.

As of circa 2016-04-11, hello following topics might include this include:
articles/sql-database/sql-database-get-started.md
articles/sql-database/sql-database-configure-firewall-settings
articles/sql-data-warehouse-get-started-provision.md

-->
### <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a>Skapa en brandväggsregel på servernivå i hello Azure-portalen

1. Klicka på hello SQL server-bladet under inställningar, **brandväggen** tooopen hello brandväggen bladet för hello SQLServer.

    <!-- ![sql server firewall](../articles/sql-database/media/sql-database-get-started/sql-server-firewall.png) -->

2. Granska hello klientens IP-adress som visas och kontrollerar att det är din IP-adress på hello Internet med en webbläsare (fråga ”vad är IP-adress). Ibland överensstämmer de inte av olika anledningar.

    <!-- ![your IP address](../articles/sql-database/media/sql-database-get-started/your-ip-address.png) -->

3. Förutsatt att hello IP-adresser matchar, klickar du på **lägga till klientens IP-Adressen** hello i verktygsfältet.

    ![Lägg till klient-IP](../articles/sql-data-warehouse/media/sql-data-warehouse-get-started-provision/add-client-ip.png)

    > [!NOTE]
    > Du kan öppna hello SQL Database-brandväggen på hello server tooa IP-adress eller ett hela adressintervallet. Öppna hello brandväggen aktiverar SQL-administratörer och användare toologin tooany databasen på hello server toowhich som de har giltiga autentiseringsuppgifter.
    >

4. Klicka på **spara** hello verktygsfältet toosave den här brandväggsregeln på servernivå och klicka sedan på **OK**.

    ![Lägg till klient-IP](../articles/sql-database/media/sql-database-get-started-portal/server-firewall-rule.png)

> [!Tip]
> Om du vill gå en självstudiekurs rekommenderar vi [Självstudiekurs om SQL Database: Skapa en server, en brandväggsregel på servernivå, en exempeldatabas, en brandväggsregel på databasnivå och ansluta till SQL Server](../articles/sql-database/sql-database-get-started.md).    
>
