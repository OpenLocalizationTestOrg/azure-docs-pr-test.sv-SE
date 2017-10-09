1. <span data-ttu-id="bee7e-101">Logga in toohello [Azure-portalen][Azure portal].</span><span class="sxs-lookup"><span data-stu-id="bee7e-101">Log on toohello [Azure portal][Azure portal].</span></span>
2. <span data-ttu-id="bee7e-102">Hello vänstra navigeringsfönstret hello-portalen, klicka på **ny**, klicka på **Enterprise Integration**, och klicka sedan på **Relay**.</span><span class="sxs-lookup"><span data-stu-id="bee7e-102">In hello left navigation pane of hello portal, click **New**, then click **Enterprise Integration**, and then click **Relay**.</span></span>
3. <span data-ttu-id="bee7e-103">I hello **skapa namnområdet** dialogrutan, ange ett namn för namnområdet.</span><span class="sxs-lookup"><span data-stu-id="bee7e-103">In hello **Create namespace** dialog, enter a namespace name.</span></span> <span data-ttu-id="bee7e-104">hello systemet kontrollerar omedelbart toosee om hello namn är tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="bee7e-104">hello system immediately checks toosee if hello name is available.</span></span>
4. <span data-ttu-id="bee7e-105">I hello **prenumeration** , Välj en Azure-prenumeration i vilka toocreate hello-namnområdet.</span><span class="sxs-lookup"><span data-stu-id="bee7e-105">In hello **Subscription** field, choose an Azure subscription in which toocreate hello namespace.</span></span>
5. <span data-ttu-id="bee7e-106">I hello  **[resursgruppen](../articles/azure-resource-manager/resource-group-portal.md)**  , Välj en befintlig resursgrupp i vilka hello namnområdet ska live eller skapa en ny.</span><span class="sxs-lookup"><span data-stu-id="bee7e-106">In hello **[Resource group](../articles/azure-resource-manager/resource-group-portal.md)** field, choose an existing resource group in which hello namespace will live, or create a new one.</span></span>      
6. <span data-ttu-id="bee7e-107">I **plats**, Välj hello land eller region där namnområdet ska finnas.</span><span class="sxs-lookup"><span data-stu-id="bee7e-107">In **Location**, choose hello country or region in which your namespace should be hosted.</span></span>
   
    ![Skapa namnområde][create-namespace]
7. <span data-ttu-id="bee7e-109">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="bee7e-109">Click **Create**.</span></span> <span data-ttu-id="bee7e-110">hello systemet skapar namnområdet och aktiverar den.</span><span class="sxs-lookup"><span data-stu-id="bee7e-110">hello system now creates your namespace and enables it.</span></span> <span data-ttu-id="bee7e-111">Efter några minuter, hello systemet tilldelar resurser för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="bee7e-111">After a few minutes, hello system provisions resources for your account.</span></span>

### <a name="obtain-hello-management-credentials"></a><span data-ttu-id="bee7e-112">Hämta autentiseringsuppgifter för hantering av hello</span><span class="sxs-lookup"><span data-stu-id="bee7e-112">Obtain hello management credentials</span></span>
1. <span data-ttu-id="bee7e-113">Hello nyskapad namnområdesnamnet på hello listan namnområden.</span><span class="sxs-lookup"><span data-stu-id="bee7e-113">In hello list of namespaces, click hello newly created namespace name.</span></span>
2. <span data-ttu-id="bee7e-114">I hello namnområde bladet, klickar du på **principer för delad åtkomst**.</span><span class="sxs-lookup"><span data-stu-id="bee7e-114">In hello namespace blade, click **Shared access policies**.</span></span>
3. <span data-ttu-id="bee7e-115">I hello **principer för delad åtkomst** bladet, klickar du på **RootManageSharedAccessKey**.</span><span class="sxs-lookup"><span data-stu-id="bee7e-115">In hello **Shared access policies** blade, click **RootManageSharedAccessKey**.</span></span>
   
    ![connection-info][connection-info]
4. <span data-ttu-id="bee7e-117">I hello **princip: RootManageSharedAccessKey** bladet Klicka hello kopiera bredvid för**sträng – primära anslutningsnyckel**, toocopy hello anslutning sträng tooyour Urklipp för senare användning.</span><span class="sxs-lookup"><span data-stu-id="bee7e-117">In hello **Policy: RootManageSharedAccessKey** blade, click hello copy button next too**Connection string–primary key**, toocopy hello connection string tooyour clipboard for later use.</span></span> <span data-ttu-id="bee7e-118">Klistra in det här värdet i Anteckningar eller på en tillfällig plats.</span><span class="sxs-lookup"><span data-stu-id="bee7e-118">Paste this value into Notepad or some other temporary location.</span></span>
   
    ![connection-string][connection-string]

5. <span data-ttu-id="bee7e-120">Hello Upprepa föregående steg, kopiera och klistra in hello värdet för **primärnyckel** tooa tillfällig plats för senare användning.</span><span class="sxs-lookup"><span data-stu-id="bee7e-120">Repeat hello previous step, copying and pasting hello value of **Primary key** tooa temporary location for later use.</span></span>  

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
