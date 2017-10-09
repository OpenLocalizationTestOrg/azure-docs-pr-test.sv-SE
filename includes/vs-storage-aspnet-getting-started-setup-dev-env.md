## <a name="set-up-hello-development-environment"></a><span data-ttu-id="ddf0c-101">Ställa in hello utvecklingsmiljö</span><span class="sxs-lookup"><span data-stu-id="ddf0c-101">Set up hello development environment</span></span>

<span data-ttu-id="ddf0c-102">Det här avsnittet beskriver hur du konfigurerar din utvecklingsmiljö, inklusive att skapa en ASP.NET MVC-app, lägger till en ansluten Services-anslutning att lägga till en domänkontrollant och namnområde direktiv krävs för att ange hello.</span><span class="sxs-lookup"><span data-stu-id="ddf0c-102">This section walks you setting up your development environment, including creating an ASP.NET MVC app, adding a Connected Services connection, adding a controller, and specifying hello required namespace directives.</span></span>

### <a name="create-an-aspnet-mvc-app-project"></a><span data-ttu-id="ddf0c-103">Skapa en ASP.NET MVC-app-projekt</span><span class="sxs-lookup"><span data-stu-id="ddf0c-103">Create an ASP.NET MVC app project</span></span>

1. <span data-ttu-id="ddf0c-104">Öppna Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ddf0c-104">Open Visual Studio.</span></span>

1. <span data-ttu-id="ddf0c-105">Välj **Arkiv -> Ny -> projekt** hello huvudmenyn</span><span class="sxs-lookup"><span data-stu-id="ddf0c-105">Select **File->New->Project** from hello main menu</span></span>

1. <span data-ttu-id="ddf0c-106">På hello **nytt projekt** dialogrutan Ange hello alternativ markerade i hello följande bild:</span><span class="sxs-lookup"><span data-stu-id="ddf0c-106">On hello **New Project** dialog, specify hello options as highlighted in hello following figure:</span></span>

    ![Skapa ASP.NET-projekt](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. <span data-ttu-id="ddf0c-108">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="ddf0c-108">Select **OK**.</span></span>

1. <span data-ttu-id="ddf0c-109">På hello **nytt ASP.NET-projekt** dialogrutan Ange hello alternativ markerade i hello följande bild:</span><span class="sxs-lookup"><span data-stu-id="ddf0c-109">On hello **New ASP.NET Project** dialog, specify hello options as highlighted in hello following figure:</span></span>

    ![Ange MVC](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

1. <span data-ttu-id="ddf0c-111">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="ddf0c-111">Select **OK**.</span></span>

### <a name="use-connected-services-tooconnect-tooan-azure-storage-account"></a><span data-ttu-id="ddf0c-112">Använda anslutna tjänster tooconnect tooan Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="ddf0c-112">Use Connected Services tooconnect tooan Azure storage account</span></span>

1. <span data-ttu-id="ddf0c-113">I hello **Solution Explorer**, högerklicka på projektet hello och hello snabbmenyn väljer **Lägg till -> ansluten Service**.</span><span class="sxs-lookup"><span data-stu-id="ddf0c-113">In hello **Solution Explorer**, right-click hello project, and from hello context menu, select **Add->Connected Service**.</span></span>

1. <span data-ttu-id="ddf0c-114">På hello **Lägg till ansluten tjänst** markerar **Azure Storage**, och välj sedan **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="ddf0c-114">On hello **Add Connected Service** dialog, select **Azure Storage**, and then select **Configure**.</span></span>

    ![Dialogrutan för anslutna Service](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. <span data-ttu-id="ddf0c-116">På hello **Azure Storage** markerar hello önskade Azure storage-konto som du vill toowork och väljer **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ddf0c-116">On hello **Azure Storage** dialog, select hello desired Azure storage account with which you want toowork, and select **Add**.</span></span>
