---
title: 'Azure Active Directory B2C: Inaktivera e-Postverifiering under registreringen konsumenten | Microsoft Docs'
description: Ett avsnitt som visar hur du inaktiverar e-Postverifiering under konsumenten registrering i Azure Active Directory B2C
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
ms.openlocfilehash: d8e44a8aade60d21734477d60bccc2bd5194436e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-disable-email-verification-during-consumer-sign-up"></a><span data-ttu-id="2f3cd-103">Azure Active Directory B2C: Inaktivera e-Postverifiering under konsumenten registrering</span><span class="sxs-lookup"><span data-stu-id="2f3cd-103">Azure Active Directory B2C: Disable email verification during consumer sign-up</span></span>
<span data-ttu-id="2f3cd-104">När aktiverat, kan Azure Active Directory (AD Azure) B2C en konsument registrera dig för program genom att tillhandahålla en e-postadress och skapa ett lokalt konto.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-104">When enabled, Azure Active Directory (Azure AD) B2C gives a consumer the ability to sign up for applications by providing an email address and creating a local account.</span></span> <span data-ttu-id="2f3cd-105">Azure AD B2C garanterar giltiga e-postadresser genom att kräva att konsumenterna kan kontrollera dem under registreringen.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-105">Azure AD B2C ensures valid email addresses by requiring consumers to verify them during the sign-up process.</span></span> <span data-ttu-id="2f3cd-106">Det förhindrar också en skadlig automatiserad process genererar falska konton för programmen.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-106">It also prevents a malicious automated process from generating fake accounts for the applications.</span></span>

<span data-ttu-id="2f3cd-107">Vissa programutvecklare föredrar att hoppa över e-Postverifiering under registreringen och i stället har konsumenter kontrollera e-postadressen senare.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-107">Some application developers prefer to skip email verification during the sign-up process and instead have consumers verify the email address later.</span></span> <span data-ttu-id="2f3cd-108">Stöd för detta kan Azure AD B2C konfigureras för att inaktivera e-verifiering.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-108">To support this, Azure AD B2C can be configured to disable email verification.</span></span> <span data-ttu-id="2f3cd-109">Gör detta skapar en jämnare registreringsprocessen och ger utvecklare möjlighet att skilja användare som har verifierats sina e-postadress från dessa användare inte har.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-109">Doing so creates a smoother sign-up process and gives developers the flexibility to differentiate the consumers that have verified their email address from those consumers that have not.</span></span>

<span data-ttu-id="2f3cd-110">Principer för registrering har e-Postverifiering aktiverad som standard.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-110">By default, sign-up policies have email verification turned on.</span></span> <span data-ttu-id="2f3cd-111">Använd följande steg för att stänga av den:</span><span class="sxs-lookup"><span data-stu-id="2f3cd-111">Use the following steps to turn it off:</span></span>

1. <span data-ttu-id="2f3cd-112">[Följ dessa steg för att gå till B2C-funktionsbladet på Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="2f3cd-112">[Follow these steps to navigate to the B2C features blade on the Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="2f3cd-113">Klicka på **principer för registrering** eller **principer för registrering eller inloggning** beroende på vad du har konfigurerat för registrering.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-113">Click **Sign-up policies** or **Sign-up or sign-in policies** depending on what you configured for sign-up.</span></span>
3. <span data-ttu-id="2f3cd-114">Klicka på principen (till exempel ”B2C_1_SiUp”) för att öppna den.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-114">Click your policy (for example, "B2C_1_SiUp") to open it.</span></span> <span data-ttu-id="2f3cd-115">Klicka på **redigera** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-115">Click **Edit** at the top of the blade.</span></span>
4. <span data-ttu-id="2f3cd-116">Klicka på **sidan anpassningar**.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-116">Click **Page UI Customization**.</span></span>
5. <span data-ttu-id="2f3cd-117">Klicka på **registreringssidan för lokalt konto**.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-117">Click **Local account sign-up page**.</span></span>
6. <span data-ttu-id="2f3cd-118">Klicka på **e-postadress** i den **namn** kolumnen under den **registreringsattribut** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-118">Click **Email Address** in the **Name** column under the **Sign-up attributes** section.</span></span>
7. <span data-ttu-id="2f3cd-119">Växla den **Kräv verifiering** att **nr**.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-119">Toggle the **Require verification** option to **No**.</span></span>
8. <span data-ttu-id="2f3cd-120">Klicka på **OK** längst ned tills du når den **Redigera princip** bladet.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-120">Click **OK** at the bottom until you reach the **Edit policy** blade.</span></span>
9. <span data-ttu-id="2f3cd-121">Klicka på **spara** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-121">Click **Save** at the top of the blade.</span></span> <span data-ttu-id="2f3cd-122">Du är klar!</span><span class="sxs-lookup"><span data-stu-id="2f3cd-122">You're done!</span></span>

> [!NOTE]
> <span data-ttu-id="2f3cd-123">Inaktivera e-Postverifiering i registreringsprocessen kan leda till att skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-123">Disabling email verification in the sign-up process may lead to spam.</span></span> <span data-ttu-id="2f3cd-124">Om du inaktiverar standardversionen rekommenderar vi att lägga till egna verifieringssystem.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-124">If you disable the default one, we recommend adding your own verification system.</span></span>
> 
> 

<span data-ttu-id="2f3cd-125">Vi är alltid öppna för feedback och förslag!</span><span class="sxs-lookup"><span data-stu-id="2f3cd-125">We are always open to feedback and suggestions!</span></span> <span data-ttu-id="2f3cd-126">Om du har problem med det här avsnittet eller rekommendationer för att förbättra det här innehållet, skulle vi uppskattar din feedback längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="2f3cd-126">If you have any difficulties with this topic, or have recommendations for improving this content, we would appreciate your feedback at the bottom of the page.</span></span> <span data-ttu-id="2f3cd-127">För funktionsbegäranden, lägga till dem i [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span><span class="sxs-lookup"><span data-stu-id="2f3cd-127">For feature requests, add them to [UserVoice](https://feedback.azure.com/forums/169401-azure-active-directory/category/160596-b2c).</span></span>