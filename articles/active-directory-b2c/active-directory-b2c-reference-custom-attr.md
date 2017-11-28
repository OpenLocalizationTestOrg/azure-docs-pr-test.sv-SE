---
title: 'Azure Active Directory B2C: Anpassade attribut som | Microsoft Docs'
description: "Hur du använder anpassade attribut i Azure Active Directory B2C för att samla in information om dina användare"
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
ms.openlocfilehash: 356aaeff3a78fc7b682d621e8e0de9312582b2fe
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-use-custom-attributes-to-collect-information-about-your-consumers"></a><span data-ttu-id="fcaca-103">Azure Active Directory B2C: Använd anpassade attribut för att samla in information om dina användare</span><span class="sxs-lookup"><span data-stu-id="fcaca-103">Azure Active Directory B2C: Use custom attributes to collect information about your consumers</span></span>
<span data-ttu-id="fcaca-104">Din Azure Active Directory (AD Azure) B2C-katalog som levereras med en inbyggd uppsättning information (attribut): Förnamn, efternamn, ort, postnummer och andra attribut.</span><span class="sxs-lookup"><span data-stu-id="fcaca-104">Your Azure Active Directory (Azure AD) B2C directory comes with a built-in set of information (attributes): Given Name, Surname, City, Postal Code, and other attributes.</span></span> <span data-ttu-id="fcaca-105">Men har alla konsumentinriktade program unika krav på vad attribut för att samla in från konsumenterna.</span><span class="sxs-lookup"><span data-stu-id="fcaca-105">However, every consumer-facing application has unique requirements on what attributes to gather from consumers.</span></span> <span data-ttu-id="fcaca-106">Du kan utöka uppsättningen attribut som lagras på varje konsumentkonto med Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="fcaca-106">With Azure AD B2C, you can extend the set of attributes stored on each consumer account.</span></span> <span data-ttu-id="fcaca-107">Du kan skapa anpassade attribut på den [Azure-portalen](https://portal.azure.com/) och använda den i din anmälan principer som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="fcaca-107">You can create custom attributes on the [Azure portal](https://portal.azure.com/) and use it in your sign-up policies, as shown below.</span></span> <span data-ttu-id="fcaca-108">Du kan också läsa och skriva dessa attribut med hjälp av den [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span><span class="sxs-lookup"><span data-stu-id="fcaca-108">You can also read and write these attributes by using the [Azure AD Graph API](active-directory-b2c-devquickstarts-graph-dotnet.md).</span></span>

> [!NOTE]
> <span data-ttu-id="fcaca-109">Använd för anpassade attribut [Azure AD Graph API Directory-schemautökningar](https://msdn.microsoft.com/library/azure/dn720459.aspx).</span><span class="sxs-lookup"><span data-stu-id="fcaca-109">Custom attributes use [Azure AD Graph API Directory Schema Extensions](https://msdn.microsoft.com/library/azure/dn720459.aspx).</span></span>
> 
> 

## <a name="create-a-custom-attribute"></a><span data-ttu-id="fcaca-110">Skapa ett anpassat attribut</span><span class="sxs-lookup"><span data-stu-id="fcaca-110">Create a custom attribute</span></span>
1. <span data-ttu-id="fcaca-111">[Följ dessa steg för att gå till B2C-funktionsbladet på Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="fcaca-111">[Follow these steps to navigate to the B2C features blade on the Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="fcaca-112">Klicka på **användarattribut**.</span><span class="sxs-lookup"><span data-stu-id="fcaca-112">Click **User attributes**.</span></span>
3. <span data-ttu-id="fcaca-113">Klicka på **+Lägg till** överst på bladet.</span><span class="sxs-lookup"><span data-stu-id="fcaca-113">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="fcaca-114">Ange en **namn** för det anpassade attributet (till exempel ”ShoeSize”) och eventuellt en **beskrivning**.</span><span class="sxs-lookup"><span data-stu-id="fcaca-114">Provide a **Name** for the custom attribute (for example, "ShoeSize") and optionally, a **Description**.</span></span> <span data-ttu-id="fcaca-115">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="fcaca-115">Click **Create**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="fcaca-116">Endast ”sträng” **datatyp** är tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="fcaca-116">Only the "String" **Data Type** is currently available.</span></span>
   > 
   > 

<span data-ttu-id="fcaca-117">Det anpassade attributet är nu tillgänglig i listan över **användarattribut**, och för användning i dina principer för registrering.</span><span class="sxs-lookup"><span data-stu-id="fcaca-117">The custom attribute is now available in the list of **User attributes**, and for use in your sign-up policies.</span></span>

## <a name="use-a-custom-attribute-in-your-sign-up-policy"></a><span data-ttu-id="fcaca-118">Använd ett anpassat attribut i principen för registrering</span><span class="sxs-lookup"><span data-stu-id="fcaca-118">Use a custom attribute in your sign-up policy</span></span>
1. <span data-ttu-id="fcaca-119">[Följ dessa steg för att gå till B2C-funktionsbladet på Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span><span class="sxs-lookup"><span data-stu-id="fcaca-119">[Follow these steps to navigate to the B2C features blade on the Azure portal](active-directory-b2c-app-registration.md#navigate-to-b2c-settings).</span></span>
2. <span data-ttu-id="fcaca-120">Klicka på **Registreringsprinciper**.</span><span class="sxs-lookup"><span data-stu-id="fcaca-120">Click **Sign-up policies**.</span></span>
3. <span data-ttu-id="fcaca-121">Klicka på principen för registrering (till exempel ”B2C_1_SiUp”) för att öppna den.</span><span class="sxs-lookup"><span data-stu-id="fcaca-121">Click your sign-up policy (for example, "B2C_1_SiUp") to open it.</span></span> <span data-ttu-id="fcaca-122">Klicka på **redigera** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="fcaca-122">Click **Edit** at the top of the blade.</span></span>
4. <span data-ttu-id="fcaca-123">Klicka på **registreringsattribut** och välj det anpassade attributet (till exempel ”ShoeSize”).</span><span class="sxs-lookup"><span data-stu-id="fcaca-123">Click **Sign-up attributes** and select the custom attribute (for example, "ShoeSize").</span></span> <span data-ttu-id="fcaca-124">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="fcaca-124">Click **OK**.</span></span>
5. <span data-ttu-id="fcaca-125">Klicka på **Programanspråk** och välj det anpassade attributet.</span><span class="sxs-lookup"><span data-stu-id="fcaca-125">Click **Application claims** and select the custom attribute.</span></span> <span data-ttu-id="fcaca-126">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="fcaca-126">Click **OK**.</span></span>
6. <span data-ttu-id="fcaca-127">Klicka på **spara** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="fcaca-127">Click **Save** at the top of the blade.</span></span>

<span data-ttu-id="fcaca-128">Du kan använda funktionen ”Kör nu” för principen för att verifiera användarfunktioner.</span><span class="sxs-lookup"><span data-stu-id="fcaca-128">You can use the "Run now" feature on the policy to verify the consumer experience.</span></span> <span data-ttu-id="fcaca-129">Du bör nu se ”ShoeSize” i listan över attribut som samlas in under registreringen konsumenten och finns i den token som skickas tillbaka till ditt program.</span><span class="sxs-lookup"><span data-stu-id="fcaca-129">You should now see "ShoeSize" in the list of attributes collected during consumer sign-up, and see it in the token sent back to your application.</span></span>

## <a name="notes"></a><span data-ttu-id="fcaca-130">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="fcaca-130">Notes</span></span>
* <span data-ttu-id="fcaca-131">Tillsammans med principer för registrering kan anpassade attribut också användas i principer för registrering eller inloggning och redigera principer profiler.</span><span class="sxs-lookup"><span data-stu-id="fcaca-131">Along with sign-up policies, custom attributes can also be used in sign-up or sign-in policies and profile editing policies.</span></span>
* <span data-ttu-id="fcaca-132">Det finns en känd begränsning för anpassade attribut.</span><span class="sxs-lookup"><span data-stu-id="fcaca-132">There is a known limitation of custom attributes.</span></span> <span data-ttu-id="fcaca-133">Det är endast skapas första gången den används i princip och inte när du lägger till den i listan över **användarattribut**.</span><span class="sxs-lookup"><span data-stu-id="fcaca-133">It is only created the first time it is used in any policy, and not when you add it to the list of **User attributes**.</span></span>

