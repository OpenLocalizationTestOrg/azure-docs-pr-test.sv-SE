---
title: 'Azure Active Directory B2C: Inaktivera e-Postverifiering under registreringen konsumenten | Microsoft Docs'
description: Ett avsnitt som visar hur toodisable e-verifiering under konsumenten registrering i Azure Active Directory B2C
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 433f32b8-96d2-4113-aa82-efcf42fa9827
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 2/06/2017
ms.author: parakhj
ms.openlocfilehash: a8a42eddcb577725f04d70e1b1ebbebf10b5937c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a><span data-ttu-id="960b6-103">Azure Active Directory B2C: Inaktivera e-Postverifiering under konsumenten registrering</span><span class="sxs-lookup"><span data-stu-id="960b6-103">Azure Active Directory B2C: Disable email verification during consumer sign-up</span></span>
<span data-ttu-id="960b6-104">När aktiverad hello Azure Active Directory (AD Azure) B2C ger en konsument möjlighet toosign för program genom att tillhandahålla en e-postadress och skapa ett lokalt konto.</span><span class="sxs-lookup"><span data-stu-id="960b6-104">When enabled, Azure Active Directory (Azure AD) B2C gives a consumer hello ability toosign up for applications by providing an email address and creating a local account.</span></span> <span data-ttu-id="960b6-105">Azure AD B2C garanterar giltiga e-postadresser genom att kräva konsumenter tooverify dem under hello registreringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="960b6-105">Azure AD B2C ensures valid email addresses by requiring consumers tooverify them during hello sign-up process.</span></span> <span data-ttu-id="960b6-106">Det förhindrar också en skadlig automatiserad process genererar falska konton för hello program.</span><span class="sxs-lookup"><span data-stu-id="960b6-106">It also prevents a malicious automated process from generating fake accounts for hello applications.</span></span>

<span data-ttu-id="960b6-107">Vissa programutvecklare föredrar tooskip e-Postverifiering under hello registreringsprocessen och i stället har konsumenter verifiera hello e-postadress senare.</span><span class="sxs-lookup"><span data-stu-id="960b6-107">Some application developers prefer tooskip email verification during hello sign-up process and instead have consumers verify hello email address later.</span></span> <span data-ttu-id="960b6-108">toosupport detta Azure AD B2C kan vara konfigurerade toodisable e-verifiering.</span><span class="sxs-lookup"><span data-stu-id="960b6-108">toosupport this, Azure AD B2C can be configured toodisable email verification.</span></span> <span data-ttu-id="960b6-109">Gör detta skapar en jämnare registreringsprocessen och ger utvecklare hello flexibilitet toodifferentiate hello konsumenter som sin e-postadress från dessa användare inte har verifierats.</span><span class="sxs-lookup"><span data-stu-id="960b6-109">Doing so creates a smoother sign-up process and gives developers hello flexibility toodifferentiate hello consumers that have verified their email address from those consumers that have not.</span></span>

<span data-ttu-id="960b6-110">Principer för registrering har e-Postverifiering aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="960b6-110">By default, sign-up policies have email verification turned on.</span></span> <span data-ttu-id="960b6-111">Använd hello följande steg tooturn av:</span><span class="sxs-lookup"><span data-stu-id="960b6-111">Use hello following steps tooturn it off:</span></span>

1. <span data-ttu-id="960b6-112">[Följ dessa steg toonavigate toohello B2C-funktionsbladet på hello Azure-portalen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="960b6-112">[Follow these steps toonavigate toohello B2C features blade on hello Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="960b6-113">Klicka på **principer för registrering** eller **principer för registrering eller inloggning** beroende på vad du har konfigurerat för registrering.</span><span class="sxs-lookup"><span data-stu-id="960b6-113">Click **Sign-up policies** or **Sign-up or sign-in policies** depending on what you configured for sign-up.</span></span>
3. <span data-ttu-id="960b6-114">Klicka på principen (till exempel ”B2C_1_SiUp”)-tooopen den.</span><span class="sxs-lookup"><span data-stu-id="960b6-114">Click your policy (for example, "B2C_1_SiUp") tooopen it.</span></span> <span data-ttu-id="960b6-115">Klicka på **redigera** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="960b6-115">Click **Edit** at hello top of hello blade.</span></span>
4. <span data-ttu-id="960b6-116">Klicka på **sidan anpassningar**.</span><span class="sxs-lookup"><span data-stu-id="960b6-116">Click **Page UI Customization**.</span></span>
5. <span data-ttu-id="960b6-117">Klicka på **registreringssidan för lokalt konto**.</span><span class="sxs-lookup"><span data-stu-id="960b6-117">Click **Local account sign-up page**.</span></span>
6. <span data-ttu-id="960b6-118">Klicka på **e-postadress** i hello **namn** kolumnen under hello **registreringsattribut** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="960b6-118">Click **Email Address** in hello **Name** column under hello **Sign-up attributes** section.</span></span>
7. <span data-ttu-id="960b6-119">Växla hello **Kräv verifiering** alternativet för**nr**.</span><span class="sxs-lookup"><span data-stu-id="960b6-119">Toggle hello **Require verification** option too**No**.</span></span>
8. <span data-ttu-id="960b6-120">Klicka på **OK** längst ned hello tills du når hello **Redigera princip** bladet.</span><span class="sxs-lookup"><span data-stu-id="960b6-120">Click **OK** at hello bottom until you reach hello **Edit policy** blade.</span></span>
9. <span data-ttu-id="960b6-121">Klicka på **spara** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="960b6-121">Click **Save** at hello top of hello blade.</span></span> <span data-ttu-id="960b6-122">Du är klar!</span><span class="sxs-lookup"><span data-stu-id="960b6-122">You're done!</span></span>

> [!NOTE]
> <span data-ttu-id="960b6-123">Inaktivera e-Postverifiering i hello registreringsprocessen kan det leda till att toospam.</span><span class="sxs-lookup"><span data-stu-id="960b6-123">Disabling email verification in hello sign-up process may lead toospam.</span></span> <span data-ttu-id="960b6-124">Om du inaktiverar hello standardversionen rekommenderar vi att lägga till egna verifieringssystem.</span><span class="sxs-lookup"><span data-stu-id="960b6-124">If you disable hello default one, we recommend adding your own verification system.</span></span>
> 
> 

<span data-ttu-id="960b6-125">Vi är alltid öppna toofeedback och förslag!</span><span class="sxs-lookup"><span data-stu-id="960b6-125">We are always open toofeedback and suggestions!</span></span> <span data-ttu-id="960b6-126">Om du har problem med det här avsnittet eller rekommendationer för att förbättra det här innehållet, skulle vi uppskattar din feedback på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="960b6-126">If you have any difficulties with this topic, or have recommendations for improving this content, we would appreciate your feedback at hello bottom of hello page.</span></span> <span data-ttu-id="960b6-127">För funktionsbegäranden, lägga till dem för[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="960b6-127">For feature requests, add them too[UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>
