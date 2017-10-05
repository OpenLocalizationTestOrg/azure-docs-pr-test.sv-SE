---
title: "Kom igång med Azure Active Directory Identity Protection och Microsoft Graph | Microsoft Docs"
description: "Innehåller en introduktion till frågan Microsoft Graph en lista över riskhändelser och tillhörande information från Azure Active Directory."
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
ms.openlocfilehash: 9b01ff86da6a1fd4a439a6ba59ea15ed6480cdad
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a><span data-ttu-id="69564-104">Kom igång med Azure Active Directory Identity Protection och Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="69564-104">Get started with Azure Active Directory Identity Protection and Microsoft Graph</span></span>
<span data-ttu-id="69564-105">Microsoft Graph är Microsoft unified API-slutpunkt och hem för [Azure Active Directory Identity Protection](active-directory-identityprotection.md) API: er.</span><span class="sxs-lookup"><span data-stu-id="69564-105">Microsoft Graph is the Microsoft unified API endpoint and the home of [Azure Active Directory Identity Protection](active-directory-identityprotection.md) APIs.</span></span> <span data-ttu-id="69564-106">Det första API **identityRiskEvents**, kan du fråga Microsoft Graph en lista över [riskerar händelser](active-directory-identityprotection-risk-events-types.md) och tillhörande information.</span><span class="sxs-lookup"><span data-stu-id="69564-106">The first API, **identityRiskEvents**, allows you to query Microsoft Graph for a list of [risk events](active-directory-identityprotection-risk-events-types.md) and associated information.</span></span> <span data-ttu-id="69564-107">Den här artikeln hjälper dig att komma igång frågar detta API.</span><span class="sxs-lookup"><span data-stu-id="69564-107">This article gets you started querying this API.</span></span> <span data-ttu-id="69564-108">En detaljerad introduktion, fullständig dokumentation och åtkomst till diagrammet Explorer finns på [Microsoft Graph plats](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="69564-108">For an in-depth introduction, full documentation, and access to the Graph Explorer, see the [Microsoft Graph site](https://graph.microsoft.io/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="69564-109">Microsoft rekommenderar att du hanterar Azure AD via [Azure AD administratörscenter](https://aad.portal.azure.com) på Azure Portal istället för via den klassiska Azure-portalen som nämns i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="69564-109">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span>

<span data-ttu-id="69564-110">Det finns tre steg för att komma åt identitetsskydd data via Microsoft Graph:</span><span class="sxs-lookup"><span data-stu-id="69564-110">There are three steps to accessing Identity Protection data through Microsoft Graph:</span></span>

1. <span data-ttu-id="69564-111">Lägg till ett program med en klienthemlighet.</span><span class="sxs-lookup"><span data-stu-id="69564-111">Add an application with a client secret.</span></span> 
2. <span data-ttu-id="69564-112">Använd den här hemligheten och några andra typer av information för att autentisera till Microsoft Graph, där du får en token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="69564-112">Use this secret and a few other pieces of information to authenticate to Microsoft Graph, where you receive an authentication token.</span></span> 
3. <span data-ttu-id="69564-113">Använda denna token för att göra förfrågningar till API-slutpunkten och komma identitetsskydd data.</span><span class="sxs-lookup"><span data-stu-id="69564-113">Use this token to make requests to the API endpoint and get Identity Protection data back.</span></span>

<span data-ttu-id="69564-114">Innan du börjar behöver du:</span><span class="sxs-lookup"><span data-stu-id="69564-114">Before you get started, you’ll need:</span></span>

* <span data-ttu-id="69564-115">Administratörsbehörighet för att skapa programmet i Azure AD</span><span class="sxs-lookup"><span data-stu-id="69564-115">Administrator privileges to create the application in Azure AD</span></span>
* <span data-ttu-id="69564-116">Namnet på din klient domän (till exempel contoso.onmicrosoft.com)</span><span class="sxs-lookup"><span data-stu-id="69564-116">The name of your tenant's domain (for example, contoso.onmicrosoft.com)</span></span>

## <a name="add-an-application-with-a-client-secret"></a><span data-ttu-id="69564-117">Lägg till ett program med en klienthemlighet</span><span class="sxs-lookup"><span data-stu-id="69564-117">Add an application with a client secret</span></span>
1. <span data-ttu-id="69564-118">[Logga in](https://manage.windowsazure.com) till din Azure klassiska portal som administratör.</span><span class="sxs-lookup"><span data-stu-id="69564-118">[Sign in](https://manage.windowsazure.com) to your Azure classic portal as an administrator.</span></span> 
2. <span data-ttu-id="69564-119">I det vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="69564-119">On on the left navigation pane, click **Active Directory**.</span></span> 
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)
3. <span data-ttu-id="69564-121">Från den **Directory** listan, Välj den katalog som du vill aktivera katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="69564-121">From the **Directory** list, select the directory for which you want to enable directory integration.</span></span>
4. <span data-ttu-id="69564-122">Klicka på menyn högst upp **program**.</span><span class="sxs-lookup"><span data-stu-id="69564-122">In the menu on the top, click **Applications**.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)
5. <span data-ttu-id="69564-124">Klicka på **Lägg till** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="69564-124">Click **Add** at the bottom of the page.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)
6. <span data-ttu-id="69564-126">På den **vad vill du göra** dialogrutan klickar du på **Lägg till ett program som min organisation utvecklar**.</span><span class="sxs-lookup"><span data-stu-id="69564-126">On the **What do you want to do** dialog, click **Add an application my organization is developing**.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)
7. <span data-ttu-id="69564-128">På den **berätta om tillämpningsprogrammet** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="69564-128">On the **Tell us about your application** dialog, perform the following steps:</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)
   
    <span data-ttu-id="69564-130">a.</span><span class="sxs-lookup"><span data-stu-id="69564-130">a.</span></span> <span data-ttu-id="69564-131">I den **namn** textruta, ange ett namn för ditt program (t.ex.: AADIP Risk händelse API-program).</span><span class="sxs-lookup"><span data-stu-id="69564-131">In the **Name** textbox, type a name for your application (e.g.: AADIP Risk Event API Application).</span></span>
   
    <span data-ttu-id="69564-132">b.</span><span class="sxs-lookup"><span data-stu-id="69564-132">b.</span></span> <span data-ttu-id="69564-133">Som **typen**väljer **webbprogram och / eller webb-API**.</span><span class="sxs-lookup"><span data-stu-id="69564-133">As **Type**, select **Web Application And / Or Web API**.</span></span>
   
    <span data-ttu-id="69564-134">c.</span><span class="sxs-lookup"><span data-stu-id="69564-134">c.</span></span> <span data-ttu-id="69564-135">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="69564-135">Click **Next**.</span></span>
8. <span data-ttu-id="69564-136">På den **appegenskaper** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="69564-136">On the **App properties** dialog, perform the following steps:</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)
   
    <span data-ttu-id="69564-138">a.</span><span class="sxs-lookup"><span data-stu-id="69564-138">a.</span></span> <span data-ttu-id="69564-139">I den **inloggnings-URL** textruta typen `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="69564-139">In the **Sign-On URL** textbox, type `http://localhost`.</span></span>
   
    <span data-ttu-id="69564-140">b.</span><span class="sxs-lookup"><span data-stu-id="69564-140">b.</span></span> <span data-ttu-id="69564-141">I den **App-ID URI** textruta typen `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="69564-141">In the **App ID URI** textbox, type `http://localhost`.</span></span>
   
    <span data-ttu-id="69564-142">c.</span><span class="sxs-lookup"><span data-stu-id="69564-142">c.</span></span> <span data-ttu-id="69564-143">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="69564-143">Click **Complete**.</span></span>

<span data-ttu-id="69564-144">Kan nu konfigurera ditt program.</span><span class="sxs-lookup"><span data-stu-id="69564-144">Your can now configure your application.</span></span>

![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)

## <a name="grant-your-application-permission-to-use-the-api"></a><span data-ttu-id="69564-146">Ge ditt program behörighet att använda API: et</span><span class="sxs-lookup"><span data-stu-id="69564-146">Grant your application permission to use the API</span></span>
1. <span data-ttu-id="69564-147">På ditt program i menyn överst, klickar du på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="69564-147">On your application's page, in the menu on the top, click **Configure**.</span></span> 
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)
2. <span data-ttu-id="69564-149">I den **behörigheter för andra program** klickar du på **lägga till program**.</span><span class="sxs-lookup"><span data-stu-id="69564-149">In the **permissions to other applications** section, click **Add application**.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)
3. <span data-ttu-id="69564-151">På den **behörigheter för andra program** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="69564-151">On the **permissions to other applications** dialog, perform the following steps:</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)
   
    <span data-ttu-id="69564-153">a.</span><span class="sxs-lookup"><span data-stu-id="69564-153">a.</span></span> <span data-ttu-id="69564-154">Välj **Microsoft Graph**.</span><span class="sxs-lookup"><span data-stu-id="69564-154">Select **Microsoft Graph**.</span></span>
   
    <span data-ttu-id="69564-155">b.</span><span class="sxs-lookup"><span data-stu-id="69564-155">b.</span></span> <span data-ttu-id="69564-156">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="69564-156">Click **Complete**.</span></span>
4. <span data-ttu-id="69564-157">Klicka på **behörigheter för program: 0**, och välj sedan **läsa alla risker händelse identitetsinformation**.</span><span class="sxs-lookup"><span data-stu-id="69564-157">Click **Application Permissions: 0**, and then select **Read all identity risk event information**.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)
5. <span data-ttu-id="69564-159">Klicka på **Spara** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="69564-159">Click **Save** at the bottom of the page.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

## <a name="get-an-access-key"></a><span data-ttu-id="69564-161">Hämta en åtkomstnyckel</span><span class="sxs-lookup"><span data-stu-id="69564-161">Get an access key</span></span>
1. <span data-ttu-id="69564-162">På sidan för ditt program i den **nycklar** väljer 1 år som varaktighet.</span><span class="sxs-lookup"><span data-stu-id="69564-162">On your application's page, in the **keys** section, select 1 year as duration.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)
2. <span data-ttu-id="69564-164">Klicka på **Spara** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="69564-164">Click **Save** at the bottom of the page.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)
3. <span data-ttu-id="69564-166">Kopiera värdet för den nya nyckeln i avsnittet nycklar och klistra in den i en säker plats.</span><span class="sxs-lookup"><span data-stu-id="69564-166">in the keys section, copy the value of your newly created key, and then paste it into a safe location.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)
   
   > [!NOTE]
   > <span data-ttu-id="69564-168">Om du tappar bort den här nyckeln kan behöver du gå tillbaka till det här avsnittet och skapa en ny nyckel.</span><span class="sxs-lookup"><span data-stu-id="69564-168">If you lose this key, you will have to return to this section and create a new key.</span></span> <span data-ttu-id="69564-169">Behåll den här nyckeln en hemlighet: alla som har åtkomst till data.</span><span class="sxs-lookup"><span data-stu-id="69564-169">Keep this key a secret: anyone who has it can access your data.</span></span>
   > 
   > 
4. <span data-ttu-id="69564-170">I den **egenskaper** avsnittet, kopiera den **klient-ID**, och klistra in den i en säker plats.</span><span class="sxs-lookup"><span data-stu-id="69564-170">In the **properties** section, copy the **Client ID**, and then paste it into a safe location.</span></span> 

## <a name="authenticate-to-microsoft-graph-and-query-the-identity-risk-events-api"></a><span data-ttu-id="69564-171">Autentisera till Microsoft Graph och frågor identitet Risk händelser API</span><span class="sxs-lookup"><span data-stu-id="69564-171">Authenticate to Microsoft Graph and query the Identity Risk Events API</span></span>
<span data-ttu-id="69564-172">Du bör nu ha:</span><span class="sxs-lookup"><span data-stu-id="69564-172">At this point, you should have:</span></span>

* <span data-ttu-id="69564-173">Klient-ID som du kopierade ovan</span><span class="sxs-lookup"><span data-stu-id="69564-173">The client ID you copied above</span></span>
* <span data-ttu-id="69564-174">Den nyckel som du kopierade ovan</span><span class="sxs-lookup"><span data-stu-id="69564-174">The key you copied above</span></span>
* <span data-ttu-id="69564-175">Namnet på domänen för din klient</span><span class="sxs-lookup"><span data-stu-id="69564-175">The name of your tenant's domain</span></span>

<span data-ttu-id="69564-176">Skicka en post-begäran till för att autentisera, `https://login.microsoft.com` med följande parametrar i brödtext:</span><span class="sxs-lookup"><span data-stu-id="69564-176">To authenticate, send a post request to `https://login.microsoft.com` with the following parameters in the body:</span></span>

* <span data-ttu-id="69564-177">grant_type ”:**client_credentials**”</span><span class="sxs-lookup"><span data-stu-id="69564-177">grant_type: “**client_credentials**”</span></span>
* <span data-ttu-id="69564-178">resurs ”:**https://graph.microsoft.com**”</span><span class="sxs-lookup"><span data-stu-id="69564-178">resource: “**https://graph.microsoft.com**”</span></span>
* <span data-ttu-id="69564-179">client_id:<your client ID></span><span class="sxs-lookup"><span data-stu-id="69564-179">client_id: <your client ID></span></span>
* <span data-ttu-id="69564-180">client_secret:<your key></span><span class="sxs-lookup"><span data-stu-id="69564-180">client_secret: <your key></span></span>

> [!NOTE]
> <span data-ttu-id="69564-181">Du måste ange värden för den **client_id** och **client_secret** parameter.</span><span class="sxs-lookup"><span data-stu-id="69564-181">You need to provide values for the **client_id** and the **client_secret** parameter.</span></span>
> 
> 

<span data-ttu-id="69564-182">Om det lyckas, returneras en autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="69564-182">If successful, this returns an authentication token.</span></span>  
<span data-ttu-id="69564-183">För att anropa API: et, skapar du en rubrik med följande parameter:</span><span class="sxs-lookup"><span data-stu-id="69564-183">To call the API, create a header with the following parameter:</span></span>

    `Authorization`=”<token_type> <access_token>"


<span data-ttu-id="69564-184">När autentisering, hittar du tokentypen och åtkomst-token i den returnerade token.</span><span class="sxs-lookup"><span data-stu-id="69564-184">When authenticating, you can find the token type and access token in the returned token.</span></span>

<span data-ttu-id="69564-185">Skicka detta huvud som en begäran till följande API-URL:`https://graph.microsoft.com/beta/identityRiskEvents`</span><span class="sxs-lookup"><span data-stu-id="69564-185">Send this header as a request to the following API URL: `https://graph.microsoft.com/beta/identityRiskEvents`</span></span>

<span data-ttu-id="69564-186">Svaret, är om det lyckas, en samling av riskhändelser identitet och associerade data i OData JSON-format, som kan parsas och hanteras som passar behoven.</span><span class="sxs-lookup"><span data-stu-id="69564-186">The response, if successful, is a collection of identity risk events and associated data in the OData JSON format, which can be parsed and handled as see fit.</span></span>

<span data-ttu-id="69564-187">Här är exempelkod för autentisering och anropa API: et med hjälp av Powershell.</span><span class="sxs-lookup"><span data-stu-id="69564-187">Here’s sample code for authenticating and calling the API using Powershell.</span></span>  
<span data-ttu-id="69564-188">Lägg till en klient-ID nyckeln och domän för innehavare.</span><span class="sxs-lookup"><span data-stu-id="69564-188">Just add your client ID, key, and tenant domain.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="69564-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="69564-189">Next steps</span></span>
<span data-ttu-id="69564-190">Grattis, du skapat din första anropet till Microsoft Graph!</span><span class="sxs-lookup"><span data-stu-id="69564-190">Congratulations, you just made your first call to Microsoft Graph!</span></span>  
<span data-ttu-id="69564-191">Nu kan du fråga identitet riskhändelser och använda data men det är lämpligt.</span><span class="sxs-lookup"><span data-stu-id="69564-191">Now you can query identity risk events and use the data however you see fit.</span></span>

<span data-ttu-id="69564-192">Om du vill veta mer om Microsoft Graph och hur du skapar program med hjälp av Graph API kan ta en titt på [dokumentationen](https://graph.microsoft.io/docs) och mycket mer på den [Microsoft Graph plats](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="69564-192">To learn more about Microsoft Graph and how to build applications using the Graph API, check out the [documentation](https://graph.microsoft.io/docs) and much more on the [Microsoft Graph site](https://graph.microsoft.io/).</span></span> <span data-ttu-id="69564-193">Kontrollera också att skapa ett bokmärke i [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) sida som visar en lista över Identity Protection API: er finns i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="69564-193">Also, make sure to bookmark the [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) page that lists all of the Identity Protection APIs available in Graph.</span></span> <span data-ttu-id="69564-194">När vi lägger till nya sätt att arbeta med Identity Protection via API ser dem på sidan.</span><span class="sxs-lookup"><span data-stu-id="69564-194">As we add new ways to work with Identity Protection via API, you’ll see them on that page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="69564-195">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="69564-195">Additional resources</span></span>
* [<span data-ttu-id="69564-196">Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="69564-196">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)
* [<span data-ttu-id="69564-197">Typer av riskhändelser som identifieras av Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="69564-197">Types of risk events detected by Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection-risk-events-types.md)
* [<span data-ttu-id="69564-198">Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="69564-198">Microsoft Graph</span></span>](https://graph.microsoft.io/)
* [<span data-ttu-id="69564-199">Översikt över Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="69564-199">Overview of Microsoft Graph</span></span>](https://graph.microsoft.io/docs)
* [<span data-ttu-id="69564-200">Azure AD Identity Protection Service rot</span><span class="sxs-lookup"><span data-stu-id="69564-200">Azure AD Identity Protection Service Root</span></span>](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

