
1. <span data-ttu-id="21168-101">Hej MainPage.xaml.cs projektfilen, lägga till följande hello **med** instruktioner:</span><span class="sxs-lookup"><span data-stu-id="21168-101">In hello MainPage.xaml.cs project file, add hello following **using** statements:</span></span>
   
        using System.Linq;        
        using Windows.Security.Credentials;
2. <span data-ttu-id="21168-102">Ersätt hello **AuthenticateAsync** metod med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="21168-102">Replace hello **AuthenticateAsync** method with hello following code:</span></span>
   
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
   
    <span data-ttu-id="21168-103">I den här versionen av **AuthenticateAsync**, hello app försöker toouse autentiseringsuppgifter lagras i hello **PasswordVault** tooaccess hello service.</span><span class="sxs-lookup"><span data-stu-id="21168-103">In this version of **AuthenticateAsync**, hello app tries toouse credentials stored in hello **PasswordVault** tooaccess hello service.</span></span> <span data-ttu-id="21168-104">En vanlig inloggning också utförs när det finns inga lagrade autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="21168-104">A regular sign-in is also performed when there is no stored credential.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="21168-105">En cachelagrad token kan ha gått och token upphör att gälla kan även uppstå efter autentisering när hello appen används.</span><span class="sxs-lookup"><span data-stu-id="21168-105">A cached token may be expired, and token expiration can also occur after authentication when hello app is in use.</span></span> <span data-ttu-id="21168-106">hur toodetermine om en token har upphört att gälla, se toolearn [söka efter utgångna autentiseringstoken](http://aka.ms/jww5vp).</span><span class="sxs-lookup"><span data-stu-id="21168-106">toolearn how toodetermine if a token is expired, see [Check for expired authentication tokens](http://aka.ms/jww5vp).</span></span> <span data-ttu-id="21168-107">En lösning toohandling auktorisering fel relaterade tooexpiring token finns hello efter [cachelagring och hantering av utgångna token i Azure Mobile Services hanteras SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx).</span><span class="sxs-lookup"><span data-stu-id="21168-107">For a solution toohandling authorization errors related tooexpiring tokens, see hello post [Caching and handling expired tokens in Azure Mobile Services managed SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx).</span></span> 
   > 
   > 
3. <span data-ttu-id="21168-108">Starta om hello app två gånger.</span><span class="sxs-lookup"><span data-stu-id="21168-108">Restart hello app twice.</span></span>
   
    <span data-ttu-id="21168-109">Observera att på hello första start, logga in med hello-providern krävs igen.</span><span class="sxs-lookup"><span data-stu-id="21168-109">Notice that on hello first start-up, sign-in with hello provider is again required.</span></span> <span data-ttu-id="21168-110">Men på hello andra omstarten hello cachelagrade autentiseringsuppgifter som ska användas och logga in kringgås.</span><span class="sxs-lookup"><span data-stu-id="21168-110">However, on hello second restart hello cached credentials are used and sign-in is bypassed.</span></span> 

