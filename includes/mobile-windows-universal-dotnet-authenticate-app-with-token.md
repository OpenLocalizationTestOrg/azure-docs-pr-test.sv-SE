
1. Hej MainPage.xaml.cs projektfilen, lägga till följande hello **med** instruktioner:
   
        using System.Linq;        
        using Windows.Security.Credentials;
2. Ersätt hello **AuthenticateAsync** metod med hello följande kod:
   
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
   
            // This sample uses hello Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;
   
            // Use hello PasswordVault toosecurely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;
   
            try
            {
                // Try tooget an existing credential from hello vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }
   
            if (credential != null)
            {
                // Create a user from hello stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;
   
                // Set hello user from hello stored credentials.
                App.MobileService.CurrentUser = user;
   
                // Consider adding a check toodetermine if hello token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.
   
                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with hello identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);
   
                    // Create and store hello user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);
   
                    success = true;
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
   
            return success;
        }
   
    I den här versionen av **AuthenticateAsync**, hello app försöker toouse autentiseringsuppgifter lagras i hello **PasswordVault** tooaccess hello service. En vanlig inloggning också utförs när det finns inga lagrade autentiseringsuppgifter.
   
   > [!NOTE]
   > En cachelagrad token kan ha gått och token upphör att gälla kan även uppstå efter autentisering när hello appen används. hur toodetermine om en token har upphört att gälla, se toolearn [söka efter utgångna autentiseringstoken](http://aka.ms/jww5vp). En lösning toohandling auktorisering fel relaterade tooexpiring token finns hello efter [cachelagring och hantering av utgångna token i Azure Mobile Services hanteras SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx). 
   > 
   > 
3. Starta om hello app två gånger.
   
    Observera att på hello första start, logga in med hello-providern krävs igen. Men på hello andra omstarten hello cachelagrade autentiseringsuppgifter som ska användas och logga in kringgås. 

