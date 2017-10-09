<span data-ttu-id="dc453-101">toobegin med hjälp av Service Bus-köer i Azure, måste du först skapa ett namnområde med ett namn som är unik i Azure.</span><span class="sxs-lookup"><span data-stu-id="dc453-101">toobegin using Service Bus queues in Azure, you must first create a namespace with a name that is unique across Azure.</span></span> <span data-ttu-id="dc453-102">Ett namnområde innehåller en omfattningsbehållare för adressering av Service Bus-resurser i ditt program.</span><span class="sxs-lookup"><span data-stu-id="dc453-102">A namespace provides a scoping container for addressing Service Bus resources within your application.</span></span>

<span data-ttu-id="dc453-103">toocreate ett namnområde:</span><span class="sxs-lookup"><span data-stu-id="dc453-103">toocreate a namespace:</span></span>

1. <span data-ttu-id="dc453-104">Logga in toohello [Azure-portalen][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="dc453-104">Log on toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="dc453-105">Hello vänstra navigeringsfönstret hello-portalen, klicka på **ny**, klicka på **Enterprise Integration**, och klicka sedan på **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="dc453-105">In hello left navigation pane of hello portal, click **New**, then click **Enterprise Integration**, and then click **Service Bus**.</span></span>
3. <span data-ttu-id="dc453-106">I hello **skapa namnområdet** dialogrutan, ange ett namn för namnområdet.</span><span class="sxs-lookup"><span data-stu-id="dc453-106">In hello **Create namespace** dialog, enter a namespace name.</span></span> <span data-ttu-id="dc453-107">hello systemet kontrollerar omedelbart toosee om hello namn är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="dc453-107">hello system immediately checks toosee if hello name is available.</span></span>
4. <span data-ttu-id="dc453-108">När gör att hello namnområdesnamnet är tillgänglig, kan du välja hello prisnivån (Basic, Standard och Premium).</span><span class="sxs-lookup"><span data-stu-id="dc453-108">After making sure hello namespace name is available, choose hello pricing tier (Basic, Standard, or Premium).</span></span>
5. <span data-ttu-id="dc453-109">I hello **prenumeration** , Välj en Azure-prenumeration i vilka toocreate hello-namnområdet.</span><span class="sxs-lookup"><span data-stu-id="dc453-109">In hello **Subscription** field, choose an Azure subscription in which toocreate hello namespace.</span></span>
6. <span data-ttu-id="dc453-110">I hello **resursgruppen** , Välj en befintlig resursgrupp i vilka hello namnområdet ska live eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="dc453-110">In hello **Resource group** field, choose an existing resource group in which hello namespace will live, or create a new one.</span></span>      
7. <span data-ttu-id="dc453-111">I **plats**, Välj hello land eller region där namnområdet ska finnas.</span><span class="sxs-lookup"><span data-stu-id="dc453-111">In **Location**, choose hello country or region in which your namespace should be hosted.</span></span>
   
    ![Skapa namnområde][create-namespace]
8. <span data-ttu-id="dc453-113">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="dc453-113">Click **Create**.</span></span> <span data-ttu-id="dc453-114">hello systemet skapar namnområdet och aktiverar den.</span><span class="sxs-lookup"><span data-stu-id="dc453-114">hello system now creates your namespace and enables it.</span></span> <span data-ttu-id="dc453-115">Du kanske toowait några minuter medan hello systemet tilldelar resurser för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="dc453-115">You might have toowait several minutes as hello system provisions resources for your account.</span></span>

### <a name="obtain-hello-management-credentials"></a><span data-ttu-id="dc453-116">Hämta autentiseringsuppgifter för hantering av hello</span><span class="sxs-lookup"><span data-stu-id="dc453-116">Obtain hello management credentials</span></span>

1. <span data-ttu-id="dc453-117">Hello nyskapad namnområdesnamnet på hello listan namnområden.</span><span class="sxs-lookup"><span data-stu-id="dc453-117">In hello list of namespaces, click hello newly created namespace name.</span></span>
2. <span data-ttu-id="dc453-118">I hello namnområde bladet, klickar du på **principer för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="dc453-118">In hello namespace blade, click **Shared access policies**.</span></span>
3. <span data-ttu-id="dc453-119">I hello **principer för delad åtkomst** bladet, klickar du på **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="dc453-119">In hello **Shared access policies** blade, click **RootManageSharedAccessKey**.</span></span>
   
    ![connection-info][connection-info]
4. <span data-ttu-id="dc453-121">I hello **princip: RootManageSharedAccessKey** bladet Klicka hello kopiera bredvid för**sträng – primära anslutningsnyckel**, toocopy hello anslutning sträng tooyour Urklipp för senare användning.</span><span class="sxs-lookup"><span data-stu-id="dc453-121">In hello **Policy: RootManageSharedAccessKey** blade, click hello copy button next too**Connection string–primary key**, toocopy hello connection string tooyour clipboard for later use.</span></span> <span data-ttu-id="dc453-122">Klistra in det här värdet i Anteckningar eller på en tillfällig plats.</span><span class="sxs-lookup"><span data-stu-id="dc453-122">Paste this value into Notepad or some other temporary location.</span></span>
   
    ![connection-string][connection-string]

5. <span data-ttu-id="dc453-124">Hello Upprepa föregående steg, kopiera och klistra in hello värdet för **primärnyckel** tooa tillfällig plats för senare användning.</span><span class="sxs-lookup"><span data-stu-id="dc453-124">Repeat hello previous step, copying and pasting hello value of **Primary key** tooa temporary location for later use.</span></span>

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
