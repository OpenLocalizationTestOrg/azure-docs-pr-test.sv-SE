## <a name="obtain-an-azure-resource-manager-token"></a><span data-ttu-id="7589a-101">Hämta en Azure Resource Manager-token</span><span class="sxs-lookup"><span data-stu-id="7589a-101">Obtain an Azure Resource Manager token</span></span>
<span data-ttu-id="7589a-102">Azure Active Directory måste autentisera alla hello-aktiviteter som du utför på resurser med hjälp av hello Azure Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="7589a-102">Azure Active Directory must authenticate all hello tasks that you perform on resources using hello Azure Resource Manager.</span></span> <span data-ttu-id="7589a-103">hello här exemplet använder lösenordsautentisering, andra metoder finns [autentisera Azure Resource Manager begär][lnk-authenticate-arm].</span><span class="sxs-lookup"><span data-stu-id="7589a-103">hello example shown here uses password authentication, for other approaches see [Authenticating Azure Resource Manager requests][lnk-authenticate-arm].</span></span>

1. <span data-ttu-id="7589a-104">Lägg till följande kod toohello hello **Main** metod i Program.cs tooretrieve en token från Azure AD med hello program-id och lösenord.</span><span class="sxs-lookup"><span data-stu-id="7589a-104">Add hello following code toohello **Main** method in Program.cs tooretrieve a token from Azure AD using hello application id and password.</span></span>
   
    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.microsoftonline.com/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
   
    if (token == null)
    {
      Console.WriteLine("Failed tooobtain hello token");
      return;
    }
    ```
2. <span data-ttu-id="7589a-105">Skapa en **ResourceManagementClient** objekt som använder hello token genom att lägga till hello efter koden toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="7589a-105">Create a **ResourceManagementClient** object that uses hello token by adding hello following code toohello end of hello **Main** method:</span></span>
   
    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```
3. <span data-ttu-id="7589a-106">Skapar eller hämtar en referens till hello resursgrupp som du använder:</span><span class="sxs-lookup"><span data-stu-id="7589a-106">Create, or obtain a reference to, hello resource group you are using:</span></span>
   
    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx