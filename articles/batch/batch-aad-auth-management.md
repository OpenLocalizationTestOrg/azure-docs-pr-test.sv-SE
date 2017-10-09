---
title: "aaaUse Azure Active Directory tooauthenticate Batch lösningar | Microsoft Docs"
description: Program som skapats med Azure resource manager och hello Batch-resursprovidern autentisera med Azure AD.
services: batch
documentationcenter: .net
author: tamram
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: batch
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: big-compute
ms.date: 04/27/2017
ms.author: tamram
ms.openlocfilehash: 192aa9f8d7cbfc0282a4a0c33ab1659f1f351525
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="authenticate-batch-management-solutions-with-active-directory"></a>Autentisera Batch hanteringslösningar med Active Directory

Program som anropar hello Azure Batch Management-tjänsten autentiseras med [Azure Active Directory] [ aad_about] (Azure AD). Azure AD är Microsofts flera innehavare molnbaserad katalog och identity management-tjänsten. Azure själva använder Azure AD för hello autentisering för sina kunder, administratörer och organisationens användare.

hello Batch Management .NET-biblioteket visar typer för att arbeta med Batch-konton, nycklar, program och programpaket. hello Batch Management .NET-biblioteket är en Azure resource provider-klient och används tillsammans med [Azure Resource Manager] [ resman_overview] toomanage dessa resurser via programmering. Azure AD är obligatoriska tooauthenticate begäranden som görs via en Azure resource provider klienten, inklusive hello Batch Management .NET-biblioteket, och via [Azure Resource Manager][resman_overview].

I den här artikeln förklarar vi med hjälp av Azure AD tooauthenticate från program som använder hello Batch Management .NET-biblioteket. Visar vi hur toouse Azure AD tooauthenticate en prenumeration administratör eller medadministratör, med hjälp av integrerad autentisering. Vi använder hello [AccountManagment] [ acct_mgmt_sample] exempelprojektet finns på GitHub, toowalk genom att använda Azure AD med hello Batch Management .NET-biblioteket.

toolearn mer information om hur du använder hello Batch Management .NET-bibliotek och exempelapp för hello AccountManagement, se [hantera Batch-konton och kvoter med hello Batch Management-klientbibliotek för .NET](batch-management-dotnet.md).

## <a name="register-your-application-with-azure-ad"></a>Registrera ditt program med Azure AD

hello Azure [Active Directory Authentication Library] [ aad_adal] (ADAL) tillhandahåller ett programmeringsgränssnitt tooAzure AD för användning i dina program. toocall ADAL från ditt program måste du registrera ditt program i en Azure AD-klient. När du registrerar ditt program kan ange du Azure AD med information om ditt program, inklusive ett namn för den inom hello Azure AD-klient. Azure AD sedan innehåller ett program-ID som du använder tooassociate ditt program med Azure AD under körning. toolearn mer om hello program-ID, se [program och tjänstens huvudnamn objekt i Azure Active Directory](../active-directory/develop/active-directory-application-objects.md).

tooregister hello AccountManagement exempelprogrammet åtgärderna hello i hello [lägga till ett program](../active-directory/develop/active-directory-integrating-applications.md#adding-an-application) i avsnittet [integrera program med Azure Active Directory] [ aad_integrate]. Ange **internt klientprogram** för hello typ av program. Hej industry standard OAuth 2.0-URI för hello **omdirigerings-URI** är `urn:ietf:wg:oauth:2.0:oob`. Du kan dock ange en giltig URI (t.ex `http://myaccountmanagementsample`) för hello **omdirigerings-URI**eftersom det inte behöver toobe verkliga endpoint:

![](./media/batch-aad-auth-management/app-registration-management-plane.png)

När du har slutfört registreringsprocessen hello visas hello program-ID samt hello objekt (tjänstens huvudnamn)-ID som anges för ditt program.  

![](./media/batch-aad-auth-management/app-registration-client-id.png)

## <a name="grant-hello-azure-resource-manager-api-access-tooyour-application"></a>Bevilja hello Azure Resource Manager API åtkomst tooyour program

Därefter behöver du toodelegate åtkomst tooyour programmet toohello Azure Resource Manager API. hello Azure AD-ID för hello Resource Manager API är **Windows Azure Service Management API**.

Följ anvisningarna i hello Azure-portalen:

1. Hello vänstra navigeringsfönstret för hello Azure-portalen och väljer **fler tjänster**, klickar du på **App registreringar**, och klicka på **Lägg till**.
2. Sök efter hello namnet på ditt program i hello lista över app registreringar:

    ![Sök efter programnamnet](./media/batch-aad-auth-management/search-app-registration.png)

3. Visa hello **inställningar** bladet. I hello **API-åtkomst** väljer **nödvändiga behörigheter**.
4. Klicka på **Lägg till** tooadd en ny nödvändig behörighet. 
5. I steg 1, ange **Windows Azure Service Management API**, Välj den API hello listan över resultat och på hello **Välj** knappen.
6. I steg 2, Välj hello kryssrutan bredvid för**åtkomst Azure klassiska distributionsmodellen som organisationen användare**, och klicka på hello **Välj** knappen.
7. Klicka på hello **klar** knappen.

Hej **nödvändiga behörigheter** bladet nu visar att tooboth beviljas behörighet tooyour programmet hello ADAL och Resource Manager API: er. Behörigheter tooADAL som standard när du först registrera din app med Azure AD.

![Delegera behörigheter toohello Azure Resource Manager API](./media/batch-aad-auth-management/required-permissions-management-plane.png)

## <a name="azure-ad-endpoints"></a>Azure AD-slutpunkter

tooauthenticate Batch Management-lösningar med Azure AD, behöver du två välkända slutpunkter.

- Hej **vanliga Azure AD-slutpunkt** ger en allmän referens samla in gränssnittet när en viss klient inte har angetts som hello fallet med integrerad autentisering:

    `https://login.microsoftonline.com/common`

- Hej **Azure Resource Manager-slutpunkt** är används tooacquire en token för att autentisera begäranden toohello Batch management-tjänsten:

    `https://management.core.windows.net/`

Hej AccountManagement exempelprogrammet definierar konstanter för dessa slutpunkter. Lämna dessa konstanter oförändrade:

```csharp
// Azure Active Directory "common" endpoint.
private const string AuthorityUri = "https://login.microsoftonline.com/common";
// Azure Resource Manager endpoint 
private const string ResourceUri = "https://management.core.windows.net/";
```

## <a name="reference-your-application-id"></a>Referens-program-ID 

Klientprogrammet använder hello program-ID (även hänvisade tooas hello klient-ID) tooaccess Azure AD under körning. När du har registrerat ditt program i hello Azure-portalen, uppdatera din kod toouse hello program-ID som tillhandahålls av Azure AD för registrerade programmet. Kopiera program-ID från hello Azure portal toohello lämplig konstant i hello AccountManagement exempelprogrammet:

```csharp
// Specify hello unique identifier (hello "Client ID") for your application. This is required so that your
// native client application (i.e. this sample) can access hello Microsoft Azure AD Graph API. For information
// about registering an application in Azure Active Directory, please see "Adding an Application" here:
// https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
private const string ClientId = "<application-id>";
```
Också kopiera hello omdirigerings-URI som du angav under hello registreringsprocessen. hello omdirigerings-URI som angetts i din kod måste matcha hello omdirigerings-URI som du angav när du registrerade hello program.

```csharp
// hello URI toowhich Azure AD will redirect in response tooan OAuth 2.0 request. This value is
// specified by you when you register an application with AAD (see ClientId comment). It does not
// need toobe a real endpoint, but must be a valid URI (e.g. https://accountmgmtsampleapp).
private const string RedirectUri = "http://myaccountmanagementsample";
```

## <a name="acquire-an-azure-ad-authentication-token"></a>Skaffa en Azure AD-token för autentisering

När du registrerar hello AccountManagement exemplet i hello Azure AD-klient och uppdatera hello exempelkod källa med värdena är hello prov klar tooauthenticate med hjälp av Azure AD. När du kör hello exempel försöker hello ADAL tooacquire en autentiseringstoken. I det här steget ombeds du ange autentiseringsuppgifterna för ditt Microsoft: 

```csharp
// Obtain an access token using hello "common" AAD resource. This allows hello application
// tooquery AAD for information that lies outside hello application's tenant (such as for
// querying subscription information in your Azure account).
AuthenticationContext authContext = new AuthenticationContext(AuthorityUri);
AuthenticationResult authResult = authContext.AcquireToken(ResourceUri,
                                                        ClientId,
                                                        new Uri(RedirectUri),
                                                        PromptBehavior.Auto);
```

När du har angett dina autentiseringsuppgifter fortsätta hello exempelprogrammet tooissue autentiserade begäranden toohello Batch management-tjänsten. 

## <a name="next-steps"></a>Nästa steg

Mer information om hur du kör hello [AccountManagement exempelprogrammet][acct_mgmt_sample], se [hantera Batch-konton och kvoter med hello Batch Management-klientbibliotek för .NET](batch-management-dotnet.md).

toolearn mer om Azure AD finns hello [Azure Active Directory-dokumentationen](https://docs.microsoft.com/azure/active-directory/). Djupgående exempel som visar hur toouse ADAL är tillgängliga i hello [Azure kodexempel](https://azure.microsoft.com/resources/samples/?service=active-directory) bibliotek.

tooauthenticate Batch-tjänstprogram med Azure AD finns [autentiserar Batch tjänstelösningar med Active Directory](batch-aad-auth.md). 


[aad_about]: ../active-directory/active-directory-whatis.md "Vad är Azure Active Directory?"
[aad_adal]: ../active-directory/active-directory-authentication-libraries.md
[aad_auth_scenarios]: ../active-directory/active-directory-authentication-scenarios.md "Autentiseringsscenarier för Azure AD"
[aad_integrate]: ../active-directory/active-directory-integrating-applications.md "Integrera program med Azure Active Directory"
[acct_mgmt_sample]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/AccountManagement
[azure_portal]: http://portal.azure.com
[resman_overview]: ../azure-resource-manager/resource-group-overview.md
