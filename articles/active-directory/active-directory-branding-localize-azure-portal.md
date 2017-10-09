---
title: "aaaAdd språkspecifik företagsanpassning tooyour inloggningssidan i hello Azure Active Directory | Microsoft Docs"
description: "Lär dig hur tooadd en språkspecifika företagets företagsanpassning bilder och text tooan Azure inloggning sida"
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
ms.openlocfilehash: 1e33c31abc242e8455290beb1f03760be7b9ac42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-language-specific-company-branding-tooyour-sign-in-page-in-hello-azure-active-directory"></a><span data-ttu-id="f96a9-103">Lägga till språkspecifik företagsanpassning tooyour inloggningssidan i hello Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="f96a9-103">Add language-specific company branding tooyour sign-in page in hello Azure Active Directory</span></span>
<span data-ttu-id="f96a9-104">tooavoid förvirring vill många företag tooapply ett konsekvent utseende på alla hello webbplatser och tjänster som de hanterar.</span><span class="sxs-lookup"><span data-stu-id="f96a9-104">tooavoid confusion, many companies want tooapply a consistent look and feel across all hello websites and services they manage.</span></span> <span data-ttu-id="f96a9-105">Azure Active Directory erbjuder den här funktionen genom att låta dig toocustomize hello utseendet på hello-inloggningssida med företagets logotyp och egna färgscheman.</span><span class="sxs-lookup"><span data-stu-id="f96a9-105">Azure Active Directory provides this capability by allowing you toocustomize hello appearance of hello sign-in page with your company logo and custom color schemes.</span></span> <span data-ttu-id="f96a9-106">hello-inloggningssida är hello sida som visas när du loggar in tooOffice 365 eller andra webbaserade program som använder Azure AD som identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="f96a9-106">hello sign-in page is hello page that appears when you sign in tooOffice 365 or other web-based applications that are using Azure AD as your identity provider.</span></span> <span data-ttu-id="f96a9-107">Du interagera med den här sidan tooenter dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f96a9-107">You interact with this page tooenter your credentials.</span></span>

## <a name="customizing-hello-sign-in-page-for-another-language"></a><span data-ttu-id="f96a9-108">Anpassa hello inloggningssidan för ett annat språk</span><span class="sxs-lookup"><span data-stu-id="f96a9-108">Customizing hello sign-in page for another language</span></span>
<span data-ttu-id="f96a9-109">Du kan lägga till språkspecifik element tooyour anpassad inloggningssida visas bara om du redan har skapat en anpassad inloggningssida som beskrivs i [lägga till företagsanpassning tooyour inloggningssidan](active-directory-branding-custom-signon-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="f96a9-109">You can add language-specific elements tooyour custom sign-in page only if you have already created a custom sign-in page as described in [Add company branding tooyour sign-in page](active-directory-branding-custom-signon-azure-portal.md).</span></span> <span data-ttu-id="f96a9-110">Du kan konfigurera en inloggningssida per katalog med en standarduppsättning med anpassningsbara element.</span><span class="sxs-lookup"><span data-stu-id="f96a9-110">You can configure one sign-in page per directory with a default set of customizable elements.</span></span> <span data-ttu-id="f96a9-111">När du har konfigurerat hello standarduppsättning sidelement kan konfigurera du fler versioner för olika språk.</span><span class="sxs-lookup"><span data-stu-id="f96a9-111">After you’ve configured hello default set of page elements, you can configure more versions for different locales.</span></span> <span data-ttu-id="f96a9-112">Du kan också blanda och matcha olika element.</span><span class="sxs-lookup"><span data-stu-id="f96a9-112">You can also mix and match various elements.</span></span> <span data-ttu-id="f96a9-113">Du kan till exempel:</span><span class="sxs-lookup"><span data-stu-id="f96a9-113">For example, you could:</span></span>

* <span data-ttu-id="f96a9-114">Skapa en standard **inloggning sidan avbildningen** som passar alla kulturer och sedan skapa specifika versioner för engelska och franska.</span><span class="sxs-lookup"><span data-stu-id="f96a9-114">Create a default **Sign-in page image** that works for all cultures, then create specific versions for English and French.</span></span> <span data-ttu-id="f96a9-115">När du ställer in din webbläsare tooone av dessa språk visas hello språkspecifika bilden, medan hello standardbilden visas för andra språk.</span><span class="sxs-lookup"><span data-stu-id="f96a9-115">When you set your browsers tooone of these two languages, hello language-specific image appears, while hello default illustration appears for all other languages.</span></span>
* <span data-ttu-id="f96a9-116">Konfigurera olika logotyper för din organisation (till exempel japanska och hebreiska versioner).</span><span class="sxs-lookup"><span data-stu-id="f96a9-116">Configure different logos for your organization (for example, Japanese or Hebrew versions).</span></span>

<span data-ttu-id="f96a9-117">Vi rekommenderar att du behåller hello antal språk variationer låg för underhåll och prestandaskäl.</span><span class="sxs-lookup"><span data-stu-id="f96a9-117">We recommend that you keep hello number of language variations low, for maintenance and performance reasons.</span></span>

<span data-ttu-id="f96a9-118">**tooadd företagets företagsanpassning tooyour katalog:**</span><span class="sxs-lookup"><span data-stu-id="f96a9-118">**tooadd company branding tooyour directory:**</span></span>

1. <span data-ttu-id="f96a9-119">Logga in toohello [Azure-portalen](https://portal.azure.com) med ett konto som är en global administratör för hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="f96a9-119">Sign in toohello [Azure portal](https://portal.azure.com) with an account that's a global admin for hello directory.</span></span>
2. <span data-ttu-id="f96a9-120">Välj **fler tjänster**, ange **användare och grupper** i hello textruta och välj sedan **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="f96a9-120">Select **More services**, enter **Users and groups** in hello text box, and then select **Enter**.</span></span>

   ![Öppna användarhantering](./media/active-directory-branding-localize-azure-portal/user-management.png)
3. <span data-ttu-id="f96a9-122">På hello **användare och grupper** bladet väljer **företagets företagsanpassning**.</span><span class="sxs-lookup"><span data-stu-id="f96a9-122">On hello **Users and groups** blade, select **Company branding**.</span></span>
4. <span data-ttu-id="f96a9-123">På hello **användare och grupper – företagets anpassning** bladet, Välj hello **lägga till språk** kommando.</span><span class="sxs-lookup"><span data-stu-id="f96a9-123">On hello **Users and groups - Company branding** blade, select hello **Add language** command.</span></span>

    ![Lägga till språkspecifik företagsanpassning element](./media/active-directory-branding-localize-azure-portal/add-language.png)
5. <span data-ttu-id="f96a9-125">Ändra hello-element som du vill toocustomize.</span><span class="sxs-lookup"><span data-stu-id="f96a9-125">Modify hello elements you want toocustomize.</span></span> <span data-ttu-id="f96a9-126">Alla element är valfria.</span><span class="sxs-lookup"><span data-stu-id="f96a9-126">All elements are optional.</span></span>
6. <span data-ttu-id="f96a9-127">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="f96a9-127">Click **Save**.</span></span>

<span data-ttu-id="f96a9-128">Det kan ta upp tooan timme för alla ändringar du gjort toohello inloggning sidan företagsanpassning tooappear.</span><span class="sxs-lookup"><span data-stu-id="f96a9-128">It can take up tooan hour for any changes you made toohello sign-in page branding tooappear.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f96a9-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f96a9-129">Next steps</span></span>
[<span data-ttu-id="f96a9-130">Lägga till företagsanpassning tooyour inloggningssidan</span><span class="sxs-lookup"><span data-stu-id="f96a9-130">Add company branding tooyour sign-in page</span></span>](active-directory-branding-custom-signon-azure-portal.md)
