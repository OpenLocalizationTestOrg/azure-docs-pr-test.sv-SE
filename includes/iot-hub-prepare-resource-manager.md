## <a name="prepare-tooauthenticate-azure-resource-manager-requests"></a>Förbereda tooauthenticate Azure Resource Manager-begäranden
Du måste autentisera alla hello-åtgärder som du utför på resurser med hjälp av hello [Azure Resource Manager] [ lnk-authenticate-arm] med Azure Active Directory (AD). Hej enklaste sättet tooconfigure toouse PowerShell eller Azure CLI.

Installera hello [Azure PowerShell-cmdlets] [ lnk-powershell-install] innan du fortsätter.

Hej följande steg visar hur tooset in lösenordsautentisering för ett AD-program med hjälp av PowerShell. Du kan köra dessa kommandon i en standard PowerShell-session.

1. Logga in tooyour Azure-prenumeration med hjälp av hello följande kommando:

    ```powershell
    Login-AzureRmAccount
    ```

1. Om du har flera Azure-prenumerationer, hello logga in tooAzure ger du åtkomst till tooall Azure-prenumerationer som är kopplade till dina autentiseringsuppgifter. Använd följande kommando toolist hello Azure-prenumerationer tillgängliga för dig toouse hello:

    ```powershell
    Get-AzureRMSubscription
    ```

    Använd följande kommando tooselect prenumeration som du vill toouse toorun hello kommandon toomanage din IoT-hubb hello. Du kan använda hello namn eller ID från hello utdata från hello föregående kommando:

    ```powershell
    Select-AzureRMSubscription `
        -SubscriptionName "{your subscription name}"
    ```

2. Anteckna din **TenantId** och **SubscriptionId**. Du behöver dem senare.
3. Skapa ett nytt Azure Active Directory-program med hjälp av hello följande kommando, där du ersätter hello platshållare:
   
   * **{Visningsnamn}:** ett visningsnamn för tillämpningsprogrammet som **MySampleApp**
   * **{URL-Adressen}:** hello URL för hello startsidan för din app som **http://mysampleapp/home**. Denna URL behöver inte toopoint tooa verkliga program.
   * **{Programidentifierare}:** en unik identifierare som **http://mysampleapp**. Denna URL behöver inte toopoint tooa verkliga program.
   * **{Lösenord}:** ett lösenord som du använder tooauthenticate med din app.
     
     ```powershell
     New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
     ```
4. Anteckna hello **ApplicationId** av hello-programmet som du skapade. Du behöver detta senare.
5. Skapa ett nytt huvudnamn för tjänsten med hjälp av hello följande kommando, där du ersätter **{MyApplicationId}** med hello **ApplicationId** hello föregående steg:
   
    ```powershell
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
6. Konfigurera en rolltilldelning med hello följande kommando, där du ersätter **{MyApplicationId}** med din **ApplicationId**.
   
    ```powershell
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```

Du är nu klar att skapa hello Azure AD-program som du kan använda tooauthenticate från din anpassade C#-program. Hello följande värden senare i den här kursen behöver du:

* Klient-ID
* SubscriptionId
* ApplicationId
* Lösenord

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps
