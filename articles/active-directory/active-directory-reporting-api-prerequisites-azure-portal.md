---
title: aaaPrerequisites tooaccess hello Azure AD reporting API | Microsoft Docs
description: "Lär dig mer om hello krav tooaccess hello Azure AD reporting API"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: ada19f69-665c-452a-8452-701029bf4252
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.reviewer: dhanyahk
ms.openlocfilehash: ec28a7530f341dda31268a978754b615c727d66f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prerequisites-tooaccess-hello-azure-ad-reporting-api"></a><span data-ttu-id="bcebb-103">Förutsättningar tooaccess hello Azure AD reporting API</span><span class="sxs-lookup"><span data-stu-id="bcebb-103">Prerequisites tooaccess hello Azure AD reporting API</span></span>

<span data-ttu-id="bcebb-104">Hej [Azure AD reporting API: er](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) ger dig programmatisk åtkomst toohello data via en uppsättning REST-baserad API: er.</span><span class="sxs-lookup"><span data-stu-id="bcebb-104">hello [Azure AD reporting APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) provide you with programmatic access toohello data through a set of REST-based APIs.</span></span> <span data-ttu-id="bcebb-105">Du kan anropa API: erna från en mängd olika programmeringsspråk och verktyg.</span><span class="sxs-lookup"><span data-stu-id="bcebb-105">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="bcebb-106">hello reporting API: N används [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize åtkomst toohello web API: er.</span><span class="sxs-lookup"><span data-stu-id="bcebb-106">hello reporting API uses [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) tooauthorize access toohello web APIs.</span></span> 

<span data-ttu-id="bcebb-107">tooget dataåtkomst toohello reporting via hello API måste du toohave en hello följande roller:</span><span class="sxs-lookup"><span data-stu-id="bcebb-107">tooget access toohello reporting data through hello API, you need toohave one of hello following roles assigned:</span></span>

- <span data-ttu-id="bcebb-108">Säkerhet läsare</span><span class="sxs-lookup"><span data-stu-id="bcebb-108">Security Reader</span></span>
- <span data-ttu-id="bcebb-109">Säkerhet Admin</span><span class="sxs-lookup"><span data-stu-id="bcebb-109">Security Admin</span></span>
- <span data-ttu-id="bcebb-110">Global administratör</span><span class="sxs-lookup"><span data-stu-id="bcebb-110">Global Admin</span></span>


<span data-ttu-id="bcebb-111">tooprepare din åtkomst toohello reporting API, måste du:</span><span class="sxs-lookup"><span data-stu-id="bcebb-111">tooprepare your access toohello reporting API, you must:</span></span>

1. <span data-ttu-id="bcebb-112">Registrera ett program</span><span class="sxs-lookup"><span data-stu-id="bcebb-112">Register an application</span></span> 
2. <span data-ttu-id="bcebb-113">Bevilja behörighet</span><span class="sxs-lookup"><span data-stu-id="bcebb-113">Grant permissions</span></span> 
3. <span data-ttu-id="bcebb-114">Samla in konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="bcebb-114">Gather configuration settings</span></span> 

<span data-ttu-id="bcebb-115">Frågor, frågor eller kommentarer finns [filen ett supportärende](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).</span><span class="sxs-lookup"><span data-stu-id="bcebb-115">For questions, issues or feedback, please [file a support ticket](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).</span></span>

## <a name="register-an-azure-active-directory-application"></a><span data-ttu-id="bcebb-116">Registrera ett Azure Active Directory-program</span><span class="sxs-lookup"><span data-stu-id="bcebb-116">Register an Azure Active Directory application</span></span>

<span data-ttu-id="bcebb-117">Tooregister en app måste även om du ansluter till hello reporting API: et med ett skript.</span><span class="sxs-lookup"><span data-stu-id="bcebb-117">You need tooregister an app even if you are accessing hello reporting API using a script.</span></span> <span data-ttu-id="bcebb-118">Detta ger dig en **program-ID**, vilket krävs för ett anrop för auktorisering och den gör att din kod tooreceive token.</span><span class="sxs-lookup"><span data-stu-id="bcebb-118">This gives you an **Application ID**, which is required for an authorization call and it enables your code tooreceive tokens.</span></span>

<span data-ttu-id="bcebb-119">tooconfigure din katalog tooaccess hello Azure AD reporting API, måste du logga in toohello Azure-portalen med Azure-administratörskontot som även är medlem i hello **Global administratör** directory roll i Azure AD-klient .</span><span class="sxs-lookup"><span data-stu-id="bcebb-119">tooconfigure your directory tooaccess hello Azure AD reporting API, you must sign in toohello Azure portal with an Azure administrator account that is also a member of hello **Global Administrator** directory role in your Azure AD tenant.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bcebb-120">Program som körs under autentiseringsuppgifterna med privilegier som ”admin” så här kan vara mycket kraftfulla, så du vara säker på att tookeep hello programmets ID-hemlighet autentiseringsuppgifter säker.</span><span class="sxs-lookup"><span data-stu-id="bcebb-120">Applications running under credentials with "admin" privileges like this can be very powerful, so please be sure tookeep hello application's ID/secret credentials secure.</span></span>
> 


<span data-ttu-id="bcebb-121">**tooregister ett Azure Active Directory-program:**</span><span class="sxs-lookup"><span data-stu-id="bcebb-121">**tooregister an Azure Active Directory application:**</span></span>

1. <span data-ttu-id="bcebb-122">I hello [Azure-portalen](https://portal.azure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-122">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="bcebb-124">På hello **Azure Active Directory** bladet, klickar du på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-124">On hello **Azure Active Directory** blade, click **App registrations**.</span></span>

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/02.png) 

3. <span data-ttu-id="bcebb-126">På hello **App registreringar** bladet i hello verktygsfältet hello överst, klickar du på **nya appregistrering**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-126">On hello **App registrations** blade, in hello toolbar on hello top, click **New application registration**.</span></span>

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/03.png)

4. <span data-ttu-id="bcebb-128">På hello **skapa** bladet utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bcebb-128">On hello **Create** blade, perform hello following steps:</span></span>

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/04.png)

    <span data-ttu-id="bcebb-130">a.</span><span class="sxs-lookup"><span data-stu-id="bcebb-130">a.</span></span> <span data-ttu-id="bcebb-131">I hello **namn** textruta typen `Reporting API application`.</span><span class="sxs-lookup"><span data-stu-id="bcebb-131">In hello **Name** textbox, type `Reporting API application`.</span></span>

    <span data-ttu-id="bcebb-132">b.</span><span class="sxs-lookup"><span data-stu-id="bcebb-132">b.</span></span> <span data-ttu-id="bcebb-133">Som **programtyp**väljer **webbapp / API**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-133">As **Application type**, select **Web app / API**.</span></span>

    <span data-ttu-id="bcebb-134">c.</span><span class="sxs-lookup"><span data-stu-id="bcebb-134">c.</span></span> <span data-ttu-id="bcebb-135">I hello **inloggnings-URL** textruta typen `https://localhost`.</span><span class="sxs-lookup"><span data-stu-id="bcebb-135">In hello **Sign-on URL** textbox, type `https://localhost`.</span></span>

    <span data-ttu-id="bcebb-136">d.</span><span class="sxs-lookup"><span data-stu-id="bcebb-136">d.</span></span> <span data-ttu-id="bcebb-137">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-137">Click **Create**.</span></span> 


## <a name="grant-permissions"></a><span data-ttu-id="bcebb-138">Bevilja behörighet</span><span class="sxs-lookup"><span data-stu-id="bcebb-138">Grant permissions</span></span> 

<span data-ttu-id="bcebb-139">hello syftet med det här steget är toogrant programmet **läsa katalogdata** behörigheter toohello **Windows Azure Active Directory** API.</span><span class="sxs-lookup"><span data-stu-id="bcebb-139">hello objective of this step is toogrant your application **Read directory data** permissions toohello **Windows Azure Active Directory** API.</span></span>

![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/16.png)
 

<span data-ttu-id="bcebb-141">**toogrant ditt program behörighet toouse hello API:**</span><span class="sxs-lookup"><span data-stu-id="bcebb-141">**toogrant your application permission toouse hello API:**</span></span>

1. <span data-ttu-id="bcebb-142">På hello **App registreringar** klickar du på bladet i hello applista **Reporting API-program**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-142">On hello **App registrations** blade, in hello apps list, click **Reporting API application**.</span></span>

2. <span data-ttu-id="bcebb-143">På hello **Reporting API-program** bladet i hello verktygsfältet hello överst, klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-143">On hello **Reporting API application** blade, in hello toolbar on hello top, click **Settings**.</span></span> 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

3. <span data-ttu-id="bcebb-145">På hello **inställningar** bladet, klickar du på **nödvändiga behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-145">On hello **Settings** blade, click **Required permissions**.</span></span> 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/06.png)

4. <span data-ttu-id="bcebb-147">På hello **nödvändiga behörigheter** bladet i hello **API** klickar du på **Windows Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-147">On hello **Required permissions** blade, in hello **API** list, click **Windows Azure Active Directory**.</span></span> 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/07.png)

5. <span data-ttu-id="bcebb-149">På hello **Aktivera åtkomst** bladet väljer **läsa katalogdata**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-149">On hello **Enable Access** blade, select **Read directory data**.</span></span> 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/08.png)

6. <span data-ttu-id="bcebb-151">Klicka i hello verktygsfältet hello längst upp **spara**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-151">In hello toolbar on hello top, click **Save**.</span></span>

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/15.png)

## <a name="gather-configuration-settings"></a><span data-ttu-id="bcebb-153">Samla in konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="bcebb-153">Gather configuration settings</span></span> 
<span data-ttu-id="bcebb-154">Det här avsnittet beskrivs hur du tooget hello följande inställningar från din katalog:</span><span class="sxs-lookup"><span data-stu-id="bcebb-154">This section shows you how tooget hello following settings from your directory:</span></span>

* <span data-ttu-id="bcebb-155">Domännamn</span><span class="sxs-lookup"><span data-stu-id="bcebb-155">Domain name</span></span>
* <span data-ttu-id="bcebb-156">Klient-ID</span><span class="sxs-lookup"><span data-stu-id="bcebb-156">Client ID</span></span>
* <span data-ttu-id="bcebb-157">Klienthemlighet</span><span class="sxs-lookup"><span data-stu-id="bcebb-157">Client secret</span></span>

<span data-ttu-id="bcebb-158">Du måste dessa värden när du konfigurerar anrop toohello reporting API.</span><span class="sxs-lookup"><span data-stu-id="bcebb-158">You need these values when configuring calls toohello reporting API.</span></span> 

### <a name="get-your-domain-name"></a><span data-ttu-id="bcebb-159">Hämta ditt domännamn</span><span class="sxs-lookup"><span data-stu-id="bcebb-159">Get your domain name</span></span>

<span data-ttu-id="bcebb-160">**tooget ditt domännamn:**</span><span class="sxs-lookup"><span data-stu-id="bcebb-160">**tooget your domain name:**</span></span>

1. <span data-ttu-id="bcebb-161">I hello [Azure-portalen](https://portal.azure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-161">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="bcebb-163">På hello **Azure Active Directory** bladet, klickar du på **domännamn**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-163">On hello **Azure Active Directory** blade, click **Domain names**.</span></span>

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/09.png) 

3. <span data-ttu-id="bcebb-165">Kopiera ditt domännamn hello listan över domäner.</span><span class="sxs-lookup"><span data-stu-id="bcebb-165">Copy your domain name from hello list of domains.</span></span>


### <a name="get-your-applications-client-id"></a><span data-ttu-id="bcebb-166">Hämta programmets klient-ID</span><span class="sxs-lookup"><span data-stu-id="bcebb-166">Get your application's client ID</span></span>

<span data-ttu-id="bcebb-167">**tooget programmets klient-ID:**</span><span class="sxs-lookup"><span data-stu-id="bcebb-167">**tooget your application's client ID:**</span></span>

1. <span data-ttu-id="bcebb-168">I hello [Azure-portalen](https://portal.azure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-168">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="bcebb-170">På hello **App registreringar** klickar du på bladet i hello applista **Reporting API-program**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-170">On hello **App registrations** blade, in hello apps list, click **Reporting API application**.</span></span>

3. <span data-ttu-id="bcebb-171">På hello **Reporting API-program** bladet på hello **program-ID**, klickar du på **klickar du på toocopy**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-171">On hello **Reporting API application** blade, at hello **Application ID**, click **Click toocopy**.</span></span>

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/11.png) 



### <a name="get-your-applications-client-secret"></a><span data-ttu-id="bcebb-173">Hämta programmets klienthemlighet</span><span class="sxs-lookup"><span data-stu-id="bcebb-173">Get your application's client secret</span></span>
<span data-ttu-id="bcebb-174">tooget programmets klienten hemliga, du behöver toocreate en ny nyckel och spara dess värde vid spara hello ny nyckel eftersom det inte är möjligt tooretrieve det här värdet senare längre.</span><span class="sxs-lookup"><span data-stu-id="bcebb-174">tooget your application's client secret, you need toocreate a new key and save its value upon saving hello new key because it is not possible tooretrieve this value later anymore.</span></span>

<span data-ttu-id="bcebb-175">**tooget klienthemlighet för ditt program:**</span><span class="sxs-lookup"><span data-stu-id="bcebb-175">**tooget your application's client secret:**</span></span>

1. <span data-ttu-id="bcebb-176">I hello [Azure-portalen](https://portal.azure.com), på hello vänstra navigeringsfönstret, klicka på **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-176">In hello [Azure portal](https://portal.azure.com), on hello left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="bcebb-178">På hello **App registreringar** klickar du på bladet i hello applista **Reporting API-program**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-178">On hello **App registrations** blade, in hello apps list, click **Reporting API application**.</span></span>


3. <span data-ttu-id="bcebb-179">På hello **Reporting API-program** bladet i hello verktygsfältet hello överst, klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-179">On hello **Reporting API application** blade, in hello toolbar on hello top, click **Settings**.</span></span> 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

4. <span data-ttu-id="bcebb-181">På hello **inställningar** bladet i hello **APIR åtkomst** klickar du på **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-181">On hello **Settings** blade, in hello **APIR Access** section, click **Keys**.</span></span> 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/12.png)


5. <span data-ttu-id="bcebb-183">På hello **nycklar** bladet utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="bcebb-183">On hello **Keys** blade, perform hello following steps:</span></span>

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/14.png)

    <span data-ttu-id="bcebb-185">a.</span><span class="sxs-lookup"><span data-stu-id="bcebb-185">a.</span></span> <span data-ttu-id="bcebb-186">I hello **beskrivning** textruta typen `Reporting API`.</span><span class="sxs-lookup"><span data-stu-id="bcebb-186">In hello **Description** textbox, type `Reporting API`.</span></span>

    <span data-ttu-id="bcebb-187">b.</span><span class="sxs-lookup"><span data-stu-id="bcebb-187">b.</span></span> <span data-ttu-id="bcebb-188">Som **Expires**väljer **i två år**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-188">As **Expires**, select **In 2 years**.</span></span>

    <span data-ttu-id="bcebb-189">c.</span><span class="sxs-lookup"><span data-stu-id="bcebb-189">c.</span></span> <span data-ttu-id="bcebb-190">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="bcebb-190">Click **Save**.</span></span>

    <span data-ttu-id="bcebb-191">d.</span><span class="sxs-lookup"><span data-stu-id="bcebb-191">d.</span></span> <span data-ttu-id="bcebb-192">Kopiera hello nyckel/värde.</span><span class="sxs-lookup"><span data-stu-id="bcebb-192">Copy hello key value.</span></span>


## <a name="next-steps"></a><span data-ttu-id="bcebb-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bcebb-193">Next Steps</span></span>
* <span data-ttu-id="bcebb-194">Skulle du som tooaccess hello data från hello Azure AD reporting API på ett programmässiga sätt?</span><span class="sxs-lookup"><span data-stu-id="bcebb-194">Would you like tooaccess hello data from hello Azure AD reporting API in a programmatic manner?</span></span> <span data-ttu-id="bcebb-195">Checka ut [komma igång med hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="bcebb-195">Check out [Getting started with hello Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="bcebb-196">Om du vill toofind mer information om Azure Active Directory reporting finns hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="bcebb-196">If you would like toofind out more about Azure Active Directory reporting, see hello [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

