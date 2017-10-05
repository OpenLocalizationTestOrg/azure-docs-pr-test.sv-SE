---
title: "Azure Active Directory B2C: Självbetjäning för lösenordsåterställning | Microsoft Docs"
description: "Ett avsnitt som visar hur du ställer in Självbetjäning för återställning av lösenord för dina användare i Azure Active Directory B2C"
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
ms.openlocfilehash: 0508868e3b00c5771cc26038a3dd71fde6625a84
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a><span data-ttu-id="fe28a-103">Azure Active Directory B2C: Konfigurera Självbetjäning för återställning av lösenord för dina användare</span><span class="sxs-lookup"><span data-stu-id="fe28a-103">Azure Active Directory B2C: Set up self-service password reset for your consumers</span></span>
<span data-ttu-id="fe28a-104">Med funktionen för återställning av självbetjäning lösenord kan dina användare (som har registrerat sig för lokala konton) återställa sina lösenord på egen hand.</span><span class="sxs-lookup"><span data-stu-id="fe28a-104">With the self-service password reset feature, your consumers (who have signed up for local accounts) can reset their passwords on their own.</span></span> <span data-ttu-id="fe28a-105">Detta minskar avsevärt belastningen på supportpersonal, särskilt om programmet har miljontals konsumenter med hjälp av den regelbundet.</span><span class="sxs-lookup"><span data-stu-id="fe28a-105">This significantly reduces the burden on your support staff, especially if your application has millions of consumers using it on a regular basis.</span></span> <span data-ttu-id="fe28a-106">För närvarande stöder vi bara med en verifierad e-postadress som en återställningsmetod för.</span><span class="sxs-lookup"><span data-stu-id="fe28a-106">Currently, we only support using a verified email address as a recovery method.</span></span> <span data-ttu-id="fe28a-107">Vi lägger till ytterligare återställningsmetoder (verifierade telefonnummer, säkerhetsfrågor osv.) i framtiden.</span><span class="sxs-lookup"><span data-stu-id="fe28a-107">We will add additional recovery methods (verified phone number, security questions, etc.) in the future.</span></span>

> [!NOTE]
> <span data-ttu-id="fe28a-108">Den här artikeln gäller självbetjäning lösenord återställning som används i samband med en princip för inloggning.</span><span class="sxs-lookup"><span data-stu-id="fe28a-108">This article applies to self-service password reset used in the context of a sign-in policy.</span></span> <span data-ttu-id="fe28a-109">Om du behöver helt anpassningsbar Återställ lösenordsprinciper anropas från din app Se [i den här artikeln](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).</span><span class="sxs-lookup"><span data-stu-id="fe28a-109">If you need fully customizable password reset policies invoked from your app, see [this article](active-directory-b2c-reference-policies.md#create-a-password-reset-policy).</span></span>
> 
> 

<span data-ttu-id="fe28a-110">Som standard i katalogen inte självbetjäning lösenord återställning aktiverat.</span><span class="sxs-lookup"><span data-stu-id="fe28a-110">By default, your directory will not have self-service password reset turned on.</span></span> <span data-ttu-id="fe28a-111">Använd följande steg för att aktivera den:</span><span class="sxs-lookup"><span data-stu-id="fe28a-111">Use the following steps to turn it on:</span></span>

1. <span data-ttu-id="fe28a-112">Logga in på [den klassiska Azure-portalen](https://manage.windowsazure.com/) som administratör för prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="fe28a-112">Sign in to the [Azure classic portal](https://manage.windowsazure.com/) as the Subscription Administrator.</span></span> <span data-ttu-id="fe28a-113">Detta är samma arbets- eller skolkonto eller samma Microsoft-konto som du använde för att skapa katalogen.</span><span class="sxs-lookup"><span data-stu-id="fe28a-113">This is the same work or school account or the same Microsoft account that you used to create your directory.</span></span>
2. <span data-ttu-id="fe28a-114">Gå till Active Directory-tillägget i navigeringsfältet till vänster.</span><span class="sxs-lookup"><span data-stu-id="fe28a-114">Navigate to the Active Directory extension on the navigation bar on the left side.</span></span>
3. <span data-ttu-id="fe28a-115">Hitta din katalog under den **Directory** och klicka på den.</span><span class="sxs-lookup"><span data-stu-id="fe28a-115">Find your directory under the **Directory** tab and click it.</span></span>
4. <span data-ttu-id="fe28a-116">Klicka på fliken **Konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="fe28a-116">Click the **Configure** tab.</span></span>
5. <span data-ttu-id="fe28a-117">Rulla ned till den **princip för lösenordsåterställning för användare** avsnittet och växla den **användare som har aktiverats för lösenordsåterställning** att **Ja**.</span><span class="sxs-lookup"><span data-stu-id="fe28a-117">Scroll down to the **User password reset policy** section and toggle the **Users enabled for password reset** option to **YES**.</span></span> <span data-ttu-id="fe28a-118">Observera att den **alternativ e-postadress** alternativet är markerat; lämna den som den är.</span><span class="sxs-lookup"><span data-stu-id="fe28a-118">Notice that the **Alternate Email Address** option is checked; leave it as it is.</span></span>
   
    ![Återställning av lösenord för självbetjäning](./media/active-directory-b2c-reference-sspr/sspr.png)
6. <span data-ttu-id="fe28a-120">Klicka på **Spara** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="fe28a-120">Click **Save** at the bottom of the page.</span></span> <span data-ttu-id="fe28a-121">Du är klar!</span><span class="sxs-lookup"><span data-stu-id="fe28a-121">You're done!</span></span>

<span data-ttu-id="fe28a-122">Om du vill testa, funktionen ”Kör nu” på valfri inloggning princip som har lokala konton som en identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="fe28a-122">To test, use the "Run now" feature on any sign-in policy that has local accounts as an identity provider.</span></span> <span data-ttu-id="fe28a-123">Vid inloggning lokalt konto sidan (där du anger en e-postadress och lösenord, eller ett användarnamn och lösenord), klicka på **kan inte komma åt ditt konto?** att verifiera användarfunktioner.</span><span class="sxs-lookup"><span data-stu-id="fe28a-123">On the local account sign-in page (where you enter an email address and password, or a username and password), click **Can't access your account?** to verify the consumer experience.</span></span>

> [!NOTE]
> <span data-ttu-id="fe28a-124">Sidorna självbetjäning lösenordet för återställning kan anpassas med hjälp av den [funktionen för företagsanpassning](../active-directory/active-directory-add-company-branding.md).</span><span class="sxs-lookup"><span data-stu-id="fe28a-124">The self-service password reset pages can be customized by using the [company branding feature](../active-directory/active-directory-add-company-branding.md).</span></span>
> 
> 

