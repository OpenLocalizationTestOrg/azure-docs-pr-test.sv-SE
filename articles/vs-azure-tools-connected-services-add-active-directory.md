---
title: "Lägga till ett Azure Active Directory med anslutna tjänster i Visual Studio | Microsoft Docs"
description: "Lägg till ett Azure Active Directory med hjälp av dialogrutan Visual Studio Lägg till anslutna tjänster"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.service: active-directory
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: kraigb
ms.openlocfilehash: a767c93fb271f3aa33d9556c69c511bcac7cb0d5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a><span data-ttu-id="3280e-103">Lägga till ett Azure Active Directory med anslutna tjänster i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3280e-103">Adding an Azure Active Directory by using Connected Services in Visual Studio</span></span>
<span data-ttu-id="3280e-104">Med hjälp av Azure Active Directory (Azure AD) kan stöder du enkel inloggning (SSO) för ASP.NET MVC-webbprogram och Active Directory-autentisering i Web API-tjänster.</span><span class="sxs-lookup"><span data-stu-id="3280e-104">By using Azure Active Directory (Azure AD), you can support Single Sign-On (SSO) for ASP.NET MVC web applications, or Active Directory Authentication in Web API services.</span></span> <span data-ttu-id="3280e-105">Användarna kan använda sina konton från Azure Active Directory för att ansluta till ditt webbprogram med Azure Active Directory-autentisering.</span><span class="sxs-lookup"><span data-stu-id="3280e-105">With Azure Active Directory Authentication, your users can use their accounts from Azure Active Directory to connect to your web applications.</span></span> <span data-ttu-id="3280e-106">Fördelarna med Azure Active Directory-autentisering med Web API är förbättrad datasäkerhet när exponera en API från ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="3280e-106">The advantages of Azure Active Directory Authentication with Web API include enhanced data security when exposing an API from a web application.</span></span> <span data-ttu-id="3280e-107">Du behöver inte hantera en separat autentiseringssystemet med ett eget konto och användaren management med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3280e-107">With Azure AD, you do not have to manage a separate authentication system with its own account and user management.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3280e-108">Krav</span><span class="sxs-lookup"><span data-stu-id="3280e-108">Prerequisites</span></span>
- <span data-ttu-id="3280e-109">Azure-konto – om du inte har ett Azure-konto kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="3280e-109">Azure account - If you don't have an Azure account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

### <a name="connect-to-azure-active-directory-using-the-connected-services-dialog"></a><span data-ttu-id="3280e-110">Ansluta till Azure Active Directory genom att använda dialogrutan anslutna tjänster</span><span class="sxs-lookup"><span data-stu-id="3280e-110">Connect to Azure Active Directory using the Connected Services dialog</span></span>
1. <span data-ttu-id="3280e-111">Skapa eller öppna ett ASP.NET MVC-projekt eller ett ASP.NET Web API-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3280e-111">In Visual Studio, create or open an ASP.NET MVC project, or an ASP.NET Web API project.</span></span>

1. <span data-ttu-id="3280e-112">Från Solution Explorer högerklickar du på den **anslutna tjänster** nod, och på snabbmenyn Välj **Lägg till anslutna tjänster**.</span><span class="sxs-lookup"><span data-stu-id="3280e-112">From the Solution Explorer, right-click the **Connected Services** node, and, from the context menu, select **Add Connected Services**.</span></span>

1. <span data-ttu-id="3280e-113">På den **anslutna tjänster** väljer **autentisering med Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="3280e-113">On the **Connected Services** page, select **Authentication with Azure Active Directory**.</span></span>
   
    ![Anslutna Tjänstesida](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. <span data-ttu-id="3280e-115">På den **introduktion** sida av den **konfigurera Azure AD Authentication** väljer **nästa**.</span><span class="sxs-lookup"><span data-stu-id="3280e-115">On the **Introduction** page of the **Configure Azure AD Authentication** wizard, select **Next**.</span></span>
   
    ![Introduktionssida](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1. <span data-ttu-id="3280e-117">På den **enkel inloggning på** sida av den **konfigurera Azure AD Authentication** guiden, Välj en domän från den **domän** listrutan.</span><span class="sxs-lookup"><span data-stu-id="3280e-117">On the **Single-Sign On** page of the **Configure Azure AD Authentication** wizard, select a domain from the **Domain** drop-down list.</span></span> <span data-ttu-id="3280e-118">Listan över domäner innehåller alla domäner som är tillgängliga med de konton som visas i dialogrutan Inställningar.</span><span class="sxs-lookup"><span data-stu-id="3280e-118">The list of domains contains all domains accessible by the accounts listed in the Account Settings dialog.</span></span> <span data-ttu-id="3280e-119">Alternativt kan du ange ett domännamn om du inte hittar det du söker efter, t.ex `mydomain.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="3280e-119">As an alternative, you can enter a domain name if you don’t find the one you’re looking for, such as `mydomain.onmicrosoft.com`.</span></span> <span data-ttu-id="3280e-120">Du kan välja alternativet för att skapa en app i Azure Active Directory eller använda inställningar från en befintlig app i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3280e-120">You can choose the option to create an Azure Active Directory app or use the settings from an existing Azure Active Directory app.</span></span> <span data-ttu-id="3280e-121">Välj **nästa** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="3280e-121">Select **Next** when done.</span></span>
   
    ![Enkel inloggning på sidan](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)

1. <span data-ttu-id="3280e-123">På den **katalogåtkomst** sida av den **konfigurera Azure AD Authentication** guiden kontrollerar du att den **läsa katalogdata** alternativet är markerat.</span><span class="sxs-lookup"><span data-stu-id="3280e-123">On the **Directory Access** page of the **Configure Azure AD Authentication** wizard, ensure that the **Read directory data** option is checked.</span></span> 
   
    ![Åtkomst katalogsidan](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. <span data-ttu-id="3280e-125">Välj **Slutför** att lägga till nödvändiga konfigurationskod och referenser till aktivera ditt projekt för Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="3280e-125">Select **Finish** to add the necessary configuration code and references to enable your project for Azure AD authentication.</span></span> <span data-ttu-id="3280e-126">Du kan se Active Directory-domänen på den [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="3280e-126">You can see the Active Directory domain on the [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="3280e-127">Visual Studio visas en [vad hände](#how-your-project-is-modified) artikel för att visa hur projektet har ändrats.</span><span class="sxs-lookup"><span data-stu-id="3280e-127">Visual Studio will display a [What Happened](#how-your-project-is-modified) article to show you how your project was modified.</span></span> <span data-ttu-id="3280e-128">Om du vill kontrollera att allt fungerar, öppna ett av de ändrade konfigurationsfilerna och kontrollera att finns inställningarna som anges i artikeln.</span><span class="sxs-lookup"><span data-stu-id="3280e-128">If you want to check that everything worked, open one of the modified configuration files and verify that the settings mentioned in the article are there.</span></span> 

## <a name="how-your-project-is-modified"></a><span data-ttu-id="3280e-129">Hur projektet har ändrats</span><span class="sxs-lookup"><span data-stu-id="3280e-129">How your project is modified</span></span>
<span data-ttu-id="3280e-130">När du kör guiden läggs Azure Active Directory och associerade referenser till ditt projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3280e-130">When you run the wizard, Visual Studio adds Azure Active Directory and associated references to your project.</span></span> <span data-ttu-id="3280e-131">Konfigurationsfiler och kodfiler i projektet ändras även om du vill lägga till stöd för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="3280e-131">Configuration files and code files in your project are also modified to add support for Azure AD.</span></span> <span data-ttu-id="3280e-132">Vissa ändringar som Visual Studio gör beror på projekttypen.</span><span class="sxs-lookup"><span data-stu-id="3280e-132">The specific modifications that Visual Studio makes depend on the project type.</span></span> <span data-ttu-id="3280e-133">Detaljerad information om hur ASP.NET MVC-projekt har ändrats, se [vilka happened – MVC projekt](http://go.microsoft.com/fwlink/p/?LinkID=513809).</span><span class="sxs-lookup"><span data-stu-id="3280e-133">For detailed information about how ASP.NET MVC projects are modified, see [What happened– MVC Projects](http://go.microsoft.com/fwlink/p/?LinkID=513809).</span></span> <span data-ttu-id="3280e-134">Web API-projekt, se [vad hände – Web API-projekt](http://go.microsoft.com/fwlink/p/?LinkId=513810).</span><span class="sxs-lookup"><span data-stu-id="3280e-134">For Web API projects, see [What happened – Web API Projects](http://go.microsoft.com/fwlink/p/?LinkId=513810).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3280e-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3280e-135">Next steps</span></span>
* [<span data-ttu-id="3280e-136">MSDN-Forum för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3280e-136">MSDN Forum for Azure Active Directory</span></span>](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)
* [<span data-ttu-id="3280e-137">Dokumentationen för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3280e-137">Azure Active Directory Documentation</span></span>](https://azure.microsoft.com/documentation/services/active-directory/)
* [<span data-ttu-id="3280e-138">Blogginlägg: Introduktion till Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3280e-138">Blog Post: Intro to Azure Active Directory</span></span>](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

