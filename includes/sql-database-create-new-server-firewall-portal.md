
<!--
includes/sql-database-create-new-server-firewall-portal.md

Latest Freshness check:  2016-11-28 , rickbyh.

As of circa 2016-04-11, hello following topics might include this include:
articles/sql-database/sql-database-get-started.md
articles/sql-database/sql-database-configure-firewall-settings
articles/sql-data-warehouse-get-started-provision.md

-->
### <a name="create-a-server-level-firewall-rule-in-hello-azure-portal"></a><span data-ttu-id="3ada4-101">Skapa en brandväggsregel på servernivå i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="3ada4-101">Create a server-level firewall rule in hello Azure portal</span></span>

1. <span data-ttu-id="3ada4-102">Klicka på hello SQL server-bladet under inställningar, **brandväggen** tooopen hello brandväggen bladet för hello SQLServer.</span><span class="sxs-lookup"><span data-stu-id="3ada4-102">On hello SQL server blade, under Settings, click **Firewall** tooopen hello Firewall blade for hello SQL server.</span></span>

    <!-- ![sql server firewall](../articles/sql-database/media/sql-database-get-started/sql-server-firewall.png) -->

2. <span data-ttu-id="3ada4-103">Granska hello klientens IP-adress som visas och kontrollerar att det är din IP-adress på hello Internet med en webbläsare (fråga ”vad är IP-adress).</span><span class="sxs-lookup"><span data-stu-id="3ada4-103">Review hello client IP address displayed and validate that this is your IP address on hello Internet using a browser of your choice (ask "what is my IP address).</span></span> <span data-ttu-id="3ada4-104">Ibland överensstämmer de inte av olika anledningar.</span><span class="sxs-lookup"><span data-stu-id="3ada4-104">Occasionally they do not match for a various reasons.</span></span>

    <!-- ![your IP address](../articles/sql-database/media/sql-database-get-started/your-ip-address.png) -->

3. <span data-ttu-id="3ada4-105">Förutsatt att hello IP-adresser matchar, klickar du på **lägga till klientens IP-Adressen** hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="3ada4-105">Assuming that hello IP addresses match, click **Add client IP** on hello toolbar.</span></span>

    ![Lägg till klient-IP](../articles/sql-data-warehouse/media/sql-data-warehouse-get-started-provision/add-client-ip.png)

    > [!NOTE]
    > <span data-ttu-id="3ada4-107">Du kan öppna hello SQL Database-brandväggen på hello server tooa IP-adress eller ett hela adressintervallet.</span><span class="sxs-lookup"><span data-stu-id="3ada4-107">You can open hello SQL Database firewall on hello server tooa single IP address or an entire range of addresses.</span></span> <span data-ttu-id="3ada4-108">Öppna hello brandväggen aktiverar SQL-administratörer och användare toologin tooany databasen på hello server toowhich som de har giltiga autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="3ada4-108">Opening hello firewall enables SQL administrators and users toologin tooany database on hello server toowhich they have valid credentials.</span></span>
    >

4. <span data-ttu-id="3ada4-109">Klicka på **spara** hello verktygsfältet toosave den här brandväggsregeln på servernivå och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3ada4-109">Click **Save** on hello toolbar toosave this server-level firewall rule and then click **OK**.</span></span>

    ![Lägg till klient-IP](../articles/sql-database/media/sql-database-get-started-portal/server-firewall-rule.png)

> [!Tip]
> <span data-ttu-id="3ada4-111">Om du vill gå en självstudiekurs rekommenderar vi [Självstudiekurs om SQL Database: Skapa en server, en brandväggsregel på servernivå, en exempeldatabas, en brandväggsregel på databasnivå och ansluta till SQL Server](../articles/sql-database/sql-database-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="3ada4-111">For a tutorial, see [SQL Database tutorial: Create a server, a server-level firewall rule, a sample database, a database-level firewall rule and connect with SQL Server](../articles/sql-database/sql-database-get-started.md).</span></span>    
>
