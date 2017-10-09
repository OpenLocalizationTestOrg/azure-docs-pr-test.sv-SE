---
title: din inloggning sidan i hello Azure Active Directory aaaCustomize | Microsoft Docs
description: "Lär dig hur tooadd en företagets företagsanpassning toohello Azure inloggning sida"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: f8b932bc-8b4f-42b5-a2d3-f2c076234a78
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: 151521e3b9cbc6a438a589735058fbff78443cf8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-company-branding-tooyour-sign-in-page-in-hello-azure-active-directory"></a><span data-ttu-id="e30af-103">Lägga till företagsanpassning tooyour inloggningssidan i hello Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e30af-103">Add company branding tooyour sign-in page in hello Azure Active Directory</span></span>
<span data-ttu-id="e30af-104">tooavoid förvirring vill många företag tooapply ett konsekvent utseende på alla hello webbplatser och tjänster som de hanterar.</span><span class="sxs-lookup"><span data-stu-id="e30af-104">tooavoid confusion, many companies want tooapply a consistent look and feel across all hello websites and services they manage.</span></span> <span data-ttu-id="e30af-105">Azure Active Directory erbjuder den här funktionen genom att låta dig toocustomize hello utseendet på hello-inloggningssida med företagets logotyp och egna färgscheman.</span><span class="sxs-lookup"><span data-stu-id="e30af-105">Azure Active Directory provides this capability by allowing you toocustomize hello appearance of hello sign-in page with your company logo and custom color schemes.</span></span> <span data-ttu-id="e30af-106">hello-inloggningssida är hello sida som visas när du loggar in tooOffice 365 eller andra webbaserade program som använder Azure AD som identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="e30af-106">hello sign-in page is hello page that appears when you sign in tooOffice 365 or other web-based applications that are using Azure AD as your identity provider.</span></span> <span data-ttu-id="e30af-107">Du interagera med den här sidan tooenter dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e30af-107">You interact with this page tooenter your credentials.</span></span>

<span data-ttu-id="e30af-108">Om du vill tooshow ditt företags varumärke, färger och andra anpassningsbara element på den här sidan, finns i följande bilder toounderstand hello skillnaden mellan två hello-upplevelser hello.</span><span class="sxs-lookup"><span data-stu-id="e30af-108">If you want tooshow your company brand, colors and other customizable elements on this page, see hello following images toounderstand hello difference between hello two experiences.</span></span>

<span data-ttu-id="e30af-109">Hej följande skärmbild visar och exempel för hello Office 365-inloggningssidan på en stationär dator **innan** en anpassning:</span><span class="sxs-lookup"><span data-stu-id="e30af-109">hello following screenshot shows and example for hello Office 365 sign-in page on a desktop computer **before** a customization:</span></span>

![Office 365-inloggningssida före anpassning](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

<span data-ttu-id="e30af-111">Hej följande skärmbild visar och exempel för hello Office 365-inloggningssidan på en stationär dator **när** en anpassning:</span><span class="sxs-lookup"><span data-stu-id="e30af-111">hello following screenshot shows and example for hello Office 365 sign-in page on a desktop computer **after** a customization:</span></span>

![Office 365-inloggningssida efter anpassning](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)

## <a name="customizing-hello-sign-in-page"></a><span data-ttu-id="e30af-113">Anpassa hello-inloggningssida</span><span class="sxs-lookup"><span data-stu-id="e30af-113">Customizing hello sign-in page</span></span>
<span data-ttu-id="e30af-114">Om du behöver webbläsarbaserad åtkomst tooyour molnappar och tjänster som din organisation prenumererar på, använder du normalt hello-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="e30af-114">Typically, if you need browser-based access tooyour cloud apps and services that your organization subscribes to, you use hello sign-in page.</span></span>

<span data-ttu-id="e30af-115">Om du har tillämpat ändringar tooyour inloggningssidan kan ta det upp tooan timme för hello ändringar tooappear.</span><span class="sxs-lookup"><span data-stu-id="e30af-115">If you have applied changes tooyour sign-in page, it can take up tooan hour for hello changes tooappear.</span></span>

<span data-ttu-id="e30af-116">En företagsanpassad inloggningssida visas bara när du besöker en tjänst med en URL som är specifik för en klientorganisation, till exempel https://outlook.com/**contoso**.com eller https://mail.**contoso**.com.</span><span class="sxs-lookup"><span data-stu-id="e30af-116">A branded sign-in page only appears when you visit a service with a tenant-specific URL such as https://outlook.com/**contoso**.com, or https://mail.**contoso**.com.</span></span>

<span data-ttu-id="e30af-117">När du besöker en tjänst med en URL som inte är specifik för en klientorganisation (t.ex. https://mail.office365.com) visas en icke företagsanpassad inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="e30af-117">When you visit a service with non-tenant specific URLs (e.g.: https://mail.office365.com), a non-branded sign-in page appears.</span></span> <span data-ttu-id="e30af-118">I så fall visas din anpassning först när du har angett ditt användar-ID eller då du har valt en användarikon.</span><span class="sxs-lookup"><span data-stu-id="e30af-118">in this case, your branding appears once you have entered your user ID or you have selected a user tile.</span></span>

> [!NOTE]
> * <span data-ttu-id="e30af-119">Domännamnet måste visas som ”aktiv” i hello **domäner** del av hello Azure-portalen där du har konfigurerat anpassningen.</span><span class="sxs-lookup"><span data-stu-id="e30af-119">Your domain name must appear as “Active" in hello **Domains** portion of hello Azure portal in which you have configured branding.</span></span> <span data-ttu-id="e30af-120">Mer information finns i [lägga till anpassade domännamn](active-directory-domains-add-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e30af-120">For more information, see [Add custom domain names](active-directory-domains-add-azure-portal.md).</span></span>
> * <span data-ttu-id="e30af-121">Inloggningssidan företagsanpassning överföra inte toohello konsumenten inloggningssidan för Microsoft.</span><span class="sxs-lookup"><span data-stu-id="e30af-121">Sign-in page branding doesn’t carry over toohello consumer sign in page of Microsoft.</span></span> <span data-ttu-id="e30af-122">Om du loggar in med ett Microsoft-konto kan du se en företagsanpassad lista över användarikoner som återges av Azure AD, men hello anpassning av din organisation gäller inte toohello Microsoft inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="e30af-122">If you sign in with a Microsoft account, you may see a branded list of user tiles rendered by Azure AD, but hello branding of your organization does not apply toohello Microsoft account sign-in page.</span></span>
>
>

<span data-ttu-id="e30af-123">På sidan logga in hello **Håll mig inloggad** checkbox tillåter en användare tooremain inloggad när de stänga och öppna webbläsaren igen.</span><span class="sxs-lookup"><span data-stu-id="e30af-123">On your sign-in page, hello **Keep me signed in** checkbox allows a user tooremain signed in when they close and re-open their browser.</span></span>

   ![Håll mig inloggad](./media/active-directory-branding-custom-signon-azure-portal/01.png)

<span data-ttu-id="e30af-125">Den påverkar inte sessionens längd.</span><span class="sxs-lookup"><span data-stu-id="e30af-125">It does not effect session lifetime.</span></span> <span data-ttu-id="e30af-126">Du kan dölja hello kryssruta på hello Azure Active Directory-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="e30af-126">You can hide hello checkbox on hello Azure Active Directory sign-in page.</span></span>
<span data-ttu-id="e30af-127">Om kryssrutan hello visas beror på hello-inställningen **Håll mig inloggad inaktiverats**.</span><span class="sxs-lookup"><span data-stu-id="e30af-127">Whether hello checkbox is displayed depends on hello setting of **Keep me signed in disabled**.</span></span>

   ![Håll mig inloggad](./media/active-directory-branding-custom-signon-azure-portal/02.png)

<span data-ttu-id="e30af-129">toohide Hej kryssrutan, konfigurera den här inställningen för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="e30af-129">toohide hello checkbox, configure this setting too**Yes**.</span></span>

> [!NOTE]
> <span data-ttu-id="e30af-130">Vissa funktioner i SharePoint Online och Office 2010 är beroende av användare kan toocheck den här rutan.</span><span class="sxs-lookup"><span data-stu-id="e30af-130">Some features of SharePoint Online and Office 2010 depend on users being able toocheck this box.</span></span> <span data-ttu-id="e30af-131">Om du konfigurerar den här inställningen toohidden kan användarna se ytterligare och oväntat prompter toosign i.</span><span class="sxs-lookup"><span data-stu-id="e30af-131">If you configure this setting toohidden, your users may see additional and unexpected prompts toosign-in.</span></span>
>
>

<span data-ttu-id="e30af-132">**tooadd företagets företagsanpassning tooyour katalog:**</span><span class="sxs-lookup"><span data-stu-id="e30af-132">**tooadd company branding tooyour directory:**</span></span>

1. <span data-ttu-id="e30af-133">Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="e30af-133">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="e30af-134">Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="e30af-134">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Öppna användarhantering](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. <span data-ttu-id="e30af-136">På hello **användare och grupper** bladet väljer **företagets företagsanpassning**.</span><span class="sxs-lookup"><span data-stu-id="e30af-136">On hello **Users and groups** blade, select **Company branding**.</span></span>
4. <span data-ttu-id="e30af-137">På hello **användare och grupper – företagets anpassning** bladet, Välj hello **redigera** kommando.</span><span class="sxs-lookup"><span data-stu-id="e30af-137">On hello **Users and groups - Company branding** blade, select hello **Edit** command.</span></span>

    ![Redigera anpassning](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. <span data-ttu-id="e30af-139">Ändra hello-element som du vill toocustomize.</span><span class="sxs-lookup"><span data-stu-id="e30af-139">Modify hello elements you want toocustomize.</span></span> <span data-ttu-id="e30af-140">Alla element är valfria.</span><span class="sxs-lookup"><span data-stu-id="e30af-140">All elements are optional.</span></span>
6. <span data-ttu-id="e30af-141">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e30af-141">Click **Save**.</span></span>

<span data-ttu-id="e30af-142">Det kan ta upp tooan timme för alla ändringar du gjort toohello inloggning sidan företagsanpassning tooappear.</span><span class="sxs-lookup"><span data-stu-id="e30af-142">It can take up tooan hour for any changes you made toohello sign-in page branding tooappear.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e30af-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e30af-143">Next steps</span></span>
[<span data-ttu-id="e30af-144">Lägga till språkspecifik företagsanpassning</span><span class="sxs-lookup"><span data-stu-id="e30af-144">Add language-specific company branding</span></span>](active-directory-branding-localize-azure-portal.md)
