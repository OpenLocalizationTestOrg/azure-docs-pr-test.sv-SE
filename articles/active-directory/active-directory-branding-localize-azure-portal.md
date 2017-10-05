---
title: "Lägga till språkspecifik företagsanpassning till inloggningssidan i Azure Active Directory | Microsoft Docs"
description: "Lär dig hur du lägger till ett visst språk företag bilder och text till en Azure-inloggningssida"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: a0310d6a-aaa7-4ea0-991d-6d3135b4382a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/04/2017
ms.author: curtand
ms.openlocfilehash: e1fe8d855386ceec39edbc985538cdf32d78a13b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-language-specific-company-branding-to-your-sign-in-page-in-the-azure-active-directory"></a><span data-ttu-id="3bc50-103">Lägga till språkspecifik företagsanpassning till inloggningssidan i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="3bc50-103">Add language-specific company branding to your sign-in page in the Azure Active Directory</span></span>
<span data-ttu-id="3bc50-104">För att undvika förvirring vill många företag använda ett enhetligt utseende på alla webbplatser och tjänster som de hanterar.</span><span class="sxs-lookup"><span data-stu-id="3bc50-104">To avoid confusion, many companies want to apply a consistent look and feel across all the websites and services they manage.</span></span> <span data-ttu-id="3bc50-105">Azure Active Directory erbjuder den här funktionen genom att låta dig anpassa utseendet på inloggningssidan med företagets logotyp och egna färgscheman.</span><span class="sxs-lookup"><span data-stu-id="3bc50-105">Azure Active Directory provides this capability by allowing you to customize the appearance of the sign-in page with your company logo and custom color schemes.</span></span> <span data-ttu-id="3bc50-106">På inloggningssidan är den sida som visas när du loggar in på Office 365 eller andra webbaserade program som använder Azure AD som identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="3bc50-106">The sign-in page is the page that appears when you sign in to Office 365 or other web-based applications that are using Azure AD as your identity provider.</span></span> <span data-ttu-id="3bc50-107">Du kan interagera med den här sidan om du vill ange dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="3bc50-107">You interact with this page to enter your credentials.</span></span>

## <a name="customizing-the-sign-in-page-for-another-language"></a><span data-ttu-id="3bc50-108">Anpassa inloggningssidan för ett annat språk</span><span class="sxs-lookup"><span data-stu-id="3bc50-108">Customizing the sign-in page for another language</span></span>
<span data-ttu-id="3bc50-109">Du kan lägga till språkspecifik element din anpassade inloggningssidan endast om du redan har skapat en anpassad inloggningssida som beskrivs i [lägga till företagsprofilering för din inloggningssidan](active-directory-branding-custom-signon-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3bc50-109">You can add language-specific elements to your custom sign-in page only if you have already created a custom sign-in page as described in [Add company branding to your sign-in page](active-directory-branding-custom-signon-azure-portal.md).</span></span> <span data-ttu-id="3bc50-110">Du kan konfigurera en inloggningssida per katalog med en standarduppsättning med anpassningsbara element.</span><span class="sxs-lookup"><span data-stu-id="3bc50-110">You can configure one sign-in page per directory with a default set of customizable elements.</span></span> <span data-ttu-id="3bc50-111">När du har konfigurerat en standarduppsättning av sidelement kan konfigurera du fler versioner för olika språk.</span><span class="sxs-lookup"><span data-stu-id="3bc50-111">After you’ve configured the default set of page elements, you can configure more versions for different locales.</span></span> <span data-ttu-id="3bc50-112">Du kan också blanda och matcha olika element.</span><span class="sxs-lookup"><span data-stu-id="3bc50-112">You can also mix and match various elements.</span></span> <span data-ttu-id="3bc50-113">Du kan till exempel:</span><span class="sxs-lookup"><span data-stu-id="3bc50-113">For example, you could:</span></span>

* <span data-ttu-id="3bc50-114">Skapa en standard **inloggning sidan avbildningen** som passar alla kulturer och sedan skapa specifika versioner för engelska och franska.</span><span class="sxs-lookup"><span data-stu-id="3bc50-114">Create a default **Sign-in page image** that works for all cultures, then create specific versions for English and French.</span></span> <span data-ttu-id="3bc50-115">När du ställer in din webbläsare till någon av dessa språk språkspecifika bilden visas, medan standardbilden visas för andra språk.</span><span class="sxs-lookup"><span data-stu-id="3bc50-115">When you set your browsers to one of these two languages, the language-specific image appears, while the default illustration appears for all other languages.</span></span>
* <span data-ttu-id="3bc50-116">Konfigurera olika logotyper för din organisation (till exempel japanska och hebreiska versioner).</span><span class="sxs-lookup"><span data-stu-id="3bc50-116">Configure different logos for your organization (for example, Japanese or Hebrew versions).</span></span>

<span data-ttu-id="3bc50-117">Vi rekommenderar att du behåller antal språk variationer låg för underhåll och prestandaskäl.</span><span class="sxs-lookup"><span data-stu-id="3bc50-117">We recommend that you keep the number of language variations low, for maintenance and performance reasons.</span></span>

<span data-ttu-id="3bc50-118">**Lägga till företagsanpassning till din katalog:**</span><span class="sxs-lookup"><span data-stu-id="3bc50-118">**To add company branding to your directory:**</span></span>

1. <span data-ttu-id="3bc50-119">Logga in på den [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för katalogen.</span><span class="sxs-lookup"><span data-stu-id="3bc50-119">Sign in to the [Azure portal](https://portal.azure.com) with an account that's a global admin for the directory.</span></span>
2. <span data-ttu-id="3bc50-120">Välj **fler tjänster**, ange **användare och grupper** i textrutan och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="3bc50-120">Select **More services**, enter **Users and groups** in the text box, and then select **Enter**.</span></span>

   ![Öppna användarhantering](./media/active-directory-branding-localize-azure-portal/user-management.png)
3. <span data-ttu-id="3bc50-122">På den **användare och grupper** bladet väljer **företagets företagsanpassning**.</span><span class="sxs-lookup"><span data-stu-id="3bc50-122">On the **Users and groups** blade, select **Company branding**.</span></span>
4. <span data-ttu-id="3bc50-123">På den **användare och grupper – företagets anpassning** bladet väljer den **lägga till språk** kommando.</span><span class="sxs-lookup"><span data-stu-id="3bc50-123">On the **Users and groups - Company branding** blade, select the **Add language** command.</span></span>

    ![Lägga till språkspecifik företagsanpassning element](./media/active-directory-branding-localize-azure-portal/add-language.png)
5. <span data-ttu-id="3bc50-125">Ändra de element som du vill anpassa.</span><span class="sxs-lookup"><span data-stu-id="3bc50-125">Modify the elements you want to customize.</span></span> <span data-ttu-id="3bc50-126">Alla element är valfria.</span><span class="sxs-lookup"><span data-stu-id="3bc50-126">All elements are optional.</span></span>
6. <span data-ttu-id="3bc50-127">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3bc50-127">Click **Save**.</span></span>

<span data-ttu-id="3bc50-128">Det kan ta upp till en timme innan ändringarna inloggningssidan företagsanpassning ska visas.</span><span class="sxs-lookup"><span data-stu-id="3bc50-128">It can take up to an hour for any changes you made to the sign-in page branding to appear.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3bc50-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3bc50-129">Next steps</span></span>
[<span data-ttu-id="3bc50-130">Lägga till företagsanpassning till din inloggningssida</span><span class="sxs-lookup"><span data-stu-id="3bc50-130">Add company branding to your sign-in page</span></span>](active-directory-branding-custom-signon-azure-portal.md)
