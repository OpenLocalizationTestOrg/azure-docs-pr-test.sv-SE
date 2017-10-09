
1. Klicka på hello **Apptjänster** , Välj din Mobile Apps-serverdel, Välj **Quickstart**, och välj sedan din klientplattform (iOS, Android, Xamarin, Cordova).

    ![Azure Portal med snabbstarten för Mobile Apps markerad][quickstart]

2. Om en databasanslutning inte konfigureras, skapar du en hello följande:

    ![Azure-portalen med Mobile Apps Connect toodatabase][connect]

    a. Skapa en ny SQL-databas och server.

    ![Azure Portal med Mobile Apps skapa ny databas och server][server]

    b. Vänta tills hello dataanslutningen har skapats.

    ![Azure Portal-meddelande om att dataanslutningen har skapats][notification]

    c. Dataanslutningen måste lyckas.

    ![Azure Portal-meddelande, "Du har redan en dataanslutning"][already-connection]

3. Under **2. Skapa ett tabell-API**, Välj Node.js för **Språk för serverdel**. 
 
4. Acceptera hello bekräftelse och välj sedan **skapa TodoItem-tabell**.  
    En ny att göra-post-tabell skapas i din databas. 

    >[!IMPORTANT]
    > Ändrar en befintlig serverdel tooNode.js skriver över allt innehåll. en .NET-serverdel Läs toocreate [arbeta med hello .NET backend-server-SDK för Mobile Apps][instructions].

<!-- Images. -->
[quickstart]: ./media/app-service-mobile-configure-new-backend/quickstart.png
[connect]: ./media/app-service-mobile-configure-new-backend/connect-to-bd.png
[notification]: ./media/app-service-mobile-configure-new-backend/notification-data-connection-create.png
[server]: ./media/app-service-mobile-configure-new-backend/create-new-server.png
[already-connection]: ./media/app-service-mobile-configure-new-backend/already-connection.png

<!-- URLs -->
[instructions]: ../articles/app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#create-app
