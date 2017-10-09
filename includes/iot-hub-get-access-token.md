## <a name="obtain-an-azure-resource-manager-token"></a>Hämta en Azure Resource Manager-token
Azure Active Directory måste autentisera alla hello-aktiviteter som du utför på resurser med hjälp av hello Azure Resource Manager. hello här exemplet använder lösenordsautentisering, andra metoder finns [autentisera Azure Resource Manager begär][lnk-authenticate-arm].

1. Lägg till följande kod toohello hello **Main** metod i Program.cs tooretrieve en token från Azure AD med hello program-id och lösenord.
   
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
2. Skapa en **ResourceManagementClient** objekt som använder hello token genom att lägga till hello efter koden toohello hello **Main** metoden:
   
    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```
3. Skapar eller hämtar en referens till hello resursgrupp som du använder:
   
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