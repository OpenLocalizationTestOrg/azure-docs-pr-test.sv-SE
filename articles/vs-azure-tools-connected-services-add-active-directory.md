---
title: "aaaAdding ett Azure Active Directory med anslutna tjänster i Visual Studio | Microsoft Docs"
description: "Lägg till ett Azure Active Directory med hjälp av dialogrutan för hello Visual Studio Lägg till anslutna tjänster"
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
ms.openlocfilehash: 26c8f68edf9ec5f7bf65cbab34e4f9b4085ed18d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a><span data-ttu-id="0af1b-103">Lägga till ett Azure Active Directory med anslutna tjänster i Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0af1b-103">Adding an Azure Active Directory by using Connected Services in Visual Studio</span></span>
<span data-ttu-id="0af1b-104">Med hjälp av Azure Active Directory (Azure AD) kan stöder du enkel inloggning (SSO) för ASP.NET MVC-webbprogram och Active Directory-autentisering i Web API-tjänster.</span><span class="sxs-lookup"><span data-stu-id="0af1b-104">By using Azure Active Directory (Azure AD), you can support Single Sign-On (SSO) for ASP.NET MVC web applications, or Active Directory Authentication in Web API services.</span></span> <span data-ttu-id="0af1b-105">Användarna kan använda sina konton från Azure Active Directory tooconnect tooyour webbprogram med Azure Active Directory-autentisering.</span><span class="sxs-lookup"><span data-stu-id="0af1b-105">With Azure Active Directory Authentication, your users can use their accounts from Azure Active Directory tooconnect tooyour web applications.</span></span> <span data-ttu-id="0af1b-106">hello fördelarna med Azure Active Directory-autentisering med Web API är förbättrad datasäkerhet när exponera en API från ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="0af1b-106">hello advantages of Azure Active Directory Authentication with Web API include enhanced data security when exposing an API from a web application.</span></span> <span data-ttu-id="0af1b-107">Med Azure AD har du inte toomanage en separat autentiseringssystemet med sin egen hantering av kontot och användare.</span><span class="sxs-lookup"><span data-stu-id="0af1b-107">With Azure AD, you do not have toomanage a separate authentication system with its own account and user management.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0af1b-108">Krav</span><span class="sxs-lookup"><span data-stu-id="0af1b-108">Prerequisites</span></span>
- <span data-ttu-id="0af1b-109">Azure-konto – om du inte har ett Azure-konto kan du [registrera dig för en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) eller [aktivera Visual Studio-prenumerantförmåner](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span><span class="sxs-lookup"><span data-stu-id="0af1b-109">Azure account - If you don't have an Azure account, you can [sign up for a free trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) or [activate your Visual Studio subscriber benefits](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).</span></span>

### <a name="connect-tooazure-active-directory-using-hello-connected-services-dialog"></a><span data-ttu-id="0af1b-110">Ansluta tooAzure Active Directory med hello anslutna tjänster dialogrutan</span><span class="sxs-lookup"><span data-stu-id="0af1b-110">Connect tooAzure Active Directory using hello Connected Services dialog</span></span>
1. <span data-ttu-id="0af1b-111">Skapa eller öppna ett ASP.NET MVC-projekt eller ett ASP.NET Web API-projekt i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0af1b-111">In Visual Studio, create or open an ASP.NET MVC project, or an ASP.NET Web API project.</span></span>

1. <span data-ttu-id="0af1b-112">Från hello Solution Explorer, högerklicka på hello **anslutna tjänster** nod, och hello snabbmenyn, Välj **Lägg till anslutna tjänster**.</span><span class="sxs-lookup"><span data-stu-id="0af1b-112">From hello Solution Explorer, right-click hello **Connected Services** node, and, from hello context menu, select **Add Connected Services**.</span></span>

1. <span data-ttu-id="0af1b-113">På hello **anslutna tjänster** väljer **autentisering med Azure Active Directory**.</span><span class="sxs-lookup"><span data-stu-id="0af1b-113">On hello **Connected Services** page, select **Authentication with Azure Active Directory**.</span></span>
   
    ![Anslutna Tjänstesida](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. <span data-ttu-id="0af1b-115">På hello **introduktion** sidan hello **konfigurera Azure AD Authentication** väljer **nästa**.</span><span class="sxs-lookup"><span data-stu-id="0af1b-115">On hello **Introduction** page of hello **Configure Azure AD Authentication** wizard, select **Next**.</span></span>
   
    ![Introduktionssida](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1. <span data-ttu-id="0af1b-117">På hello **enkel inloggning på** sidan hello **konfigurera Azure AD Authentication** guiden, Välj en domän från hello **domän** listrutan.</span><span class="sxs-lookup"><span data-stu-id="0af1b-117">On hello **Single-Sign On** page of hello **Configure Azure AD Authentication** wizard, select a domain from hello **Domain** drop-down list.</span></span> <span data-ttu-id="0af1b-118">hello listan över domäner innehåller alla domäner som är tillgängliga med hello kontona i hello kontoinställningar dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="0af1b-118">hello list of domains contains all domains accessible by hello accounts listed in hello Account Settings dialog.</span></span> <span data-ttu-id="0af1b-119">Alternativt kan du ange ett domännamn om du inte hittar hello något du söker efter, t.ex `mydomain.onmicrosoft.com`.</span><span class="sxs-lookup"><span data-stu-id="0af1b-119">As an alternative, you can enter a domain name if you don’t find hello one you’re looking for, such as `mydomain.onmicrosoft.com`.</span></span> <span data-ttu-id="0af1b-120">Du kan välja hello alternativet toocreate en app i Azure Active Directory eller använda hello inställningar från en befintlig app i Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="0af1b-120">You can choose hello option toocreate an Azure Active Directory app or use hello settings from an existing Azure Active Directory app.</span></span> <span data-ttu-id="0af1b-121">Välj **nästa** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="0af1b-121">Select **Next** when done.</span></span>
   
    ![Enkel inloggning på sidan](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)

1. <span data-ttu-id="0af1b-123">På hello **katalogåtkomst** sidan hello **konfigurera Azure AD Authentication** guiden, se till att hello **läsa katalogdata** alternativet är markerat.</span><span class="sxs-lookup"><span data-stu-id="0af1b-123">On hello **Directory Access** page of hello **Configure Azure AD Authentication** wizard, ensure that hello **Read directory data** option is checked.</span></span> 
   
    ![Åtkomst katalogsidan](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. <span data-ttu-id="0af1b-125">Välj **Slutför** tooadd hello nödvändig konfiguration kod och referenser tooenable ditt projekt för Azure AD-autentisering.</span><span class="sxs-lookup"><span data-stu-id="0af1b-125">Select **Finish** tooadd hello necessary configuration code and references tooenable your project for Azure AD authentication.</span></span> <span data-ttu-id="0af1b-126">Du kan se hello Active Directory-domän på hello [Azure-portalen](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span><span class="sxs-lookup"><span data-stu-id="0af1b-126">You can see hello Active Directory domain on hello [Azure portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).</span></span>

1. <span data-ttu-id="0af1b-127">Visual Studio visas en [vad hände](#how-your-project-is-modified) artikel tooshow du hur projektet har ändrats.</span><span class="sxs-lookup"><span data-stu-id="0af1b-127">Visual Studio will display a [What Happened](#how-your-project-is-modified) article tooshow you how your project was modified.</span></span> <span data-ttu-id="0af1b-128">Om du vill toocheck som allt arbete, öppna en av hello ändrat konfigurationsfiler och kontrollera att hello-inställningar som anges i hello artikeln är det.</span><span class="sxs-lookup"><span data-stu-id="0af1b-128">If you want toocheck that everything worked, open one of hello modified configuration files and verify that hello settings mentioned in hello article are there.</span></span> 

## <a name="how-your-project-is-modified"></a><span data-ttu-id="0af1b-129">Hur projektet har ändrats</span><span class="sxs-lookup"><span data-stu-id="0af1b-129">How your project is modified</span></span>
<span data-ttu-id="0af1b-130">När du kör guiden hello Visual Studio lägger till Azure Active Directory och associerade referenser tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="0af1b-130">When you run hello wizard, Visual Studio adds Azure Active Directory and associated references tooyour project.</span></span> <span data-ttu-id="0af1b-131">Konfigurationsfiler och kodfiler i projektet är också ändrade tooadd stöd för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="0af1b-131">Configuration files and code files in your project are also modified tooadd support for Azure AD.</span></span> <span data-ttu-id="0af1b-132">hello specifika ändringar som Visual Studio gör beror på hello-projekt.</span><span class="sxs-lookup"><span data-stu-id="0af1b-132">hello specific modifications that Visual Studio makes depend on hello project type.</span></span> <span data-ttu-id="0af1b-133">Detaljerad information om hur ASP.NET MVC-projekt har ändrats, se [vilka happened – MVC projekt](http://go.microsoft.com/fwlink/p/?LinkID=513809).</span><span class="sxs-lookup"><span data-stu-id="0af1b-133">For detailed information about how ASP.NET MVC projects are modified, see [What happened– MVC Projects](http://go.microsoft.com/fwlink/p/?LinkID=513809).</span></span> <span data-ttu-id="0af1b-134">Web API-projekt, se [vad hände – Web API-projekt](http://go.microsoft.com/fwlink/p/?LinkId=513810).</span><span class="sxs-lookup"><span data-stu-id="0af1b-134">For Web API projects, see [What happened – Web API Projects](http://go.microsoft.com/fwlink/p/?LinkId=513810).</span></span>

## <a name="next-steps"></a><span data-ttu-id="0af1b-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0af1b-135">Next steps</span></span>
* [<span data-ttu-id="0af1b-136">MSDN-Forum för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0af1b-136">MSDN Forum for Azure Active Directory</span></span>](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)
* [<span data-ttu-id="0af1b-137">Dokumentationen för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0af1b-137">Azure Active Directory Documentation</span></span>](https://azure.microsoft.com/documentation/services/active-directory/)
* [<span data-ttu-id="0af1b-138">Blogginlägg: Introduktion tooAzure Active Directory</span><span class="sxs-lookup"><span data-stu-id="0af1b-138">Blog Post: Intro tooAzure Active Directory</span></span>](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)

