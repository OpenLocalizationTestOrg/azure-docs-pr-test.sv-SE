
hello föregående exempel visade en standard inloggning, vilket kräver hello klienten toocontact båda hello identitet providern och hello Azure serverdelstjänsten varje gång hello appen startar. Den här metoden är ineffektiv och du kan ha problem med användning om många kunder försök toostart appen samtidigt. Är det bättre toocache hello autentiseringstoken returneras av hello Azure-tjänsten och försök toouse detta innan du börjar använda en provider-baserad inloggning.

> [!NOTE]
> Du kan cachelagra hello-token som utfärdas av hello backend-Azure-tjänsten oavsett om du använder klient-hanteras eller service autentisering. Den här kursen använder autentisering som hanteras av tjänsten.
>
>

1. Öppna hello ToDoActivity.java-filen och Lägg till följande importuttryck hello:

        import android.content.Context;
        import android.content.SharedPreferences;
        import android.content.SharedPreferences.Editor;
2. Lägg till följande medlemmar toohello hello `ToDoActivity` klass.

        public static final String SHAREDPREFFILE = "temp";    
        public static final String USERIDPREF = "uid";    
        public static final String TOKENPREF = "tkn";    
3. Hej ToDoActivity.java-filen, lägga till definitionen för hello hello `cacheUserToken` metod.

        private void cacheUserToken(MobileServiceUser user)
        {
            SharedPreferences prefs = getSharedPreferences(SHAREDPREFFILE, Context.MODE_PRIVATE);
            Editor editor = prefs.edit();
            editor.putString(USERIDPREF, user.getUserId());
            editor.putString(TOKENPREF, user.getAuthenticationToken());
            editor.commit();
        }    

    Den här metoden lagrar hello användar-ID och token i en fil med inställningar som har markerats som privat. Detta ska skydda åtkomst toohello cache så att andra appar på hello enhet inte har toohello åtkomsttoken. hello inställningar är i begränsat läge för hello app. Om någon får åtkomst till toohello enheter är det dock möjligt att de kan få åtkomst till toohello token-cache på annat sätt.

   > [!NOTE]
   > Om token åtkomst tooyour data betraktas som känslig och någon kan få åtkomst till toohello enheter kan du skydda hello token med kryptering. En helt säker lösning ligger utanför den här självstudiekursen hello men och beror på dina säkerhetskrav.
   >
   >
4. Hej ToDoActivity.java-filen, lägga till definitionen för hello hello `loadUserTokenCache` metod.

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
5. I hello *ToDoActivity.java* fil ersätter hello `authenticate` metod med hello följande metod som använder token-cache. Ändra hello inloggningsprovidern om du vill toouse ett annat konto än Google.

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
6. Skapa hello appen och testa autentisering med hjälp av ett giltigt konto. Kör minst två gånger. Under hello först köra, bör du får en fråga toosign i och skapa hello token-cache. Därefter försöker varje körning tooload hello token-cache för autentisering. Du får inte vara nödvändiga toosign i.
