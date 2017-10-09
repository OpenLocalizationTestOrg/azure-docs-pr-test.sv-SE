---
title: "aaaQuickstart för hello Azure AD Graph API | Microsoft Docs"
description: "hello Azure Active Directory Graph API ger Programmeringsåtkomst tooAzure AD via OData REST API-slutpunkter. Program kan använda hello Graph API tooperform skapa, läsa, uppdatera och ta bort CRUD-åtgärder i katalogdata och objekt."
services: active-directory
documentationcenter: n/a
author: viv-liu
manager: mbaldwin
editor: 
tags: 
ms.assetid: 9dc268a9-32e8-402c-a43f-02b183c295c5
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/28/2017
ms.author: viviali
ms.custom: aaddev
ms.openlocfilehash: b4d3c57f06d212b1d095578f19bb86c932dbcc33
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-for-hello-azure-ad-graph-api"></a>Snabbstart för hello Azure AD Graph API
hello Azure Active Directory (AD) Graph API ger Programmeringsåtkomst tooAzure AD via OData REST API-slutpunkter. Program kan använda hello Graph API tooperform skapa, läsa, uppdatera och ta bort CRUD-åtgärder i katalogdata och objekt. Du kan till exempel använda hello Graph API toocreate en ny användare, visa eller uppdatera användarens egenskaper, ändra användarens lösenord, kontrollera gruppmedlemskap för rollbaserad åtkomst, inaktivera eller ta bort hello användare. Läs mer om hello Graph API-funktioner och Programscenarier [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) och [krav för Azure AD Graph API](https://msdn.microsoft.com/library/hh974476.aspx). 

> [!IMPORTANT]
> Vi rekommenderar starkt att du använder [Microsoft Graph](https://developer.microsoft.com/graph) i stället för Azure AD Graph API tooaccess Azure Active Directory-resurser. Vårt utvecklingsarbete koncentreras nu till Microsoft Graph och inga fler förbättringar planeras för Azure AD Graph API. Det finns ett begränsat antal scenarier som Azure AD Graph API kan fortfarande vara lämplig. Mer information finns i hello [Microsoft Graph eller hello Azure AD Graph](https://dev.office.com/blogs/microsoft-graph-or-azure-ad-graph) blogginlägget i hello Office Dev Center.
> 
> 

## <a name="how-tooconstruct-a-graph-api-url"></a>Hur tooconstruct en Graph API-URL
Du kan använda webbadresserna baserat på hello Open Data (OData)-protokollet i Graph API, tooaccess katalogdata och objekt (med andra ord resurser eller entiteter) som du vill mot tooperform CRUD-åtgärder. hello URL: er som används i Graph API består av fyra delar: tjänsten rot, klient-ID, resursens sökväg och frågealternativ sträng: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Ta hello exempel på hello följande URL: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

* **Tjänsten rot**: I Azure AD Graph API hello service rot är alltid https://graph.windows.net.
* **Identifierare för innehavare**: det här avsnittet kan vara en verifierad (registrerat) domännamn, i föregående exempel hello contoso.com. Det kan också vara en klient objekt-ID eller hello ”ut” eller ”me” alias. Mer information finns i [adressering entiteter och åtgärder i hello Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)).
* **Resurssökvägen**: det här avsnittet för en URL som identifierar hello resurs toobe har åtgärdat med (användare, grupper, en viss användare eller en viss grupp, etc.) Hello-exemplet ovan är det hello översta nivån ”grupper” tooaddress som angetts för resursen. Du kan även lösa en specifik enhet, till exempel ”användare / {objectId}” eller ”användare/userPrincipalName”.
* **Fråga parametrar**: ett frågetecken (?) separerar hello resurs sökvägssektion från hello frågan parametrar avsnitt. Hej ”api-version”-frågeparameter krävs för alla förfrågningar i hello Graph API. hello Graph API stöder också hello följande alternativ för OData-frågan: **$filter**, **$orderby**, **$expand**, **$top**, och **$format**. följande frågealternativ för hello stöds inte för närvarande: **$count**, **$inlinecount**, och **$skip**. Mer information finns i [stöds frågor, filter och växling alternativ i Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Graph API-versioner
Du kan ange hello version för en begäran för Graph API i hello ”api-version”-frågeparameter. För version 1.5 och senare använda du ett numeriskt versionsvärde. API-version = 1.6. För tidigare versioner måste använda du en datumsträng som stämmer överens toohello formatet ÅÅÅÅ-MM-DD; till exempel api-version = 2013-11-08. För förhandsgranskningsfunktioner, Använd hello sträng ”betaversionen”; till exempel api-version = beta. Mer information om skillnaderna mellan Graph API-versioner finns [Azure AD Graph API versionshantering](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>Graph API-metadata
tooreturn hello Graph API metadatafil lägga till hello ”$metadata” segment efter hello klient-ID i hello URL för exemplet hello följande URL: en returnerar metadata för ett företag som demo: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. Du kan ange URL: en i hello adressfältet i en web webbläsare toosee hello metadata. Hej CSDL Metadatadokumentet returnerade beskriver hello entiteter och komplexa typer, deras egenskaper och hello funktioner och åtgärder som exponeras av hello version av Graph API du begärt. Utelämna parametern api-version för hello returnerar metadata för hello senaste versionen.

## <a name="common-queries"></a>Vanliga frågor
[Azure AD Graph API vanliga frågor](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) visar en lista över vanliga frågor som kan användas med hello Azure AD-diagram, inklusive frågor som kan använda tooaccess översta resurser i katalogen och frågor tooperform åtgärder i din katalog.

Till exempel `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` returnerar företagsinformation för directory contoso.com.

Eller `https://graph.windows.net/contoso.com/users?api-version=1.6` visar en lista över alla användarobjekt i hello directory contoso.com.

## <a name="using-hello-graph-explorer"></a>Med hjälp av hello diagrammet Explorer
Du kan använda hello diagrammet Explorer för hello Azure AD Graph API tooquery hello katalogdata när du skapar ditt program.

hello följer hello utdata skulle se om du hade toonavigate toohello diagram Explorer, logga in och ange `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` toodisplay alla hello användare i hello inloggade användarens katalog:

![Azure AD graph api explorer](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**Läs in hello diagrammet Explorer**: tooload hello verktyg, navigera för[https://graphexplorer.azurewebsites.net/](https://graphexplorer.azurewebsites.net/). Klicka på **inloggning** och logga in med ditt Azure AD-kontot autentiseringsuppgifter toorun hello diagrammet Explorer mot din klient. Om du kör diagrammet Explorer mot en egen klient, måste du eller din administratör tooconsent under inloggning. Om du har en Office 365-prenumeration kan ha du automatiskt en Azure AD-klient. hello-autentiseringsuppgifter som du använder toosign i tooOffice 365 är i själva verket, Azure AD-konton, och du kan använda dessa autentiseringsuppgifter med diagrammet Explorer.

**Köra en fråga**: toorun en fråga, Skriv in frågan i textrutan för hello begäran och klickar på **hämta** eller klicka på hello **ange** nyckel. hello resultat visas hello svar i rutan. Till exempel `https://graph.windows.net/myorganization/groups?api-version=1.6` visar en lista över alla gruppobjekt hello inloggade användarens katalog.

Obs hello följande funktioner och begränsningar i hello diagrammet Explorer:

* Anger kapaciteten för Komplettera automatiskt på resursen. toosee begär den här funktionen, klicka på hello textruta (där hello företags-URL visas). Du kan välja en resurs från hello listrutan.
* Stöder hello ”me” och ”ut” adressering alias. Du kan till exempel använda `https://graph.windows.net/me?api-version=1.6` tooreturn hello användarobjektet hello inloggade användaren eller `https://graph.windows.net/myorganization/users?api-version=1.6` tooreturn alla användare i hello aktuell katalog.
* Ett svar huvuden avsnitt. Det här avsnittet kan användas för toohelp felsökning av problem när du kör frågor.
* En JSON-visningsprogram för hello svar med visa och Dölj funktioner.
* Inget stöd för att visa en miniatyrbild.

## <a name="using-fiddler-toowrite-toohello-directory"></a>Med Fiddler toowrite toohello directory
Du kan använda hello Fiddler Web felsökare toopractice utför skriva åtgärder mot Azure AD-katalogen för den här snabbstartsguide hello avses. Mer information och tooinstall Fiddler finns [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler).

I hello exemplet nedan använder Fiddler Web felsökare toocreate en ny säkerhetsgrupp 'MyTestGroup' i Azure AD-katalogen.

**Hämta en åtkomst-token**: tooaccess Azure AD Graph klienter är obligatoriska toosuccessfully autentisera tooAzure AD först. Mer information finns i [Autentiseringsscenarier för Azure AD](active-directory-authentication-scenarios.md).

**Skapa och köra en fråga**: fullständig hello följande steg:

1. Öppna Fiddler Web felsökare och växla toohello **Composer** fliken.
2. Eftersom du vill toocreate en ny säkerhetsgrupp markerar **efter** som hello HTTP-metoden från hello nedrullningsbara menyn. Mer information om åtgärder och behörigheter på ett gruppobjekt finns [grupp](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) inom hello [Azure AD Graph REST API-referens](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).
3. I hello fältet bredvid för**Post**, Skriv i hello följande som hello URL-begäran: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`.
   
   > [!NOTE]
   > Du måste ersätta mytenantdomain med hello domännamnet för din egen Azure AD-katalog.
   > 
   > 
4. Hello direkt under Post nedrullningsbara och Skriv i hello följande:
   
    ```
   Host: graph.windows.net
   Authorization: Bearer <your access token>
   Content-Type: application/json
   ```
   
   > [!NOTE]
   > Ersätt din &lt;åtkomst-token&gt; med hello åtkomsttoken för Azure AD-katalogen.
   > 
   > 
5. I hello **Begärandetext** anger hello följande:
   
    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
   ```
   
    Mer information om hur du skapar grupper finns [Skapa grupp](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

Mer information om Azure AD-entiteter och typer som visas av diagram och information om hello-åtgärder som kan utföras på dem med Graph finns [Azure AD Graph REST API-referens](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Nästa steg
* Mer information om hello [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
* Lär dig mer om [Azure AD Graph API Behörighetsomfattningen](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)

