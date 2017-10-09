
1. <span data-ttu-id="8ea63-101">Klicka på hello **Apptjänster** , Välj din Mobile Apps-serverdel, Välj **Quickstart**, och välj sedan din klientplattform (iOS, Android, Xamarin, Cordova).</span><span class="sxs-lookup"><span data-stu-id="8ea63-101">Click hello **App Services** button, select your Mobile Apps back end, select **Quickstart**, and then select your client platform (iOS, Android, Xamarin, Cordova).</span></span>

    ![Azure Portal med snabbstarten för Mobile Apps markerad][quickstart]

2. <span data-ttu-id="8ea63-103">Om en databasanslutning inte konfigureras, skapar du en hello följande:</span><span class="sxs-lookup"><span data-stu-id="8ea63-103">If a database connection is not configured, create one by doing hello following:</span></span>

    ![Azure-portalen med Mobile Apps Connect toodatabase][connect]

    <span data-ttu-id="8ea63-105">a.</span><span class="sxs-lookup"><span data-stu-id="8ea63-105">a.</span></span> <span data-ttu-id="8ea63-106">Skapa en ny SQL-databas och server.</span><span class="sxs-lookup"><span data-stu-id="8ea63-106">Create a new SQL database and server.</span></span>

    ![Azure Portal med Mobile Apps skapa ny databas och server][server]

    <span data-ttu-id="8ea63-108">b.</span><span class="sxs-lookup"><span data-stu-id="8ea63-108">b.</span></span> <span data-ttu-id="8ea63-109">Vänta tills hello dataanslutningen har skapats.</span><span class="sxs-lookup"><span data-stu-id="8ea63-109">Wait until hello data connection is successfully created.</span></span>

    ![Azure Portal-meddelande om att dataanslutningen har skapats][notification]

    <span data-ttu-id="8ea63-111">c.</span><span class="sxs-lookup"><span data-stu-id="8ea63-111">c.</span></span> <span data-ttu-id="8ea63-112">Dataanslutningen måste lyckas.</span><span class="sxs-lookup"><span data-stu-id="8ea63-112">Data connection must be successful.</span></span>

    ![Azure Portal-meddelande, "Du har redan en dataanslutning"][already-connection]

3. <span data-ttu-id="8ea63-114">Under **2. Skapa ett tabell-API**, Välj Node.js för **Språk för serverdel**.</span><span class="sxs-lookup"><span data-stu-id="8ea63-114">Under **2. Create a table API**, select Node.js for **Backend language**.</span></span> 
 
4. <span data-ttu-id="8ea63-115">Acceptera hello bekräftelse och välj sedan **skapa TodoItem-tabell**.</span><span class="sxs-lookup"><span data-stu-id="8ea63-115">Accept hello acknowledgment, and then select **Create TodoItem table**.</span></span>  
    <span data-ttu-id="8ea63-116">En ny att göra-post-tabell skapas i din databas.</span><span class="sxs-lookup"><span data-stu-id="8ea63-116">This action creates a new to-do item table in your database.</span></span> 

    >[!IMPORTANT]
    > <span data-ttu-id="8ea63-117">Ändrar en befintlig serverdel tooNode.js skriver över allt innehåll.</span><span class="sxs-lookup"><span data-stu-id="8ea63-117">Switching an existing back end tooNode.js overwrites all contents.</span></span> <span data-ttu-id="8ea63-118">en .NET-serverdel Läs toocreate [arbeta med hello .NET backend-server-SDK för Mobile Apps][instructions].</span><span class="sxs-lookup"><span data-stu-id="8ea63-118">toocreate a .NET back end instead, see [Work with hello .NET back-end server SDK for Mobile Apps][instructions].</span></span>

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
