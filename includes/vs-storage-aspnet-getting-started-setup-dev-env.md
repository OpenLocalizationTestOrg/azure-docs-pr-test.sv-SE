## <a name="set-up-hello-development-environment"></a>Ställa in hello utvecklingsmiljö

Det här avsnittet beskriver hur du konfigurerar din utvecklingsmiljö, inklusive att skapa en ASP.NET MVC-app, lägger till en ansluten Services-anslutning att lägga till en domänkontrollant och namnområde direktiv krävs för att ange hello.

### <a name="create-an-aspnet-mvc-app-project"></a>Skapa en ASP.NET MVC-app-projekt

1. Öppna Visual Studio.

1. Välj **Arkiv -> Ny -> projekt** hello huvudmenyn

1. På hello **nytt projekt** dialogrutan Ange hello alternativ markerade i hello följande bild:

    ![Skapa ASP.NET-projekt](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-1.png)

1. Välj **OK**.

1. På hello **nytt ASP.NET-projekt** dialogrutan Ange hello alternativ markerade i hello följande bild:

    ![Ange MVC](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-2.png)

1. Välj **OK**.

### <a name="use-connected-services-tooconnect-tooan-azure-storage-account"></a>Använda anslutna tjänster tooconnect tooan Azure storage-konto

1. I hello **Solution Explorer**, högerklicka på projektet hello och hello snabbmenyn väljer **Lägg till -> ansluten Service**.

1. På hello **Lägg till ansluten tjänst** markerar **Azure Storage**, och välj sedan **konfigurera**.

    ![Dialogrutan för anslutna Service](./media/vs-storage-aspnet-getting-started-setup-dev-env/vs-storage-aspnet-getting-started-setup-dev-env-3.png)

1. På hello **Azure Storage** markerar hello önskade Azure storage-konto som du vill toowork och väljer **Lägg till**.
