## <a name="create-hello-webapi-project"></a><span data-ttu-id="c4428-101">Skapa hello-WebAPI-projekt</span><span class="sxs-lookup"><span data-stu-id="c4428-101">Create hello WebAPI Project</span></span>
<span data-ttu-id="c4428-102">En ny ASP.NET-WebAPI-serverdel kommer att skapas i hello avsnitten som följer och det finns tre huvudsakliga syfte:</span><span class="sxs-lookup"><span data-stu-id="c4428-102">A new ASP.NET WebAPI backend will be created in hello sections that follow and it will have three main purposes:</span></span>

1. <span data-ttu-id="c4428-103">**Autentisering av klienter**: en meddelandehanteraren kommer att läggas till senare tooauthenticate klientbegäranden och associera hello användare med hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="c4428-103">**Authenticating Clients**: A message handler will be added later tooauthenticate client requests and associate hello user with hello request.</span></span>
2. <span data-ttu-id="c4428-104">**Klienten Notification registreringar**: senare, du lägger till en domänkontrollant toohandle nya registreringar för en klient enheten tooreceive meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c4428-104">**Client Notification Registrations**: Later, you will add a controller toohandle new registrations for a client device tooreceive notifications.</span></span> <span data-ttu-id="c4428-105">hello Autentiserat användarnamn automatiskt till toohello registrering som en [taggen](https://msdn.microsoft.com/library/azure/dn530749.aspx).</span><span class="sxs-lookup"><span data-stu-id="c4428-105">hello authenticated user name will automatically be added toohello registration as a [tag](https://msdn.microsoft.com/library/azure/dn530749.aspx).</span></span>
3. <span data-ttu-id="c4428-106">**Skicka meddelanden tooClients**: senare, du lägger också till en domänkontrollant tooprovide ett sätt för en användare tootrigger en säker push-toodevices och klienter som är associerade med hello-tagg.</span><span class="sxs-lookup"><span data-stu-id="c4428-106">**Sending Notifications tooClients**: Later, you will also add a controller tooprovide a way for a user tootrigger a secure push toodevices and clients associated with hello tag.</span></span> 

<span data-ttu-id="c4428-107">hello följande steg visar hur toocreate hello ny ASP.NET-WebAPI-serverdel:</span><span class="sxs-lookup"><span data-stu-id="c4428-107">hello following steps show how toocreate hello new ASP.NET WebAPI backend:</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="c4428-108">Om du använder Visual Studio 2015 eller tidigare, innan du påbörjar den här självstudiekursen, se till att du har installerat hello senaste versionen av hello NuGet Package Manager.</span><span class="sxs-lookup"><span data-stu-id="c4428-108">If you are using Visual Studio 2015 or earlier, before starting this tutorial, please ensure that you have installed hello latest version of hello NuGet Package Manager.</span></span> <span data-ttu-id="c4428-109">toocheck starta Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c4428-109">toocheck, start Visual Studio.</span></span> <span data-ttu-id="c4428-110">Från hello **verktyg** -menyn klickar du på **tillägg och uppdateringar**.</span><span class="sxs-lookup"><span data-stu-id="c4428-110">From hello **Tools** menu, click **Extensions and Updates**.</span></span> <span data-ttu-id="c4428-111">Sök efter **NuGet Package Manager** för din version av Visual Studio och kontrollera att du har hello senaste versionen.</span><span class="sxs-lookup"><span data-stu-id="c4428-111">Search for **NuGet Package Manager** for your version of Visual Studio, and make sure you have hello latest version.</span></span> <span data-ttu-id="c4428-112">Om inte, avinstallera och sedan installera om hello NuGet Package Manager.</span><span class="sxs-lookup"><span data-stu-id="c4428-112">If not, please uninstall, then reinstall hello NuGet Package Manager.</span></span>
> 
> ![][B4]
> 
> [!NOTE]
> <span data-ttu-id="c4428-113">Kontrollera att du har installerat hello Visual Studio [Azure SDK](https://azure.microsoft.com/downloads/) för distribution av webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="c4428-113">Make sure you have installed hello Visual Studio [Azure SDK](https://azure.microsoft.com/downloads/) for website deployment.</span></span>
> 
> 

1. <span data-ttu-id="c4428-114">Starta Visual Studio eller Visual Studio Express.</span><span class="sxs-lookup"><span data-stu-id="c4428-114">Start Visual Studio or Visual Studio Express.</span></span> <span data-ttu-id="c4428-115">Klicka på **Server Explorer** och logga in tooyour Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="c4428-115">Click **Server Explorer** and sign in tooyour Azure account.</span></span> <span data-ttu-id="c4428-116">Visual Studio behöver du loggat in toocreate hello webbplatsresurser på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="c4428-116">Visual Studio will need you signed in toocreate hello web site resources on your account.</span></span>
2. <span data-ttu-id="c4428-117">I Visual Studio klickar du på **filen**, klicka på **ny**, sedan **projekt**, expandera **mallar**, **Visual C#**, klicka på **Web** och **ASP.NET-webbprogram**, hello typnamn **AppBackend**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4428-117">In Visual Studio, click **File**, then click **New**, then **Project**, expand **Templates**, **Visual C#**, then click **Web** and **ASP.NET Web Application**, type hello name **AppBackend**, and then click **OK**.</span></span> 
   
    ![][B1]
3. <span data-ttu-id="c4428-118">I hello **nytt ASP.NET-projekt** dialogrutan klickar du på **Web API**, klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c4428-118">In hello **New ASP.NET Project** dialog, click **Web API**, then click **OK**.</span></span>
   
    ![][B2]
4. <span data-ttu-id="c4428-119">I hello **konfigurera Microsoft Azure Web App** dialogrutan Välj en prenumeration och en **programtjänstplanen** du redan har skapat.</span><span class="sxs-lookup"><span data-stu-id="c4428-119">In hello **Configure Microsoft Azure Web App** dialog, choose a subscription, and an **App Service plan** you have already created.</span></span> <span data-ttu-id="c4428-120">Du kan också välja **skapar en ny app service-plan** och skapa en hello dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="c4428-120">You can also choose **Create a new app service plan** and create one from hello dialog.</span></span> <span data-ttu-id="c4428-121">Du behöver ingen databas för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c4428-121">You do not need a database for this tutorial.</span></span> <span data-ttu-id="c4428-122">När du har valt din apptjänstplan, klickar du på **OK** toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="c4428-122">Once you have selected your app service plan, click **OK** toocreate hello project.</span></span>
   
    ![][B5]

## <a name="authenticating-clients-toohello-webapi-backend"></a><span data-ttu-id="c4428-123">Autentisera klienter toohello WebAPI Backend</span><span class="sxs-lookup"><span data-stu-id="c4428-123">Authenticating Clients toohello WebAPI Backend</span></span>
<span data-ttu-id="c4428-124">I det här avsnittet skapar du en ny meddelande hanterare klass med namnet **AuthenticationTestHandler** för hello nya serverdel.</span><span class="sxs-lookup"><span data-stu-id="c4428-124">In this section, you will create a new message handler class named **AuthenticationTestHandler** for hello new backend.</span></span> <span data-ttu-id="c4428-125">Den här klassen härleds från [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) och läggs till som en meddelandehanteraren så att den kan bearbeta alla begäranden som kommer till hello backend.</span><span class="sxs-lookup"><span data-stu-id="c4428-125">This class is derived from [DelegatingHandler](https://msdn.microsoft.com/library/system.net.http.delegatinghandler.aspx) and added as a message handler so it can process all requests coming into hello backend.</span></span> 

1. <span data-ttu-id="c4428-126">I Solution Explorer högerklickar du på hello **AppBackend** projektet, klicka på **Lägg till**, klicka på **klassen**.</span><span class="sxs-lookup"><span data-stu-id="c4428-126">In Solution Explorer, right-click hello **AppBackend** project, click **Add**, then click **Class**.</span></span> <span data-ttu-id="c4428-127">Namnge hello nya klassen **AuthenticationTestHandler.cs**, och klicka på **Lägg till** toogenerate hello-klassen.</span><span class="sxs-lookup"><span data-stu-id="c4428-127">Name hello new class **AuthenticationTestHandler.cs**, and click **Add** toogenerate hello class.</span></span> <span data-ttu-id="c4428-128">Den här klassen kommer att använda tooauthenticate användare med hjälp av *grundläggande autentisering* för enkelhetens skull.</span><span class="sxs-lookup"><span data-stu-id="c4428-128">This class will be used tooauthenticate users using *Basic Authentication* for simplicity.</span></span> <span data-ttu-id="c4428-129">Observera att din app kan använda alla autentiseringsscheman.</span><span class="sxs-lookup"><span data-stu-id="c4428-129">Note that your app can use any authentication scheme.</span></span>
2. <span data-ttu-id="c4428-130">Lägg till följande hello i AuthenticationTestHandler.cs, `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="c4428-130">In AuthenticationTestHandler.cs, add hello following `using` statements:</span></span>
   
        using System.Net.Http;
        using System.Threading;
        using System.Security.Principal;
        using System.Net;
        using System.Text;
        using System.Threading.Tasks;

3. <span data-ttu-id="c4428-131">I AuthenticationTestHandler.cs, ersätter hello `AuthenticationTestHandler` klassen definition med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="c4428-131">In AuthenticationTestHandler.cs, replacing hello `AuthenticationTestHandler` class definition with hello following code.</span></span> 
   
    <span data-ttu-id="c4428-132">Den här hanteraren godkänner hello begäran när hello följande tre villkor uppfylls:</span><span class="sxs-lookup"><span data-stu-id="c4428-132">This handler will authorize hello request when hello following three conditions are all true:</span></span>
   
   * <span data-ttu-id="c4428-133">hello-begäran med en *auktorisering* huvud.</span><span class="sxs-lookup"><span data-stu-id="c4428-133">hello request included an *Authorization* header.</span></span> 
   * <span data-ttu-id="c4428-134">hello begäran använder *grundläggande* autentisering.</span><span class="sxs-lookup"><span data-stu-id="c4428-134">hello request uses *basic* authentication.</span></span> 
   * <span data-ttu-id="c4428-135">hello Användarsträng och hello lösenordssträngen är hello samma sträng.</span><span class="sxs-lookup"><span data-stu-id="c4428-135">hello user name string and hello password string are hello same string.</span></span>
     
     <span data-ttu-id="c4428-136">Annars avvisas hello begäran.</span><span class="sxs-lookup"><span data-stu-id="c4428-136">Otherwise, hello request will be rejected.</span></span> <span data-ttu-id="c4428-137">Det här är inte en riktig autentiserings- och auktoriseringsmetod.</span><span class="sxs-lookup"><span data-stu-id="c4428-137">This is not a true authentication and authorization approach.</span></span> <span data-ttu-id="c4428-138">Det är bara ett väldigt enkelt exempel för den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c4428-138">It is just a very simple example for this tutorial.</span></span>
     
     <span data-ttu-id="c4428-139">Om begäran hälsningsmeddelande autentiseras och auktoriseras av hello `AuthenticationTestHandler`, och sedan hello grundläggande autentisering användaren kommer att bifogade toohello aktuella begäran på hello [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx).</span><span class="sxs-lookup"><span data-stu-id="c4428-139">If hello request message is authenticated and authorized by hello `AuthenticationTestHandler`, then hello basic authentication user will be attached toohello current request on hello [HttpContext](https://msdn.microsoft.com/library/system.web.httpcontext.current.aspx).</span></span> <span data-ttu-id="c4428-140">Användarinformation i hello HttpContext kommer att användas av en annan domänkontrollant (RegisterController) senare tooadd en [taggen](https://msdn.microsoft.com/library/azure/dn530749.aspx) toohello meddelandebegäran för registrering.</span><span class="sxs-lookup"><span data-stu-id="c4428-140">User information in hello HttpContext will be used by another controller (RegisterController) later tooadd a [tag](https://msdn.microsoft.com/library/azure/dn530749.aspx) toohello notification registration request.</span></span>
     
       <span data-ttu-id="c4428-141">public class AuthenticationTestHandler : DelegatingHandler   {       protected override Task<HttpResponseMessage> SendAsync(       HttpRequestMessage request, CancellationToken cancellationToken)       {           var authorizationHeader = request.Headers.GetValues("Authorization").First();</span><span class="sxs-lookup"><span data-stu-id="c4428-141">public class AuthenticationTestHandler : DelegatingHandler   {       protected override Task<HttpResponseMessage> SendAsync(       HttpRequestMessage request, CancellationToken cancellationToken)       {           var authorizationHeader = request.Headers.GetValues("Authorization").First();</span></span>
     
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
       <span data-ttu-id="c4428-142">}</span><span class="sxs-lookup"><span data-stu-id="c4428-142">}</span></span>
     
     > [!NOTE]
     > <span data-ttu-id="c4428-143">**Säkerhetsmeddelande**: hello `AuthenticationTestHandler` klassen inte ange SANT autentisering.</span><span class="sxs-lookup"><span data-stu-id="c4428-143">**Security Note**: hello `AuthenticationTestHandler` class does not provide true authentication.</span></span> <span data-ttu-id="c4428-144">Det är används endast toomimic grundläggande autentisering och är inte säker.</span><span class="sxs-lookup"><span data-stu-id="c4428-144">It is used only toomimic basic authentication and is not secure.</span></span> <span data-ttu-id="c4428-145">Du måste implementera en säker autentiseringsmekanism i dina tjänster och program i produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="c4428-145">You must implement a secure authentication mechanism in your production applications and services.</span></span>                
     > 
     > 
4. <span data-ttu-id="c4428-146">Lägg till följande kod hello slutet av hello hello `Register` metod i hello **App_Start/WebApiConfig.cs** klassen tooregister hello meddelandehanteraren:</span><span class="sxs-lookup"><span data-stu-id="c4428-146">Add hello following code at hello end of hello `Register` method in hello **App_Start/WebApiConfig.cs** class tooregister hello message handler:</span></span>
   
        config.MessageHandlers.Add(new AuthenticationTestHandler());
5. <span data-ttu-id="c4428-147">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="c4428-147">Save your changes.</span></span>

## <a name="registering-for-notifications-using-hello-webapi-backend"></a><span data-ttu-id="c4428-148">Registrering för meddelanden med hello WebAPI Backend</span><span class="sxs-lookup"><span data-stu-id="c4428-148">Registering for Notifications using hello WebAPI Backend</span></span>
<span data-ttu-id="c4428-149">I det här avsnittet kommer vi lägga till en ny domänkontrollant toohello WebAPI-serverdel toohandle begär tooregister en användare och enhet för meddelanden med hello-klientbibliotek för notification hub.</span><span class="sxs-lookup"><span data-stu-id="c4428-149">In this section, we will add a new controller toohello WebAPI backend toohandle requests tooregister a user and device for notifications using hello client library for notification hubs.</span></span> <span data-ttu-id="c4428-150">hello controller lägger till en användartagg för hello användare som autentiserades och bifogas toohello HttpContext av hello `AuthenticationTestHandler`.</span><span class="sxs-lookup"><span data-stu-id="c4428-150">hello controller will add a user tag for hello user that was authenticated and attached toohello HttpContext by hello `AuthenticationTestHandler`.</span></span> <span data-ttu-id="c4428-151">hello taggen har hello strängformat `"username:<actual username>"`.</span><span class="sxs-lookup"><span data-stu-id="c4428-151">hello tag will have hello string format, `"username:<actual username>"`.</span></span>

1. <span data-ttu-id="c4428-152">I Solution Explorer högerklickar du på hello **AppBackend** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="c4428-152">In Solution Explorer, right-click hello **AppBackend** project and then click **Manage NuGet Packages**.</span></span>
2. <span data-ttu-id="c4428-153">Hello vänster, klickar du på **Online**, och Sök efter **Microsoft.Azure.NotificationHubs** i hello **Sök** rutan.</span><span class="sxs-lookup"><span data-stu-id="c4428-153">On hello left-hand side, click **Online**, and search for **Microsoft.Azure.NotificationHubs** in hello **Search** box.</span></span>
3. <span data-ttu-id="c4428-154">I listan över sökresultat hello klickar du på **Microsoft Azure Notification Hub**, och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="c4428-154">In hello results list, click **Microsoft Azure Notification Hubs**, and then click **Install**.</span></span> <span data-ttu-id="c4428-155">Slutföra hello installationen och sedan stänga hello NuGet package manager-fönstret.</span><span class="sxs-lookup"><span data-stu-id="c4428-155">Complete hello installation, then close hello NuGet package manager window.</span></span>
   
    <span data-ttu-id="c4428-156">Detta lägger till en referens toohello Azure Notification Hubs SDK med hjälp av hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-paketet</a>.</span><span class="sxs-lookup"><span data-stu-id="c4428-156">This adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
4. <span data-ttu-id="c4428-157">Vi kommer nu att skapa en ny klassfil som representerar hello anslutning med notification hub används toosend meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c4428-157">We will now create a new class file that represents hello connection with notification hub used toosend notifications.</span></span> <span data-ttu-id="c4428-158">Hello Solution Explorer, högerklicka på hello **modeller** mappen, klicka på **Lägg till**, klicka på **klassen**.</span><span class="sxs-lookup"><span data-stu-id="c4428-158">In hello Solution Explorer, right-click hello **Models** folder, click **Add**, then click **Class**.</span></span> <span data-ttu-id="c4428-159">Namnge hello nya klassen **Notifications.cs**, klicka på **Lägg till** toogenerate hello klass.</span><span class="sxs-lookup"><span data-stu-id="c4428-159">Name hello new class **Notifications.cs**, then click **Add** toogenerate hello class.</span></span> 
   
    ![][B6]
5. <span data-ttu-id="c4428-160">Lägg till följande hello i Notifications.cs, `using` instruktionen hello överst i filen hello:</span><span class="sxs-lookup"><span data-stu-id="c4428-160">In Notifications.cs, add hello following `using` statement at hello top of hello file:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
6. <span data-ttu-id="c4428-161">Ersätt hello `Notifications` klassen med följande hello och se till att tooreplace hello två platshållare med hello anslutningssträngen (med fullständig åtkomst) för din meddelandehubb och hello hubbnamn (på [klassiska Azure-portalen ](http://manage.windowsazure.com)):</span><span class="sxs-lookup"><span data-stu-id="c4428-161">Replace hello `Notifications` class definition with hello following and make sure tooreplace hello two placeholders with hello connection string (with full access) for your notification hub, and hello hub name (available at [Azure Classic Portal](http://manage.windowsazure.com)):</span></span>
   
        public class Notifications
        {
            public static Notifications Instance = new Notifications();
   
            public NotificationHubClient Hub { get; set; }
   
            private Notifications() {
                Hub = NotificationHubClient.CreateClientFromConnectionString("<your hub's DefaultFullSharedAccessSignature>", 
                                                                             "<hub name>");
            }
        }
7. <span data-ttu-id="c4428-162">Nu ska vi skapa en ny kontrollant med namnet **RegisterController**.</span><span class="sxs-lookup"><span data-stu-id="c4428-162">Next we will create a new controller named **RegisterController**.</span></span> <span data-ttu-id="c4428-163">I Solution Explorer högerklickar du på hello **domänkontrollanter** mappen, klicka på **Lägg till**, klicka på **domänkontrollant**.</span><span class="sxs-lookup"><span data-stu-id="c4428-163">In Solution Explorer, right-click hello **Controllers** folder, then click **Add**, then click **Controller**.</span></span> <span data-ttu-id="c4428-164">Klicka på hello **Web API 2-styrenhet – tom** objektet och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="c4428-164">Click hello **Web API 2 Controller -- Empty** item, and then click **Add**.</span></span> <span data-ttu-id="c4428-165">Namnge hello nya klassen **RegisterController**, och klicka sedan på **Lägg till** igen toogenerate hello-styrenhet.</span><span class="sxs-lookup"><span data-stu-id="c4428-165">Name hello new class **RegisterController**, and then click **Add** again toogenerate hello controller.</span></span>
   
    ![][B7]
   
    ![][B8]
8. <span data-ttu-id="c4428-166">Lägg till följande hello i RegisterController.cs, `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="c4428-166">In RegisterController.cs, add hello following `using` statements:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.NotificationHubs.Messaging;
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
9. <span data-ttu-id="c4428-167">Lägg till följande kod i hello hello `RegisterController` klassen definition.</span><span class="sxs-lookup"><span data-stu-id="c4428-167">Add hello following code inside hello `RegisterController` class definition.</span></span> <span data-ttu-id="c4428-168">Observera att i den här koden vi lägga till en användartagg för hello användare är anslutna toohello HttpContext.</span><span class="sxs-lookup"><span data-stu-id="c4428-168">Note that in this code, we add a user tag for hello user this is attached toohello HttpContext.</span></span> <span data-ttu-id="c4428-169">hello användaren autentiserades och bifogas toohello HttpContext av hello meddelandefilter som vi har lagt till `AuthenticationTestHandler`.</span><span class="sxs-lookup"><span data-stu-id="c4428-169">hello user was authenticated and attached toohello HttpContext by hello message filter we added, `AuthenticationTestHandler`.</span></span> <span data-ttu-id="c4428-170">Du kan också lägga till valfria kontrollerar tooverify som hello användaren har behörighet tooregister för hello begärt taggar.</span><span class="sxs-lookup"><span data-stu-id="c4428-170">You can also add optional checks tooverify that hello user has rights tooregister for hello requested tags.</span></span>
   
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
10. <span data-ttu-id="c4428-171">Spara ändringarna.</span><span class="sxs-lookup"><span data-stu-id="c4428-171">Save your changes.</span></span>

## <a name="sending-notifications-from-hello-webapi-backend"></a><span data-ttu-id="c4428-172">Skicka meddelanden från hello WebAPI Backend</span><span class="sxs-lookup"><span data-stu-id="c4428-172">Sending Notifications from hello WebAPI Backend</span></span>
<span data-ttu-id="c4428-173">Lägg till en ny domänkontrollant som Exponerar en metod för klienten enheter toosend ett meddelande utifrån hello användarnamn taggen med Azure Notification Hubs Service Management Library i hello ASP.NET-WebAPI-serverdel i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c4428-173">In this section you add a new controller that exposes a way for client devices toosend a notification based on hello username tag using Azure Notification Hubs Service Management Library in hello ASP.NET WebAPI backend.</span></span>

1. <span data-ttu-id="c4428-174">Skapa en till kontrollant med namnet **NotificationsController**.</span><span class="sxs-lookup"><span data-stu-id="c4428-174">Create another new controller named **NotificationsController**.</span></span> <span data-ttu-id="c4428-175">Skapa den hello samma sätt som du skapade hello **RegisterController** hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c4428-175">Create it hello same way you created hello **RegisterController** in hello previous section.</span></span>
2. <span data-ttu-id="c4428-176">Lägg till följande hello i NotificationsController.cs, `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="c4428-176">In NotificationsController.cs, add hello following `using` statements:</span></span>
   
        using AppBackend.Models;
        using System.Threading.Tasks;
        using System.Web;
3. <span data-ttu-id="c4428-177">Lägg till följande metod toohello hello **NotificationsController** klass.</span><span class="sxs-lookup"><span data-stu-id="c4428-177">Add hello following method toohello **NotificationsController** class.</span></span>
   
    <span data-ttu-id="c4428-178">Den här koden skickar en meddelandetyp som baseras på hello Platform Notification Service (PNS) `pns` parameter.</span><span class="sxs-lookup"><span data-stu-id="c4428-178">This code send a notification type based on hello Platform Notification Service (PNS) `pns` parameter.</span></span> <span data-ttu-id="c4428-179">Hej värdet för `to_tag` är används tooset hello *användarnamn* tagg på hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="c4428-179">hello value of `to_tag` is used tooset hello *username* tag on hello message.</span></span> <span data-ttu-id="c4428-180">Den här taggen måste matcha en username-tagg för en aktiv meddelandehubbsregistrering.</span><span class="sxs-lookup"><span data-stu-id="c4428-180">This tag must match a username tag of an active notification hub registration.</span></span> <span data-ttu-id="c4428-181">hello-meddelande är hämtade från hello brödtext hello POST-begäran och formaterad för hello mål PNS.</span><span class="sxs-lookup"><span data-stu-id="c4428-181">hello notification message is pulled from hello body of hello POST request and formatted for hello target PNS.</span></span> 
   
    <span data-ttu-id="c4428-182">Beroende på hello Platform Notification Service (PNS) att din enheter som stöds använder tooreceive meddelanden, stöds olika meddelanden i olika format.</span><span class="sxs-lookup"><span data-stu-id="c4428-182">Depending on hello Platform Notification Service (PNS) that your supported devices use tooreceive notifications, different notifications are supported using different formats.</span></span> <span data-ttu-id="c4428-183">På Windows-enheter kan du till exempel använda ett [popup-meddelande med WNS](https://msdn.microsoft.com/library/windows/apps/br230849.aspx) som inte stöds direkt av en annan PNS.</span><span class="sxs-lookup"><span data-stu-id="c4428-183">For example on Windows devices, you could use a [toast notification with WNS](https://msdn.microsoft.com/library/windows/apps/br230849.aspx) that isn't directly supported by another PNS.</span></span> <span data-ttu-id="c4428-184">Så måste din serverdel tooformat hello-meddelande till ett meddelande som stöds för hello PNS enheter planera toosupport.</span><span class="sxs-lookup"><span data-stu-id="c4428-184">So your backend would need tooformat hello notification into a supported notification for hello PNS of devices you plan toosupport.</span></span> <span data-ttu-id="c4428-185">Använd hello lämpliga skicka API på hello [NotificationHubClient-klass](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx)</span><span class="sxs-lookup"><span data-stu-id="c4428-185">Then use hello appropriate send API on hello [NotificationHubClient class](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.notificationhubclient_methods.aspx)</span></span>
   
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
4. <span data-ttu-id="c4428-186">Tryck på **F5** toorun hello program och tooensure hello noggrannhet arbete hittills.</span><span class="sxs-lookup"><span data-stu-id="c4428-186">Press **F5** toorun hello application and tooensure hello accuracy of your work so far.</span></span> <span data-ttu-id="c4428-187">hello app ska starta en webbläsare och visa hello ASP.NET-startsidan.</span><span class="sxs-lookup"><span data-stu-id="c4428-187">hello app should launch a web browser and display hello ASP.NET home page.</span></span> 

## <a name="publish-hello-new-webapi-backend"></a><span data-ttu-id="c4428-188">Publicera hello nya WebAPI Backend</span><span class="sxs-lookup"><span data-stu-id="c4428-188">Publish hello new WebAPI Backend</span></span>
1. <span data-ttu-id="c4428-189">Nu ska vi distribuera den här appen tooan Azure-webbplats i ordning toomake den tillgänglig från alla enheter.</span><span class="sxs-lookup"><span data-stu-id="c4428-189">Now we will deploy this app tooan Azure Website in order toomake it accessible from all devices.</span></span> <span data-ttu-id="c4428-190">Högerklicka på hello **AppBackend** projektet och välj **publicera**.</span><span class="sxs-lookup"><span data-stu-id="c4428-190">Right-click on hello **AppBackend** project and select **Publish**.</span></span>
2. <span data-ttu-id="c4428-191">Välj **Microsoft Azure App Service** som publiceringsmål och klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="c4428-191">Select **Microsoft Azure App Service** as your publish target and click **Publish**.</span></span> <span data-ttu-id="c4428-192">Då öppnas hello dialogrutan Skapa Apptjänst, som hjälper dig att skapa alla hello Azure resurser toorun hello ASP.NET-webbapp i Azure.</span><span class="sxs-lookup"><span data-stu-id="c4428-192">This opens hello Create App Service dialog, which helps you create all hello necessary Azure resources toorun hello ASP.NET web app in Azure.</span></span>

    ![][B15]
3. <span data-ttu-id="c4428-193">I hello **skapa App Service** dialogrutan Välj ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="c4428-193">In hello **Create App Service** dialog, select your Azure account.</span></span> <span data-ttu-id="c4428-194">Klicka på **Ändra typ** och välj **Webbapp**.</span><span class="sxs-lookup"><span data-stu-id="c4428-194">Click **Change Type** and select **Web App**.</span></span> <span data-ttu-id="c4428-195">Hålla hello **Webbprogramnamnet** angivna och välj hello **prenumeration**, **resursgruppen**, och **Apptjänstplan**.</span><span class="sxs-lookup"><span data-stu-id="c4428-195">Keep hello **Web App Name** given and select hello **Subscription**, **Resource Group**, and **App Service Plan**.</span></span>  <span data-ttu-id="c4428-196">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="c4428-196">Click **Create**.</span></span>

4. <span data-ttu-id="c4428-197">Anteckna hello **Webbadress** egenskap i hello **sammanfattning** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c4428-197">Make a note of hello **Site URL** property in hello **Summary** section.</span></span> <span data-ttu-id="c4428-198">Vi kommer att referera toothis URL: en som din *backend endpoint* senare i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="c4428-198">We will refer toothis URL as your *backend endpoint* later in this tutorial.</span></span> <span data-ttu-id="c4428-199">Klicka på **Publicera**.</span><span class="sxs-lookup"><span data-stu-id="c4428-199">Click **Publish**.</span></span>

5. <span data-ttu-id="c4428-200">När hello guiden slutförs kommer den publicerar hello ASP.NET web app tooAzure och sedan startar hello app i hello standardwebbläsare.</span><span class="sxs-lookup"><span data-stu-id="c4428-200">Once hello wizard completes, it publishes hello ASP.NET web app tooAzure, and then launches hello app in hello default browser.</span></span>  <span data-ttu-id="c4428-201">Programmet kan visas i Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="c4428-201">Your application will be viewable in Azure App Services.</span></span>

<span data-ttu-id="c4428-202">hello URL: en använder hello webbprogrammets namn som du angav tidigare, med hello format http://<app_name>.azurewebsites.net.</span><span class="sxs-lookup"><span data-stu-id="c4428-202">hello URL uses hello web app name that you specified earlier, with hello format http://<app_name>.azurewebsites.net.</span></span>

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
