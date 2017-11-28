## <a name="set-up-the-development-environment"></a><span data-ttu-id="13686-101">Konfigurera utvecklingsmiljön</span><span class="sxs-lookup"><span data-stu-id="13686-101">Set up the development environment</span></span>

<span data-ttu-id="13686-102">Det här avsnittet vägleder dig ställa in din utvecklingsmiljö, inklusive att skapa en ASP.NET MVC-app, lägger till en ansluten Services-anslutning, lägger till en domänkontrollant och att ange de nödvändiga namnområde direktiv.</span><span class="sxs-lookup"><span data-stu-id="13686-102">This section walks you setting up your development environment, including creating an ASP.NET MVC app, adding a Connected Services connection, adding a controller, and specifying the required namespace directives.</span></span>

### <a name="create-an-aspnet-mvc-app-project"></a><span data-ttu-id="13686-103">Skapa en ASP.NET MVC-app-projekt</span><span class="sxs-lookup"><span data-stu-id="13686-103">Create an ASP.NET MVC app project</span></span>

1. <span data-ttu-id="13686-104">Öppna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="13686-104">Open Visual Studio.</span></span>

1. <span data-ttu-id="13686-105">Välj **Arkiv -> Ny -> projekt** från huvudmenyn</span><span class="sxs-lookup"><span data-stu-id="13686-105">Select **File->New->Project** from the main menu</span></span>

1. <span data-ttu-id="13686-106">På den **nytt projekt** dialogrutan, ange alternativ som är markerade i följande bild:</span><span class="sxs-lookup"><span data-stu-id="13686-106">On the **New Project** dialog, specify the options as highlighted in the following figure:</span></span>

    ![Skapa ASP.NET-projekt](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. <span data-ttu-id="13686-108">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="13686-108">Select **OK**.</span></span>

1. <span data-ttu-id="13686-109">På den **nytt ASP.NET-projekt** dialogrutan, ange alternativ som är markerade i följande bild:</span><span class="sxs-lookup"><span data-stu-id="13686-109">On the **New ASP.NET Project** dialog, specify the options as highlighted in the following figure:</span></span>

    ![Ange MVC](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

1. <span data-ttu-id="13686-111">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="13686-111">Select **OK**.</span></span>

### <a name="use-connected-services-to-connect-to-an-azure-storage-account"></a><span data-ttu-id="13686-112">Använda anslutna Services för att ansluta till ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="13686-112">Use Connected Services to connect to an Azure storage account</span></span>

1. <span data-ttu-id="13686-113">I den **Solution Explorer**, högerklicka på projektet och på snabbmenyn väljer **Lägg till -> ansluten Service**.</span><span class="sxs-lookup"><span data-stu-id="13686-113">In the **Solution Explorer**, right-click the project, and from the context menu, select **Add->Connected Service**.</span></span>

1. <span data-ttu-id="13686-114">På den **Lägg till ansluten tjänst** markerar **Azure Storage**, och välj sedan **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="13686-114">On the **Add Connected Service** dialog, select **Azure Storage**, and then select **Configure**.</span></span>

    ![Dialogrutan för anslutna Service](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. <span data-ttu-id="13686-116">På den **Azure Storage** dialogrutan Välj önskad Azure storage-kontot som du vill arbeta och välj **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="13686-116">On the **Azure Storage** dialog, select the desired Azure storage account with which you want to work, and select **Add**.</span></span>
