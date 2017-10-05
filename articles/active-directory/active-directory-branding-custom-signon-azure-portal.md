---
title: Anpassa inloggningssidan i Azure Active Directory | Microsoft Docs
description: "Lär dig hur du lägger till en företagsanpassning till Azure inloggningssidan"
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
ms.openlocfilehash: 27590c018ea55e9793246c7a4cab10f934ea502b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-company-branding-to-your-sign-in-page-in-the-azure-active-directory"></a><span data-ttu-id="b64a2-103">Lägga till företagsanpassning till inloggningssidan i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="b64a2-103">Add company branding to your sign-in page in the Azure Active Directory</span></span>
<span data-ttu-id="b64a2-104">För att undvika förvirring vill många företag använda ett enhetligt utseende på alla webbplatser och tjänster som de hanterar.</span><span class="sxs-lookup"><span data-stu-id="b64a2-104">To avoid confusion, many companies want to apply a consistent look and feel across all the websites and services they manage.</span></span> <span data-ttu-id="b64a2-105">Azure Active Directory erbjuder den här funktionen genom att låta dig anpassa utseendet på inloggningssidan med företagets logotyp och egna färgscheman.</span><span class="sxs-lookup"><span data-stu-id="b64a2-105">Azure Active Directory provides this capability by allowing you to customize the appearance of the sign-in page with your company logo and custom color schemes.</span></span> <span data-ttu-id="b64a2-106">På inloggningssidan är den sida som visas när du loggar in på Office 365 eller andra webbaserade program som använder Azure AD som identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="b64a2-106">The sign-in page is the page that appears when you sign in to Office 365 or other web-based applications that are using Azure AD as your identity provider.</span></span> <span data-ttu-id="b64a2-107">Du kan interagera med den här sidan om du vill ange dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="b64a2-107">You interact with this page to enter your credentials.</span></span>

<span data-ttu-id="b64a2-108">Om du vill att ditt företags varumärke, färger och andra anpassningsbara element ska återges på den här sidan tittar du på följande bilder som visar skillnaden mellan de båda upplevelserna.</span><span class="sxs-lookup"><span data-stu-id="b64a2-108">If you want to show your company brand, colors and other customizable elements on this page, see the following images to understand the difference between the two experiences.</span></span>

<span data-ttu-id="b64a2-109">Följande skärmbild visar ett exempel på Office 365-inloggningssidan på en stationär dator **före** en anpassning:</span><span class="sxs-lookup"><span data-stu-id="b64a2-109">The following screenshot shows and example for the Office 365 sign-in page on a desktop computer **before** a customization:</span></span>

![Office 365-inloggningssida före anpassning](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-before-customization.png)

<span data-ttu-id="b64a2-111">Följande skärmbild visar ett exempel på Office 365-inloggningssidan på en stationär dator **efter** en anpassning:</span><span class="sxs-lookup"><span data-stu-id="b64a2-111">The following screenshot shows and example for the Office 365 sign-in page on a desktop computer **after** a customization:</span></span>

![Office 365-inloggningssida efter anpassning](./media/active-directory-branding-custom-signon-azure-portal/sign-in-page-after-customization.png)

## <a name="customizing-the-sign-in-page"></a><span data-ttu-id="b64a2-113">Anpassa inloggningssidan</span><span class="sxs-lookup"><span data-stu-id="b64a2-113">Customizing the sign-in page</span></span>
<span data-ttu-id="b64a2-114">Du använder vanligtvis inloggningssidan om du behöver webbläsarbaserad åtkomst till molnappar och tjänster som din organisation prenumererar på.</span><span class="sxs-lookup"><span data-stu-id="b64a2-114">Typically, if you need browser-based access to your cloud apps and services that your organization subscribes to, you use the sign-in page.</span></span>

<span data-ttu-id="b64a2-115">Om du har gjort ändringar på inloggningssidan kan det ta upp till en timme innan ändringarna visas.</span><span class="sxs-lookup"><span data-stu-id="b64a2-115">If you have applied changes to your sign-in page, it can take up to an hour for the changes to appear.</span></span>

<span data-ttu-id="b64a2-116">En företagsanpassad inloggningssida visas bara när du besöker en tjänst med en URL som är specifik för en klientorganisation, till exempel https://outlook.com/**contoso**.com eller https://mail.**contoso**.com.</span><span class="sxs-lookup"><span data-stu-id="b64a2-116">A branded sign-in page only appears when you visit a service with a tenant-specific URL such as https://outlook.com/**contoso**.com, or https://mail.**contoso**.com.</span></span>

<span data-ttu-id="b64a2-117">När du besöker en tjänst med en URL som inte är specifik för en klientorganisation (t.ex. https://mail.office365.com) visas en icke företagsanpassad inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="b64a2-117">When you visit a service with non-tenant specific URLs (e.g.: https://mail.office365.com), a non-branded sign-in page appears.</span></span> <span data-ttu-id="b64a2-118">I så fall visas din anpassning först när du har angett ditt användar-ID eller då du har valt en användarikon.</span><span class="sxs-lookup"><span data-stu-id="b64a2-118">in this case, your branding appears once you have entered your user ID or you have selected a user tile.</span></span>

> [!NOTE]
> * <span data-ttu-id="b64a2-119">Domännamnet måste visas som ”aktiv” i den **domäner** del av Azure-portalen där du har konfigurerat anpassningen.</span><span class="sxs-lookup"><span data-stu-id="b64a2-119">Your domain name must appear as “Active" in the **Domains** portion of the Azure portal in which you have configured branding.</span></span> <span data-ttu-id="b64a2-120">Mer information finns i [lägga till anpassade domännamn](active-directory-domains-add-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b64a2-120">For more information, see [Add custom domain names](active-directory-domains-add-azure-portal.md).</span></span>
> * <span data-ttu-id="b64a2-121">Anpassningen av inloggningssidan överförs inte till Microsofts konsumentinloggningssida.</span><span class="sxs-lookup"><span data-stu-id="b64a2-121">Sign-in page branding doesn’t carry over to the consumer sign in page of Microsoft.</span></span> <span data-ttu-id="b64a2-122">Om du loggar in med ett Microsoft-konto kan du se en företagsanpassad lista över användarikoner som återges av Azure AD, men anpassning av din organisation inte gäller för Microsoft-konto-inloggningssidan.</span><span class="sxs-lookup"><span data-stu-id="b64a2-122">If you sign in with a Microsoft account, you may see a branded list of user tiles rendered by Azure AD, but the branding of your organization does not apply to the Microsoft account sign-in page.</span></span>
>
>

<span data-ttu-id="b64a2-123">På inloggningssidan gör kryssrutan **Håll mig inloggad** att användaren kan förbli inloggad när de stänger och öppnar webblösaren.</span><span class="sxs-lookup"><span data-stu-id="b64a2-123">On your sign-in page, the **Keep me signed in** checkbox allows a user to remain signed in when they close and re-open their browser.</span></span>

   ![Håll mig inloggad](./media/active-directory-branding-custom-signon-azure-portal/01.png)

<span data-ttu-id="b64a2-125">Den påverkar inte sessionens längd.</span><span class="sxs-lookup"><span data-stu-id="b64a2-125">It does not effect session lifetime.</span></span> <span data-ttu-id="b64a2-126">Du kan dölja kryssrutan på inloggningssidan för Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="b64a2-126">You can hide the checkbox on the Azure Active Directory sign-in page.</span></span>
<span data-ttu-id="b64a2-127">Om kryssrutan visas beror på inställningen för **Håll mig inloggad inaktiverats**.</span><span class="sxs-lookup"><span data-stu-id="b64a2-127">Whether the checkbox is displayed depends on the setting of **Keep me signed in disabled**.</span></span>

   ![Håll mig inloggad](./media/active-directory-branding-custom-signon-azure-portal/02.png)

<span data-ttu-id="b64a2-129">Konfigurera den här inställningen om du vill dölja kryssrutan **Ja**.</span><span class="sxs-lookup"><span data-stu-id="b64a2-129">To hide the checkbox, configure this setting to **Yes**.</span></span>

> [!NOTE]
> <span data-ttu-id="b64a2-130">För vissa funktioner i SharePoint Online och Office 2010 måste användarna kunna markera den här kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="b64a2-130">Some features of SharePoint Online and Office 2010 depend on users being able to check this box.</span></span> <span data-ttu-id="b64a2-131">Om du konfigurerar den här inställningen som dold kan eventuellt ytterligare och oväntade uppmaningar att logga in visas för dina användare.</span><span class="sxs-lookup"><span data-stu-id="b64a2-131">If you configure this setting to hidden, your users may see additional and unexpected prompts to sign-in.</span></span>
>
>

<span data-ttu-id="b64a2-132">**Lägga till företagsanpassning till din katalog:**</span><span class="sxs-lookup"><span data-stu-id="b64a2-132">**To add company branding to your directory:**</span></span>

1. <span data-ttu-id="b64a2-133">Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.</span><span class="sxs-lookup"><span data-stu-id="b64a2-133">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="b64a2-134">Välj **fler tjänster**, ange **användare och grupper** i textrutan och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="b64a2-134">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Öppna användarhantering](./media/active-directory-branding-custom-signon-azure-portal/user-management.png)
3. <span data-ttu-id="b64a2-136">På den **användare och grupper** bladet väljer **företagets företagsanpassning**.</span><span class="sxs-lookup"><span data-stu-id="b64a2-136">On the **Users and groups** blade, select **Company branding**.</span></span>
4. <span data-ttu-id="b64a2-137">På den **användare och grupper – företagets anpassning** bladet väljer den **redigera** kommando.</span><span class="sxs-lookup"><span data-stu-id="b64a2-137">On the **Users and groups - Company branding** blade, select the **Edit** command.</span></span>

    ![Redigera anpassning](./media/active-directory-branding-custom-signon-azure-portal/edit-branding.png)
5. <span data-ttu-id="b64a2-139">Ändra de element som du vill anpassa.</span><span class="sxs-lookup"><span data-stu-id="b64a2-139">Modify the elements you want to customize.</span></span> <span data-ttu-id="b64a2-140">Alla element är valfria.</span><span class="sxs-lookup"><span data-stu-id="b64a2-140">All elements are optional.</span></span>
6. <span data-ttu-id="b64a2-141">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b64a2-141">Click **Save**.</span></span>

<span data-ttu-id="b64a2-142">Det kan ta upp till en timme innan ändringarna inloggningssidan företagsanpassning ska visas.</span><span class="sxs-lookup"><span data-stu-id="b64a2-142">It can take up to an hour for any changes you made to the sign-in page branding to appear.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b64a2-143">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b64a2-143">Next steps</span></span>
[<span data-ttu-id="b64a2-144">Lägga till språkspecifik företagsanpassning</span><span class="sxs-lookup"><span data-stu-id="b64a2-144">Add language-specific company branding</span></span>](active-directory-branding-localize-azure-portal.md)
