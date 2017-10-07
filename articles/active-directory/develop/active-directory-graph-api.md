---
title: aaaAzure Active Directory Graph API | Microsoft Docs
description: "En översikt och quickstart guide för hello Graph-API som gör att programmatisk åtkomst till tooAzure AD via REST API-slutpunkter."
services: active-directory
documentationcenter: 
author: viv-liu
manager: mbaldwin
editor: mbaldwin
ms.assetid: 5471ad74-20b3-44df-a2b5-43cde2c0a045
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/27/2017
ms.author: viviali
ms.custom: aaddev
ms.openlocfilehash: cde1dd86b0ca1dc24a5b46dc578b6245ba98751f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-graph-api"></a>Azure Active Directory Graph API
> [!IMPORTANT]
> Vi rekommenderar starkt att du använder [Microsoft Graph](https://graph.microsoft.io/) i stället för Azure AD Graph API tooaccess Azure Active Directory-resurser. Vårt utvecklingsarbete koncentreras nu till Microsoft Graph och inga fler förbättringar planeras för Azure AD Graph API. Det finns ett begränsat antal scenarier som Azure AD Graph API kan fortfarande vara lämplig. Mer information finns i hello [Microsoft Graph eller hello Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) blogginlägget i hello Office Dev Center.
> 
> 

hello Azure Active Directory Graph API ger Programmeringsåtkomst tooAzure AD via REST API-slutpunkter. Program kan använda hello Graph API tooperform skapa, läsa, uppdatera och ta bort CRUD-åtgärder i katalogdata och objekt. Hello Graph API stöder till exempel följande vanliga åtgärder för ett användarobjekt hello:

* Skapa en ny användare i en katalog
* Hämta en användares detaljerade egenskaper, till exempel deras grupper
* Uppdatera egenskaperna för en användare, till exempel deras plats och telefonnummer, eller ändra sina lösenord
* Kontrollera en användares gruppmedlemskap för rollbaserad åtkomst
* Inaktiverar ett användarkonto eller ta bort den helt

Du kan utföra liknande åtgärder på andra objekt, till exempel grupper och program i tillägg toouser objekt. toocall hello Graph API på en katalog, hello programmet måste registreras med Azure AD och vara konfigurerad tooallow access toohello-katalogen. Detta uppnås normalt flöde en användare eller administratör medgivande.

toobegin med hello Azure Active Directory Graph API, se hello [Snabbstartsguide för Graph API](active-directory-graph-api-quickstart.md), eller visa hello [interaktiva Graph API-referensdokumentation](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="features"></a>Funktioner
hello Graph API ger hello följande funktioner:

* **REST API-slutpunkter**: hello Graph API är en RESTful-tjänst består av slutpunkter som hämtas med hjälp av HTTP-begäranden som standard. hello Graph API stöder XML eller Javascript Object Notation (JSON) innehållstyper för begäranden och -svar. Mer information finns i [Azure AD Graph REST API-referens](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
* **Autentisering med Azure AD**: varje begäran toohello Graph API måste autentiseras genom att lägga till en JSON-Webbtoken (JWT) i hello Authorization-huvud för hello-begäran. Denna token förvärvas genom att göra en förfrågan tooAzure Annonsens token för slutpunkt och anger giltiga autentiseringsuppgifter. Du kan använda hello OAuth 2.0-klientautentiseringsuppgifter eller hello auktoriseringskod bevilja flödet tooacquire token toocall-hello diagram. Mer information [OAuth 2.0 i Azure AD](https://msdn.microsoft.com/library/azure/dn645545.aspx).
* **Rollbaserad auktorisering (RBAC)**: säkerhetsgrupper är används tooperform RBAC i hello Graph API. Till exempel om du vill toodetermine om en användare har åtkomst tooa specifik resurs, hello program kan anropa hello [Kontrollera gruppmedlemskap (transitiva)](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#FunctionsandactionsongroupsCheckmembershipinaspecificgrouptransitive) åtgärd som returnerar true eller false.
* **Differentiella frågan**: Om du vill toocheck för ändringar i en katalog mellan två tidsperioder utan toomake frekventa frågor toohello Graph API kan du göra en differentiell fråga. Den här typen av begäran returneras endast hello ändringar mellan hello tidigare differentiell fråga och hello aktuella begäran. Mer information finns i [Azure AD Graph API differentiell frågan](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-differential-query).
* **Katalogtillägg**: Om du utvecklar ett program som behöver tooread eller skriva unika egenskaper för katalogobjekt som du kan registrera och använda tillägget värden med hjälp av hello Graph API. Om ditt program kräver en Skype-ID-egenskap för varje användare, kan du registrera nya hello-egenskapen i hello directory och den blir tillgänglig på alla användarobjekt. Mer information finns i [Azure AD Graph API Directory-schemautökningar](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-directory-schema-extensions).
* **Skyddas av behörighetsomfattningen**: AAD Graph API exponerar behörighetsomfattningen som aktiverar secure/godkänt åtkomst tooAAD data och stöder många olika apptyper som klienten, inklusive:
  
  * de med ett användargränssnitt som är delegerad åtkomst till toodata via tillstånd från hello inloggade användaren (delegerad)
  * de som använder definiera program – rollbaserad åtkomstkontroll, till exempel service-daemon klienter (app-roller)
    
    Både delegerade och app rollen behörighetsomfattningen representerar privilegium som exponeras av hello Graph API och kan begäras av klientprogram via behörigheter för registrering av program [funktioner i hello Azure-portalen](https://portal.azure.com). Klienter kan kontrollera hello behörighetsomfattningen beviljas toothem genom att inspektera hello omfång (”scp”) anspråk togs emot i hello åtkomsttoken för delegerade behörigheter och anspråk för app-rollbehörighet på hello roller (”roller”). Lär dig mer om [Azure AD Graph API Behörighetsomfattningen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes).

## <a name="scenarios"></a>Scenarier
hello Graph API kan många scenarier för programmet. hello följande scenarier är de vanligaste hello:

* **Affärsprogram (enstaka klient)**: I det här scenariot fungerar en enterprise-utvecklare för en organisation som har en Office 365-prenumeration. hello utvecklare skapar ett webbprogram som interagerar med Azure AD tooperform uppgifter exempelvis tilldela en licens tooa användare. Den här aktiviteten kräver åtkomst toohello Graph API, så hello developer registrerar hello enstaka klient program i Azure AD och konfigurerar Läs- och skrivbehörighet för hello Graph API. Hej program är konfigurerade toouse antingen egna autentiseringsuppgifter eller de hello för närvarande på användarens tooacquire token toocall hello Graph API.
* **Programvara som en tjänst (flera innehavare)**: I det här scenariot en oberoende programvaruleverantör (ISV) utveckla värdbaserade flera innehavare webbprogram som ger användare hanteringsfunktioner för andra organisationer som använder Azure AD. Dessa funktioner kräver åtkomst toodirectory objekt och så hello programmet måste toocall hello Graph API. hello developer registrerar hello program i Azure AD, konfigurerar den toorequire Läs och skrivbehörighet för hello Graph API och sedan aktiverar extern åtkomst så att andra organisationer kan godkänna toouse hello program i deras katalog. När en användare i en annan organisation autentiseras toohello program för hello första gången, visas en medgivande dialogruta med hello behörigheter hello program begär.  Ge ditt medgivande sedan ger programmet hello begärda de behörigheter toohello Graph API i katalogen hello användaren. Mer information om hello medgivande framework finns [översikt över hello medgivande Framework](active-directory-integrating-applications.md).

## <a name="see-also"></a>Se även
[Snabbstartsguide för Azure AD Graph API](active-directory-graph-api-quickstart.md)

[AD Graph REST-dokumentation](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)

[Utvecklarhandbok för Azure Active Directory](active-directory-developers-guide.md)

