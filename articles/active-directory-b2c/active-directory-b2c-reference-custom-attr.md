---
title: 'Azure Active Directory B2C: Anpassade attribut som | Microsoft Docs'
description: "Hur toouse anpassade attribut i Azure Active Directory B2C toocollect information om dina användare"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 055ffb0a-197b-4716-8dad-1fd8a01e174f
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: fb1bff77ad54c246c6d2f69f39c03eafb76fe6bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-use-custom-attributes-toocollect-information-about-your-consumers"></a><span data-ttu-id="0772f-103">Azure Active Directory B2C: Använd anpassade attribut toocollect information om dina användare för</span><span class="sxs-lookup"><span data-stu-id="0772f-103">Azure Active Directory B2C: Use custom attributes toocollect information about your consumers</span></span>
<span data-ttu-id="0772f-104">Din Azure Active Directory (AD Azure) B2C-katalog som levereras med en inbyggd uppsättning information (attribut): Förnamn, efternamn, ort, postnummer och andra attribut.</span><span class="sxs-lookup"><span data-stu-id="0772f-104">Your Azure Active Directory (Azure AD) B2C directory comes with a built-in set of information (attributes): Given Name, Surname, City, Postal Code, and other attributes.</span></span> <span data-ttu-id="0772f-105">Men har alla konsumentinriktade program unika krav på vilka attribut toogather från konsumenterna.</span><span class="sxs-lookup"><span data-stu-id="0772f-105">However, every consumer-facing application has unique requirements on what attributes toogather from consumers.</span></span> <span data-ttu-id="0772f-106">Du kan utöka hello uppsättning attribut som lagras på varje konsumentkonto med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="0772f-106">With Azure AD B2C, you can extend hello set of attributes stored on each consumer account.</span></span> <span data-ttu-id="0772f-107">Du kan skapa anpassade attribut på hello [Azure-portalen](https://portal.azure.com/) och använda den i din anmälan principer som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="0772f-107">You can create custom attributes on hello [Azure portal](https://portal.azure.com/) and use it in your sign-up policies, as shown below.</span></span> <span data-ttu-id="0772f-108">Du kan också läsa och skriva dessa attribut med hjälp av hello [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="0772f-108">You can also read and write these attributes by using hello [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span></span>

> [!NOTE]
> <span data-ttu-id="0772f-109">Använd för anpassade attribut [Azure AD Graph API Directory-schemautökningar](https://msdn.microsoft.com/library/azure/dn720459.aspx).</span><span class="sxs-lookup"><span data-stu-id="0772f-109">Custom attributes use [Azure AD Graph API Directory Schema Extensions](https://msdn.microsoft.com/library/azure/dn720459.aspx).</span></span>
> 
> 

## <a name="create-a-custom-attribute"></a><span data-ttu-id="0772f-110">Skapa ett anpassat attribut</span><span class="sxs-lookup"><span data-stu-id="0772f-110">Create a custom attribute</span></span>
1. <span data-ttu-id="0772f-111">[Följ dessa steg toonavigate toohello B2C-funktionsbladet på hello Azure-portalen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="0772f-111">[Follow these steps toonavigate toohello B2C features blade on hello Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="0772f-112">Klicka på **användarattribut**.</span><span class="sxs-lookup"><span data-stu-id="0772f-112">Click **User attributes**.</span></span>
3. <span data-ttu-id="0772f-113">Klicka på **+ Lägg till** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="0772f-113">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="0772f-114">Ange en **namn** för hello anpassat attribut (till exempel ”ShoeSize”) och eventuellt en **beskrivning**.</span><span class="sxs-lookup"><span data-stu-id="0772f-114">Provide a **Name** for hello custom attribute (for example, "ShoeSize") and optionally, a **Description**.</span></span> <span data-ttu-id="0772f-115">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0772f-115">Click **Create**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0772f-116">Endast hello ”sträng” **datatyp** är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="0772f-116">Only hello "String" **Data Type** is currently available.</span></span>
   > 
   > 

<span data-ttu-id="0772f-117">hello-attributet är nu tillgänglig hello listan över **användarattribut**, och för användning i dina principer för registrering.</span><span class="sxs-lookup"><span data-stu-id="0772f-117">hello custom attribute is now available in hello list of **User attributes**, and for use in your sign-up policies.</span></span>

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a><span data-ttu-id="0772f-118">Använd ett anpassat attribut i principen för registrering</span><span class="sxs-lookup"><span data-stu-id="0772f-118">Use a custom attribute in your sign-up policy</span></span>
1. <span data-ttu-id="0772f-119">[Följ dessa steg toonavigate toohello B2C-funktionsbladet på hello Azure-portalen](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="0772f-119">[Follow these steps toonavigate toohello B2C features blade on hello Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="0772f-120">Klicka på **Registreringsprinciper**.</span><span class="sxs-lookup"><span data-stu-id="0772f-120">Click **Sign-up policies**.</span></span>
3. <span data-ttu-id="0772f-121">Klicka på principen för registrering (till exempel ”B2C_1_SiUp”)-tooopen den.</span><span class="sxs-lookup"><span data-stu-id="0772f-121">Click your sign-up policy (for example, "B2C_1_SiUp") tooopen it.</span></span> <span data-ttu-id="0772f-122">Klicka på **redigera** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="0772f-122">Click **Edit** at hello top of hello blade.</span></span>
4. <span data-ttu-id="0772f-123">Klicka på **registreringsattribut** och välj hello anpassat attribut (till exempel ”ShoeSize”).</span><span class="sxs-lookup"><span data-stu-id="0772f-123">Click **Sign-up attributes** and select hello custom attribute (for example, "ShoeSize").</span></span> <span data-ttu-id="0772f-124">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0772f-124">Click **OK**.</span></span>
5. <span data-ttu-id="0772f-125">Klicka på **Programanspråk** och välj hello anpassade attribut.</span><span class="sxs-lookup"><span data-stu-id="0772f-125">Click **Application claims** and select hello custom attribute.</span></span> <span data-ttu-id="0772f-126">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0772f-126">Click **OK**.</span></span>
6. <span data-ttu-id="0772f-127">Klicka på **spara** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="0772f-127">Click **Save** at hello top of hello blade.</span></span>

<span data-ttu-id="0772f-128">Du kan använda funktionen för hello ”kör nu” på hello princip tooverify hello användarfunktioner.</span><span class="sxs-lookup"><span data-stu-id="0772f-128">You can use hello "Run now" feature on hello policy tooverify hello consumer experience.</span></span> <span data-ttu-id="0772f-129">Du bör nu se ”ShoeSize” i hello lista över attribut som samlas in under registreringen konsumenten och finns i hello token skickade tillbaka tooyour program.</span><span class="sxs-lookup"><span data-stu-id="0772f-129">You should now see "ShoeSize" in hello list of attributes collected during consumer sign-up, and see it in hello token sent back tooyour application.</span></span>

## <a name="notes"></a><span data-ttu-id="0772f-130">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="0772f-130">Notes</span></span>
* <span data-ttu-id="0772f-131">Tillsammans med principer för registrering kan anpassade attribut också användas i principer för registrering eller inloggning och redigera principer profiler.</span><span class="sxs-lookup"><span data-stu-id="0772f-131">Along with sign-up policies, custom attributes can also be used in sign-up or sign-in policies and profile editing policies.</span></span>
* <span data-ttu-id="0772f-132">Det finns en känd begränsning för anpassade attribut.</span><span class="sxs-lookup"><span data-stu-id="0772f-132">There is a known limitation of custom attributes.</span></span> <span data-ttu-id="0772f-133">Det är bara skapade hello första gången den används i princip och inte när du lägger till den toohello lista över **användarattribut**.</span><span class="sxs-lookup"><span data-stu-id="0772f-133">It is only created hello first time it is used in any policy, and not when you add it toohello list of **User attributes**.</span></span>

