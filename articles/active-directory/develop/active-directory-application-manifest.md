---
title: aaaUnderstanding hello Azure Active Directory Application Manifest | Microsoft Docs
description: "Detaljerad information om hello Azure Active Directorys programmanifest, som representerar ett programs identitet konfiguration i en Azure AD-klient och är används toofacilitate OAuth-auktorisering och medgivande upplevelse."
services: active-directory
documentationcenter: 
author: sureshja
manager: mbaldwin
editor: 
ms.assetid: 4804f3d4-0ff1-4280-b663-f8f10d54d184
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: sureshja
ms.custom: aaddev
ms.reviewer: elisol
ms.openlocfilehash: 096c9d5501edbfc08731fea670cee559d4442ad1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understanding-hello-azure-active-directory-application-manifest"></a>Förstå hello Azure Active Directory-programmanifestet
Program som integreras med Azure Active Directory (AD) måste vara registrerad med en Azure AD-klient som tillhandahåller en permanent identitet konfiguration för hello program. Den här konfigurationen samråd vid körning, aktivera scenarier som gör att ett program toooutsource och broker autentisering/auktorisering via Azure AD. Mer information om hello Azure AD-programmodell finns hello [att lägga till, uppdatera och ta bort ett program] [ ADD-UPD-RMV-APP] artikel.

## <a name="updating-an-applications-identity-configuration"></a>Uppdatera konfigurationen för ett program identitet
Det finns faktiskt flera alternativ för att uppdatera hello egenskaper för ett program identitet konfiguration, där olika funktioner och frihetsgrader problem, inklusive hello följande:

* Hej  **[Azure portal] [ AZURE-PORTAL] webbanvändargränssnitt** kan du tooupdate hello vanligaste egenskaper för ett program. Det här är hello snabbaste och minst fel lätt sätt att uppdatera egenskaperna för ditt program, men inte ge fullständig åtkomst tooall egenskaper, som hello följande två metoder.
* För mer avancerade scenarier där du behöver tooupdate egenskaper som inte exponeras i hello klassiska Azure-portalen, kan du ändra hello **programmanifestet**. Detta är hello fokus i den här artikeln som beskrivs i detalj i hello nästa avsnitt.
* Det är också möjligt för**skriva ett program som använder hello [Graph API] [ GRAPH-API]**  tooupdate ditt program som kräver hello de flesta arbete. Detta kan vara ett bra alternativ men om du skriver hanteringsprogramvara eller behöver tooupdate programegenskaper regelbundet i obevakat läge.

## <a name="using-hello-application-manifest-tooupdate-an-applications-identity-configuration"></a>Med hjälp av hello application manifest tooupdate identitet en programkonfigurationen
Via hello [Azure-portalen][AZURE-PORTAL], du kan hantera identitet programkonfigurationen genom att uppdatera hello programmanifest med hello infogade manifestet editor. Du kan också hämta och överföra hello programmanifestet som en JSON-fil. Inga faktiska filen lagras i hello directory. hello programmanifestet är bara en HTTP GET-åtgärd på hello Azure AD Graph API programmet entiteten och hello överföringen är en korrigering av HTTP-åtgärd på hello programmet entitet.

Därför i ordning toounderstand hello format och egenskaper för hello applikationsmanifestet måste tooreference hello Graph API [programmet entiteten] [ APPLICATION-ENTITY] dokumentation. Exempel på uppdateringar som kan utföras om programmets manifest överför:

* **Deklarera behörighetsomfattningen (oauth2Permissions)** visas av ditt webb-API. I avsnittet hello ”Exposing webb-API: er tooOther program” i [integrera program med Azure Active Directory] [ INTEGRATING-APPLICATIONS-AAD] information om hur du implementerar personifiering med hello oauth2Permissions delegerad behörighet omfång. Som tidigare nämnts programmet Entitetsegenskaper dokumenteras i hello Graph API [entitetstyper och komplexa typen] [ APPLICATION-ENTITY] referensartikeln, inklusive hello oauth2Permissions-egenskap som är en samlingen av typen [OAuth2Permission][APPLICATION-ENTITY-OAUTH2-PERMISSION].
* **Deklarera programroller (appRoles) som exponeras av din app**. hello programmet entitetens appRoles egenskapen är en mängd av typen [AppRole][APPLICATION-ENTITY-APP-ROLE]. Se hello [rollbaserad åtkomstkontroll i molnprogram med Azure AD] [ RBAC-CLOUD-APPS-AZUREAD] artikel exempel implementering.
* **Deklarera kända klienten program (knownClientApplications)**, vilket gör att du toologically oavgjort resultat hello samtycke hello angetts klienten program toohello resurs/webb-API.
* **Begär gruppmedlemskap för Azure AD tooissue anspråk** för hello inloggad användare (groupMembershipClaims).  Det kan också vara konfigurerade tooissue anspråk om hello användarens directory medlemskap i roller. Se hello [auktorisering i molnprogram med hjälp av AD-grupper] [ AAD-GROUPS-FOR-AUTHORIZATION] artikel exempel implementering.
* **Tillåt ditt program toosupport OAuth 2.0 Implicit bevilja** flöden (oauth2AllowImplicitFlow). Den här typen av bevilja flödet används med inbäddade JavaScript-webbsidor eller enda sidan program (SPA). Mer information om hello implicit authorization grant finns [förstå hello OAuth2 implicit bevilja flödet i Azure Active Directory][IMPLICIT-GRANT].
* **Aktivera användning av X509 certifikat som hello hemlig nyckel** (keyCredentials). Se hello [skapa tjänsten och daemon appar i Office 365] [ O365-SERVICE-DAEMON-APPS] och [Developer's guide tooauth med Azure Resource Manager API] [ DEV-GUIDE-TO-AUTH-WITH-ARM] artiklar för exempel på implementering.
* **Lägg till en ny App-ID URI** för programmet (identifierURIs[]). URI: er för App-ID toouniquely identifiera ett program i sin Azure AD-klient (eller över flera Azure AD-innehavare för scenarier med flera innehavare när kvalificerat via verifierad domän). De används när du begär behörighet tooa resursprogram eller skaffa en åtkomst-token för en resursprogram. När du uppdaterar det här elementet görs hello samma uppdatering toohello motsvarande tjänstens huvudnamn servicePrincipalNames [] samling som bor i hello program hemma klient.

hello applikationsmanifestet innehåller också ett bra sätt tootrack hello tillståndet för programmet registreringen. Eftersom den är tillgänglig i JSON-format, kontrolleras hello filen representation i source-kontroll, tillsammans med ditt programs källkod.

## <a name="step-by-step-example"></a>Steg för steg-exempel
Nu kan gå igenom hello steg krävs tooupdate programmets identitet konfigurationen med hjälp av hello programmanifestet. Vi betona en hello föregående exempel visar hur toodeclare en ny behörighet scope på en resursprogram:

1. Logga in toohello [Azure-portalen][AZURE-PORTAL].
2. När du har autentiserats, Välj Azure AD-klienten genom att välja den hello övre högra hörnet av hello sida.
3. Välj **Azure Active Directory** tillägget från hello kvar navigeringsfönstret och klicka på **App registreringar**.
4. Hitta hello program du vill tooupdate i hello listan och klicka på den.
5. Hello programmet sidan klickar du på **Manifest** tooopen hello infogade manifestet editor. 
6. Du kan redigera hello manifest med den här redigeraren direkt. Observera att hello-manifestet följer hello schema för hello [programmet entiteten] [ APPLICATION-ENTITY] som vi nämnt tidigare: till exempel antar vi vill tooimplement/exponera en ny behörighet kallas ”Employees.Read.All” på vår resursprogram (API) du bara lägger till en ny per sekund elementet toohello oauth2Permissions samling ie:
   
        "oauth2Permissions": [
        {
        "adminConsentDescription": "Allow hello application tooaccess MyWebApplication on behalf of hello signed-in user.",
        "adminConsentDisplayName": "Access MyWebApplication",
        "id": "aade5b35-ea3e-481c-b38d-cba4c78682a0",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow hello application tooaccess MyWebApplication on your behalf.",
        "userConsentDisplayName": "Access MyWebApplication",
        "value": "user_impersonation"
        },
        {
        "adminConsentDescription": "Allow hello application toohave read-only access tooall Employee data.",
        "adminConsentDisplayName": "Read-only access tooEmployee records",
        "id": "2b351394-d7a7-4a84-841e-08a6a17e4cb8",
        "isEnabled": true,
        "type": "User",
        "userConsentDescription": "Allow hello application toohave read-only access tooyour Employee data.",
        "userConsentDisplayName": "Read-only access tooyour Employee records",
        "value": "Employees.Read.All"
        }
        ],
   
    hello-posten måste vara unikt och därför måste du generera en ny globalt unikt ID (GUID) för hello `"id"` egenskapen. I det här fallet eftersom vi har angetts `"type": "User"`, den här behörigheten kan vara tilllåten tooby något konto autentiseras av hello Azure AD-klient som hello resurs-API-program är registrerat. Detta ger hello klienten program behörighet tooaccess det åt hello-konto. hello beskrivning och visa namnet strängar används under medgivande och för att visa hello Azure-portalen.
6. När du är klar uppdaterar hello manifestet klickar du på **spara** toosave hello manifest.  
   
Nu när hello manifestet sparas, kan du ge en registrerad klient programmet åtkomst toohello nya tillstånd vi lades till ovan. Nu kan du använda hello Azure portal Webbgränssnittet i stället för att redigera hello klientprogrammets manifest:  

1. Först gå toohello **inställningar** bladet för hello klienten programmet toowhich gärna tooadd åtkomst toohello nytt API, klickar du på **nödvändiga behörigheter** och välj **väljer en API** .
2. Sedan visas med hello lista över registrerade resursprogram-API: er i hello-klient. Klicka på hello resurs programmet tooselect eller hello-typnamn för hello programmet hello-sökrutan. När du har hittat hello programmet, klickar du på **Välj**.  
3. Detta tar du toohello **Välj behörigheter** sidan som visar hello lista över behörigheter för program och delegerade behörigheter som är tillgängliga för hello resursprogram. Välj hello ny behörighet i ordning tooadd den begärda toohello klienten i behörighetslistan. Den här nya behörigheten kommer att lagras i hello klientprogrammets identitet konfiguration, hello ”requiredResourceAccess” samlingsegenskap.


Det var allt. Nu körs programmen med sina nya identity-konfigurationen.

## <a name="next-steps"></a>Nästa steg
* Mer information om hello förhållandet mellan program och tjänstens huvudnamn objekt i ett program, se [program och tjänstens huvudnamn objekt i Azure AD][AAD-APP-OBJECTS].
* Se hello [Azure AD-utvecklare ordlista] [ AAD-DEVELOPER-GLOSSARY] definitioner av vissa begrepp för utvecklare hello core Azure Active Directory (AD).

Använd hello kommentarer nedan tooprovide feedback och hjälp oss att förfina och utforma innehållet.

<!--article references -->
[AAD-APP-OBJECTS]: active-directory-application-objects.md
[AAD-DEVELOPER-GLOSSARY]: active-directory-dev-glossary.md
[AAD-GROUPS-FOR-AUTHORIZATION]: http://www.dushyantgill.com/blog/2014/12/10/authorization-cloud-applications-using-ad-groups/
[ADD-UPD-RMV-APP]: active-directory-integrating-applications.md
[APPLICATION-ENTITY]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[APPLICATION-ENTITY-APP-ROLE]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#approle-type
[APPLICATION-ENTITY-OAUTH2-PERMISSION]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#oauth2permission-type
[AZURE-PORTAL]: https://portal.azure.com
[DEV-GUIDE-TO-AUTH-WITH-ARM]: http://www.dushyantgill.com/blog/2015/05/23/developers-guide-to-auth-with-azure-resource-manager-api/
[GRAPH-API]: active-directory-graph-api.md
[IMPLICIT-GRANT]: active-directory-dev-understanding-oauth2-implicit-grant.md
[INTEGRATING-APPLICATIONS-AAD]: https://azure.microsoft.com/documentation/articles/active-directory-integrating-applications/
[O365-PERM-DETAILS]: https://msdn.microsoft.com/office/office365/HowTo/application-manifest
[O365-SERVICE-DAEMON-APPS]: https://msdn.microsoft.com/office/office365/howto/building-service-apps-in-office-365
[RBAC-CLOUD-APPS-AZUREAD]: http://www.dushyantgill.com/blog/2014/12/10/roles-based-access-control-in-cloud-applications-using-azure-ad/

