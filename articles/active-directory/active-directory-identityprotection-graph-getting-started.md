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
# <a name="get-started-with-azure-active-directory-identity-protection-and-microsoft-graph"></a><span data-ttu-id="9c1e1-104">Kom igång med Azure Active Directory Identity Protection och Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="9c1e1-104">Get started with Azure Active Directory Identity Protection and Microsoft Graph</span></span>
<span data-ttu-id="9c1e1-105">Microsoft Graph är hello Microsoft unified API-slutpunkt och hello hem av [Azure Active Directory Identity Protection](active-directory-identityprotection.md) API: er.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-105">Microsoft Graph is hello Microsoft unified API endpoint and hello home of [Azure Active Directory Identity Protection](active-directory-identityprotection.md) APIs.</span></span> <span data-ttu-id="9c1e1-106">hello första API, **identityRiskEvents**, kan du tooquery Microsoft Graph för en lista över [riskerar händelser](active-directory-identityprotection-risk-events-types.md) och tillhörande information.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-106">hello first API, **identityRiskEvents**, allows you tooquery Microsoft Graph for a list of [risk events](active-directory-identityprotection-risk-events-types.md) and associated information.</span></span> <span data-ttu-id="9c1e1-107">Den här artikeln hjälper dig att komma igång frågar detta API.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-107">This article gets you started querying this API.</span></span> <span data-ttu-id="9c1e1-108">En detaljerad introduktion, fullständig dokumentation och åtkomst toohello diagram Explorer finns hello [Microsoft Graph plats](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="9c1e1-108">For an in-depth introduction, full documentation, and access toohello Graph Explorer, see hello [Microsoft Graph site](https://graph.microsoft.io/).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9c1e1-109">Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-109">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span>

<span data-ttu-id="9c1e1-110">Det finns tre steg tooaccessing identitetsskydd data via Microsoft Graph:</span><span class="sxs-lookup"><span data-stu-id="9c1e1-110">There are three steps tooaccessing Identity Protection data through Microsoft Graph:</span></span>

1. <span data-ttu-id="9c1e1-111">Lägg till ett program med en klienthemlighet.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-111">Add an application with a client secret.</span></span> 
2. <span data-ttu-id="9c1e1-112">Använd den här hemligheten och några andra typer av information tooauthenticate tooMicrosoft diagram, där du får en token för autentisering.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-112">Use this secret and a few other pieces of information tooauthenticate tooMicrosoft Graph, where you receive an authentication token.</span></span> 
3. <span data-ttu-id="9c1e1-113">Använd den här token toomake begäranden toohello API-slutpunkten och komma identitetsskydd data.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-113">Use this token toomake requests toohello API endpoint and get Identity Protection data back.</span></span>

<span data-ttu-id="9c1e1-114">Innan du börjar behöver du:</span><span class="sxs-lookup"><span data-stu-id="9c1e1-114">Before you get started, you’ll need:</span></span>

* <span data-ttu-id="9c1e1-115">Administratören privilegier toocreate hello program i Azure AD</span><span class="sxs-lookup"><span data-stu-id="9c1e1-115">Administrator privileges toocreate hello application in Azure AD</span></span>
* <span data-ttu-id="9c1e1-116">hello namnet på din klient domän (till exempel contoso.onmicrosoft.com)</span><span class="sxs-lookup"><span data-stu-id="9c1e1-116">hello name of your tenant's domain (for example, contoso.onmicrosoft.com)</span></span>

## <a name="add-an-application-with-a-client-secret"></a><span data-ttu-id="9c1e1-117">Lägg till ett program med en klienthemlighet</span><span class="sxs-lookup"><span data-stu-id="9c1e1-117">Add an application with a client secret</span></span>
1. <span data-ttu-id="9c1e1-118">[Logga in](https://manage.windowsazure.com) tooyour klassiska Azure-portalen som administratör.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-118">[Sign in](https://manage.windowsazure.com) tooyour Azure classic portal as an administrator.</span></span> 
2. <span data-ttu-id="9c1e1-119">I hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-119">On on hello left navigation pane, click **Active Directory**.</span></span> 
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_01.png)
3. <span data-ttu-id="9c1e1-121">Från hello **Directory** listan, Välj hello katalog som du vill tooenable katalogintegrering.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-121">From hello **Directory** list, select hello directory for which you want tooenable directory integration.</span></span>
4. <span data-ttu-id="9c1e1-122">Hello-menyn överst hello **program**.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-122">In hello menu on hello top, click **Applications**.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_02.png)
5. <span data-ttu-id="9c1e1-124">Klicka på **Lägg till** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-124">Click **Add** at hello bottom of hello page.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_03.png)
6. <span data-ttu-id="9c1e1-126">På hello **vad vill du vill toodo** dialogrutan klickar du på **Lägg till ett program som min organisation utvecklar**.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-126">On hello **What do you want toodo** dialog, click **Add an application my organization is developing**.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_04.png)
7. <span data-ttu-id="9c1e1-128">På hello **berätta om tillämpningsprogrammet** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9c1e1-128">On hello **Tell us about your application** dialog, perform hello following steps:</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_05.png)
   
    <span data-ttu-id="9c1e1-130">a.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-130">a.</span></span> <span data-ttu-id="9c1e1-131">I hello **namn** textruta, ange ett namn för ditt program (t.ex.: AADIP Risk händelse API-program).</span><span class="sxs-lookup"><span data-stu-id="9c1e1-131">In hello **Name** textbox, type a name for your application (e.g.: AADIP Risk Event API Application).</span></span>
   
    <span data-ttu-id="9c1e1-132">b.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-132">b.</span></span> <span data-ttu-id="9c1e1-133">Som **typen**väljer **webbprogram och / eller webb-API**.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-133">As **Type**, select **Web Application And / Or Web API**.</span></span>
   
    <span data-ttu-id="9c1e1-134">c.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-134">c.</span></span> <span data-ttu-id="9c1e1-135">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-135">Click **Next**.</span></span>
8. <span data-ttu-id="9c1e1-136">På hello **appegenskaper** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9c1e1-136">On hello **App properties** dialog, perform hello following steps:</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_06.png)
   
    <span data-ttu-id="9c1e1-138">a.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-138">a.</span></span> <span data-ttu-id="9c1e1-139">I hello **inloggnings-URL** textruta typen `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-139">In hello **Sign-On URL** textbox, type `http://localhost`.</span></span>
   
    <span data-ttu-id="9c1e1-140">b.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-140">b.</span></span> <span data-ttu-id="9c1e1-141">I hello **App-ID URI** textruta typen `http://localhost`.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-141">In hello **App ID URI** textbox, type `http://localhost`.</span></span>
   
    <span data-ttu-id="9c1e1-142">c.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-142">c.</span></span> <span data-ttu-id="9c1e1-143">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="9c1e1-143">Click **Complete**.</span></span>

<span data-ttu-id="9c1e1-144">Kan nu konfigurera ditt program.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-144">Your can now configure your application.</span></span>

![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_07.png)

## <a name="grant-your-application-permission-toouse-hello-api"></a><span data-ttu-id="9c1e1-146">Ge ditt program behörighet toouse hello API</span><span class="sxs-lookup"><span data-stu-id="9c1e1-146">Grant your application permission toouse hello API</span></span>
1. <span data-ttu-id="9c1e1-147">På ditt program i hello menyn hello överst, klickar du på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-147">On your application's page, in hello menu on hello top, click **Configure**.</span></span> 
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_08.png)
2. <span data-ttu-id="9c1e1-149">I hello **behörigheter tooother program** klickar du på **lägga till program**.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-149">In hello **permissions tooother applications** section, click **Add application**.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_09.png)
3. <span data-ttu-id="9c1e1-151">På hello **behörigheter tooother program** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="9c1e1-151">On hello **permissions tooother applications** dialog, perform hello following steps:</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_10.png)
   
    <span data-ttu-id="9c1e1-153">a.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-153">a.</span></span> <span data-ttu-id="9c1e1-154">Välj **Microsoft Graph**.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-154">Select **Microsoft Graph**.</span></span>
   
    <span data-ttu-id="9c1e1-155">b.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-155">b.</span></span> <span data-ttu-id="9c1e1-156">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="9c1e1-156">Click **Complete**.</span></span>
4. <span data-ttu-id="9c1e1-157">Klicka på **behörigheter för program: 0**, och välj sedan **läsa alla risker händelse identitetsinformation**.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-157">Click **Application Permissions: 0**, and then select **Read all identity risk event information**.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_11.png)
5. <span data-ttu-id="9c1e1-159">Klicka på **spara** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-159">Click **Save** at hello bottom of hello page.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)

## <a name="get-an-access-key"></a><span data-ttu-id="9c1e1-161">Hämta en åtkomstnyckel</span><span class="sxs-lookup"><span data-stu-id="9c1e1-161">Get an access key</span></span>
1. <span data-ttu-id="9c1e1-162">Ditt program i på sidan hello **nycklar** väljer 1 år som varaktighet.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-162">On your application's page, in hello **keys** section, select 1 year as duration.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_13.png)
2. <span data-ttu-id="9c1e1-164">Klicka på **spara** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-164">Click **Save** at hello bottom of hello page.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_12.png)
3. <span data-ttu-id="9c1e1-166">Kopiera hello värdet för den nya nyckeln under hello nycklar och klistra in den i en säker plats.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-166">in hello keys section, copy hello value of your newly created key, and then paste it into a safe location.</span></span>
   
    ![Skapa ett program](./media/active-directory-identityprotection-graph-getting-started/tutorial_general_14.png)
   
   > [!NOTE]
   > <span data-ttu-id="9c1e1-168">Om du tappar bort den här nyckeln kan du ha tooreturn toothis avsnittet och skapa en ny nyckel.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-168">If you lose this key, you will have tooreturn toothis section and create a new key.</span></span> <span data-ttu-id="9c1e1-169">Behåll den här nyckeln en hemlighet: alla som har åtkomst till data.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-169">Keep this key a secret: anyone who has it can access your data.</span></span>
   > 
   > 
4. <span data-ttu-id="9c1e1-170">I hello **egenskaper** avsnitt, kopiera hello **klient-ID**, och klistra in den i en säker plats.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-170">In hello **properties** section, copy hello **Client ID**, and then paste it into a safe location.</span></span> 

## <a name="authenticate-toomicrosoft-graph-and-query-hello-identity-risk-events-api"></a><span data-ttu-id="9c1e1-171">Autentisera tooMicrosoft diagram och fråga hello identitet Risk händelser API</span><span class="sxs-lookup"><span data-stu-id="9c1e1-171">Authenticate tooMicrosoft Graph and query hello Identity Risk Events API</span></span>
<span data-ttu-id="9c1e1-172">Du bör nu ha:</span><span class="sxs-lookup"><span data-stu-id="9c1e1-172">At this point, you should have:</span></span>

* <span data-ttu-id="9c1e1-173">hello klient-ID som du kopierade ovan</span><span class="sxs-lookup"><span data-stu-id="9c1e1-173">hello client ID you copied above</span></span>
* <span data-ttu-id="9c1e1-174">hello-nyckel som du kopierade ovan</span><span class="sxs-lookup"><span data-stu-id="9c1e1-174">hello key you copied above</span></span>
* <span data-ttu-id="9c1e1-175">hello namnet på din klient domän</span><span class="sxs-lookup"><span data-stu-id="9c1e1-175">hello name of your tenant's domain</span></span>

<span data-ttu-id="9c1e1-176">tooauthenticate skicka en post-begäran för`https://login.microsoft.com` med följande parametrar i hello brödtext hello:</span><span class="sxs-lookup"><span data-stu-id="9c1e1-176">tooauthenticate, send a post request too`https://login.microsoft.com` with hello following parameters in hello body:</span></span>

* <span data-ttu-id="9c1e1-177">grant_type ”:**client_credentials**”</span><span class="sxs-lookup"><span data-stu-id="9c1e1-177">grant_type: “**client_credentials**”</span></span>
* <span data-ttu-id="9c1e1-178">resurs ”:**https://graph.microsoft.com**”</span><span class="sxs-lookup"><span data-stu-id="9c1e1-178">resource: “**https://graph.microsoft.com**”</span></span>
* <span data-ttu-id="9c1e1-179">client_id:<your client ID></span><span class="sxs-lookup"><span data-stu-id="9c1e1-179">client_id: <your client ID></span></span>
* <span data-ttu-id="9c1e1-180">client_secret:<your key></span><span class="sxs-lookup"><span data-stu-id="9c1e1-180">client_secret: <your key></span></span>

> [!NOTE]
> <span data-ttu-id="9c1e1-181">Du behöver tooprovide värden för hello **client_id** och hello **client_secret** parameter.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-181">You need tooprovide values for hello **client_id** and hello **client_secret** parameter.</span></span>
> 
> 

<span data-ttu-id="9c1e1-182">Om det lyckas, returneras en autentiseringstoken.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-182">If successful, this returns an authentication token.</span></span>  
<span data-ttu-id="9c1e1-183">toocall hello API, skapa ett huvud med hello följande parameter:</span><span class="sxs-lookup"><span data-stu-id="9c1e1-183">toocall hello API, create a header with hello following parameter:</span></span>

    `Authorization`=”<token_type> <access_token>"


<span data-ttu-id="9c1e1-184">När autentisering, hittar du hello tokentypen och åtkomst-token i hello returnerade token.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-184">When authenticating, you can find hello token type and access token in hello returned token.</span></span>

<span data-ttu-id="9c1e1-185">Skicka detta huvud som en begäran toohello följande API-URL:`https://graph.microsoft.com/beta/identityRiskEvents`</span><span class="sxs-lookup"><span data-stu-id="9c1e1-185">Send this header as a request toohello following API URL: `https://graph.microsoft.com/beta/identityRiskEvents`</span></span>

<span data-ttu-id="9c1e1-186">hello svar, om det lyckas, är en samling identitet riskhändelser och tillhörande data i hello OData JSON-format som kan parsas och hanteras som passar.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-186">hello response, if successful, is a collection of identity risk events and associated data in hello OData JSON format, which can be parsed and handled as see fit.</span></span>

<span data-ttu-id="9c1e1-187">Här är exempelkod för autentisering och anropa hello-API med hjälp av Powershell.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-187">Here’s sample code for authenticating and calling hello API using Powershell.</span></span>  
<span data-ttu-id="9c1e1-188">Lägg till en klient-ID nyckeln och domän för innehavare.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-188">Just add your client ID, key, and tenant domain.</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="9c1e1-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9c1e1-189">Next steps</span></span>
<span data-ttu-id="9c1e1-190">Grattis, du skapat din första anropet tooMicrosoft diagram!</span><span class="sxs-lookup"><span data-stu-id="9c1e1-190">Congratulations, you just made your first call tooMicrosoft Graph!</span></span>  
<span data-ttu-id="9c1e1-191">Nu kan du fråga identitet riskhändelser och använder hello data men det är lämpligt.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-191">Now you can query identity risk events and use hello data however you see fit.</span></span>

<span data-ttu-id="9c1e1-192">toolearn mer om Microsoft Graph och hur toobuild program med hjälp av hello Graph API kolla hello [dokumentationen](https://graph.microsoft.io/docs) och mycket mer på hello [Microsoft Graph plats](https://graph.microsoft.io/).</span><span class="sxs-lookup"><span data-stu-id="9c1e1-192">toolearn more about Microsoft Graph and how toobuild applications using hello Graph API, check out hello [documentation](https://graph.microsoft.io/docs) and much more on hello [Microsoft Graph site](https://graph.microsoft.io/).</span></span> <span data-ttu-id="9c1e1-193">Se också till att toobookmark hello [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) sida som visar en lista över hello Identity Protection API: er finns i diagrammet.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-193">Also, make sure toobookmark hello [Azure AD Identity Protection API](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root) page that lists all of hello Identity Protection APIs available in Graph.</span></span> <span data-ttu-id="9c1e1-194">När vi lägger till nya sätt toowork med identitetsskydd via API ser dem på sidan.</span><span class="sxs-lookup"><span data-stu-id="9c1e1-194">As we add new ways toowork with Identity Protection via API, you’ll see them on that page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9c1e1-195">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="9c1e1-195">Additional resources</span></span>
* [<span data-ttu-id="9c1e1-196">Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="9c1e1-196">Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection.md)
* [<span data-ttu-id="9c1e1-197">Typer av riskhändelser som identifieras av Azure Active Directory Identity Protection</span><span class="sxs-lookup"><span data-stu-id="9c1e1-197">Types of risk events detected by Azure Active Directory Identity Protection</span></span>](active-directory-identityprotection-risk-events-types.md)
* [<span data-ttu-id="9c1e1-198">Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="9c1e1-198">Microsoft Graph</span></span>](https://graph.microsoft.io/)
* [<span data-ttu-id="9c1e1-199">Översikt över Microsoft Graph</span><span class="sxs-lookup"><span data-stu-id="9c1e1-199">Overview of Microsoft Graph</span></span>](https://graph.microsoft.io/docs)
* [<span data-ttu-id="9c1e1-200">Azure AD Identity Protection Service rot</span><span class="sxs-lookup"><span data-stu-id="9c1e1-200">Azure AD Identity Protection Service Root</span></span>](https://graph.microsoft.io/docs/api-reference/beta/resources/identityprotection_root)

