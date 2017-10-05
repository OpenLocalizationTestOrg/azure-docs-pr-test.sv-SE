---
title: "Förutsättningar för att få åtkomst till Azure AD reporting API. | Microsoft Docs"
description: "Lär dig mer om förutsättningar för att kunna komma åt Azure AD reporting API"
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
ms.openlocfilehash: 6e409fc56b77f37dac7f37382e664c577666ad4d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a><span data-ttu-id="1f081-104">Förutsättningar för att få åtkomst till Azure AD reporting API</span><span class="sxs-lookup"><span data-stu-id="1f081-104">Prerequisites to access the Azure AD reporting API</span></span>
<span data-ttu-id="1f081-105">Den [Azure AD reporting API: er](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) ger dig programmatisk åtkomst till data via en uppsättning REST-baserad API: er.</span><span class="sxs-lookup"><span data-stu-id="1f081-105">The [Azure AD reporting APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) provide you with programmatic access to the data through a set of REST-based APIs.</span></span> <span data-ttu-id="1f081-106">Du kan anropa API: erna från en mängd olika programmeringsspråk och verktyg.</span><span class="sxs-lookup"><span data-stu-id="1f081-106">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="1f081-107">Reporting API: N används [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) att bevilja åtkomst till webb-API: er.</span><span class="sxs-lookup"><span data-stu-id="1f081-107">The reporting API uses [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) to authorize access to the web APIs.</span></span> 

<span data-ttu-id="1f081-108">För att förbereda din åtkomst till reporting API, måste du:</span><span class="sxs-lookup"><span data-stu-id="1f081-108">To prepare your access to the reporting API, you must:</span></span>

1. <span data-ttu-id="1f081-109">Skapa ett program i Azure AD-klient</span><span class="sxs-lookup"><span data-stu-id="1f081-109">Create an application in your Azure AD tenant</span></span> 
2. <span data-ttu-id="1f081-110">Bevilja programmet behörighet att komma åt Azure AD-data</span><span class="sxs-lookup"><span data-stu-id="1f081-110">Grant the application appropriate permissions to access the Azure AD data</span></span>
3. <span data-ttu-id="1f081-111">Samla in konfigurationsinställningar från katalogen</span><span class="sxs-lookup"><span data-stu-id="1f081-111">Gather configuration settings from your directory</span></span>

<span data-ttu-id="1f081-112">För frågor, frågor eller kommentarer, kontakta [AAD Reporting hjälper](mailto:aadreportinghelp@microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="1f081-112">For questions, issues or feedback, please contact [AAD Reporting Help](mailto:aadreportinghelp@microsoft.com).</span></span>

## <a name="create-an-azure-ad-application"></a><span data-ttu-id="1f081-113">Skapa en Azure AD-program</span><span class="sxs-lookup"><span data-stu-id="1f081-113">Create an Azure AD application</span></span>
<span data-ttu-id="1f081-114">Om du vill konfigurera din katalog för att komma åt Azure AD reporting API, måste du logga in på den klassiska Azure-portalen med ett administratörskonto för Azure-prenumeration som även är medlem i rollen Global administratör katalog i Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="1f081-114">To configure your directory to access the Azure AD reporting API, you must sign in to the Azure classic portal with an Azure subscription administrator account that is also a member of the Global Administrator directory role in your Azure AD tenant.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1f081-115">Program som körs under autentiseringsuppgifterna med privilegier som ”admin” så här kan vara mycket kraftfulla, så kontrollera att det för att skydda programmets ID-hemlighet autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="1f081-115">Applications running under credentials with "admin" privileges like this can be very powerful, so please be sure to keep the application's ID/secret credentials secure.</span></span>
> 
> 

1. <span data-ttu-id="1f081-116">I den [klassiska Azure-portalen](https://manage.windowsazure.com), klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1f081-116">In the [Azure classic portal](https://manage.windowsazure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="1f081-118">Från den **active directory** väljer du din katalog.</span><span class="sxs-lookup"><span data-stu-id="1f081-118">From the **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="1f081-119">Klicka på menyn högst upp **program**.</span><span class="sxs-lookup"><span data-stu-id="1f081-119">In the menu on the top, click **Applications**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="1f081-121">Klicka på raden längst **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="1f081-121">On the bottom bar, click **Add**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/03.png) 
5. <span data-ttu-id="1f081-123">På den **vad vill du göra?** dialogrutan klickar du på **Lägg till ett program som min organisation utvecklar**.</span><span class="sxs-lookup"><span data-stu-id="1f081-123">On the **What do you want to do?** dialog, click **Add an application my organization is developing**.</span></span> 
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/04.png) 
6. <span data-ttu-id="1f081-125">På den **berätta om tillämpningsprogrammet** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1f081-125">On the **Tell us about your application** dialog, perform the following steps:</span></span> 
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/05.png) 
   
    <span data-ttu-id="1f081-127">a.</span><span class="sxs-lookup"><span data-stu-id="1f081-127">a.</span></span> <span data-ttu-id="1f081-128">I den **namn** textruta, ange ett namn (t.ex.: Reporting API-program).</span><span class="sxs-lookup"><span data-stu-id="1f081-128">In the **Name** textbox, type a name (e.g.: Reporting API Application).</span></span>
   
    <span data-ttu-id="1f081-129">b.</span><span class="sxs-lookup"><span data-stu-id="1f081-129">b.</span></span> <span data-ttu-id="1f081-130">Välj **webbprogram och/eller webb-API**.</span><span class="sxs-lookup"><span data-stu-id="1f081-130">Select **Web application and/or web API**.</span></span>
   
    <span data-ttu-id="1f081-131">c.</span><span class="sxs-lookup"><span data-stu-id="1f081-131">c.</span></span> <span data-ttu-id="1f081-132">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="1f081-132">Click **Next**.</span></span>
7. <span data-ttu-id="1f081-133">På den **appegenskaper** dialogrutan, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1f081-133">On the **App properties** dialog, perform the following steps:</span></span> 
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/06.png) 
   
    <span data-ttu-id="1f081-135">a.</span><span class="sxs-lookup"><span data-stu-id="1f081-135">a.</span></span> <span data-ttu-id="1f081-136">I den **inloggnings-URL** textruta typen `https://localhost`.</span><span class="sxs-lookup"><span data-stu-id="1f081-136">In the **Sign-on URL** textbox, type `https://localhost`.</span></span>
   
    <span data-ttu-id="1f081-137">b.</span><span class="sxs-lookup"><span data-stu-id="1f081-137">b.</span></span> <span data-ttu-id="1f081-138">I den **App-ID URI** textruta typen ```https://localhost/ReportingApiApp```.</span><span class="sxs-lookup"><span data-stu-id="1f081-138">In the **App ID URI** textbox, type ```https://localhost/ReportingApiApp```.</span></span>
   
    <span data-ttu-id="1f081-139">c.</span><span class="sxs-lookup"><span data-stu-id="1f081-139">c.</span></span> <span data-ttu-id="1f081-140">Klicka på **Complete** (Slutför).</span><span class="sxs-lookup"><span data-stu-id="1f081-140">Click **Complete**.</span></span>

## <a name="grant-your-application-permission-to-use-the-api"></a><span data-ttu-id="1f081-141">Ge ditt program behörighet att använda API: et</span><span class="sxs-lookup"><span data-stu-id="1f081-141">Grant your application permission to use the API</span></span>
1. <span data-ttu-id="1f081-142">I den [klassiska Azure-portalen](https://manage.windowsazure.com/), klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1f081-142">In the [Azure classic portal](https://manage.windowsazure.com/), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="1f081-144">Från den **active directory** väljer du din katalog.</span><span class="sxs-lookup"><span data-stu-id="1f081-144">From the **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="1f081-145">Klicka på menyn högst upp **program**.</span><span class="sxs-lookup"><span data-stu-id="1f081-145">In the menu on the top, click **Applications**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/02.png)
4. <span data-ttu-id="1f081-147">Välj ditt nya program i listan med program.</span><span class="sxs-lookup"><span data-stu-id="1f081-147">In the applications list, select your newly created application.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="1f081-149">Klicka på menyn högst upp **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="1f081-149">In the menu on the top, click **Configure**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="1f081-151">I den **behörigheter för andra program** avsnittet för den **Azure Active Directory** resurs, klicka på den **programbehörigheter** listrutan och välj sedan **läsa katalogdata**.</span><span class="sxs-lookup"><span data-stu-id="1f081-151">In the **Permissions to other applications** section, for the **Azure Active Directory** resource, click the **Application Permissions** drop-down list, and then select **Read directory data**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/09.png)
7. <span data-ttu-id="1f081-153">Klicka på raden längst **spara**.</span><span class="sxs-lookup"><span data-stu-id="1f081-153">On the bottom bar, click **Save**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/10.png)

## <a name="gather-configuration-settings-from-your-directory"></a><span data-ttu-id="1f081-155">Samla in konfigurationsinställningar från katalogen</span><span class="sxs-lookup"><span data-stu-id="1f081-155">Gather configuration settings from your directory</span></span>
<span data-ttu-id="1f081-156">Det här avsnittet visar hur du får följande inställningar från din katalog:</span><span class="sxs-lookup"><span data-stu-id="1f081-156">This section shows you how to get the following settings from your directory:</span></span>

* <span data-ttu-id="1f081-157">Domännamn</span><span class="sxs-lookup"><span data-stu-id="1f081-157">Domain name</span></span>
* <span data-ttu-id="1f081-158">Klient-ID</span><span class="sxs-lookup"><span data-stu-id="1f081-158">Client ID</span></span>
* <span data-ttu-id="1f081-159">Klienthemlighet</span><span class="sxs-lookup"><span data-stu-id="1f081-159">Client secret</span></span>

<span data-ttu-id="1f081-160">Du måste dessa värden när du konfigurerar anrop reporting-API: et.</span><span class="sxs-lookup"><span data-stu-id="1f081-160">You need these values when configuring calls to the reporting API.</span></span> 

### <a name="get-your-domain-name"></a><span data-ttu-id="1f081-161">Hämta ditt domännamn</span><span class="sxs-lookup"><span data-stu-id="1f081-161">Get your domain name</span></span>
1. <span data-ttu-id="1f081-162">I den [klassiska Azure-portalen](https://manage.windowsazure.com), klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1f081-162">In the [Azure classic portal](https://manage.windowsazure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="1f081-164">Från den **active directory** väljer du din katalog.</span><span class="sxs-lookup"><span data-stu-id="1f081-164">From the **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="1f081-165">Klicka på menyn högst upp **domäner**.</span><span class="sxs-lookup"><span data-stu-id="1f081-165">In the menu on the top, click **Domains**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/11.png) 
4. <span data-ttu-id="1f081-167">I den **domännamn** kolumn, kopiera ditt domännamn.</span><span class="sxs-lookup"><span data-stu-id="1f081-167">In the **Domain Name** column, copy your domain name.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/12.png) 

### <a name="get-the-applications-client-id"></a><span data-ttu-id="1f081-169">Hämta programmets klient-ID</span><span class="sxs-lookup"><span data-stu-id="1f081-169">Get the application's client ID</span></span>
1. <span data-ttu-id="1f081-170">I den [klassiska Azure-portalen](https://manage.windowsazure.com), klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1f081-170">In the [Azure classic portal](https://manage.windowsazure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="1f081-172">Från den **active directory** väljer du din katalog.</span><span class="sxs-lookup"><span data-stu-id="1f081-172">From the **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="1f081-173">Klicka på menyn högst upp **program**.</span><span class="sxs-lookup"><span data-stu-id="1f081-173">In the menu on the top, click **Applications**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="1f081-175">Välj ditt nya program i listan med program.</span><span class="sxs-lookup"><span data-stu-id="1f081-175">In the applications list, select your newly created application.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="1f081-177">Klicka på menyn högst upp **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="1f081-177">In the menu on the top, click **Configure**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="1f081-179">Kopiera ditt **klient-ID**.</span><span class="sxs-lookup"><span data-stu-id="1f081-179">Copy your **Client ID**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/13.png)

### <a name="get-the-applications-client-secret"></a><span data-ttu-id="1f081-181">Hämta programmets klienthemlighet</span><span class="sxs-lookup"><span data-stu-id="1f081-181">Get the application's client secret</span></span>
<span data-ttu-id="1f081-182">För att få ditt program klienthemlighet, måste du skapa en ny nyckel och spara dess värde vid sparas den nya nyckeln eftersom det inte går att hämta det här värdet senare längre.</span><span class="sxs-lookup"><span data-stu-id="1f081-182">To get your application's client secret, you need to create a new key and save its value upon saving the new key because it is not possible to retrieve this value later anymore.</span></span>

1. <span data-ttu-id="1f081-183">I den [klassiska Azure-portalen](https://manage.windowsazure.com), klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="1f081-183">In the [Azure classic portal](https://manage.windowsazure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/01.png) 
2. <span data-ttu-id="1f081-185">Från den **active directory** väljer du din katalog.</span><span class="sxs-lookup"><span data-stu-id="1f081-185">From the **active directory** list, select your directory.</span></span>
3. <span data-ttu-id="1f081-186">Klicka på menyn högst upp **program**.</span><span class="sxs-lookup"><span data-stu-id="1f081-186">In the menu on the top, click **Applications**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/02.png) 
4. <span data-ttu-id="1f081-188">Välj ditt nya program i listan med program.</span><span class="sxs-lookup"><span data-stu-id="1f081-188">In the applications list, select your newly created application.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/07.png)
5. <span data-ttu-id="1f081-190">Klicka på menyn högst upp **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="1f081-190">In the menu on the top, click **Configure**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/08.png)
6. <span data-ttu-id="1f081-192">I den **nycklar** avsnittet, utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="1f081-192">In the **Keys** section, perform the following steps:</span></span> 
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/14.png)
   
    <span data-ttu-id="1f081-194">a.</span><span class="sxs-lookup"><span data-stu-id="1f081-194">a.</span></span> <span data-ttu-id="1f081-195">Markera en varaktighet från listan varaktighet</span><span class="sxs-lookup"><span data-stu-id="1f081-195">From the duration list, select a duration</span></span>
   
    <span data-ttu-id="1f081-196">b.</span><span class="sxs-lookup"><span data-stu-id="1f081-196">b.</span></span> <span data-ttu-id="1f081-197">Klicka på raden längst **spara**.</span><span class="sxs-lookup"><span data-stu-id="1f081-197">On the bottom bar, click **Save**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites/10.png)
   
    <span data-ttu-id="1f081-199">c.</span><span class="sxs-lookup"><span data-stu-id="1f081-199">c.</span></span> <span data-ttu-id="1f081-200">Kopiera värdet för nyckeln.</span><span class="sxs-lookup"><span data-stu-id="1f081-200">Copy the key value.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1f081-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1f081-201">Next Steps</span></span>
* <span data-ttu-id="1f081-202">Vill du komma åt data från Azure AD reporting API på ett programmässiga sätt?</span><span class="sxs-lookup"><span data-stu-id="1f081-202">Would you like to access the data from the Azure AD reporting API in a programmatic manner?</span></span> <span data-ttu-id="1f081-203">Checka ut [komma igång med Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="1f081-203">Check out [Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="1f081-204">Om du vill veta mer om Azure Active Directory reporting finns i [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="1f081-204">If you would like to find out more about Azure Active Directory reporting, see the [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

