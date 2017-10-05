---
title: 'Azure Active Directory B2C: LinkedIn-konfiguration | Microsoft Docs'
description: "Ange registrering och inloggning för konsumenter med LinkedIn-konton i dina program som skyddas av Azure Active Directory B2C"
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: fa51a16b-9ce9-4e27-9eff-0869b4c4f0ef
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 1a6c4b19261aa34e668554ccad2b6340cddf9bf5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a><span data-ttu-id="f8212-103">Azure Active Directory B2C: Ange registrering och inloggning för konsumenter med LinkedIn-konton</span><span class="sxs-lookup"><span data-stu-id="f8212-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with LinkedIn accounts</span></span>
## <a name="create-a-linkedin-application"></a><span data-ttu-id="f8212-104">Skapa ett LinkedIn-program</span><span class="sxs-lookup"><span data-stu-id="f8212-104">Create a LinkedIn application</span></span>
<span data-ttu-id="f8212-105">Om du vill använda LinkedIn som en identitetsleverantör i Azure Active Directory (AD Azure) B2C måste du skapa ett LinkedIn-program och ange rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="f8212-105">To use LinkedIn as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a LinkedIn application and supply it with the right parameters.</span></span> <span data-ttu-id="f8212-106">Du behöver en LinkedIn-konto för att göra detta.</span><span class="sxs-lookup"><span data-stu-id="f8212-106">You need a LinkedIn account to do this.</span></span> <span data-ttu-id="f8212-107">Om du inte har något du kan hämta den på [https://www.linkedin.com/](https://www.linkedin.com/).</span><span class="sxs-lookup"><span data-stu-id="f8212-107">If you don’t have one, you can get it at [https://www.linkedin.com/](https://www.linkedin.com/).</span></span>

1. <span data-ttu-id="f8212-108">Gå till den [LinkedIn utvecklare webbplats](https://www.developer.linkedin.com/) och logga in med autentiseringsuppgifterna för ditt LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="f8212-108">Go to the [LinkedIn Developers website](https://www.developer.linkedin.com/) and sign in with your LinkedIn account credentials.</span></span>
2. <span data-ttu-id="f8212-109">Klicka på **Mina appar** i övre menyraden och klicka sedan på **skapa program**.</span><span class="sxs-lookup"><span data-stu-id="f8212-109">Click **My Apps** in the top menu bar and then click **Create Application**.</span></span>
   
    ![LinkedIn - ny app](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. <span data-ttu-id="f8212-111">I den **skapa ett nytt program** formuläret, Fyll i den relevanta informationen (**företagsnamn**, **namn**, **beskrivning**, **programmets logotypen URL**, **användningen av**, **Webbadress**, **företags-e-** och **telefon**).</span><span class="sxs-lookup"><span data-stu-id="f8212-111">In the **Create a New Application** form, fill in the relevant information (**Company Name**, **Name**, **Description**, **Application Logo URL**, **Application Use**, **Website URL**, **Business Email** and **Business Phone**).</span></span>
4. <span data-ttu-id="f8212-112">Godkänner den **LinkedIn API användningsvillkoren** och på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="f8212-112">Agree to the **LinkedIn API Terms of Use** and click **Submit**.</span></span>
   
    ![LinkedIn - registrera appen](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. <span data-ttu-id="f8212-114">Kopiera värdena i **klient-ID** och **Klienthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="f8212-114">Copy the values of **Client ID** and **Client Secret**.</span></span> <span data-ttu-id="f8212-115">(Du hittar dem under **autentiseringsnycklar**.) Du måste båda för att konfigurera LinkedIn som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="f8212-115">(You can find them under **Authentication Keys**.) You will need both of them to configure LinkedIn as an identity provider in your tenant.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="f8212-116">**Klienthemlighet** är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="f8212-116">**Client Secret** is an important security credential.</span></span>
   > 
   > 
6. <span data-ttu-id="f8212-117">Ange `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i den **behörighet omdirigerings-URL: er** fält (under **OAuth 2.0**).</span><span class="sxs-lookup"><span data-stu-id="f8212-117">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Authorized Redirect URLs** field (under **OAuth 2.0**).</span></span> <span data-ttu-id="f8212-118">Ersätt **{klient}** med din klient namn (till exempel contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="f8212-118">Replace **{tenant}** with your tenant's name (for example, contoso.onmicrosoft.com).</span></span> <span data-ttu-id="f8212-119">Klicka på **Lägg till**, och klicka sedan på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="f8212-119">Click **Add**, and then click **Update**.</span></span> <span data-ttu-id="f8212-120">Den **{klient}** värdet är skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="f8212-120">The **{tenant}** value is case-sensitive.</span></span>
   
    ![LinkedIn - installationsprogrammet för app](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="f8212-122">Konfigurera LinkedIn som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="f8212-122">Configure LinkedIn as an identity provider in your tenant</span></span>
1. <span data-ttu-id="f8212-123">Följ dessa steg för att [gå till B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f8212-123">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="f8212-124">Klicka på B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="f8212-124">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="f8212-125">Klicka på **+Lägg till** överst på bladet.</span><span class="sxs-lookup"><span data-stu-id="f8212-125">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="f8212-126">Ange ett eget **namn** för providerkonfigurationen identitet.</span><span class="sxs-lookup"><span data-stu-id="f8212-126">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="f8212-127">Ange till exempel ”LI”.</span><span class="sxs-lookup"><span data-stu-id="f8212-127">For example, enter "LI".</span></span>
5. <span data-ttu-id="f8212-128">Klicka på **identitet providertyp**väljer **LinkedIn**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f8212-128">Click **Identity provider type**, select **LinkedIn**, and click **OK**.</span></span>
6. <span data-ttu-id="f8212-129">Klicka på **ställa in den här identitetsleverantör** och ange klient-ID och klienthemlighet för LinkedIn-program som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="f8212-129">Click **Set up this identity provider** and enter the client ID and client secret of the LinkedIn application that you created earlier.</span></span>
7. <span data-ttu-id="f8212-130">Klicka på **OK** och klicka sedan på **skapa** spara LinkedIn-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="f8212-130">Click **OK** and then click **Create** to save your LinkedIn configuration.</span></span>

