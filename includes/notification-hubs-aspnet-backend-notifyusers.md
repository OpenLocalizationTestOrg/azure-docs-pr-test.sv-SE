## <a name="create-hello-webapi-project"></a>Skapa hello-WebAPI-projekt
En ny ASP.NET-WebAPI-serverdel kommer att skapas i hello avsnitten som följer och det finns tre huvudsakliga syfte:

1. **Autentisering av klienter**: en meddelandehanteraren kommer att läggas till senare tooauthenticate klientbegäranden och associera hello användare med hello-begäran.
2. **Klienten Notification registreringar**: senare, du lägger till en domänkontrollant toohandle nya registreringar för en klient enheten tooreceive meddelanden. hello Autentiserat användarnamn automatiskt till toohello registrering som en [taggen](https://msdn.microsoft.com/library/azure/dn530749.aspx).
3. **Skicka meddelanden tooClients**: senare, du lägger också till en domänkontrollant tooprovide ett sätt för en användare tootrigger en säker push-toodevices och klienter som är associerade med hello-tagg. 

hello följande steg visar hur toocreate hello ny ASP.NET-WebAPI-serverdel: 

> [!IMPORTANT]
> Om du använder Visual Studio 2015 eller tidigare, innan du påbörjar den här självstudiekursen, se till att du har installerat hello senaste versionen av hello NuGet Package Manager. toocheck starta Visual Studio. Från hello **verktyg** -menyn klickar du på **tillägg och uppdateringar**. Sök efter **NuGet Package Manager** för din version av Visual Studio och kontrollera att du har hello senaste versionen. Om inte, avinstallera och sedan installera om hello NuGet Package Manager.
> 
> ![][B4]
> 
> [!NOTE]
> Kontrollera att du har installerat hello Visual Studio [Azure SDK](https://azure.microsoft.com/downloads/) för distribution av webbplatsen.
> 
> 

1. Starta Visual Studio eller Visual Studio Express. Klicka på **Server Explorer** och logga in tooyour Azure-konto. Visual Studio behöver du loggat in toocreate hello webbplatsresurser på ditt konto.
2. I Visual Studio klickar du på **filen**, klicka på **ny**, sedan **projekt**, expandera **mallar**, **Visual C#**, klicka på **Web** och **ASP.NET-webbprogram**, hello typnamn **AppBackend**, och klicka sedan på **OK**. 
   
    ![][B1]
3. I hello **nytt ASP.NET-projekt** dialogrutan klickar du på **Web API**, klicka på **OK**.
   
    ![][B2]
4. I hello **konfigurera Microsoft Azure Web App** dialogrutan Välj en prenumeration och en **programtjänstplanen** du redan har skapat. Du kan också välja **skapar en ny app service-plan** och skapa en hello dialogrutan. Du behöver ingen databas för den här självstudiekursen. När du har valt din apptjänstplan, klickar du på **OK** toocreate hello projektet.
   
    ![][B5]

## <a name="authenticating-clients-toohello-webapi-backend"></a>Autentisera klienter toohello WebAPI Backend
I det här avsnittet skapar du en ny meddelande hanterare klass med namnet **AuthenticationTestHandler** för hello nya serverdel. Den här klassen härleds från [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) och läggs till som en meddelandehanteraren så att den kan bearbeta alla begäranden som kommer till hello backend. 

1. I Solution Explorer högerklickar du på hello **AppBackend** projektet, klicka på **Lägg till**, klicka på **klassen**. Namnge hello nya klassen **AuthenticationTestHandler.cs**, och klicka på **Lägg till** toogenerate hello-klassen. Den här klassen kommer att använda tooauthenticate användare med hjälp av *grundläggande autentisering* för enkelhetens skull. Observera att din app kan använda alla autentiseringsscheman.
2. Lägg till följande hello i AuthenticationTestHandler.cs, `using` instruktioner:
   
        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Text;
        using System.Threading.Tasks;

3. I AuthenticationTestHandler.cs, ersätter hello `AuthenticationTestHandler` klassen definition med hello följande kod. 
   
    Den här hanteraren godkänner hello begäran när hello följande tre villkor uppfylls:
   
   * hello-begäran med en *auktorisering* huvud. 
   * hello begäran använder *grundläggande* autentisering. 
   * hello Användarsträng och hello lösenordssträngen är hello samma sträng.
     
     Annars avvisas hello begäran. Det här är inte en riktig autentiserings- och auktoriseringsmetod. Det är bara ett väldigt enkelt exempel för den här självstudiekursen.
     
     Om begäran hälsningsmeddelande autentiseras och auktoriseras av hello `AuthenticationTestHandler`, och sedan hello grundläggande autentisering användaren kommer att bifogade toohello aktuella begäran på hello [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx). Användarinformation i hello HttpContext kommer att användas av en annan domänkontrollant (RegisterController) senare tooadd en [taggen](https://msdn.microsoft.com/library/azure/dn530749.aspx) toohello meddelandebegäran för registrering.
     
       public class AuthenticationTestHandler : DelegatingHandler   {       protected override Task<HttpResponseMessage> SendAsync(       HttpRequestMessage request, CancellationToken cancellationToken)       {           var authorizationHeader = request.Headers.GetValues("Authorization").First();
     
               if (authorizationHeader != null && authorizationHeader
                   .StartsWith("Basic ", StringComparison.InvariantCultureIgnoreCase))
               {
                   string authorizationUserAndPwdBase64 =
                       authorizationHeader.Substring("Basic ".Length);
                   string authorizationUserAndPwd = Encoding.Default
                       .GetString(Convert.FromBase64String(authorizationUserAndPwdBase64));
                   string user = authorizationUserAndPwd.Split(':')[0];
                   string password = authorizationUserAndPwd.Split(':')[1];
     
                   if (verifyUserAndPwd(user, password))
                   {
                       // Attach hello new principal object toohello current HttpContext object
                       HttpContext.Current.User =
                           new GenericPrincipal(new GenericIdentity(user), new string[0]);
                       System.Threading.Thread.CurrentPrincipal =
                           System.Web.HttpContext.Current.User;
                   }
                   else return Unauthorized();
               }
               else return Unauthorized();
     
               return base.SendAsync(request, cancellationToken);
           }
     
           private bool verifyUserAndPwd(string user, string password)
           {
               // This is not a real authentication scheme.
               return user == password;
           }
     
           private Task<HttpResponseMessage> Unauthorized()
           {
               var response = new HttpResponseMessage(HttpStatusCode.Forbidden);
               var tsc = new TaskCompletionSource<HttpResponseMessage>();
               tsc.SetResult(response);
               return tsc.Task;
           }
       }
     
     > [!NOTE]
     > **Säkerhetsmeddelande**: hello `AuthenticationTestHandler` klassen inte ange SANT autentisering. Det är används endast toomimic grundläggande autentisering och är inte säker. Du måste implementera en säker autentiseringsmekanism i dina tjänster och program i produktionsmiljön.                
     > 
     > 
4. Lägg till följande kod hello slutet av hello hello `Register` metod i hello **App_Start/WebApiConfig.cs** klassen tooregister hello meddelandehanteraren:
   
        config.MessageHandlers.Add(new AuthenticationTestHandler());
5. Spara ändringarna.

## <a name="registering-for-notifications-using-hello-webapi-backend"></a>Registrering för meddelanden med hello WebAPI Backend
I det här avsnittet kommer vi lägga till en ny domänkontrollant toohello WebAPI-serverdel toohandle begär tooregister en användare och enhet för meddelanden med hello-klientbibliotek för notification hub. hello controller lägger till en användartagg för hello användare som autentiserades och bifogas toohello HttpContext av hello `AuthenticationTestHandler`. hello taggen har hello strängformat `"username:<actual username>"`.

1. I Solution Explorer högerklickar du på hello **AppBackend** projektet och klicka sedan på **hantera NuGet-paket**.
2. Hello vänster, klickar du på **Online**, och Sök efter **Microsoft.Azure.NotificationHubs** i hello **Sök** rutan.
3. I listan över sökresultat hello klickar du på **Microsoft Azure Notification Hub**, och klicka sedan på **installera**. Slutföra hello installationen och sedan stänga hello NuGet package manager-fönstret.
   
    Detta lägger till en referens toohello Azure Notification Hubs SDK med hjälp av hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-paketet</a>.
4. Vi kommer nu att skapa en ny klassfil som representerar hello anslutning med notification hub används toosend meddelanden. Hello Solution Explorer, högerklicka på hello **modeller** mappen, klicka på **Lägg till**, klicka på **klassen**. Namnge hello nya klassen **Notifications.cs**, klicka på **Lägg till** toogenerate hello klass. 
   
    ![][B6]
5. Lägg till följande hello i Notifications.cs, `using` instruktionen hello överst i filen hello:
   
        using Microsoft.Azure.NotificationHubs;
6. Ersätt hello `Notifications` klassen med följande hello och se till att tooreplace hello två platshållare med hello anslutningssträngen (med fullständig åtkomst) för din meddelandehubb och hello hubbnamn (på [klassiska Azure-portalen ](http://manage.windowsazure.com)):
   
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }
7. Nu ska vi skapa en ny kontrollant med namnet **RegisterController**. I Solution Explorer högerklickar du på hello **domänkontrollanter** mappen, klicka på **Lägg till**, klicka på **domänkontrollant**. Klicka på hello **Web API 2-styrenhet – tom** objektet och klicka sedan på **Lägg till**. Namnge hello nya klassen **RegisterController**, och klicka sedan på **Lägg till** igen toogenerate hello-styrenhet.
   
    ![][B7]
   
    ![][B8]
8. Lägg till följande hello i RegisterController.cs, `using` instruktioner:
   
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
9. Lägg till följande kod i hello hello `RegisterController` klassen definition. Observera att i den här koden vi lägga till en användartagg för hello användare är anslutna toohello HttpContext. hello användaren autentiserades och bifogas toohello HttpContext av hello meddelandefilter som vi har lagt till `AuthenticationTestHandler`. Du kan också lägga till valfria kontrollerar tooverify som hello användaren har behörighet tooregister för hello begärt taggar.
   
        private NotificationHubClient hub;
   
        public RegisterController()
        {
            hub = Notifications.Instance.Hub;
        }
   
        public class DeviceRegistration
        {
            public string Platform { get; set; }
            public string Handle { get; set; }
            public string[] Tags { get; set; }
        }
   
        // POST api/register
        // This creates a registration id
        public async Task<string> Post(string handle = null)
        {
            string newRegistrationId = null;
   
            // make sure there are no existing registrations for this push handle (used for iOS and Android)
            if (handle != null)
            {
                var registrations = await hub.GetRegistrationsByChannelAsync(handle, 100);
   
                foreach (RegistrationDescription registration in registrations)
                {
                    if (newRegistrationId == null)
                    {
                        newRegistrationId = registration.RegistrationId;
                    }
                    else
                    {
                        await hub.DeleteRegistrationAsync(registration);
                    }
                }
            }
   
            if (newRegistrationId == null) 
                newRegistrationId = await hub.CreateRegistrationIdAsync();
   
            return newRegistrationId;
        }
   
        // PUT api/register/5
        // This creates or updates a registration (with provided channelURI) at hello specified id
        public async Task<HttpResponseMessage> Put(string id, DeviceRegistration deviceUpdate)
        {
            RegistrationDescription registration = null;
            switch (deviceUpdate.Platform)
            {
                case "mpns":
                    registration = new MpnsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "wns":
                    registration = new WindowsRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "apns":
                    registration = new AppleRegistrationDescription(deviceUpdate.Handle);
                    break;
                case "gcm":
                    registration = new GcmRegistrationDescription(deviceUpdate.Handle);
                    break;
                default:
                    throw new HttpResponseException(HttpStatusCode.BadRequest);
            }
   
            registration.RegistrationId = id;
            var username = HttpContext.Current.User.Identity.Name;
   
            // add check if user is allowed tooadd these tags
            registration.Tags = new HashSet<string>(deviceUpdate.Tags);
            registration.Tags.Add("username:" + username);
   
            try
            {
                await hub.CreateOrUpdateRegistrationAsync(registration);
            }
            catch (MessagingException e)
            {
                ReturnGoneIfHubResponseIsGone(e);
            }
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
        // DELETE api/register/5
        public async Task<HttpResponseMessage> Delete(string id)
        {
            await hub.DeleteRegistrationAsync(id);
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
        private static void ReturnGoneIfHubResponseIsGone(MessagingException e)
        {
            var webex = e.InnerException as WebException;
            if (webex.Status == WebExceptionStatus.ProtocolError)
            {
                var response = (HttpWebResponse)webex.Response;
                if (response.StatusCode == HttpStatusCode.Gone)
                    throw new HttpRequestException(HttpStatusCode.Gone.ToString());
            }
        }
10. Spara ändringarna.

## <a name="sending-notifications-from-hello-webapi-backend"></a>Skicka meddelanden från hello WebAPI Backend
Lägg till en ny domänkontrollant som Exponerar en metod för klienten enheter toosend ett meddelande utifrån hello användarnamn taggen med Azure Notification Hubs Service Management Library i hello ASP.NET-WebAPI-serverdel i det här avsnittet.

1. Skapa en till kontrollant med namnet **NotificationsController**. Skapa den hello samma sätt som du skapade hello **RegisterController** hello föregående avsnitt.
2. Lägg till följande hello i NotificationsController.cs, `using` instruktioner:
   
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
3. Lägg till följande metod toohello hello **NotificationsController** klass.
   
    Den här koden skickar en meddelandetyp som baseras på hello Platform Notification Service (PNS) `pns` parameter. Hej värdet för `to_tag` är används tooset hello *användarnamn* tagg på hello-meddelande. Den här taggen måste matcha en username-tagg för en aktiv meddelandehubbsregistrering. hello-meddelande är hämtade från hello brödtext hello POST-begäran och formaterad för hello mål PNS. 
   
    Beroende på hello Platform Notification Service (PNS) att din enheter som stöds använder tooreceive meddelanden, stöds olika meddelanden i olika format. På Windows-enheter kan du till exempel använda ett [popup-meddelande med WNS](https://msdn.microsoft.com/library/windows/apps/br230849.aspx) som inte stöds direkt av en annan PNS. Så måste din serverdel tooformat hello-meddelande till ett meddelande som stöds för hello PNS enheter planera toosupport. Använd hello lämpliga skicka API på hello [NotificationHubClient-klass](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx)
   
        public async Task<HttpResponseMessage> Post(string pns, [FromBody]string message, string to_tag)
        {
            var user = HttpContext.Current.User.Identity.Name;
            string[] userTag = new string[2];
            userTag[0] = "username:" + to_tag;
            userTag[1] = "from:" + user;
   
            Microsoft.Azure.NotificationHubs.NotificationOutcome outcome = null;
            HttpStatusCode ret = HttpStatusCode.InternalServerError;
   
            switch (pns.ToLower())
            {
                case "wns":
                    // Windows 8.1 / Windows Phone 8.1
                    var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">" + 
                                "From " + user + ": " + message + "</text></binding></visual></toast>";
                    outcome = await Notifications.Instance.Hub.SendWindowsNativeNotificationAsync(toast, userTag);
                    break;
                case "apns":
                    // iOS
                    var alert = "{\"aps\":{\"alert\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendAppleNativeNotificationAsync(alert, userTag);
                    break;
                case "gcm":
                    // Android
                    var notif = "{ \"data\" : {\"message\":\"" + "From " + user + ": " + message + "\"}}";
                    outcome = await Notifications.Instance.Hub.SendGcmNativeNotificationAsync(notif, userTag);
                    break;
            }
   
            if (outcome != null)
            {
                if (!((outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Abandoned) ||
                    (outcome.State == Microsoft.Azure.NotificationHubs.NotificationOutcomeState.Unknown)))
                {
                    ret = HttpStatusCode.OK;
                }
            }
   
            return Request.CreateResponse(ret);
        }
4. Tryck på **F5** toorun hello program och tooensure hello noggrannhet arbete hittills. hello app ska starta en webbläsare och visa hello ASP.NET-startsidan. 

## <a name="publish-hello-new-webapi-backend"></a>Publicera hello nya WebAPI Backend
1. Nu ska vi distribuera den här appen tooan Azure-webbplats i ordning toomake den tillgänglig från alla enheter. Högerklicka på hello **AppBackend** projektet och välj **publicera**.
2. Välj **Microsoft Azure App Service** som publiceringsmål och klicka på **Publicera**. Då öppnas hello dialogrutan Skapa Apptjänst, som hjälper dig att skapa alla hello Azure resurser toorun hello ASP.NET-webbapp i Azure.

    ![][B15]
3. I hello **skapa App Service** dialogrutan Välj ditt Azure-konto. Klicka på **Ändra typ** och välj **Webbapp**. Hålla hello **Webbprogramnamnet** angivna och välj hello **prenumeration**, **resursgruppen**, och **Apptjänstplan**.  Klicka på **Skapa**.

4. Anteckna hello **Webbadress** egenskap i hello **sammanfattning** avsnitt. Vi kommer att referera toothis URL: en som din *backend endpoint* senare i den här kursen. Klicka på **Publicera**.

5. När hello guiden slutförs kommer den publicerar hello ASP.NET web app tooAzure och sedan startar hello app i hello standardwebbläsare.  Programmet kan visas i Azure App Service.

hello URL: en använder hello webbprogrammets namn som du angav tidigare, med hello format http://<app_name>.azurewebsites.net.

[B1]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push1.png
[B2]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push2.png
[B3]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push3.png
[B4]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push4.png
[B5]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push5.png
[B6]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push6.png
[B7]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push7.png
[B8]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push8.png
[B14]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-secure-push14.png
[B15]: ./media/notification-hubs-aspnet-backend-notifyusers/publish-to-app-service.png
[B16]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users16.PNG
[B18]: ./media/notification-hubs-aspnet-backend-notifyusers/notification-hubs-notify-users18.PNG
