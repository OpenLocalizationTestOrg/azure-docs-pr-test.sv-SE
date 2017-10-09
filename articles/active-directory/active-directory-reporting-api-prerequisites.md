---
title: aaaPrerequisites tooaccess hello Azure AD reporting API. | Microsoft Docs
description: "Lär dig mer om hello krav tooaccess hello Azure AD reporting API"
services: active-directory
documentationcenter: 
author: dhanyahk
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: dhanyahk;markvi
ms.openlocfilehash: e9d7ceaedb07d18fbd75b70d68b5cfbebc756c36
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prerequisites-tooaccess-hello-azure-ad-reporting-api"></a><span data-ttu-id="f8791-104">Förutsättningar tooaccess hello Azure AD reporting API</span><span class="sxs-lookup"><span data-stu-id="f8791-104">Prerequisites tooaccess hello Azure AD reporting API</span></span>
<span data-ttu-id="f8791-105">Hej [Azure AD reporting API: er](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) ger dig programmatisk åtkomst toohello data via en uppsättning REST-baserad API: er.</span><span class="sxs-lookup"><span data-stu-id="f8791-105">hello [Azure AD reporting APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) provide you with programmatic access toohello data through a set of REST-based APIs.</span></span> <span data-ttu-id="f8791-106">Du kan anropa API: erna från en mängd olika programmeringsspråk och verktyg.</span><span class="sxs-lookup"><span data-stu-id="f8791-106">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="f8791-107">hello reporting API: N används [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize åtkomst toohello web API: er.</span><span class="sxs-lookup"><span data-stu-id="f8791-107">hello reporting API uses [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize access toohello web APIs.</span></span> 

<span data-ttu-id="f8791-108">tooprepare din åtkomst toohello reporting API, måste du:</span><span class="sxs-lookup"><span data-stu-id="f8791-108">tooprepare your access toohello reporting API, you must:</span></span>

1. <span data-ttu-id="f8791-109">Skapa ett program i Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="f8791-109">Create an application in your Azure AD tenant</span></span> 
2. <span data-ttu-id="f8791-110">Bevilja hello program behörighet tooaccess hello Azure AD-data</span><span class="sxs-lookup"><span data-stu-id="f8791-110">Grant hello application appropriate permissions tooaccess hello Azure AD data</span></span>
3. <span data-ttu-id="f8791-111">Samla in konfigurationsinställningar från katalogen</span><span class="sxs-lookup"><span data-stu-id="f8791-111">Gather configuration settings from your directory</span></span>

<span data-ttu-id="f8791-112">För frågor, frågor eller kommentarer, kontakta [AAD Reporting hjälper](mailto:aadreportinghelp@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="f8791-112">For questions, issues or feedback, please contact [AAD Reporting Help](mailto:aadreportinghelp@microsoft.com).</span></span>

## <a name="create-an-azure-ad-application"></a><span data-ttu-id="f8791-113">Skapa en Azure AD-program</span><span class="sxs-lookup"><span data-stu-id="f8791-113">Create an Azure AD application</span></span>
<span data-ttu-id="f8791-114">tooconfigure din katalog tooaccess hello Azure AD reporting API, måste du logga in toohello klassiska Azure-portalen med ett administratörskonto för Azure-prenumeration som även är medlem i rollen hello Global administratör katalog i Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="f8791-114">tooconfigure your directory tooaccess hello Azure AD reporting API, you must sign in toohello Azure classic portal with an Azure subscription administrator account that is also a member of hello Global Administrator directory role in your Azure AD tenant.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f8791-115">Program som körs under autentiseringsuppgifterna med privilegier som ”admin” så här kan vara mycket kraftfulla, så du vara säker på att tookeep hello programmets ID-hemlighet autentiseringsuppgifter säker.</span><span class="sxs-lookup"><span data-stu-id="f8791-115">Applications running under credentials with "admin" privileges like this can be very powerful, so please be sure tookeep hello application's ID/secret credentials secure.</span></span>
> 
> 

1. <span data-ttu-id="f8791-116">I hello [klassiska Azure-portalen](https://manage.windowsazure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f8791-116">In hello [Azure classic portal](https://manage.windowsazure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="f8791-118">Från hello **active directory** väljer du din katalog.</span><span class="sxs-lookup"><span data-stu-id="f8791-118">From hello **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="f8791-119">Hello-menyn överst hello **program**.</span><span class="sxs-lookup"><span data-stu-id="f8791-119">In hello menu on hello top, click **Applications**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="f8791-121">Klicka på hello nedre **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="f8791-121">On hello bottom bar, click **Add**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/03.png) 
5. <span data-ttu-id="f8791-123">På hello **vad vill du vill toodo?** dialogrutan klickar du på **Lägg till ett program som min organisation utvecklar**.</span><span class="sxs-lookup"><span data-stu-id="f8791-123">On hello **What do you want toodo?** dialog, click **Add an application my organization is developing**.</span></span> 
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/04.png) 
6. <span data-ttu-id="f8791-125">På hello **berätta om tillämpningsprogrammet** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f8791-125">On hello **Tell us about your application** dialog, perform hello following steps:</span></span> 
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/05.png) 
   
    <span data-ttu-id="f8791-127">a.</span><span class="sxs-lookup"><span data-stu-id="f8791-127">a.</span></span> <span data-ttu-id="f8791-128">I hello **namn** textruta, ange ett namn (t.ex.: Reporting API-program).</span><span class="sxs-lookup"><span data-stu-id="f8791-128">In hello **Name** textbox, type a name (e.g.: Reporting API Application).</span></span>
   
    <span data-ttu-id="f8791-129">b.</span><span class="sxs-lookup"><span data-stu-id="f8791-129">b.</span></span> <span data-ttu-id="f8791-130">Välj **webbprogram och/eller webb-API**.</span><span class="sxs-lookup"><span data-stu-id="f8791-130">Select **Web application and/or web API**.</span></span>
   
    <span data-ttu-id="f8791-131">c.</span><span class="sxs-lookup"><span data-stu-id="f8791-131">c.</span></span> <span data-ttu-id="f8791-132">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="f8791-132">Click **Next**.</span></span>
7. <span data-ttu-id="f8791-133">På hello **appegenskaper** dialogrutan utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f8791-133">On hello **App properties** dialog, perform hello following steps:</span></span> 
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/06.png) 
   
    <span data-ttu-id="f8791-135">a.</span><span class="sxs-lookup"><span data-stu-id="f8791-135">a.</span></span> <span data-ttu-id="f8791-136">I hello **inloggnings-URL** textruta typen `https://localhost`.</span><span class="sxs-lookup"><span data-stu-id="f8791-136">In hello **Sign-on URL** textbox, type `https://localhost`.</span></span>
   
    <span data-ttu-id="f8791-137">b.</span><span class="sxs-lookup"><span data-stu-id="f8791-137">b.</span></span> <span data-ttu-id="f8791-138">I hello **App-ID URI** textruta typen ```https://localhost/ReportingApiApp```.</span><span class="sxs-lookup"><span data-stu-id="f8791-138">In hello **App ID URI** textbox, type ```https://localhost/ReportingApiApp```.</span></span>
   
    <span data-ttu-id="f8791-139">c.</span><span class="sxs-lookup"><span data-stu-id="f8791-139">c.</span></span> <span data-ttu-id="f8791-140">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="f8791-140">Click **Complete**.</span></span>

## <a name="grant-your-application-permission-toouse-hello-api"></a><span data-ttu-id="f8791-141">Ge ditt program behörighet toouse hello API</span><span class="sxs-lookup"><span data-stu-id="f8791-141">Grant your application permission toouse hello API</span></span>
1. <span data-ttu-id="f8791-142">I hello [klassiska Azure-portalen](https://manage.windowsazure.com/), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f8791-142">In hello [Azure classic portal](https://manage.windowsazure.com/), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="f8791-144">Från hello **active directory** väljer du din katalog.</span><span class="sxs-lookup"><span data-stu-id="f8791-144">From hello **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="f8791-145">Hello-menyn överst hello **program**.</span><span class="sxs-lookup"><span data-stu-id="f8791-145">In hello menu on hello top, click **Applications**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/02.png)
4. <span data-ttu-id="f8791-147">Välj ditt nya program i listan med program hello.</span><span class="sxs-lookup"><span data-stu-id="f8791-147">In hello applications list, select your newly created application.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="f8791-149">Hello-menyn överst hello **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="f8791-149">In hello menu on hello top, click **Configure**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="f8791-151">I hello **behörigheter tooother program** för hello **Azure Active Directory** resurs, klicka på hello **programbehörigheter** listrutan, och sedan Välj **läsa katalogdata**.</span><span class="sxs-lookup"><span data-stu-id="f8791-151">In hello **Permissions tooother applications** section, for hello **Azure Active Directory** resource, click hello **Application Permissions** drop-down list, and then select **Read directory data**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/09.png)
7. <span data-ttu-id="f8791-153">Klicka på hello nedre **spara**.</span><span class="sxs-lookup"><span data-stu-id="f8791-153">On hello bottom bar, click **Save**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/10.png)

## <a name="gather-configuration-settings-from-your-directory"></a><span data-ttu-id="f8791-155">Samla in konfigurationsinställningar från katalogen</span><span class="sxs-lookup"><span data-stu-id="f8791-155">Gather configuration settings from your directory</span></span>
<span data-ttu-id="f8791-156">Det här avsnittet beskrivs hur du tooget hello följande inställningar från din katalog:</span><span class="sxs-lookup"><span data-stu-id="f8791-156">This section shows you how tooget hello following settings from your directory:</span></span>

* <span data-ttu-id="f8791-157">Domännamn</span><span class="sxs-lookup"><span data-stu-id="f8791-157">Domain name</span></span>
* <span data-ttu-id="f8791-158">Klient-ID</span><span class="sxs-lookup"><span data-stu-id="f8791-158">Client ID</span></span>
* <span data-ttu-id="f8791-159">Klienthemlighet</span><span class="sxs-lookup"><span data-stu-id="f8791-159">Client secret</span></span>

<span data-ttu-id="f8791-160">Du måste dessa värden när du konfigurerar anrop toohello reporting API.</span><span class="sxs-lookup"><span data-stu-id="f8791-160">You need these values when configuring calls toohello reporting API.</span></span> 

### <a name="get-your-domain-name"></a><span data-ttu-id="f8791-161">Hämta ditt domännamn</span><span class="sxs-lookup"><span data-stu-id="f8791-161">Get your domain name</span></span>
1. <span data-ttu-id="f8791-162">I hello [klassiska Azure-portalen](https://manage.windowsazure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f8791-162">In hello [Azure classic portal](https://manage.windowsazure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="f8791-164">Från hello **active directory** väljer du din katalog.</span><span class="sxs-lookup"><span data-stu-id="f8791-164">From hello **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="f8791-165">Hello-menyn överst hello **domäner**.</span><span class="sxs-lookup"><span data-stu-id="f8791-165">In hello menu on hello top, click **Domains**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/11.png) 
4. <span data-ttu-id="f8791-167">I hello **domännamn** kolumn, kopiera ditt domännamn.</span><span class="sxs-lookup"><span data-stu-id="f8791-167">In hello **Domain Name** column, copy your domain name.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/12.png) 

### <a name="get-hello-applications-client-id"></a><span data-ttu-id="f8791-169">Hämta hello programmets klient-ID</span><span class="sxs-lookup"><span data-stu-id="f8791-169">Get hello application's client ID</span></span>
1. <span data-ttu-id="f8791-170">I hello [klassiska Azure-portalen](https://manage.windowsazure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f8791-170">In hello [Azure classic portal](https://manage.windowsazure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="f8791-172">Från hello **active directory** väljer du din katalog.</span><span class="sxs-lookup"><span data-stu-id="f8791-172">From hello **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="f8791-173">Hello-menyn överst hello **program**.</span><span class="sxs-lookup"><span data-stu-id="f8791-173">In hello menu on hello top, click **Applications**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="f8791-175">Välj ditt nya program i listan med program hello.</span><span class="sxs-lookup"><span data-stu-id="f8791-175">In hello applications list, select your newly created application.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="f8791-177">Hello-menyn överst hello **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="f8791-177">In hello menu on hello top, click **Configure**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="f8791-179">Kopiera ditt **klient-ID**.</span><span class="sxs-lookup"><span data-stu-id="f8791-179">Copy your **Client ID**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/13.png)

### <a name="get-hello-applications-client-secret"></a><span data-ttu-id="f8791-181">Skaffa hello programmet klienthemlighet</span><span class="sxs-lookup"><span data-stu-id="f8791-181">Get hello application's client secret</span></span>
<span data-ttu-id="f8791-182">tooget programmets klienten hemliga, du behöver toocreate en ny nyckel och spara dess värde vid spara hello ny nyckel eftersom det inte är möjligt tooretrieve det här värdet senare längre.</span><span class="sxs-lookup"><span data-stu-id="f8791-182">tooget your application's client secret, you need toocreate a new key and save its value upon saving hello new key because it is not possible tooretrieve this value later anymore.</span></span>

1. <span data-ttu-id="f8791-183">I hello [klassiska Azure-portalen](https://manage.windowsazure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="f8791-183">In hello [Azure classic portal](https://manage.windowsazure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="f8791-185">Från hello **active directory** väljer du din katalog.</span><span class="sxs-lookup"><span data-stu-id="f8791-185">From hello **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="f8791-186">Hello-menyn överst hello **program**.</span><span class="sxs-lookup"><span data-stu-id="f8791-186">In hello menu on hello top, click **Applications**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="f8791-188">Välj ditt nya program i listan med program hello.</span><span class="sxs-lookup"><span data-stu-id="f8791-188">In hello applications list, select your newly created application.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="f8791-190">Hello-menyn överst hello **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="f8791-190">In hello menu on hello top, click **Configure**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="f8791-192">I hello **nycklar** avsnittet, utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f8791-192">In hello **Keys** section, perform hello following steps:</span></span> 
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/14.png)
   
    <span data-ttu-id="f8791-194">a.</span><span class="sxs-lookup"><span data-stu-id="f8791-194">a.</span></span> <span data-ttu-id="f8791-195">Välj en varaktighet hello varaktighet listan</span><span class="sxs-lookup"><span data-stu-id="f8791-195">From hello duration list, select a duration</span></span>
   
    <span data-ttu-id="f8791-196">b.</span><span class="sxs-lookup"><span data-stu-id="f8791-196">b.</span></span> <span data-ttu-id="f8791-197">Klicka på hello nedre **spara**.</span><span class="sxs-lookup"><span data-stu-id="f8791-197">On hello bottom bar, click **Save**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/10.png)
   
    <span data-ttu-id="f8791-199">c.</span><span class="sxs-lookup"><span data-stu-id="f8791-199">c.</span></span> <span data-ttu-id="f8791-200">Kopiera hello nyckel/värde.</span><span class="sxs-lookup"><span data-stu-id="f8791-200">Copy hello key value.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f8791-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f8791-201">Next Steps</span></span>
* <span data-ttu-id="f8791-202">Skulle du som tooaccess hello data från hello Azure AD reporting API på ett programmässiga sätt?</span><span class="sxs-lookup"><span data-stu-id="f8791-202">Would you like tooaccess hello data from hello Azure AD reporting API in a programmatic manner?</span></span> <span data-ttu-id="f8791-203">Checka ut [komma igång med hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="f8791-203">Check out [Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="f8791-204">Om du vill toofind mer information om Azure Active Directory reporting finns hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="f8791-204">If you would like toofind out more about Azure Active Directory reporting, see hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

