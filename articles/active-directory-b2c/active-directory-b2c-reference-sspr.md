---
title: "Azure Active Directory B2C: Självbetjäning för lösenordsåterställning | Microsoft Docs"
description: "Ett avsnitt som visar hur tooset självbetjäning lösenord återställa för dina användare i Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: curtand
ms.assetid: c87ed86e-1520-42b1-8c31-46cd44ed5310
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: ff87eea42259b610702da73af35d0a38eb7dd33d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a><span data-ttu-id="76c6a-103">Azure Active Directory B2C: Konfigurera Självbetjäning för återställning av lösenord för dina användare</span><span class="sxs-lookup"><span data-stu-id="76c6a-103">Azure Active Directory B2C: Set up self-service password reset for your consumers</span></span>
<span data-ttu-id="76c6a-104">Med hello Självbetjäning för återställning av lösenord funktion, dina användare (som har registrerat sig för lokala konton) kan återställa sina lösenord på egen hand.</span><span class="sxs-lookup"><span data-stu-id="76c6a-104">With hello self-service password reset feature, your consumers (who have signed up for local accounts) can reset their passwords on their own.</span></span> <span data-ttu-id="76c6a-105">Detta minskar avsevärt hello belastningen på supportpersonal, särskilt om programmet har miljontals konsumenter med hjälp av den regelbundet.</span><span class="sxs-lookup"><span data-stu-id="76c6a-105">This significantly reduces hello burden on your support staff, especially if your application has millions of consumers using it on a regular basis.</span></span> <span data-ttu-id="76c6a-106">För närvarande stöder vi bara med en verifierad e-postadress som en återställningsmetod för.</span><span class="sxs-lookup"><span data-stu-id="76c6a-106">Currently, we only support using a verified email address as a recovery method.</span></span> <span data-ttu-id="76c6a-107">Vi lägger till ytterligare återställningsmetoder (verifierade telefonnummer, säkerhetsfrågor osv.) hello framtiden.</span><span class="sxs-lookup"><span data-stu-id="76c6a-107">We will add additional recovery methods (verified phone number, security questions, etc.) in hello future.</span></span>

> [!NOTE]
> <span data-ttu-id="76c6a-108">Den här artikeln gäller tooself service lösenord återställning som används i hello kontexten för en princip för inloggning.</span><span class="sxs-lookup"><span data-stu-id="76c6a-108">This article applies tooself-service password reset used in hello context of a sign-in policy.</span></span> <span data-ttu-id="76c6a-109">Om du behöver helt anpassningsbar Återställ lösenordsprinciper anropas från din app Se [i den här artikeln](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).</span><span class="sxs-lookup"><span data-stu-id="76c6a-109">If you need fully customizable password reset policies invoked from your app, see [this article](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).</span></span>
> 
> 

<span data-ttu-id="76c6a-110">Som standard i katalogen inte självbetjäning lösenord återställning aktiverat.</span><span class="sxs-lookup"><span data-stu-id="76c6a-110">By default, your directory will not have self-service password reset turned on.</span></span> <span data-ttu-id="76c6a-111">Använd hello följande steg tooturn på:</span><span class="sxs-lookup"><span data-stu-id="76c6a-111">Use hello following steps tooturn it on:</span></span>

1. <span data-ttu-id="76c6a-112">Logga in toohello [klassiska Azure-portalen](https://manage.windowsazure.com/) som Hej administratör för prenumeration.</span><span class="sxs-lookup"><span data-stu-id="76c6a-112">Sign in toohello [Azure classic portal](https://manage.windowsazure.com/) as hello Subscription Administrator.</span></span> <span data-ttu-id="76c6a-113">Detta är hello samma arbets- eller skolkonto eller hello samma Microsoft-konto som du använde toocreate din katalog.</span><span class="sxs-lookup"><span data-stu-id="76c6a-113">This is hello same work or school account or hello same Microsoft account that you used toocreate your directory.</span></span>
2. <span data-ttu-id="76c6a-114">Navigera toohello Active Directory-tillägget hello navigeringsfältet hello vänster.</span><span class="sxs-lookup"><span data-stu-id="76c6a-114">Navigate toohello Active Directory extension on hello navigation bar on hello left side.</span></span>
3. <span data-ttu-id="76c6a-115">Hitta din katalog under hello **Directory** och klicka på den.</span><span class="sxs-lookup"><span data-stu-id="76c6a-115">Find your directory under hello **Directory** tab and click it.</span></span>
4. <span data-ttu-id="76c6a-116">Klicka på hello **konfigurera** fliken.</span><span class="sxs-lookup"><span data-stu-id="76c6a-116">Click hello **Configure** tab.</span></span>
5. <span data-ttu-id="76c6a-117">Bläddra nedåt toohello **princip för lösenordsåterställning för användare** avsnittet och växla hello **användare som har aktiverats för lösenordsåterställning** alternativet för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="76c6a-117">Scroll down toohello **User password reset policy** section and toggle hello **Users enabled for password reset** option too**YES**.</span></span> <span data-ttu-id="76c6a-118">Observera att hello **alternativ e-postadress** alternativet är markerat; lämna den som den är.</span><span class="sxs-lookup"><span data-stu-id="76c6a-118">Notice that hello **Alternate Email Address** option is checked; leave it as it is.</span></span>
   
    ![Återställning av lösenord för självbetjäning](./media/active-directory-b2c-reference-sspr/sspr.png)
6. <span data-ttu-id="76c6a-120">Klicka på **spara** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="76c6a-120">Click **Save** at hello bottom of hello page.</span></span> <span data-ttu-id="76c6a-121">Du är klar!</span><span class="sxs-lookup"><span data-stu-id="76c6a-121">You're done!</span></span>

<span data-ttu-id="76c6a-122">tootest Använd hello ”kör nu” funktioner på Logga in princip som har lokala konton som en identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="76c6a-122">tootest, use hello "Run now" feature on any sign-in policy that has local accounts as an identity provider.</span></span> <span data-ttu-id="76c6a-123">På hello lokalt konto logga in sidan (där du anger en e-postadress och lösenord, eller ett användarnamn och lösenord), klicka på **kan inte komma åt ditt konto?** tooverify hello användarfunktioner.</span><span class="sxs-lookup"><span data-stu-id="76c6a-123">On hello local account sign-in page (where you enter an email address and password, or a username and password), click **Can't access your account?** tooverify hello consumer experience.</span></span>

> [!NOTE]
> <span data-ttu-id="76c6a-124">hello självbetjäning lösenord Återställ sidor kan anpassas med hjälp av hello [funktionen för företagsanpassning](../active-directory/active-directory-add-company-branding.md).</span><span class="sxs-lookup"><span data-stu-id="76c6a-124">hello self-service password reset pages can be customized by using hello [company branding feature](../active-directory/active-directory-add-company-branding.md).</span></span>
> 
> 

