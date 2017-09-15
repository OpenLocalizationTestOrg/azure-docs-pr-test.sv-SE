<span data-ttu-id="9c8d5-101">För att komma igång med Service Bus-köer i Azure måste du först skapa ett namnområde med ett namn som är unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-101">To begin using Service Bus queues in Azure, you must first create a namespace with a name that is unique across Azure.</span></span> <span data-ttu-id="9c8d5-102">Ett namnområde innehåller en omfattningsbehållare för adressering av Service Bus-resurser i ditt program.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-102">A namespace provides a scoping container for addressing Service Bus resources within your application.</span></span>

<span data-ttu-id="9c8d5-103">Så här skapar du ett namnområde:</span><span class="sxs-lookup"><span data-stu-id="9c8d5-103">To create a namespace:</span></span>

1. <span data-ttu-id="9c8d5-104">Logga in på [Azure portal][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="9c8d5-104">Log on to the [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="9c8d5-105">I det vänstra navigationsfältet i portalen klickar du på **Nytt**, på **Enterprise Integration** och sedan på **Service Bus**.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-105">In the left navigation pane of the portal, click **New**, then click **Enterprise Integration**, and then click **Service Bus**.</span></span>
3. <span data-ttu-id="9c8d5-106">I dialogrutan **Skapa namnområde** anger du ett namn för namnområdet.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-106">In the **Create namespace** dialog, enter a namespace name.</span></span> <span data-ttu-id="9c8d5-107">Systemet kontrollerar omedelbart om namnet är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-107">The system immediately checks to see if the name is available.</span></span>
4. <span data-ttu-id="9c8d5-108">När du har kontrollerat att namnet för namnområdet är tillgängligt, väljer du prisnivå (Basic, Standard eller Premium).</span><span class="sxs-lookup"><span data-stu-id="9c8d5-108">After making sure the namespace name is available, choose the pricing tier (Basic, Standard, or Premium).</span></span>
5. <span data-ttu-id="9c8d5-109">I fältet **Prenumeration** väljer du en Azure-prenumeration för vilken du vill skapa namnområdet.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-109">In the **Subscription** field, choose an Azure subscription in which to create the namespace.</span></span>
6. <span data-ttu-id="9c8d5-110">I fältet **Resursgrupp** väljer du en befintlig resursgrupp där namnområdet ska finnas eller skapar en ny.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-110">In the **Resource group** field, choose an existing resource group in which the namespace will live, or create a new one.</span></span>      
7. <span data-ttu-id="9c8d5-111">I **Plats** väljer du land eller region där namnområdet ska finnas.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-111">In **Location**, choose the country or region in which your namespace should be hosted.</span></span>
   
    ![Skapa namnområde][create-namespace]
8. <span data-ttu-id="9c8d5-113">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-113">Click **Create**.</span></span> <span data-ttu-id="9c8d5-114">Systemet skapar namnområdet och aktiverar det.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-114">The system now creates your namespace and enables it.</span></span> <span data-ttu-id="9c8d5-115">Du kan behöva vänta några minuter medan systemet tilldelar resurser till ditt konto.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-115">You might have to wait several minutes as the system provisions resources for your account.</span></span>

### <a name="obtain-the-management-credentials"></a><span data-ttu-id="9c8d5-116">Hämta autentiseringsuppgifterna för hantering</span><span class="sxs-lookup"><span data-stu-id="9c8d5-116">Obtain the management credentials</span></span>

1. <span data-ttu-id="9c8d5-117">I listan över namnområden, klickar du på det nyligen skapade namnområdet.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-117">In the list of namespaces, click the newly created namespace name.</span></span>
2. <span data-ttu-id="9c8d5-118">På namnområdesbladet klickar du på **Principer för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-118">In the namespace blade, click **Shared access policies**.</span></span>
3. <span data-ttu-id="9c8d5-119">På bladet **Principer för delad åtkomst** klickar du på **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-119">In the **Shared access policies** blade, click **RootManageSharedAccessKey**.</span></span>
   
    ![connection-info][connection-info]
4. <span data-ttu-id="9c8d5-121">På bladet **Princip: RootManageSharedAccessKey** klickar du på kopieringsknappen bredvid **Anslutningssträng – Primärnyckel** för att kopiera anslutningssträngen till Urklipp för senare användning.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-121">In the **Policy: RootManageSharedAccessKey** blade, click the copy button next to **Connection string–primary key**, to copy the connection string to your clipboard for later use.</span></span> <span data-ttu-id="9c8d5-122">Klistra in det här värdet i Anteckningar eller på en tillfällig plats.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-122">Paste this value into Notepad or some other temporary location.</span></span>
   
    ![connection-string][connection-string]

5. <span data-ttu-id="9c8d5-124">Upprepa föregående steg, kopiera och klistra in värdet för **primärnyckeln** till en tillfällig plats för senare användning.</span><span class="sxs-lookup"><span data-stu-id="9c8d5-124">Repeat the previous step, copying and pasting the value of **Primary key** to a temporary location for later use.</span></span>

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
