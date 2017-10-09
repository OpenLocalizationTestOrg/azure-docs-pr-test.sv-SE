## <a name="prepare-tooauthenticate-azure-resource-manager-requests"></a><span data-ttu-id="d588a-101">Förbereda tooauthenticate Azure Resource Manager-begäranden</span><span class="sxs-lookup"><span data-stu-id="d588a-101">Prepare tooauthenticate Azure Resource Manager requests</span></span>
<span data-ttu-id="d588a-102">Du måste autentisera alla hello-åtgärder som du utför på resurser med hjälp av hello [Azure Resource Manager] [ lnk-authenticate-arm] med Azure Active Directory (AD).</span><span class="sxs-lookup"><span data-stu-id="d588a-102">You must authenticate all hello operations that you perform on resources using hello [Azure Resource Manager][lnk-authenticate-arm] with Azure Active Directory (AD).</span></span> <span data-ttu-id="d588a-103">Hej enklaste sättet tooconfigure toouse PowerShell eller Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="d588a-103">hello easiest way tooconfigure this is toouse PowerShell or Azure CLI.</span></span>

<span data-ttu-id="d588a-104">Installera hello [Azure PowerShell-cmdlets] [ lnk-powershell-install] innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="d588a-104">Install hello [Azure PowerShell cmdlets][lnk-powershell-install] before you continue.</span></span>

<span data-ttu-id="d588a-105">Hej följande steg visar hur tooset in lösenordsautentisering för ett AD-program med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="d588a-105">hello following steps show how tooset up password authentication for an AD application using PowerShell.</span></span> <span data-ttu-id="d588a-106">Du kan köra dessa kommandon i en standard PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="d588a-106">You can run these commands in a standard PowerShell session.</span></span>

1. <span data-ttu-id="d588a-107">Logga in tooyour Azure-prenumeration med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="d588a-107">Log in tooyour Azure subscription using hello following command:</span></span>

    ```powershell
    Login-AzureRmAccount
    ```

1. <span data-ttu-id="d588a-108">Om du har flera Azure-prenumerationer, hello logga in tooAzure ger du åtkomst till tooall Azure-prenumerationer som är kopplade till dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="d588a-108">If you have multiple Azure subscriptions, signing in tooAzure grants you access tooall hello Azure subscriptions associated with your credentials.</span></span> <span data-ttu-id="d588a-109">Använd följande kommando toolist hello Azure-prenumerationer tillgängliga för dig toouse hello:</span><span class="sxs-lookup"><span data-stu-id="d588a-109">Use hello following command toolist hello Azure subscriptions available for you toouse:</span></span>

    ```powershell
    Get-AzureRMSubscription
    ```

    <span data-ttu-id="d588a-110">Använd följande kommando tooselect prenumeration som du vill toouse toorun hello kommandon toomanage din IoT-hubb hello.</span><span class="sxs-lookup"><span data-stu-id="d588a-110">Use hello following command tooselect subscription that you want toouse toorun hello commands toomanage your IoT hub.</span></span> <span data-ttu-id="d588a-111">Du kan använda hello namn eller ID från hello utdata från hello föregående kommando:</span><span class="sxs-lookup"><span data-stu-id="d588a-111">You can use either hello subscription name or ID from hello output of hello previous command:</span></span>

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

2. <span data-ttu-id="d588a-112">Anteckna din **TenantId** och **SubscriptionId**.</span><span class="sxs-lookup"><span data-stu-id="d588a-112">Make a note of your **TenantId** and **SubscriptionId**.</span></span> <span data-ttu-id="d588a-113">Du behöver dem senare.</span><span class="sxs-lookup"><span data-stu-id="d588a-113">You need them later.</span></span>
3. <span data-ttu-id="d588a-114">Skapa ett nytt Azure Active Directory-program med hjälp av hello följande kommando, där du ersätter hello platshållare:</span><span class="sxs-lookup"><span data-stu-id="d588a-114">Create a new Azure Active Directory application using hello following command, replacing hello place holders:</span></span>
   
   * <span data-ttu-id="d588a-115">**{Visningsnamn}:** ett visningsnamn för tillämpningsprogrammet som **MySampleApp**</span><span class="sxs-lookup"><span data-stu-id="d588a-115">**{Display name}:** a display name for your application such as **MySampleApp**</span></span>
   * <span data-ttu-id="d588a-116">**{URL-Adressen}:** hello URL för hello startsidan för din app som **http://mysampleapp/home**.</span><span class="sxs-lookup"><span data-stu-id="d588a-116">**{Home page URL}:** hello URL of hello home page of your app such as **http://mysampleapp/home**.</span></span> <span data-ttu-id="d588a-117">Denna URL behöver inte toopoint tooa verkliga program.</span><span class="sxs-lookup"><span data-stu-id="d588a-117">This URL does not need toopoint tooa real application.</span></span>
   * <span data-ttu-id="d588a-118">**{Programidentifierare}:** en unik identifierare som **http://mysampleapp**.</span><span class="sxs-lookup"><span data-stu-id="d588a-118">**{Application identifier}:** A unique identifier such as **http://mysampleapp**.</span></span> <span data-ttu-id="d588a-119">Denna URL behöver inte toopoint tooa verkliga program.</span><span class="sxs-lookup"><span data-stu-id="d588a-119">This URL does not need toopoint tooa real application.</span></span>
   * <span data-ttu-id="d588a-120">**{Lösenord}:** ett lösenord som du använder tooauthenticate med din app.</span><span class="sxs-lookup"><span data-stu-id="d588a-120">**{Password}:** A password that you use tooauthenticate with your app.</span></span>
     
     ```powershell
     New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
     ```
4. <span data-ttu-id="d588a-121">Anteckna hello **ApplicationId** av hello-programmet som du skapade.</span><span class="sxs-lookup"><span data-stu-id="d588a-121">Make a note of hello **ApplicationId** of hello application you created.</span></span> <span data-ttu-id="d588a-122">Du behöver detta senare.</span><span class="sxs-lookup"><span data-stu-id="d588a-122">You need this later.</span></span>
5. <span data-ttu-id="d588a-123">Skapa ett nytt huvudnamn för tjänsten med hjälp av hello följande kommando, där du ersätter **{MyApplicationId}** med hello **ApplicationId** hello föregående steg:</span><span class="sxs-lookup"><span data-stu-id="d588a-123">Create a new service principal using hello following command, replacing **{MyApplicationId}** with hello **ApplicationId** from hello previous step:</span></span>
   
    ```powershell
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
6. <span data-ttu-id="d588a-124">Konfigurera en rolltilldelning med hello följande kommando, där du ersätter **{MyApplicationId}** med din **ApplicationId**.</span><span class="sxs-lookup"><span data-stu-id="d588a-124">Set up a role assignment using hello following command, replacing **{MyApplicationId}** with your **ApplicationId**.</span></span>
   
    ```powershell
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```

<span data-ttu-id="d588a-125">Du är nu klar att skapa hello Azure AD-program som du kan använda tooauthenticate från din anpassade C#-program.</span><span class="sxs-lookup"><span data-stu-id="d588a-125">You have now finished creating hello Azure AD application that enables you tooauthenticate from your custom C# application.</span></span> <span data-ttu-id="d588a-126">Hello följande värden senare i den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="d588a-126">You need hello following values later in this tutorial:</span></span>

* <span data-ttu-id="d588a-127">Klient-ID</span><span class="sxs-lookup"><span data-stu-id="d588a-127">TenantId</span></span>
* <span data-ttu-id="d588a-128">SubscriptionId</span><span class="sxs-lookup"><span data-stu-id="d588a-128">SubscriptionId</span></span>
* <span data-ttu-id="d588a-129">ApplicationId</span><span class="sxs-lookup"><span data-stu-id="d588a-129">ApplicationId</span></span>
* <span data-ttu-id="d588a-130">Lösenord</span><span class="sxs-lookup"><span data-stu-id="d588a-130">Password</span></span>

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
