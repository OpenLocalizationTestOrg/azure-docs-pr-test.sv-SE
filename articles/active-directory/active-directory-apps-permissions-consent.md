---
title: "Appar, behörigheter och godkännande i Azure Active Directory.| Microsoft Docs"
description: "Azure AD Connect integrerar dina lokala kataloger med Azure Active Directory. Detta gör att du tooprovide en gemensam identitet för Office 365 och Azure SaaS-program som är integrerade med Azure AD."
keywords: "Introduktion tooAzure AD, appar, vad är Azure AD Connect, installera active directory"
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.assetid: 25897cc4-7687-49b6-b0d5-71f51302b6b1
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/31/2017
ms.author: billmath
ms.reviewer: jesakowi
ms.custom: oldportal;it-pro;
ms.openlocfilehash: af0c2669199736fdb41e85876a7e3a7064e80770
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="apps-permissions-and-consent-in-azure-active-directory"></a>Appar, behörigheter och godkännande i Azure Active Directory
Du kan lägga till program tooyour katalog i Azure Active Directory.  hello program kan variera beroende på hello typ av program.  tooview program i hello klassiska portal, väljer du en katalog och Välj program.

![](media/active-directory-apps-permissions-consent/apps1.png)

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln.

## <a name="types-of-apps"></a>Typer av appar

1. **Appar för en enda klient** </br>
    - **Stöd för en innehavare appar** -ofta kallas tooas branschspecifika (LOB)-appar. Detta gäller hello där någon inom din organisation utvecklar egna app och vill att användarna i hello organisation toobe kan toosign i toohello app.
    ![](media/active-directory-apps-permissions-consent/apps2.png)
    - **Appen Proxy appar** – när du exponera ett lokalt program med Azure AD App Proxy en enskild klient app har registrerats i din klient (i tillägget toohello tjänsten App Proxy). Den här appen är det som representerar ditt lokala program i alla molninteraktioner (till exempel autentisering). (App Proxy kräver Azure AD Basic eller högre.)


2. **Appar för flera klienter**
    - **Flera innehavare appar som andra kan godkänna** - liknande för ”stöd för en innehavare appar som din organisation utvecklar”. hello största skillnaden (förutom hello logiken i själva hello appen) är att användare från andra klienter också kan godkänna tooand inloggning toohello app.</br>
    ![](media/active-directory-apps-permissions-consent/apps4.png)
    - **Appar för flera klienter som andra utvecklar, som Contoso kan godkänna**. (Eller bara ”godkända appar”.) Detta är hello Vänd sida av ”flera innehavare appar som din organisation utvecklar”. När en annan organisation utvecklar en app för flera innehavare, användare av din organisation medgivande toohello appen och logga in tooit.
    - **Första parts Microsoft-appar** – Appar som representerar Microsoft-tjänster. Medgivande drivs av hello fakta som du registrerar dig för hello-tjänsten. Det finns ibland särskilda UX och logik för vissa appar från första part som används ofta när principer runt åtkomst toohello app.</br>
    ![](media/active-directory-apps-permissions-consent/apps3.png)
    - **Appar förintegrerade** -appar som är tillgängliga i hello Azure AD App-galleri som du kan lägga till tooyour directory tooprovide enkel inloggning (och i vissa fall kan etablera) toopopular SaaS-appar.
    - **Enkel inloggning i Azure AD**: ”Verklig” enkel inloggning för appar som kan integreras med Azure AD via ett inloggningsprotokoll som stöds, t.ex. SAML 2.0 eller OpenID Connect. hello guiden vägleder dig genom att ställa in.
    - **Lösenord för enkel inloggning**: Azure AD lagras på ett säkert sätt hello användarens autentiseringsuppgifter för hello app och hello autentiseringsuppgifter är ”matas in” i hello inloggning form av hello webbläsartillägget för Azure AD App-åtkomst. Kallas även för ”lösenordsvalv”.

## <a name="permissions"></a>Behörigheter

När en app har registrerats definierar hello-användaren som utför hello appregistrering (det vill säga hello developer) vilka behörigheter hello program behöver tillgång till och vilka resurser. (hello resurser är själva, definierad som andra appar.) Någon skapar en e-post reader-appen skulle till exempel tillstånd att appen kräver hello ”komma åt postlådor som hello inloggade användare” behörighet i hello ”Office 365 Exchange Online” resurs:
    
![](media/active-directory-apps-permissions-consent/apps6.png)

För att en app (hello klient) toorequest vissa behörigheter från en annan app (hello resurs) definierar hello utvecklare av hello resurs app hello behörigheter som finns. I vårt exempel Microsoft hello ägare hello ”Office 365 Exchange Online” resurs app har definierat en behörighetsgrupp med namnet ”komma åt postlådor som hello inloggade användare”.

När du definierar behörigheter måste hello apputvecklaren definiera om hello behörighet kan vara godkänt för, eller om det krävs admin medgivande. Detta gör att utvecklare tooallow användare tooconsent på sina egna tooapps begär endast Låg känslighet behörigheter, men kräver administratörer tooconsent toomore känsliga behörigheter. Till exempel Hej ”Azure Active Directory” resurs app, har definierats, så att användarna kan godkänna tooapps, begär begränsad läsbehörighet.  För fullständig läsbehörighet och skrivbehörighet krävs administratörens godkännande.

Eftersom interna klienter inte autentiseras kan en app som definierats som en intern klientapp endast begära delegerade behörigheter. Det innebär att det alltid måste finnas en verklig användare när en token begärs. Webbappar och webb-API:er (konfidentiella klienter) måste alltid autentisera med Azure AD när en åtkomsttoken begärs. Vilket innebär att de har också hello möjlighet att begära appen endast behörigheter. Om till exempel en backend-tjänst måste tooauthenticate tooanother backend-tjänst. Program som begär appspecifika behörigheter kräver alltid administratörens godkännande.

Sammanfattningsvis:



- En app (klient) anger hello behörigheter för andra appar (resurser).
- En app (resurs) anger vilka behörigheter som är exponerade tooother appar (klienter).
- En behörighet kan vara en appspecifik behörighet, eller en delegerad behörighet.
- En delegerad behörighet kan märkas som ”tillåter användargodkännande” eller ”kräver administratörsgodkännande”.
- En app kan fungera som en klient (genom att fastställa att den behöver behörigheter tooa resurs), som en resurs (genom att fastställa vilka behörigheter som den exponerar) eller båda.

## <a name="controls"></a>Kontroller

hello följer en lista över hello olika admin-kontroller som är tillgängliga för det här problemet. Hej administratör kontroller kan användas i hello klassiska portalen från konfigurera hello-katalogen.

![](media/active-directory-apps-permissions-consent/apps7.png)

I hello Azure portal under **hantera**, **användarinställningar**.

![](media/active-directory-apps-permissions-consent/apps11.png)



- Du kan styra om användare kan godkänna tooapps:

I hello klassiska portal, väljer **användare kan ge program behörigheter tooaccess sina data.**
![](media/active-directory-apps-permissions-consent/apps8.png)

Välj i hello Azure-portalen, **användare kan låta appar tooaccess sina data**.
![](media/active-directory-apps-permissions-consent/apps12.png)



- Du kan styra om användare kan registrera sina egna LOB-appar med stöd för en innehavare: hello klassiska portal Välj **användare kan lägga till integrerade program.**
![](media/active-directory-apps-permissions-consent/apps9.png)

Välj i hello Azure-portalen, **användare kan låta appar tooaccess sina data**.
![](media/active-directory-apps-permissions-consent/apps13.png)

>[!NOTE]
>Även om du tillåter användare tooregister stöd för en innehavare LOB-appar, finns gränser toowhat kan registreras.  
>Ett exempel är utvecklare som inte är katalogadministratörer.
>
>- Användare kan inte göra en app för en enda klient till en app för flera klienter.
>- När du registrerar en klient LOB-appar kan användarna begära behörigheter endast appen tooother appar.
>- När du registrerar en klient LOB-appar kan användarna begära delegerade behörigheter tooother appar om de behörigheterna som kräver godkännande av administratören.
>- Användare kan inte göra ändringar tooapps som de inte är ägare till.



- Du kan styra om användare kan lägga till förintegrerade appar som använder lösenord för enkel inloggning (SSO eller lösenordsvalv) ![](media/active-directory-apps-permissions-consent/apps10.png)



- Du kan styra när program kan nås, så kallad ”villkorlig åtkomst”. Tänk på detta gäller både toohello klientappen och toohello resurs app. Så att du ställer in en princip för villkorlig åtkomst som säger hello ”Office 365 Exchange Online” appen endast kan nås från datorer som är kompatibla.  Den här principen startar också när en användare försöker toouse ett klientprogram som begär behörighet tooExchange Online.



- Du har insyn i vilka appar har tilllåten tooand vilka som används.

1.  När en användare godkänner tooan app, har en ServicePrincipal-objektet skapats i hello-klient. Skapa en ServicePrincipal ingår i hello kontrollrapport.
2.  Inloggningsaktivitet Användarrapporter berätta vilken app hello användare loggar in på. 

## <a name="example"></a>Exempel

Exempelvis ta hello ”FabrikamMail för Office 365” app som du har lagt märke till användare i din klient loggar in på. ”FabrikamMail” är en e-postapp för Android, som publicerats av ”Fabrikam, Inc.”. Detta hamnar i hello ”flera innehavare appar andra utveckla som Contoso kan samtycker till att”.

Om användare tillåts tooconsent kan få de en fråga hello medgivande första gången de loggar in:![](media/active-directory-apps-permissions-consent/apps14.png)

”Åtkomst till dina postlådor” är hello användarinriktad medgivande sträng för hello ”komma åt postlådor som hello inloggade användare” behörighet som exponeras av ”Office 365 Exchange Online” (det vill säga Exchange).

Du kan se hello behörigheter genom att leta upp hello ServicePrincipal-objekt för Exchange (hello resurs) som har lagts till när du registrerade dig för Office 365. Du kan se hello ServicePrincipal objekt av en ”instans” hello-appen i din klient som används för att registrera olika alternativ och konfigurationer.  Du kan se detta med hjälp av hello `Get-AzureADServicePrincipal` i PowerShell.

    PS C:\> Get-AzureADServicePrincipal -ObjectId 383f7b97-6754-4d3d-9474-3908ebcba1c6 | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : Office 365 Exchange Online
    AppId                     : 00000002-0000-0ff1-ce00-000000000000
    AppOwnerTenantId          : 
    AppRoleAssignmentRequired : False
    AppRoles                  : {...}
    DisplayName               : Microsoft.Exchange
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {...
                                , class OAuth2Permission {
                                  AdminConsentDescription : Allows hello app toohave hello same access toomailboxes as hello signed-in user via Exchange Web Services.
                                  AdminConsentDisplayName : Access mailboxes as hello signed-in user via Exchange Web Services
                                  Id                      : 3b5f3d61-589b-4a3c-a359-5dd4b5ee5bd5
                                  IsEnabled               : True
                                  Type                    : User
                                  UserConsentDescription  : Allows hello app full access tooyour mailboxes on your behalf.
                                  UserConsentDisplayName  : Access your mailboxes
                                  Value                   : full_access_as_user
                                },
                                ...}
    PasswordCredentials       : {}
    PublisherName             : 
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {00000002-0000-0ff1-ce00-000000000000/outlook.office365.com, 00000002-0000-0ff1-ce00-000000000000/mail.office365.com, 00000002-0000-0ff1-ce00-000000000000/outlook.com, 
                                00000002-0000-0ff1-ce00-000000000000/*.outlook.com...}
    Tags                      : {}

Medgivande initieras när hello användaren klickar på ”Acceptera”. Först skapas en ServicePrincipal-objekt för ”FabrikamMail för Office 365” i hello-klient. Hej ServicePrincipal ser ut ungefär så här:

    PS C:\> Get-AzureADServicePrincipal -SearchString "FabrikamMail for Office 365" | fl *
    
    DeletionTimeStamp         : 
    ObjectId                  : a8b16333-851d-42e8-acd2-eac155849b37
    ObjectType                : ServicePrincipal
    AccountEnabled            : True
    AppDisplayName            : FabrikamMail for Office 365
    AppId                     : aba7c072-2267-4031-8960-e7a2db6e0590
    AppOwnerTenantId          : 4a4076e0-a70f-41c6-b819-6f9c4a86df89
    AppRoleAssignmentRequired : False
    AppRoles                  : {}
    DisplayName               : FabrikamMail for Office 365
    ErrorUrl                  : 
    Homepage                  : 
    KeyCredentials            : {}
    LogoutUrl                 : 
    Oauth2Permissions         : {}
    PasswordCredentials       : {}
    PublisherName             : Fabrikam, Inc.
    ReplyUrl                  : 
    SamlMetadataUrl           : 
    ServicePrincipalNames     : {aba7c072-2267-4031-8960-e7a2db6e0590}
    Tags                      : {WindowsAzureActiveDirectoryIntegratedApp}

Principer tooan app skapar en Oauth2PermissionGrant länk mellan hello följande:
  
- hello användarobjekt
- Hej klientappar ServicePrincipalName (SPN)
- hello resurs appar ServicePrincipalName (SPN)
- behörigheter i hello resurs app.  

Hello gäller FabrikamMail, ser ut ungefär så här:

    PS C:\> Get-AzureADUserOAuth2PermissionGrant -ObjectId ddiggle@aadpremiumlab.onmicrosoft.com | fl *
    
    ClientId    : a8b16333-851d-42e8-acd2-eac155849b37
    ConsentType : Principal
    ExpiryTime  : 05/15/2017 07:02:39 AM
    ObjectId    : M2OxqB2F6EKs0urBVYSbN5d7PzhUZz1NlH25COvLocbJjoxkUFfRQauryBKwBWet
    PrincipalId : 648c8ec9-5750-41d1-abab-c812b00567ad
    ResourceId  : 383f7b97-6754-4d3d-9474-3908ebcba1c6
    Scope       : full_access_as_user
    StartTime   : 01/01/0001 12:00:00 AM

(**ClientId** är Fabrikammail's service principal objekt-ID (hello som du just har skapat), **PrincipalId** är hello användaren objekt-ID (för hello användare som godkänt), **ResourceId**är Exchange's service principal objekt-ID, Scope är hello behörighet i Exchange som har godkänt för).

Om användarna inte får tooconsent, visas en skärm som säger behörigheten krävs.

