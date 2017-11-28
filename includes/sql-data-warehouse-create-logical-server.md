### <a name="create-a-new-logical-sql-server-in-the-azure-portal"></a><span data-ttu-id="327e5-101">Skapa en ny logisk SQL-server på Azure Portal</span><span class="sxs-lookup"><span data-stu-id="327e5-101">Create a new logical SQL server in the Azure portal</span></span>

1. <span data-ttu-id="327e5-102">Klicka på **Nytt**, sök efter **logisk server** och tryck på **Retur**.</span><span class="sxs-lookup"><span data-stu-id="327e5-102">Click **New**, search **logical server**, and then hit **ENTER**.</span></span>

    ![sök efter logisk server](./media/sql-data-warehouse-create-logical-server/search-logical-server.png)
2. <span data-ttu-id="327e5-104">Välj **SQL-server (logisk server)**</span><span class="sxs-lookup"><span data-stu-id="327e5-104">Select **SQL server (logical server)**</span></span> 

    ![välj logisk server](./media/sql-data-warehouse-create-logical-server/select-logical-server.png)
  
3. <span data-ttu-id="327e5-106">Öppna bladet för den nya SQL-servern (logisk server) genom att klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="327e5-106">Click **Create** to open the new SQL Server (logical server) blade.</span></span>

   <span data-ttu-id="327e5-107"><kbd>![öppna logiska serverblad](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd> <kbd> ![logiska serverblad](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png)</kbd></span><span class="sxs-lookup"><span data-stu-id="327e5-107"><kbd> ![open logical server blade](./media/sql-data-warehouse-create-logical-server/open-logical-server-blade.png) </kbd> <kbd>![logical server blade](./media/sql-data-warehouse-create-logical-server/logical-server-blade.png) </kbd></span></span>
  
3. <span data-ttu-id="327e5-108">Ange ett giltigt namn för den nya logiska servern i rutan för servernamn på bladet SQL Server (logisk server).</span><span class="sxs-lookup"><span data-stu-id="327e5-108">In the SQL Server (logical server) blade's server name text box, provide a valid name for the new logical server.</span></span> <span data-ttu-id="327e5-109">En grön kryssmarkering visar att du har angett ett giltigt namn.</span><span class="sxs-lookup"><span data-stu-id="327e5-109">A green check mark indicates that you have provided a valid name.</span></span>
    
    ![Namn på ny server](./media/sql-data-warehouse-create-logical-server/new-name-logical-server.png)

    > [!IMPORTANT]
    > <span data-ttu-id="327e5-111">Det fullständigt kvalificerade namnet för den nya servern är <ditt_servernamn>. database.windows.net.</span><span class="sxs-lookup"><span data-stu-id="327e5-111">The fully qualified name for your new server will be <your_server_name>.database.windows.net.</span></span>
    >
    
4. <span data-ttu-id="327e5-112">I textrutan Inloggning för serveradministratör anger du ett användarnamn för SQL-autentiseringsinloggningen för den här servern.</span><span class="sxs-lookup"><span data-stu-id="327e5-112">In the Server admin login text box, provide a user name for the SQL authentication login for this server.</span></span> <span data-ttu-id="327e5-113">Den här inloggningen kallas den primära server-inloggningen.</span><span class="sxs-lookup"><span data-stu-id="327e5-113">This login is known as the server principal login.</span></span> <span data-ttu-id="327e5-114">En grön kryssmarkering visar att du har angett ett giltigt namn.</span><span class="sxs-lookup"><span data-stu-id="327e5-114">A green check mark indicates that you have provided a valid name.</span></span>
    
    ![SQL-administratörsinloggning](./media/sql-data-warehouse-create-logical-server/sql-admin-login.png)
5. <span data-ttu-id="327e5-116">I textrutorna **Lösenord** och **Bekräfta lösenord** anger du ett lösenord för inloggningskontot för serverhuvudobjektet.</span><span class="sxs-lookup"><span data-stu-id="327e5-116">In the **Password** and **Confirm password** text boxes, provide a password for the server principal login account.</span></span> <span data-ttu-id="327e5-117">En grön kryssmarkering visar att du har angett ett giltigt lösenord.</span><span class="sxs-lookup"><span data-stu-id="327e5-117">A green check mark indicates that you have provided a valid password.</span></span>
    
    ![SQL-administratörslösenord](./media/sql-data-warehouse-create-logical-server/sql-admin-password.png)
6. <span data-ttu-id="327e5-119">Välj en prenumeration som du har behörighet att skapa objekt i.</span><span class="sxs-lookup"><span data-stu-id="327e5-119">Select a subscription in which you have permission to create objects.</span></span>

    ![prenumeration](./media/sql-data-warehouse-create-logical-server/subscription.png)
7. <span data-ttu-id="327e5-121">I textrutan Resursgrupp väljer du **Skapa nytt** och anger sedan ett giltigt namn för den nya resursgruppen i textrutan Resursgrupp. (Du kan också använda en befintlig resursgrupp om du redan har skapat en.)</span><span class="sxs-lookup"><span data-stu-id="327e5-121">In the Resource group text box, select **Create new** and then, in the resource group text box, provide a valid name for the new resource group (you can also use an existing resource group if you have already created one for yourself).</span></span> <span data-ttu-id="327e5-122">En grön kryssmarkering visar att du har angett ett giltigt namn.</span><span class="sxs-lookup"><span data-stu-id="327e5-122">A green check mark indicates that you have provided a valid name.</span></span>

    ![Ny resursgrupp](./media/sql-data-warehouse-create-logical-server/new-resource-group.png)

8. <span data-ttu-id="327e5-124">I textrutan **Plats** väljer du ett lämpligt datacenter för din plats, t.ex. ”Australien, östra”.</span><span class="sxs-lookup"><span data-stu-id="327e5-124">In the **Location** text box, select a data center appropriate to your location - such as "Australia East".</span></span>
    
    ![Serverplats](./media/sql-data-warehouse-create-logical-server/server-location.png)
    
    > [!TIP]
    > <span data-ttu-id="327e5-126">Du kan inte ändra kryssrutan för **Ge Azure-tjänster åtkomst till servern** på det här bladet.</span><span class="sxs-lookup"><span data-stu-id="327e5-126">The checkbox for **Allow azure services to access server** cannot be changed on this blade.</span></span> <span data-ttu-id="327e5-127">Du kan ändra den här inställningen på bladet Serverbrandvägg.</span><span class="sxs-lookup"><span data-stu-id="327e5-127">You can change this setting on the server firewall blade.</span></span> <span data-ttu-id="327e5-128">Mer information finns i [Get started with security](../articles/sql-database/sql-database-manage-servers-portal.md) (Komma igång med säkerhet).</span><span class="sxs-lookup"><span data-stu-id="327e5-128">For more information, see [Get started with security](../articles/sql-database/sql-database-manage-servers-portal.md).</span></span>
    >
    
9. <span data-ttu-id="327e5-129">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="327e5-129">Click **Create**.</span></span>

    ![Knappen Skapa](./media/sql-data-warehouse-create-logical-server/create.png)

