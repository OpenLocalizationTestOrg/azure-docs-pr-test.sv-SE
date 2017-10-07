---
title: "aaaGet igång med Azure Active Directory Identity Protection och Microsoft Graph | Microsoft Docs"
description: "Ger en introduktion tooquery Microsoft Graph för en lista över riskhändelser och tillhörande information från Azure Active Directory."
services: active-directory
keywords: "Azure active directory identitetsskydd, risk händelse, säkerhetsproblem, säkerhetsprinciper, Microsoft Graph"
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: fa109ba7-a914-437b-821d-2bd98e681386
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 75b8b7629a0120d8101f6fde0d9163122503d276
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a>Kom igång med Azure Active Directory Identity Protection och Microsoft Graph
Microsoft Graph är hello Microsoft unified API-slutpunkt och hello hem av [Azure Active Directory Identity Protection](active-directory-identityprotection.md) API: er. hello första API, **identityRiskEvents**, kan du tooquery Microsoft Graph för en lista över [riskerar händelser](active-directory-identityprotection-risk-events-types.md) och tillhörande information. Den här artikeln hjälper dig att komma igång frågar detta API. En detaljerad introduktion, fullständig dokumentation och åtkomst toohello diagram Explorer finns hello [Microsoft Graph plats](https://graph.microsoft.io/).

> [!IMPORTANT]
> Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln.

Det finns tre steg tooaccessing identitetsskydd data via Microsoft Graph:

1. Lägg till ett program med en klienthemlighet. 
2. Använd den här hemligheten och några andra typer av information tooauthenticate tooMicrosoft diagram, där du får en token för autentisering. 
3. Använd den här token toomake begäranden toohello API-slutpunkten och komma identitetsskydd data.

Innan du börjar behöver du:

* Administratören privilegier toocreate hello program i Azure AD
* hello namnet på din klient domän (till exempel contoso.onmicrosoft.com)

## <a name="add-an-application-with-a-client-secret"></a>Lägg till ett program med en klienthemlighet
1. [Logga in](https://manage.windowsazure.com) tooyour klassiska Azure-portalen som administratör. 
2. I hello vänstra navigeringsfönstret, klicka på **Active Directory**. 
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)
3. Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.
4. Hello-menyn överst hello **program**.
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)
5. Klicka på **Lägg till** på hello hello sidans nederkant.
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)
6. På hello **vad vill du vill toodo** dialogrutan klickar du på **Lägg till ett program som min organisation utvecklar**.
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)
7. På hello **berätta om tillämpningsprogrammet** dialogrutan utföra hello följande steg:
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)
   
    a. I hello **namn** textruta, ange ett namn för ditt program (t.ex.: AADIP Risk händelse API-program).
   
    b. Som **typen**väljer **webbprogram och / eller webb-API**.
   
    c. Klicka på **Nästa**.
8. På hello **appegenskaper** dialogrutan utföra hello följande steg:
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)
   
    a. I hello **inloggnings-URL** textruta typen `http://localhost`.
   
    b. I hello **App-ID URI** textruta typen `http://localhost`.
   
    c. Klicka på **Complete** (Slutför).

Kan nu konfigurera ditt program.

![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)

## <a name="grant-your-application-permission-toouse-hello-api"></a>Ge ditt program behörighet toouse hello API
1. På ditt program i hello menyn hello överst, klickar du på **konfigurera**. 
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)
2. I hello **behörigheter tooother program** klickar du på **lägga till program**.
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)
3. På hello **behörigheter tooother program** dialogrutan utföra hello följande steg:
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)
   
    a. Välj **Microsoft Graph**.
   
    b. Klicka på **Complete** (Slutför).
4. Klicka på **behörigheter för program: 0**, och välj sedan **läsa alla risker händelse identitetsinformation**.
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)
5. Klicka på **spara** på hello hello sidans nederkant.
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

## <a name="get-an-access-key"></a>Hämta en åtkomstnyckel
1. Ditt program i på sidan hello **nycklar** väljer 1 år som varaktighet.
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)
2. Klicka på **spara** på hello hello sidans nederkant.
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)
3. Kopiera hello värdet för den nya nyckeln under hello nycklar och klistra in den i en säker plats.
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)
   
   > [!NOTE]
   > Om du tappar bort den här nyckeln kan du ha tooreturn toothis avsnittet och skapa en ny nyckel. Behåll den här nyckeln en hemlighet: alla som har åtkomst till data.
   > 
   > 
4. I hello **egenskaper** avsnitt, kopiera hello **klient-ID**, och klistra in den i en säker plats. 

## <a name="authenticate-toomicrosoft-graph-and-query-hello-identity-risk-events-api"></a>Autentisera tooMicrosoft diagram och fråga hello identitet Risk händelser API
Du bör nu ha:

* hello klient-ID som du kopierade ovan
* hello-nyckel som du kopierade ovan
* hello namnet på din klient domän

tooauthenticate skicka en post-begäran för`https://login.microsoft.com` med följande parametrar i hello brödtext hello:

* grant_type ”:**client_credentials**”
* resurs ”:**https://graph.microsoft.com**”
* client_id:<your client ID>
* client_secret:<your key>

> [!NOTE]
> Du behöver tooprovide värden för hello **client_id** och hello **client_secret** parameter.
> 
> 

Om det lyckas, returneras en autentiseringstoken.  
toocall hello API, skapa ett huvud med hello följande parameter:

    `Authorization`=”<token_type> <access_token>"


När autentisering, hittar du hello tokentypen och åtkomst-token i hello returnerade token.

Skicka detta huvud som en begäran toohello följande API-URL:`https://graph.microsoft.com/beta/identityRiskEvents`

hello svar, om det lyckas, är en samling identitet riskhändelser och tillhörande data i hello OData JSON-format som kan parsas och hanteras som passar.

Här är exempelkod för autentisering och anropa hello-API med hjälp av Powershell.  
Lägg till en klient-ID nyckeln och domän för innehavare.

    $ClientID       = "<your client ID here>"        # Should be a ~36 hex character string; insert your info here
    $ClientSecret   = "<your client secret here>"    # Should be a ~44 character string; insert your info here
    $tenantdomain   = "<your tenant domain here>"    # For example, contoso.onmicrosoft.com

    $loginURL       = "https://login.microsoft.com"
    $resource       = "https://graph.microsoft.com"

    $body       = @{grant_type="client_credentials";resource=$resource;client_id=$ClientID;client_secret=$ClientSecret}
    $oauth      = Invoke-RestMethod -Method Post -Uri $loginURL/$tenantdomain/oauth2/token?api-version=1.0 -Body $body

    Write-Output $oauth

    if ($oauth.access_token -ne $null) {
        $headerParams = @{'Authorization'="$($oauth.token_type) $($oauth.access_token)"}

        $url = "https://graph.microsoft.com/beta/identityRiskEvents"
        Write-Output $url

        $myReport = (Invoke-WebRequest -UseBasicParsing -Headers $headerParams -Uri $url)

        foreach ($event in ($myReport.Content | ConvertFrom-Json).value) {
            Write-Output $event
        }

    } else {
        Write-Host "ERROR: No Access Token"
    } 


## <a name="next-steps"></a>Nästa steg
Grattis, du skapat din första anropet tooMicrosoft diagram!  
Nu kan du fråga identitet riskhändelser och använder hello data men det är lämpligt.

toolearn mer om Microsoft Graph och hur toobuild program med hjälp av hello Graph API kolla hello [dokumentationen](https://graph.microsoft.io/docs) och mycket mer på hello [Microsoft Graph plats](https://graph.microsoft.io/). Se också till att toobookmark hello [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) sida som visar en lista över hello Identity Protection API: er finns i diagrammet. När vi lägger till nya sätt toowork med identitetsskydd via API ser dem på sidan.

## <a name="additional-resources"></a>Ytterligare resurser
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md)
* [Typer av riskhändelser som identifieras av Azure Active Directory Identity Protection](active-directory-identityprotection-risk-events-types.md)
* [Microsoft Graph](https://graph.microsoft.io/)
* [Översikt över Microsoft Graph](https://graph.microsoft.io/docs)
* [Azure AD Identity Protection Service rot](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

