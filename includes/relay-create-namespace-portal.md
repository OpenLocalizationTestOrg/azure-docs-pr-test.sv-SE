1. <span data-ttu-id="68080-101">Logga in på [Azure portal][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="68080-101">Log on to the [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="68080-102">I det vänstra navigationsfältet i portalen klickar du på **Nytt**, på **Enterprise Integration** och sedan på **Relay**.</span><span class="sxs-lookup"><span data-stu-id="68080-102">In the left navigation pane of the portal, click **New**, then click **Enterprise Integration**, and then click **Relay**.</span></span>
3. <span data-ttu-id="68080-103">I dialogrutan **Skapa namnområde** anger du ett namn för namnområdet.</span><span class="sxs-lookup"><span data-stu-id="68080-103">In the **Create namespace** dialog, enter a namespace name.</span></span> <span data-ttu-id="68080-104">Systemet kontrollerar omedelbart om namnet är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="68080-104">The system immediately checks to see if the name is available.</span></span>
4. <span data-ttu-id="68080-105">I fältet **Prenumeration** väljer du en Azure-prenumeration för vilken du vill skapa namnområdet.</span><span class="sxs-lookup"><span data-stu-id="68080-105">In the **Subscription** field, choose an Azure subscription in which to create the namespace.</span></span>
5. <span data-ttu-id="68080-106">I fältet **[Resursgrupp](../articles/azure-resource-manager/resource-group-portal.md)** väljer du en befintlig resursgrupp där namnområdet ska finnas eller skapar en ny.</span><span class="sxs-lookup"><span data-stu-id="68080-106">In the **[Resource group](../articles/azure-resource-manager/resource-group-portal.md)** field, choose an existing resource group in which the namespace will live, or create a new one.</span></span>      
6. <span data-ttu-id="68080-107">I **Plats** väljer du land eller region där namnområdet ska finnas.</span><span class="sxs-lookup"><span data-stu-id="68080-107">In **Location**, choose the country or region in which your namespace should be hosted.</span></span>
   
    ![Skapa namnområde][create-namespace]
7. <span data-ttu-id="68080-109">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="68080-109">Click **Create**.</span></span> <span data-ttu-id="68080-110">Systemet skapar namnområdet och aktiverar det.</span><span class="sxs-lookup"><span data-stu-id="68080-110">The system now creates your namespace and enables it.</span></span> <span data-ttu-id="68080-111">Efter ett par minuter etableras resurser för ditt konto i systemet.</span><span class="sxs-lookup"><span data-stu-id="68080-111">After a few minutes, the system provisions resources for your account.</span></span>

### <a name="obtain-the-management-credentials"></a><span data-ttu-id="68080-112">Hämta autentiseringsuppgifterna för hantering</span><span class="sxs-lookup"><span data-stu-id="68080-112">Obtain the management credentials</span></span>
1. <span data-ttu-id="68080-113">I listan över namnområden, klickar du på det nyligen skapade namnområdet.</span><span class="sxs-lookup"><span data-stu-id="68080-113">In the list of namespaces, click the newly created namespace name.</span></span>
2. <span data-ttu-id="68080-114">På namnområdesbladet klickar du på **Principer för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="68080-114">In the namespace blade, click **Shared access policies**.</span></span>
3. <span data-ttu-id="68080-115">På bladet **Principer för delad åtkomst** klickar du på **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="68080-115">In the **Shared access policies** blade, click **RootManageSharedAccessKey**.</span></span>
   
    ![connection-info][connection-info]
4. <span data-ttu-id="68080-117">På bladet **Princip: RootManageSharedAccessKey** klickar du på kopieringsknappen bredvid **Anslutningssträng – Primärnyckel** för att kopiera anslutningssträngen till Urklipp för senare användning.</span><span class="sxs-lookup"><span data-stu-id="68080-117">In the **Policy: RootManageSharedAccessKey** blade, click the copy button next to **Connection string–primary key**, to copy the connection string to your clipboard for later use.</span></span> <span data-ttu-id="68080-118">Klistra in det här värdet i Anteckningar eller på en tillfällig plats.</span><span class="sxs-lookup"><span data-stu-id="68080-118">Paste this value into Notepad or some other temporary location.</span></span>
   
    ![connection-string][connection-string]

5. <span data-ttu-id="68080-120">Upprepa föregående steg, kopiera och klistra in värdet för **primärnyckeln** till en tillfällig plats för senare användning.</span><span class="sxs-lookup"><span data-stu-id="68080-120">Repeat the previous step, copying and pasting the value of **Primary key** to a temporary location for later use.</span></span>  

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
