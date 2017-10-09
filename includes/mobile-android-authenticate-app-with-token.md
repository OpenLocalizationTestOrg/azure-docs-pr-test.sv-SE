
<span data-ttu-id="79c55-101">hello föregående exempel visade en standard inloggning, vilket kräver hello klienten toocontact båda hello identitet providern och hello Azure serverdelstjänsten varje gång hello appen startar.</span><span class="sxs-lookup"><span data-stu-id="79c55-101">hello previous example showed a standard sign-in, which requires hello client toocontact both hello identity provider and hello back-end Azure service every time hello app starts.</span></span> <span data-ttu-id="79c55-102">Den här metoden är ineffektiv och du kan ha problem med användning om många kunder försök toostart appen samtidigt.</span><span class="sxs-lookup"><span data-stu-id="79c55-102">This method is inefficient, and you can have usage-related issues if many customers try toostart your app simultaneously.</span></span> <span data-ttu-id="79c55-103">Är det bättre toocache hello autentiseringstoken returneras av hello Azure-tjänsten och försök toouse detta innan du börjar använda en provider-baserad inloggning.</span><span class="sxs-lookup"><span data-stu-id="79c55-103">A better approach is toocache hello authorization token returned by hello Azure service, and try toouse this first before using a provider-based sign-in.</span></span>

> [!NOTE]
> <span data-ttu-id="79c55-104">Du kan cachelagra hello-token som utfärdas av hello backend-Azure-tjänsten oavsett om du använder klient-hanteras eller service autentisering.</span><span class="sxs-lookup"><span data-stu-id="79c55-104">You can cache hello token issued by hello back-end Azure service regardless of whether you are using client-managed or service-managed authentication.</span></span> <span data-ttu-id="79c55-105">Den här kursen använder autentisering som hanteras av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="79c55-105">This tutorial uses service-managed authentication.</span></span>
>
>

1. <span data-ttu-id="79c55-106">Öppna hello ToDoActivity.java-filen och Lägg till följande importuttryck hello:</span><span class="sxs-lookup"><span data-stu-id="79c55-106">Open hello ToDoActivity.java file and add hello following import statements:</span></span>

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;
2. <span data-ttu-id="79c55-107">Lägg till följande medlemmar toohello hello `ToDoActivity` klass.</span><span class="sxs-lookup"><span data-stu-id="79c55-107">Add hello following members toohello `ToDoActivity` class.</span></span>

        public static final String SHAREDPREFFILE = "temp";    
        public static final String USERIDPREF = "uid";    
        public static final String TOKENPREF = "tkn";    
3. <span data-ttu-id="79c55-108">Hej ToDoActivity.java-filen, lägga till definitionen för hello hello `cacheUserToken` metod.</span><span class="sxs-lookup"><span data-stu-id="79c55-108">In hello ToDoActivity.java file, add hello following definition for hello `cacheUserToken` method.</span></span>

        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }    

    <span data-ttu-id="79c55-109">Den här metoden lagrar hello användar-ID och token i en fil med inställningar som har markerats som privat.</span><span class="sxs-lookup"><span data-stu-id="79c55-109">This method stores hello user ID and token in a preference file that is marked private.</span></span> <span data-ttu-id="79c55-110">Detta ska skydda åtkomst toohello cache så att andra appar på hello enhet inte har toohello åtkomsttoken.</span><span class="sxs-lookup"><span data-stu-id="79c55-110">This should protect access toohello cache so that other apps on hello device do not have access toohello token.</span></span> <span data-ttu-id="79c55-111">hello inställningar är i begränsat läge för hello app.</span><span class="sxs-lookup"><span data-stu-id="79c55-111">hello preference is sandboxed for hello app.</span></span> <span data-ttu-id="79c55-112">Om någon får åtkomst till toohello enheter är det dock möjligt att de kan få åtkomst till toohello token-cache på annat sätt.</span><span class="sxs-lookup"><span data-stu-id="79c55-112">However, if someone gains access toohello device, it is possible that they may gain access toohello token cache through other means.</span></span>

   > [!NOTE]
   > <span data-ttu-id="79c55-113">Om token åtkomst tooyour data betraktas som känslig och någon kan få åtkomst till toohello enheter kan du skydda hello token med kryptering.</span><span class="sxs-lookup"><span data-stu-id="79c55-113">You can further protect hello token with encryption, if token access tooyour data is considered highly sensitive and someone may gain access toohello device.</span></span> <span data-ttu-id="79c55-114">En helt säker lösning ligger utanför den här självstudiekursen hello men och beror på dina säkerhetskrav.</span><span class="sxs-lookup"><span data-stu-id="79c55-114">A completely secure solution is beyond hello scope of this tutorial, however, and depends on your security requirements.</span></span>
   >
   >
4. <span data-ttu-id="79c55-115">Hej ToDoActivity.java-filen, lägga till definitionen för hello hello `loadUserTokenCache` metod.</span><span class="sxs-lookup"><span data-stu-id="79c55-115">In hello ToDoActivity.java file, add hello following definition for hello `loadUserTokenCache` method.</span></span>

        private boolean loadUserTokenCache(MobileServiceClient client)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            String userId = prefs.getString(USERIDPREF, null);
            if (userId == null)
                return false;
            String token = prefs.getString(TOKENPREF, null);
            if (token == null)
                return false;

            MobileServiceUser user = new MobileServiceUser(userId);
            user.setAuthenticationToken(token);
            client.setCurrentUser(user);

            return true;
        }
5. <span data-ttu-id="79c55-116">I hello *ToDoActivity.java* fil ersätter hello `authenticate` metod med hello följande metod som använder token-cache.</span><span class="sxs-lookup"><span data-stu-id="79c55-116">In hello *ToDoActivity.java* file, replace hello `authenticate` method with hello following method, which uses a token cache.</span></span> <span data-ttu-id="79c55-117">Ändra hello inloggningsprovidern om du vill toouse ett annat konto än Google.</span><span class="sxs-lookup"><span data-stu-id="79c55-117">Change hello login provider if you want toouse an account other than Google.</span></span>

        private void authenticate() {
            // We first try tooload a token cache if one exists.
            if (loadUserTokenCache(mClient))
            {
                createTable();
            }
            // If we failed tooload a token cache, login and create a token cache
            else
            {
                // Login using hello Google provider.    
                ListenableFuture<MobileServiceUser> mLogin = mClient.login(MobileServiceAuthenticationProvider.Google);

                Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                    @Override
                    public void onFailure(Throwable exc) {
                        createAndShowDialog("You must log in. Login Required", "Error");
                    }           
                    @Override
                    public void onSuccess(MobileServiceUser user) {
                        createAndShowDialog(String.format(
                                "You are now logged in - %1$2s",
                                user.getUserId()), "Success");
                        cacheUserToken(mClient.getCurrentUser());
                        createTable();    
                    }
                });
            }
        }
6. <span data-ttu-id="79c55-118">Skapa hello appen och testa autentisering med hjälp av ett giltigt konto.</span><span class="sxs-lookup"><span data-stu-id="79c55-118">Build hello app and test authentication using a valid account.</span></span> <span data-ttu-id="79c55-119">Kör minst två gånger.</span><span class="sxs-lookup"><span data-stu-id="79c55-119">Run it at least twice.</span></span> <span data-ttu-id="79c55-120">Under hello först köra, bör du får en fråga toosign i och skapa hello token-cache.</span><span class="sxs-lookup"><span data-stu-id="79c55-120">During hello first run, you should receive a prompt toosign in and create hello token cache.</span></span> <span data-ttu-id="79c55-121">Därefter försöker varje körning tooload hello token-cache för autentisering.</span><span class="sxs-lookup"><span data-stu-id="79c55-121">After that, each run attempts tooload hello token cache for authentication.</span></span> <span data-ttu-id="79c55-122">Du får inte vara nödvändiga toosign i.</span><span class="sxs-lookup"><span data-stu-id="79c55-122">You should not be required toosign in.</span></span>
