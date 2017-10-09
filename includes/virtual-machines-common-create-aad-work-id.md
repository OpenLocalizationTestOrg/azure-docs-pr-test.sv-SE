
<br>

> [!NOTE]
> <span data-ttu-id="aab53-101">Om du fick ett användarnamn och lösenord som en administratör, det är troligt att du redan har ett arbets- eller skola ID (kallas även ibland ett *organisations-ID*).</span><span class="sxs-lookup"><span data-stu-id="aab53-101">If you were given a user name and password by an administrator, there's a good chance that you already have a work or school ID (also sometimes called an *organizational ID*).</span></span> <span data-ttu-id="aab53-102">I så fall, kan du direkt börja toouse din Azure-konto tooaccess Azure-resurser som kräver ett.</span><span class="sxs-lookup"><span data-stu-id="aab53-102">If so, you can immediately begin toouse your Azure account tooaccess Azure resources that require one.</span></span> <span data-ttu-id="aab53-103">Om du upptäcker att du inte kan använda dessa resurser, kan du behöva tooreturn toothis artikel för hjälp.</span><span class="sxs-lookup"><span data-stu-id="aab53-103">If you find that you cannot use those resources, you may need tooreturn toothis article for help.</span></span> <span data-ttu-id="aab53-104">Mer information finns i [konton som du kan använda för inloggning](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SignInAccounts) och [hur Azure-prenumerationen är relaterade tooAzure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SubRelationToDir).</span><span class="sxs-lookup"><span data-stu-id="aab53-104">For more information, see [Accounts that you can use for sign in](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SignInAccounts) and [How an Azure subscription is related tooAzure AD](https://msdn.microsoft.com/library/azure/dn629581.aspx#BKMK_SubRelationToDir).</span></span>
> 
> 

<span data-ttu-id="aab53-105">hello stegen är enkel.</span><span class="sxs-lookup"><span data-stu-id="aab53-105">hello steps are simple.</span></span> <span data-ttu-id="aab53-106">Du behöver toolocate din signerade på identiteten i hello klassiska Azure-portalen identifiera standard Azure Active Directory-domänen och lägga till en ny användare tooit som administratör för Azure samtidigt.</span><span class="sxs-lookup"><span data-stu-id="aab53-106">You need toolocate your signed on identity in hello Azure classic portal, discover your default Azure Active Directory domain, and add a new user tooit as an Azure co-administrator.</span></span>

## <a name="locate-your-default-directory-in-hello-azure-classic-portal"></a><span data-ttu-id="aab53-107">Leta upp din standardkatalog i hello klassiska Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="aab53-107">Locate your default directory in hello Azure classic portal</span></span>
<span data-ttu-id="aab53-108">Starta genom att logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com) med ditt personliga Microsoft-kontoidentitet.</span><span class="sxs-lookup"><span data-stu-id="aab53-108">Start by logging in toohello [Azure classic portal](https://manage.windowsazure.com) with your personal Microsoft account identity.</span></span> <span data-ttu-id="aab53-109">När du är inloggad, bläddra nedåt hello blå panelen på vänster sida hello och klicka **ACTIVE DIRECTORY**.</span><span class="sxs-lookup"><span data-stu-id="aab53-109">After you are logged in, scroll down hello blue panel on hello left side and click **ACTIVE DIRECTORY**.</span></span>

![Azure Active Directory](./media/virtual-machines-common-create-aad-work-id/azureactivedirectorywidget.png)

<span data-ttu-id="aab53-111">Låt oss börja med att hitta information om din identitet i Azure.</span><span class="sxs-lookup"><span data-stu-id="aab53-111">Let's start by finding some information about your identity in Azure.</span></span> <span data-ttu-id="aab53-112">Du bör se något liknande hello efter i hello huvudfönstret, visar att du har en standardkatalog.</span><span class="sxs-lookup"><span data-stu-id="aab53-112">You should see something like hello following in hello main pane, showing that you have one default directory.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/defaultaadlisting.png)

<span data-ttu-id="aab53-113">Låt oss ta reda på mer information om den.</span><span class="sxs-lookup"><span data-stu-id="aab53-113">Let's find out some more information about it.</span></span> <span data-ttu-id="aab53-114">Klicka på hello standard directory raden, som ger dig till hello standardegenskaperna för katalogen.</span><span class="sxs-lookup"><span data-stu-id="aab53-114">Click hello default directory row, which brings you into hello default directory properties.</span></span>  

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectorypage.png)

<span data-ttu-id="aab53-115">tooview hello standarddomännamnet, klickar du på **DOMÄNER**.</span><span class="sxs-lookup"><span data-stu-id="aab53-115">tooview hello default domain name, click **DOMAINS**.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/domainclicktoseeyourdefaultdomain.png)

<span data-ttu-id="aab53-116">Här bör du kunna toosee att Azure Active Directory när hello Azure-konto har skapats, skapas en personlig standarddomän som är ett hash-värde (ett tal som genereras från en textsträng) för din personligt ID-nummer som används som en underdomän till onmicrosoft.com. Det är hello domän toowhich nu ska du lägga till en ny användare.</span><span class="sxs-lookup"><span data-stu-id="aab53-116">Here you should be able toosee that when hello Azure account was created, Azure Active Directory created a personal default domain that is a hash value (a number generated from a string of text) of your personal ID used as a subdomain of onmicrosoft.com. That's hello domain toowhich you will now add a new user.</span></span>

## <a name="creating-a-new-user-in-hello-default-domain"></a><span data-ttu-id="aab53-117">Skapa en ny användare i hello standarddomän</span><span class="sxs-lookup"><span data-stu-id="aab53-117">Creating a new user in hello default domain</span></span>
<span data-ttu-id="aab53-118">Klicka på **användare** och leta efter din enda personligt konto.</span><span class="sxs-lookup"><span data-stu-id="aab53-118">Click **USERS** and look for your single personal account.</span></span> <span data-ttu-id="aab53-119">Du bör se i hello **ursprung** kolumn som det är en **Microsoft-konto**.</span><span class="sxs-lookup"><span data-stu-id="aab53-119">You should see in hello **SOURCED FROM** column that it is a **Microsoft account**.</span></span> <span data-ttu-id="aab53-120">Vi vill toocreate en användare i din standard. onmicrosoft.com Azure Active Directory-domän.</span><span class="sxs-lookup"><span data-stu-id="aab53-120">We want toocreate a user in your default .onmicrosoft.com Azure Active Directory domain.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryuserslisting.png)

<span data-ttu-id="aab53-121">Vi toofollow [instruktionerna](https://technet.microsoft.com/library/hh967632.aspx#BKMK_1) i hello följande steg, men använder ett exempel.</span><span class="sxs-lookup"><span data-stu-id="aab53-121">We're going toofollow [these instructions](https://technet.microsoft.com/library/hh967632.aspx#BKMK_1) in hello next few steps, but use a specific example.</span></span>

<span data-ttu-id="aab53-122">Hello längst hello-sidan, klickar du på **+ Lägg till användare**.</span><span class="sxs-lookup"><span data-stu-id="aab53-122">At hello bottom of hello page, click **+ADD USER**.</span></span> <span data-ttu-id="aab53-123">Skriv hello nytt användarnamn hello sida som visas, och att Hej **typ av användare** en **ny användare i din organisation**.</span><span class="sxs-lookup"><span data-stu-id="aab53-123">In hello page that appears, type hello new user name, and make hello **Type of User** a **New user in your organization**.</span></span> <span data-ttu-id="aab53-124">I det här exemplet hello nytt användarnamn är `ahmet`.</span><span class="sxs-lookup"><span data-stu-id="aab53-124">In this example, hello new user name is `ahmet`.</span></span> <span data-ttu-id="aab53-125">Välj hello standarddomän som du identifierade tidigare som hello domän för ahmet's e-postadress.</span><span class="sxs-lookup"><span data-stu-id="aab53-125">Select hello default domain that you discovered previously as hello domain for ahmet's email address.</span></span> <span data-ttu-id="aab53-126">Klicka på hello pilen Nästa när du är klar.</span><span class="sxs-lookup"><span data-stu-id="aab53-126">Click hello next arrow when finished.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/addingauserwithdirectorydropdown.png)

<span data-ttu-id="aab53-127">Lägg till mer information för Ahmet, men se till att tooselect hello lämpliga **ROLLEN** värde.</span><span class="sxs-lookup"><span data-stu-id="aab53-127">Add more details for Ahmet, but make sure tooselect hello appropriate **ROLE** value.</span></span> <span data-ttu-id="aab53-128">Det är enkelt toouse **Global administratör** toomake att allt fungerar, men om du kan använda en mindre roll, som är en bra idé.</span><span class="sxs-lookup"><span data-stu-id="aab53-128">It's easy toouse **Global Admin** toomake sure things are working, but if you can use a lesser role, that's a good idea.</span></span> <span data-ttu-id="aab53-129">Det här exemplet används hello **användaren** roll.</span><span class="sxs-lookup"><span data-stu-id="aab53-129">This example uses hello **User** role.</span></span> <span data-ttu-id="aab53-130">(Läs mer på [administratörsbehörigheter efter roll](https://msdn.microsoft.com/library/azure/dn468213.aspx#BKMK_1).) Aktivera inte Multi-Factor authentication såvida du inte vill toouse flerfunktionsautentisering för varje logga in igen.</span><span class="sxs-lookup"><span data-stu-id="aab53-130">(Find out more at [Administrator permissions by role](https://msdn.microsoft.com/library/azure/dn468213.aspx#BKMK_1).) Do not enable multi-factor authentication unless you want toouse multifactor authentication for each log in operation.</span></span> <span data-ttu-id="aab53-131">Klicka på pilen för hello Nästa när du är klar.</span><span class="sxs-lookup"><span data-stu-id="aab53-131">Click hello next arrow when you're finished.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/userprofileuseradmin.png)

<span data-ttu-id="aab53-132">Klicka på hello **skapa** knappen toogenerate och visar ett tillfälligt lösenord för Ahmet.</span><span class="sxs-lookup"><span data-stu-id="aab53-132">Click hello **create** button toogenerate and display a temporary password for Ahmet.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/gettemporarypasswordforuser.png)

<span data-ttu-id="aab53-133">Kopiera hello användarens e-postadress eller använda **skicka lösenord i e-post**.</span><span class="sxs-lookup"><span data-stu-id="aab53-133">Copy hello user name email address, or use **SEND PASSWORD IN EMAIL**.</span></span> <span data-ttu-id="aab53-134">Du behöver hello information toolog på inom kort.</span><span class="sxs-lookup"><span data-stu-id="aab53-134">You'll need hello information toolog on shortly.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/receivedtemporarypassworddialog.png)

<span data-ttu-id="aab53-135">Nu bör du se hello ny användare **Ahmet hello Developer**, ursprung från Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="aab53-135">Now you should see hello new user, **Ahmet hello Developer**, sourced from Azure Active Directory.</span></span> <span data-ttu-id="aab53-136">Du har skapat hello nytt arbete eller skola identitet med Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="aab53-136">You've created hello new work or school identity with Azure Active Directory.</span></span> <span data-ttu-id="aab53-137">Men den här identiteten ännu inte har behörigheter toouse Azure resurser.</span><span class="sxs-lookup"><span data-stu-id="aab53-137">However, this identity does not yet have permissions toouse Azure resources.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/defaultdirectoryusersaftercreate.png)

<span data-ttu-id="aab53-138">Om du använder **skicka lösenord i e-post**, hello efter typ av e-post skickas.</span><span class="sxs-lookup"><span data-stu-id="aab53-138">If you use **SEND PASSWORD IN EMAIL**, hello following kind of email is sent.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/emailreceivedfromnewusercreation.png)

## <a name="adding-azure-co-administrator-rights-for-subscriptions"></a><span data-ttu-id="aab53-139">Lägga till Azure medadministratör rättigheter för prenumerationer</span><span class="sxs-lookup"><span data-stu-id="aab53-139">Adding Azure co-administrator rights for subscriptions</span></span>
<span data-ttu-id="aab53-140">Nu behöver du tooadd hello ny användare som medadministratör för prenumerationen så hello ny användare kan logga in toohello hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="aab53-140">Now you need tooadd hello new user as a co-administrator of your subscription so hello new user can sign in toohello Management Portal.</span></span> <span data-ttu-id="aab53-141">detta, i hello nedre vänstra panelen klickar du på toodo **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="aab53-141">toodo this, in hello lower-left panel click **Settings**.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/thesettingswidget.png)

<span data-ttu-id="aab53-142">Hello huvudinställningarna Klicka på **ADMINISTRATÖRER** på hello top- och du bör se bara din personliga Microsoft-kontoidentitet.</span><span class="sxs-lookup"><span data-stu-id="aab53-142">In hello main settings area, click **ADMINISTRATORS** at hello top and you should see only your personal Microsoft account identity.</span></span> <span data-ttu-id="aab53-143">Hello längst hello-sidan, klickar du på **+ Lägg till** toospecify för en medadministratör.</span><span class="sxs-lookup"><span data-stu-id="aab53-143">At hello bottom of hello page, click **+ADD** toospecify a co-administrator.</span></span> <span data-ttu-id="aab53-144">Här kan ange hello e-postadress hello ny användare som du har skapat, inklusive din standarddomän.</span><span class="sxs-lookup"><span data-stu-id="aab53-144">Here, enter hello email address of hello new user you had created, including your default domain.</span></span> <span data-ttu-id="aab53-145">I hello nästa skärmbild visas en grön bockmarkering nästa toohello användare för hello standardkatalogen.</span><span class="sxs-lookup"><span data-stu-id="aab53-145">As shown in hello next screenshot, a green check mark appears next toohello user for hello default directory.</span></span> <span data-ttu-id="aab53-146">Kom ihåg tooselect alla hello prenumerationer som du vill att den här användaren toobe kan tooadminister.</span><span class="sxs-lookup"><span data-stu-id="aab53-146">Remember tooselect all of hello subscriptions that you would like this user toobe able tooadminister.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/addingnewuserascoadmin.png)

<span data-ttu-id="aab53-147">Du bör nu se två användare, inklusive din nya medadministratör identitet när du är klar.</span><span class="sxs-lookup"><span data-stu-id="aab53-147">When you are done, you should now see two users, including your new co-administrator identity.</span></span> <span data-ttu-id="aab53-148">Logga ut från hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="aab53-148">Log out of hello portal.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/newuseraddedascoadministrator.png)

## <a name="logging-in-and-changing-hello-new-users-password"></a><span data-ttu-id="aab53-149">Logga in och ändra hello nya lösenord</span><span class="sxs-lookup"><span data-stu-id="aab53-149">Logging in and changing hello new user's password</span></span>
<span data-ttu-id="aab53-150">Logga in som hello nya användare som du skapade.</span><span class="sxs-lookup"><span data-stu-id="aab53-150">Log in as hello new user you created.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/signinginwithnewuser.png)

<span data-ttu-id="aab53-151">Du kommer direkt att ange toocreate ett nytt lösenord.</span><span class="sxs-lookup"><span data-stu-id="aab53-151">You will immediately be prompted toocreate a new password.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/mustupdateyourpassword.png)

<span data-ttu-id="aab53-152">Du bör tilldelas lyckade som ser ut som följande hello.</span><span class="sxs-lookup"><span data-stu-id="aab53-152">You should be rewarded with success that looks like hello following.</span></span>

![](./media/virtual-machines-common-create-aad-work-id/successtourdialog.png)

## <a name="next-steps"></a><span data-ttu-id="aab53-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aab53-153">Next steps</span></span>
<span data-ttu-id="aab53-154">Nu kan du använda din nya Azure Active Directory identitet toouse [Azure-resurs mallar](../articles/xplat-cli-azure-resource-manager.md).</span><span class="sxs-lookup"><span data-stu-id="aab53-154">You can now use your new Azure Active Directory identity toouse [Azure resource group templates](../articles/xplat-cli-azure-resource-manager.md).</span></span>

    azure login
    info:    Executing command login
    warn:    Please note that currently you can login only via Microsoft organizational account or service principal. For instructions on how tooset them up, please read http://aka.ms/Dhf67j.
    Username: ahmet@aztrainpassxxxxxoutlook.onmicrosoft.com
    Password: *********
    /info:    Added subscription Azure Pass
    info:    Setting subscription Azure Pass as default
    +
    info:    login command OK
    ralph@local:~$ azure config mode arm
    info:    New mode is arm
    ralph@local:~$ azure group list
    info:    Executing command group list
    + Listing resource groups
    info:    No matched resource groups were found
    info:    group list command OK
    ralph@local:~$ azure group create newgroup westus
    info:    Executing command group create
    + Getting resource group newgroup
    + Creating resource group newgroup
    info:    Created resource group newgroup
    data:    Id:                  /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/newgroup
    data:    Name:                newgroup
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags:
    data:
    info:    group create command OK
