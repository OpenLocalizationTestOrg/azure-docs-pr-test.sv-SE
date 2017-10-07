---
title: "aaaUse Azure Active Directory tooauthenticate Azure Batch-tjänstelösningar | Microsoft Docs"
description: "Batch har stöd för Azure AD för autentisering från hello Batch-tjänsten."
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 06/20/2017
ms.author: tamram
ms.openlocfilehash: 6c825c30f1c80bb059a797a2e78367e599acd109
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-batch-service-solutions-with-active-directory"></a>Autentisera Batch tjänstelösningar med Active Directory

Azure Batch stöder autentisering med [Azure Active Directory] [ aad_about] (Azure AD). Azure AD är Microsofts flera innehavare molnbaserad katalog och identity management-tjänsten. Azure själva använder Azure AD tooauthenticate dess kunder, administratörer och användare i organisationer.

När du använder Azure AD-autentisering med Azure Batch kan du autentisera i ett av två sätt:

- Med hjälp av **integrerad autentisering** tooauthenticate en användare som interagerar med programmet hello. Ett program med integrerad autentisering samlar in en användares autentiseringsuppgifter och använder dessa autentiseringsuppgifter tooauthenticate åtkomst tooBatch resurser.
- Med hjälp av en **tjänstens huvudnamn** tooauthenticate en oövervakad. Ett huvudnamn för tjänsten definierar hello principer och behörigheter för ett program i ordning toorepresent hello program vid åtkomst till resurser vid körning.

toolearn mer om Azure AD finns hello [Azure Active Directory-dokumentationen](https://docs.microsoft.com/azure/active-directory/).

## <a name="authentication-and-pool-allocation-mode"></a>Läget för autentisering och pool allokering

När du skapar ett Batch-konto kan ange du där pooler för detta konto bör tilldelas. Du kan välja tooallocate pooler i hello standardabonnemang Batch-tjänsten eller i en prenumeration för användaren. Du väljer påverkar hur du autentisera åtkomst tooresources på det kontot.

- **Batch-tjänstprenumeration**. Som standard tilldelas Batch-pooler i en prenumeration på Batch-tjänsten. Om du väljer det här alternativet kan du kan autentisera åtkomst tooresources på det kontot antingen med [delad nyckel](https://docs.microsoft.com/rest/api/batchservice/authenticate-requests-to-the-azure-batch-service) eller med Azure AD.
- **Användaren prenumeration.** Du kan välja tooallocate Batch-pooler i en prenumeration för användare som du anger. Om du väljer det här alternativet måste du autentisera med Azure AD.

## <a name="endpoints-for-authentication"></a>Slutpunkter för autentisering

tooauthenticate Batch-program med Azure AD, måste tooinclude vissa välkända slutpunkter i koden.

### <a name="azure-ad-endpoint"></a>Azure AD-slutpunkt

grundläggande hello Azure AD myndighet slutpunkten är:

`https://login.microsoftonline.com/`

tooauthenticate med Azure AD, Använd den här slutpunkten tillsammans med hello klient-ID (katalog-ID). hello klient-ID identifierar hello Azure AD-klient toouse för autentisering. tooretrieve Hej klient-ID, Följ stegen i hello [hämta hello klient-ID för Azure Active Directory](#get-the-tenant-id-for-your-active-directory):

`https://login.microsoftonline.com/<tenant-id>`

> [!NOTE] 
> hello klient-specifika slutpunkt krävs när du autentiserar med hjälp av ett huvudnamn för tjänsten. 
> 
> hello klient-specifika slutpunkt är valfritt när du autentiserar med integrerad autentisering, men rekommenderas. Du kan också använda vanliga hello Azure AD-slutpunkten. hello vanliga endpoint ger en allmän referens samla in gränssnittet när en viss klient inte har angetts. vanliga hello-slutpunkten är `https://login.microsoftonline.com/common`.
>
>

Läs mer om Azure AD-slutpunkter [Autentiseringsscenarier för Azure AD][aad_auth_scenarios].

### <a name="batch-resource-endpoint"></a>Slutpunkten för batch-resurs

Använd hello **Azure Batch resurs endpoint** tooacquire en token för att autentisera begäranden toohello Batch-tjänsten:

`https://batch.core.windows.net/`

## <a name="register-your-application-with-a-tenant"></a>Registrera ditt program till en klient

hello första steget i med hjälp av Azure AD tooauthenticate registrerar ditt program i en Azure AD-klient. Registrera ditt program kan du toocall hello Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) från din kod. Hej ADAL tillhandahåller ett API för att autentisera med Azure AD från ditt program. Registrera ditt program krävs om du planerar toouse integrerad autentisering eller ett huvudnamn för tjänsten.

När du registrerar ditt program kan du ange information om ditt program tooAzure AD. Azure AD sedan innehåller ett program-ID som du använder tooassociate ditt program med Azure AD under körning. toolearn mer om hello program-ID, se [program och tjänstens huvudnamn objekt i Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).

tooregister Batch programmet, följ hello stegen i hello [lägga till ett program](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) i avsnittet [integrera program med Azure Active Directory][aad_integrate]. Om du registrerar ditt program som det ursprungliga programmet kan du ange en giltig URI för hello **omdirigerings-URI**. Det behöver inte toobe en verklig slutpunkt.

När du har registrerat ditt program, ser du hello program-ID:

![Registrera ditt Batch-program med Azure AD](./media/batch-aad-auth/app-registration-data-plane.png)

Mer information om hur du registrerar ett program med Azure AD finns [Autentiseringsscenarier för Azure AD](../active-directory/develop/active-directory-authentication-scenarios.md).

## <a name="get-hello-tenant-id-for-your-active-directory"></a>Hämta hello klient-ID för ditt Active Directory

hello klient-ID identifierar hello Azure AD-klient som tillhandahåller autentisering services tooyour-programmet. tooget Hej klient-ID, gör du följande:

1. Välj din Active Directory i hello Azure-portalen.
2. Klicka på **Egenskaper**.
3. Kopiera hello GUID-värde för hello directory-ID. Det här värdet kallas även hello klient-ID.

![Kopiera hello katalog-ID](./media/batch-aad-auth/aad-directory-id.png)


## <a name="use-integrated-authentication"></a>Använda integrerad autentisering

tooauthenticate med integrerad autentisering måste du toogrant ditt program behörighet tooconnect toohello API för Batch-tjänsten. Det här steget kan ditt program tooauthenticate anrop toohello Batch-tjänsten API med Azure AD.

När du har [registrerade programmet](#register-your-application-with-an-azure-ad-tenant), Följ dessa steg i hello Azure portal toogrant den komma åt toohello Batch-tjänsten:

1. Hello vänstra navigeringsfönstret för hello Azure-portalen och väljer **fler tjänster**, klickar du på **App registreringar**.
2. Sök efter hello namnet på ditt program i hello lista över app registreringar:

    ![Sök efter programnamnet](./media/batch-aad-auth/search-app-registration.png)

3. Öppna hello **inställningar** bladet för ditt program. I hello **API-åtkomst** väljer **nödvändiga behörigheter**.
4. I hello **nödvändiga behörigheter** bladet, klickar du på hello **Lägg till** knappen.
5. I steg 1 ska söka efter hello Batch-API. Sök efter var och en av de här strängarna tills du hittar hello-API:
    1. **MicrosoftAzureBatch**.
    2. **Microsoft Azure Batch**. Nyare Azure AD-klientorganisationer kan använda det här namnet.
    3. **ddbf3205-c6bd-46ae-8127-60eb93363864** är hello-ID för hello Batch-API. 
6. När du har hittat hello Batch-API och på hello **Välj** knappen.
6. I steg 2, Välj hello kryssrutan bredvid för**Azure Batch-tjänsten för dataåtkomst** och klicka på hello **Välj** knappen.
7. Klicka på hello **klar** knappen.

Hej **nödvändiga behörigheter** bladet nu visas som Azure AD-program har åtkomst till tooboth ADAL och hello API för Batch-tjänsten. Behörigheter tooADAL automatiskt när du först registrera din app med Azure AD.

![Bevilja API-behörigheter](./media/batch-aad-auth/required-permissions-data-plane.png)

## <a name="use-a-service-principal"></a>Använd ett huvudnamn för tjänsten 

tooauthenticate ett program som körs obevakad, Använd ett huvudnamn för tjänsten. När du har registrerat ditt program, gör du så här i hello Azure portal tooconfigure en tjänstens huvudnamn:

1. Begär en hemlig nyckel för ditt program.
2. Tilldela ett RBAC rollen tooyour program.

### <a name="request-a-secret-key-for-your-application"></a>Begär en hemlig nyckel för ditt program

När programmet autentiseras med ett huvudnamn för tjänsten skickar den både hello program-ID och hemlig nyckel tooAzure AD. Du behöver toocreate och kopiera hello hemlig nyckel toouse från din kod.

Följ anvisningarna i hello Azure-portalen:

1. Hello vänstra navigeringsfönstret för hello Azure-portalen och väljer **fler tjänster**, klickar du på **App registreringar**.
2. Sök efter hello namnet på ditt program i hello lista över app registreringar.
3. Visa hello **inställningar** bladet. I hello **API-åtkomst** väljer **nycklar**.
4. toocreate en nyckel, ange en beskrivning för hello nyckeln. Välj sedan en varaktighet för hello nyckeln för en eller två år. 
5. Klicka på hello **spara** knappen toocreate och visa hello nyckel. Kopiera hello nyckelvärdet tooa säker plats som du inte kan tooaccess den igen när du lämnar hello-bladet. 

    ![Skapa en hemlig nyckel](./media/batch-aad-auth/secret-key.png)

### <a name="assign-an-rbac-role-tooyour-application"></a>Tilldela ett RBAC rollen tooyour program

tooauthenticate med ett huvudnamn för tjänsten måste tooassign ett RBAC rollen tooyour program. Följ de här stegen:

1. Navigera toohello Batch-kontot som används av programmet hello Azure-portalen.
2. I hello **inställningar** bladet för hello Batch-kontot, Välj **Access Control (IAM)**.
3. Klicka på hello **Lägg till** knappen. 
4. Från hello **rollen** listrutan, Välj antingen hello _deltagare_ eller _Reader_ roll för ditt program. Mer information om dessa roller finns [Kom igång med rollbaserad åtkomstkontroll i hello Azure-portalen](../active-directory/role-based-access-control-what-is.md).  
5. I hello **Välj** anger hello namnet på ditt program. Välj programmet hello listan och klickar på **spara**.

Ditt program bör nu visas i dina inställningar för åtkomstkontroll med en RBAC-roll som tilldelats. 

![Tilldela ett RBAC rollen tooyour program](./media/batch-aad-auth/app-rbac-role.png)

### <a name="get-hello-tenant-id-for-your-azure-active-directory"></a>Hämta hello klient-ID för Azure Active Directory

hello klient-ID identifierar hello Azure AD-klient som tillhandahåller autentisering services tooyour-programmet. tooget Hej klient-ID, gör du följande:

1. Välj din Active Directory i hello Azure-portalen.
2. Klicka på **Egenskaper**.
3. Kopiera hello GUID-värde för hello directory-ID. Det här värdet kallas även hello klient-ID.

![Kopiera hello katalog-ID](./media/batch-aad-auth/aad-directory-id.png)


## <a name="code-examples"></a>Kodexempel

hello kodexempel i det här avsnittet visar hur tooauthenticate med hjälp av Azure AD-integrerad autentisering och med ett huvudnamn för tjänsten. Dessa kodexempel använda .NET, men hello begrepp är liknande för andra språk.

> [!NOTE]
> En Azure AD authentication token upphör att gälla efter en timme. När du använder en långlivade **BatchClient** objekt, rekommenderar vi att du hämtar en token från ADAL på varje begäran tooensure du alltid har en giltig token. 
>
>
> tooachieve detta i .NET, skriva en metod som hämtar hello-token från Azure AD och överför den metoden tooa **BatchTokenCredentials** objektet som ett ombud. hello ombud metoden anropas för varje begäran toohello Batch-tjänsten tooensure som en giltig token har angetts. Som standard cachelagrar ADAL-token så att en ny token hämtas från Azure AD bara när det behövs. Mer information om token i Azure AD finns [Autentiseringsscenarier för Azure AD][aad_auth_scenarios].
>
>

### <a name="code-example-using-azure-ad-integrated-authentication-with-batch-net"></a>Exempel: använda Azure AD-integrerad autentisering med Batch .NET

tooauthenticate med integrerad autentisering från Batch .NET referens hello [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) paket- och hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) paketet.

Inkludera hello följande `using` instruktioner i koden:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Referens hello Azure AD-slutpunkt i din kod, inklusive hello klient-ID. tooretrieve Hej klient-ID, Följ stegen i hello [hämta hello klient-ID för Azure Active Directory](#get-the-tenant-id-for-your-active-directory):

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Referera till hello Batch tjänstslutpunkten resurs:

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Referera till Batch-kontot:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Ange hello program-ID (klient-ID) för ditt program. hello program-ID är tillgänglig från din appregistrering i hello Azure-portalen:

```csharp
private const string ClientId = "<application-id>";
```

Också kopiera hello omdirigerings-URI som du angav under hello registreringsprocessen. Hej omdirigerings-URI som anges i din kod måste matcha hello omdirigerings-URI som du angav när du registrerade programmet hello:

```csharp
private const string RedirectUri = "http://mybatchdatasample";
```

Skriva ett återanrop metoden tooacquire hello autentiseringstoken från Azure AD. Hej **GetAuthenticationTokenAsync** Återanropsmetoden som visas här anropar ADAL tooauthenticate när en användare interagerar med programmet hello. Hej **AcquireTokenAsync** metoden tillhandahålls av ADAL prompter hello användaren sina autentiseringsuppgifter och hello programmet fortsätter när hello användare ger dem (om det redan har cachelagrade autentiseringsuppgifter):

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    var authContext = new AuthenticationContext(AuthorityUri);

    // Acquire hello authentication token from Azure AD.
    var authResult = await authContext.AcquireTokenAsync(BatchResourceUri, 
                                                        ClientId, 
                                                        new Uri(RedirectUri), 
                                                        new PlatformParameters(PromptBehavior.Auto));

    return authResult.AccessToken;
}
```

Skapa en **BatchTokenCredentials** objekt som tar hello delegat som en parameter. Använd dessa autentiseringsuppgifter tooopen en **BatchClient** objekt. Du kan använda som **BatchClient** objekt för efterföljande åtgärder mot hello Batch-tjänsten:

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

### <a name="code-example-using-an-azure-ad-service-principal-with-batch-net"></a>Exempel: med hjälp av en Azure AD-tjänstens huvudnamn med Batch .NET

tooauthenticate med ett huvudnamn för tjänsten från Batch .NET referens hello [Azure Batch .NET](https://www.nuget.org/packages/Azure.Batch/) paket- och hello [ADAL](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) paketet.

Inkludera hello följande `using` instruktioner i koden:

```csharp
using Microsoft.Azure.Batch;
using Microsoft.Azure.Batch.Auth;
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Referens hello Azure AD-slutpunkt i din kod, inklusive hello klient-ID. Du måste ange en slutpunkt för klienten när du använder ett huvudnamn för tjänsten. tooretrieve Hej klient-ID, Följ stegen i hello [hämta hello klient-ID för Azure Active Directory](#get-the-tenant-id-for-your-active-directory):

```csharp
private const string AuthorityUri = "https://login.microsoftonline.com/<tenant-id>";
```

Referera till hello Batch tjänstslutpunkten resurs:  

```csharp
private const string BatchResourceUri = "https://batch.core.windows.net/";
```

Referera till Batch-kontot:

```csharp
private const string BatchAccountUrl = "https://myaccount.mylocation.batch.azure.com";
```

Ange hello program-ID (klient-ID) för ditt program. hello program-ID är tillgänglig från din appregistrering i hello Azure-portalen:

```csharp
private const string ClientId = "<application-id>";
```

Ange hello hemlig nyckel som du kopierade från hello Azure-portalen:

```csharp
private const string ClientKey = "<secret-key>";
```

Skriva ett återanrop metoden tooacquire hello autentiseringstoken från Azure AD. Hej **GetAuthenticationTokenAsync** Återanropsmetoden som visas här anrop ADAL för obevakad autentisering:

```csharp
public static async Task<string> GetAuthenticationTokenAsync()
{
    AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
    AuthenticationResult authResult = await authContext.AcquireTokenAsync(BatchResourceUri, new ClientCredential(ClientId, ClientKey));

    return authResult.AccessToken;
}
```

Skapa en **BatchTokenCredentials** objekt som tar hello delegat som en parameter. Använd dessa autentiseringsuppgifter tooopen en **BatchClient** objekt. Du kan sedan använda **BatchClient** objekt för efterföljande åtgärder mot hello Batch-tjänsten:

```csharp
public static async Task PerformBatchOperations()
{
    Func<Task<string>> tokenProvider = () => GetAuthenticationTokenAsync();

    using (var client = await BatchClient.OpenAsync(new BatchTokenCredentials(BatchAccountUrl, tokenProvider)))
    {
        await client.JobOperations.ListJobs().ToListAsync();
    }
}
```

## <a name="next-steps"></a>Nästa steg

toolearn mer om Azure AD finns hello [Azure Active Directory-dokumentationen](https://docs.microsoft.com/azure/active-directory/). Djupgående exempel som visar hur toouse ADAL är tillgängliga i hello [Azure kodexempel](https://azure.microsoft.com/resources/samples/?service=active-directory) bibliotek.

toolearn mer om tjänstens huvudnamn finns [program och tjänstens huvudnamn objekt i Azure Active Directory](../active-directory/develop/active-directory-application-objects.md). toocreate en tjänstens huvudnamn med hjälp av hello Azure portal, se [använda portalen toocreate Active Directory-program och tjänstens huvudnamn som kan komma åt resurser](../resource-group-create-service-principal-portal.md). Du kan också skapa ett huvudnamn för tjänsten med PowerShell eller Azure CLI. 

tooauthenticate Batch-hantering av program med Azure AD finns [autentiserar Batch hanteringslösningar med Active Directory](batch-aad-auth-management.md). 

[aad_about]: ../active-directory/active-directory-whatis.md "Vad är Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Autentiseringsscenarier för Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrera program med Azure Active Directory"
[azure_portal]: http://portal.azure.com
