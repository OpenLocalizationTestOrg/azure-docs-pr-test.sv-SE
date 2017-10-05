---
title: "Förutsättningar för att få åtkomst till Azure AD reporting API | Microsoft Docs"
description: "Lär dig mer om förutsättningar för att kunna komma åt Azure AD reporting API"
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
ms.openlocfilehash: 5fafd83c337e3c73260d89cdad7409a01ce5855b
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="prerequisites-to-access-the-azure-ad-reporting-api"></a><span data-ttu-id="a1078-103">Förutsättningar för att få åtkomst till Azure AD reporting API</span><span class="sxs-lookup"><span data-stu-id="a1078-103">Prerequisites to access the Azure AD reporting API</span></span>

<span data-ttu-id="a1078-104">Den [Azure AD reporting API: er](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) ger dig programmatisk åtkomst till data via en uppsättning REST-baserad API: er.</span><span class="sxs-lookup"><span data-stu-id="a1078-104">The [Azure AD reporting APIs](https://msdn.microsoft.com/library/azure/ad/graph/howto/azure-ad-reports-and-events-preview) provide you with programmatic access to the data through a set of REST-based APIs.</span></span> <span data-ttu-id="a1078-105">Du kan anropa API: erna från en mängd olika programmeringsspråk och verktyg.</span><span class="sxs-lookup"><span data-stu-id="a1078-105">You can call these APIs from a variety of programming languages and tools.</span></span>

<span data-ttu-id="a1078-106">Reporting API: N används [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) att bevilja åtkomst till webb-API: er.</span><span class="sxs-lookup"><span data-stu-id="a1078-106">The reporting API uses [OAuth](https://msdn.microsoft.com/library/azure/dn645545.aspx) to authorize access to the web APIs.</span></span> 

<span data-ttu-id="a1078-107">Om du vill få åtkomst till rapporteringsdata via API: et, måste du ha något av följande roller:</span><span class="sxs-lookup"><span data-stu-id="a1078-107">To get access to the reporting data through the API, you need to have one of the following roles assigned:</span></span>

- <span data-ttu-id="a1078-108">Säkerhet läsare</span><span class="sxs-lookup"><span data-stu-id="a1078-108">Security Reader</span></span>
- <span data-ttu-id="a1078-109">Säkerhet Admin</span><span class="sxs-lookup"><span data-stu-id="a1078-109">Security Admin</span></span>
- <span data-ttu-id="a1078-110">Global administratör</span><span class="sxs-lookup"><span data-stu-id="a1078-110">Global Admin</span></span>


<span data-ttu-id="a1078-111">För att förbereda din åtkomst till reporting API, måste du:</span><span class="sxs-lookup"><span data-stu-id="a1078-111">To prepare your access to the reporting API, you must:</span></span>

1. <span data-ttu-id="a1078-112">Registrera ett program</span><span class="sxs-lookup"><span data-stu-id="a1078-112">Register an application</span></span> 
2. <span data-ttu-id="a1078-113">Bevilja behörighet</span><span class="sxs-lookup"><span data-stu-id="a1078-113">Grant permissions</span></span> 
3. <span data-ttu-id="a1078-114">Samla in konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="a1078-114">Gather configuration settings</span></span> 

<span data-ttu-id="a1078-115">Frågor, frågor eller kommentarer finns [filen ett supportärende](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).</span><span class="sxs-lookup"><span data-stu-id="a1078-115">For questions, issues or feedback, please [file a support ticket](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-troubleshooting-support-howto).</span></span>

## <a name="register-an-azure-active-directory-application"></a><span data-ttu-id="a1078-116">Registrera ett Azure Active Directory-program</span><span class="sxs-lookup"><span data-stu-id="a1078-116">Register an Azure Active Directory application</span></span>

<span data-ttu-id="a1078-117">Du måste registrera en app, även om du ansluter till reporting API: et med ett skript.</span><span class="sxs-lookup"><span data-stu-id="a1078-117">You need to register an app even if you are accessing the reporting API using a script.</span></span> <span data-ttu-id="a1078-118">Detta ger dig en **program-ID**, vilket krävs för ett anrop för auktorisering och det möjliggör koden för att ta emot token.</span><span class="sxs-lookup"><span data-stu-id="a1078-118">This gives you an **Application ID**, which is required for an authorization call and it enables your code to receive tokens.</span></span>

<span data-ttu-id="a1078-119">Om du vill konfigurera din katalog för att komma åt Azure AD reporting API som du måste logga in på Azure-portalen med Azure-administratörskontot som även är medlem i den **Global administratör** directory roll i Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="a1078-119">To configure your directory to access the Azure AD reporting API, you must sign in to the Azure portal with an Azure administrator account that is also a member of the **Global Administrator** directory role in your Azure AD tenant.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a1078-120">Program som körs under autentiseringsuppgifterna med privilegier som ”admin” så här kan vara mycket kraftfulla, så kontrollera att det för att skydda programmets ID-hemlighet autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a1078-120">Applications running under credentials with "admin" privileges like this can be very powerful, so please be sure to keep the application's ID/secret credentials secure.</span></span>
> 


<span data-ttu-id="a1078-121">**Registrera ett Azure Active Directory-program:**</span><span class="sxs-lookup"><span data-stu-id="a1078-121">**To register an Azure Active Directory application:**</span></span>

1. <span data-ttu-id="a1078-122">I den [Azure-portalen](https://portal.azure.com), klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a1078-122">In the [Azure portal](https://portal.azure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="a1078-124">På den **Azure Active Directory** bladet, klickar du på **App registreringar**.</span><span class="sxs-lookup"><span data-stu-id="a1078-124">On the **Azure Active Directory** blade, click **App registrations**.</span></span>

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/02.png) 

3. <span data-ttu-id="a1078-126">På den **App registreringar** bladet i verktygsfältet högst upp, klickar du på **nya appregistrering**.</span><span class="sxs-lookup"><span data-stu-id="a1078-126">On the **App registrations** blade, in the toolbar on the top, click **New application registration**.</span></span>

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/03.png)

4. <span data-ttu-id="a1078-128">På den **skapa** bladet utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a1078-128">On the **Create** blade, perform the following steps:</span></span>

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/04.png)

    <span data-ttu-id="a1078-130">a.</span><span class="sxs-lookup"><span data-stu-id="a1078-130">a.</span></span> <span data-ttu-id="a1078-131">I den **namn** textruta typen `Reporting API application`.</span><span class="sxs-lookup"><span data-stu-id="a1078-131">In the **Name** textbox, type `Reporting API application`.</span></span>

    <span data-ttu-id="a1078-132">b.</span><span class="sxs-lookup"><span data-stu-id="a1078-132">b.</span></span> <span data-ttu-id="a1078-133">Som **programtyp**väljer **webbapp / API**.</span><span class="sxs-lookup"><span data-stu-id="a1078-133">As **Application type**, select **Web app / API**.</span></span>

    <span data-ttu-id="a1078-134">c.</span><span class="sxs-lookup"><span data-stu-id="a1078-134">c.</span></span> <span data-ttu-id="a1078-135">I den **inloggnings-URL** textruta typen `https://localhost`.</span><span class="sxs-lookup"><span data-stu-id="a1078-135">In the **Sign-on URL** textbox, type `https://localhost`.</span></span>

    <span data-ttu-id="a1078-136">d.</span><span class="sxs-lookup"><span data-stu-id="a1078-136">d.</span></span> <span data-ttu-id="a1078-137">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="a1078-137">Click **Create**.</span></span> 


## <a name="grant-permissions"></a><span data-ttu-id="a1078-138">Bevilja behörighet</span><span class="sxs-lookup"><span data-stu-id="a1078-138">Grant permissions</span></span> 

<span data-ttu-id="a1078-139">Syftet med det här steget är att ge ditt program **läsa katalogdata** behörigheter till den **Windows Azure Active Directory** API.</span><span class="sxs-lookup"><span data-stu-id="a1078-139">The objective of this step is to grant your application **Read directory data** permissions to the **Windows Azure Active Directory** API.</span></span>

![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/16.png)
 

<span data-ttu-id="a1078-141">**Ge ditt program behörighet att använda API:**</span><span class="sxs-lookup"><span data-stu-id="a1078-141">**To grant your application permission to use the API:**</span></span>

1. <span data-ttu-id="a1078-142">På den **App registreringar** klickar du på bladet i listan över appar **Reporting API-program**.</span><span class="sxs-lookup"><span data-stu-id="a1078-142">On the **App registrations** blade, in the apps list, click **Reporting API application**.</span></span>

2. <span data-ttu-id="a1078-143">På den **Reporting API-program** bladet i verktygsfältet högst upp, klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="a1078-143">On the **Reporting API application** blade, in the toolbar on the top, click **Settings**.</span></span> 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

3. <span data-ttu-id="a1078-145">På den **inställningar** bladet, klickar du på **nödvändiga behörigheter**.</span><span class="sxs-lookup"><span data-stu-id="a1078-145">On the **Settings** blade, click **Required permissions**.</span></span> 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/06.png)

4. <span data-ttu-id="a1078-147">På den **nödvändiga behörigheter** blad i den **API** klickar du på **Windows Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a1078-147">On the **Required permissions** blade, in the **API** list, click **Windows Azure Active Directory**.</span></span> 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/07.png)

5. <span data-ttu-id="a1078-149">På den **Aktivera åtkomst** bladet väljer **läsa katalogdata**.</span><span class="sxs-lookup"><span data-stu-id="a1078-149">On the **Enable Access** blade, select **Read directory data**.</span></span> 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/08.png)

6. <span data-ttu-id="a1078-151">Klicka på i verktygsfältet högst upp **spara**.</span><span class="sxs-lookup"><span data-stu-id="a1078-151">In the toolbar on the top, click **Save**.</span></span>

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/15.png)

## <a name="gather-configuration-settings"></a><span data-ttu-id="a1078-153">Samla in konfigurationsinställningar</span><span class="sxs-lookup"><span data-stu-id="a1078-153">Gather configuration settings</span></span> 
<span data-ttu-id="a1078-154">Det här avsnittet visar hur du får följande inställningar från din katalog:</span><span class="sxs-lookup"><span data-stu-id="a1078-154">This section shows you how to get the following settings from your directory:</span></span>

* <span data-ttu-id="a1078-155">Domännamn</span><span class="sxs-lookup"><span data-stu-id="a1078-155">Domain name</span></span>
* <span data-ttu-id="a1078-156">Klient-ID</span><span class="sxs-lookup"><span data-stu-id="a1078-156">Client ID</span></span>
* <span data-ttu-id="a1078-157">Klienthemlighet</span><span class="sxs-lookup"><span data-stu-id="a1078-157">Client secret</span></span>

<span data-ttu-id="a1078-158">Du måste dessa värden när du konfigurerar anrop reporting-API: et.</span><span class="sxs-lookup"><span data-stu-id="a1078-158">You need these values when configuring calls to the reporting API.</span></span> 

### <a name="get-your-domain-name"></a><span data-ttu-id="a1078-159">Hämta ditt domännamn</span><span class="sxs-lookup"><span data-stu-id="a1078-159">Get your domain name</span></span>

<span data-ttu-id="a1078-160">**Hämta ditt domännamn:**</span><span class="sxs-lookup"><span data-stu-id="a1078-160">**To get your domain name:**</span></span>

1. <span data-ttu-id="a1078-161">I den [Azure-portalen](https://portal.azure.com), klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a1078-161">In the [Azure portal](https://portal.azure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="a1078-163">På den **Azure Active Directory** bladet, klickar du på **domännamn**.</span><span class="sxs-lookup"><span data-stu-id="a1078-163">On the **Azure Active Directory** blade, click **Domain names**.</span></span>

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/09.png) 

3. <span data-ttu-id="a1078-165">Kopiera ditt domännamn från listan över domäner.</span><span class="sxs-lookup"><span data-stu-id="a1078-165">Copy your domain name from the list of domains.</span></span>


### <a name="get-your-applications-client-id"></a><span data-ttu-id="a1078-166">Hämta programmets klient-ID</span><span class="sxs-lookup"><span data-stu-id="a1078-166">Get your application's client ID</span></span>

<span data-ttu-id="a1078-167">**Hämta programmets klient-ID:**</span><span class="sxs-lookup"><span data-stu-id="a1078-167">**To get your application's client ID:**</span></span>

1. <span data-ttu-id="a1078-168">I den [Azure-portalen](https://portal.azure.com), klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a1078-168">In the [Azure portal](https://portal.azure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="a1078-170">På den **App registreringar** klickar du på bladet i listan över appar **Reporting API-program**.</span><span class="sxs-lookup"><span data-stu-id="a1078-170">On the **App registrations** blade, in the apps list, click **Reporting API application**.</span></span>

3. <span data-ttu-id="a1078-171">På den **Reporting API-program** bladet på den **program-ID**, klickar du på **Klicka för att kopiera**.</span><span class="sxs-lookup"><span data-stu-id="a1078-171">On the **Reporting API application** blade, at the **Application ID**, click **Click to copy**.</span></span>

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/11.png) 



### <a name="get-your-applications-client-secret"></a><span data-ttu-id="a1078-173">Hämta programmets klienthemlighet</span><span class="sxs-lookup"><span data-stu-id="a1078-173">Get your application's client secret</span></span>
<span data-ttu-id="a1078-174">För att få ditt program klienthemlighet, måste du skapa en ny nyckel och spara dess värde vid sparas den nya nyckeln eftersom det inte går att hämta det här värdet senare längre.</span><span class="sxs-lookup"><span data-stu-id="a1078-174">To get your application's client secret, you need to create a new key and save its value upon saving the new key because it is not possible to retrieve this value later anymore.</span></span>

<span data-ttu-id="a1078-175">**Hämta programmets klienthemlighet:**</span><span class="sxs-lookup"><span data-stu-id="a1078-175">**To get your application's client secret:**</span></span>

1. <span data-ttu-id="a1078-176">I den [Azure-portalen](https://portal.azure.com), klicka på det vänstra navigeringsfönstret **Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="a1078-176">In the [Azure portal](https://portal.azure.com), on the left navigation pane, click **Active Directory**.</span></span>
   
    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/01.png) 

2. <span data-ttu-id="a1078-178">På den **App registreringar** klickar du på bladet i listan över appar **Reporting API-program**.</span><span class="sxs-lookup"><span data-stu-id="a1078-178">On the **App registrations** blade, in the apps list, click **Reporting API application**.</span></span>


3. <span data-ttu-id="a1078-179">På den **Reporting API-program** bladet i verktygsfältet högst upp, klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="a1078-179">On the **Reporting API application** blade, in the toolbar on the top, click **Settings**.</span></span> 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/05.png)

4. <span data-ttu-id="a1078-181">På den **inställningar** blad i den **APIR åtkomst** klickar du på **nycklar**.</span><span class="sxs-lookup"><span data-stu-id="a1078-181">On the **Settings** blade, in the **APIR Access** section, click **Keys**.</span></span> 

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/12.png)


5. <span data-ttu-id="a1078-183">På den **nycklar** bladet utför följande steg:</span><span class="sxs-lookup"><span data-stu-id="a1078-183">On the **Keys** blade, perform the following steps:</span></span>

    ![Registrera program](./media/active-directory-reporting-api-prerequisites-azure-portal/14.png)

    <span data-ttu-id="a1078-185">a.</span><span class="sxs-lookup"><span data-stu-id="a1078-185">a.</span></span> <span data-ttu-id="a1078-186">I den **beskrivning** textruta typen `Reporting API`.</span><span class="sxs-lookup"><span data-stu-id="a1078-186">In the **Description** textbox, type `Reporting API`.</span></span>

    <span data-ttu-id="a1078-187">b.</span><span class="sxs-lookup"><span data-stu-id="a1078-187">b.</span></span> <span data-ttu-id="a1078-188">Som **Expires**väljer **i två år**.</span><span class="sxs-lookup"><span data-stu-id="a1078-188">As **Expires**, select **In 2 years**.</span></span>

    <span data-ttu-id="a1078-189">c.</span><span class="sxs-lookup"><span data-stu-id="a1078-189">c.</span></span> <span data-ttu-id="a1078-190">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="a1078-190">Click **Save**.</span></span>

    <span data-ttu-id="a1078-191">d.</span><span class="sxs-lookup"><span data-stu-id="a1078-191">d.</span></span> <span data-ttu-id="a1078-192">Kopiera värdet för nyckeln.</span><span class="sxs-lookup"><span data-stu-id="a1078-192">Copy the key value.</span></span>


## <a name="next-steps"></a><span data-ttu-id="a1078-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a1078-193">Next Steps</span></span>
* <span data-ttu-id="a1078-194">Vill du komma åt data från Azure AD reporting API på ett programmässiga sätt?</span><span class="sxs-lookup"><span data-stu-id="a1078-194">Would you like to access the data from the Azure AD reporting API in a programmatic manner?</span></span> <span data-ttu-id="a1078-195">Checka ut [komma igång med Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span><span class="sxs-lookup"><span data-stu-id="a1078-195">Check out [Getting started with the Azure Active Directory Reporting API](active-directory-reporting-api-getting-started.md).</span></span>
* <span data-ttu-id="a1078-196">Om du vill veta mer om Azure Active Directory reporting finns i [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span><span class="sxs-lookup"><span data-stu-id="a1078-196">If you would like to find out more about Azure Active Directory reporting, see the [Azure Active Directory Reporting Guide](active-directory-reporting-guide.md).</span></span>  

