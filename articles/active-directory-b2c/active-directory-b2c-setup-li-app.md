---
title: 'Azure Active Directory B2C: LinkedIn-konfiguration | Microsoft Docs'
description: "Ange tooconsumers för registrering och inloggning med LinkedIn-konton i dina program som skyddas av Azure Active Directory B2C"
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
ms.openlocfilehash: 6c5233ef48b24968fd6383f470b5d8a969a78ecf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-linkedin-accounts"></a><span data-ttu-id="0b9b1-103">Azure Active Directory B2C: Ge registrering och inloggning tooconsumers LinkedIn-konton</span><span class="sxs-lookup"><span data-stu-id="0b9b1-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with LinkedIn accounts</span></span>
## <a name="create-a-linkedin-application"></a><span data-ttu-id="0b9b1-104">Skapa ett LinkedIn-program</span><span class="sxs-lookup"><span data-stu-id="0b9b1-104">Create a LinkedIn application</span></span>
<span data-ttu-id="0b9b1-105">toouse LinkedIn som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate ett LinkedIn-program och lämna hello rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-105">toouse LinkedIn as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a LinkedIn application and supply it with hello right parameters.</span></span> <span data-ttu-id="0b9b1-106">Du behöver en LinkedIn konto toodo detta.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-106">You need a LinkedIn account toodo this.</span></span> <span data-ttu-id="0b9b1-107">Om du inte har något du kan hämta den på [https://www.linkedin.com/](https://www.linkedin.com/).</span><span class="sxs-lookup"><span data-stu-id="0b9b1-107">If you don’t have one, you can get it at [https://www.linkedin.com/](https://www.linkedin.com/).</span></span>

1. <span data-ttu-id="0b9b1-108">Gå toohello [LinkedIn utvecklare webbplats](https://www.developer.linkedin.com/) och logga in med autentiseringsuppgifterna för ditt LinkedIn.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-108">Go toohello [LinkedIn Developers website](https://www.developer.linkedin.com/) and sign in with your LinkedIn account credentials.</span></span>
2. <span data-ttu-id="0b9b1-109">Klicka på **Mina appar** i hello översta menyraden och klicka sedan på **skapa program**.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-109">Click **My Apps** in hello top menu bar and then click **Create Application**.</span></span>
   
    ![LinkedIn - ny app](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)
3. <span data-ttu-id="0b9b1-111">I hello **skapa ett nytt program** formuläret, Fyll i relevanta hello (**företagsnamn**, **namn**, **beskrivning**, **Programmets logotypen URL**, **användningen av**, **Webbadress**, **företags-e-** och **telefon**).</span><span class="sxs-lookup"><span data-stu-id="0b9b1-111">In hello **Create a New Application** form, fill in hello relevant information (**Company Name**, **Name**, **Description**, **Application Logo URL**, **Application Use**, **Website URL**, **Business Email** and **Business Phone**).</span></span>
4. <span data-ttu-id="0b9b1-112">Godkänner toohello **LinkedIn API användningsvillkoren** och på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-112">Agree toohello **LinkedIn API Terms of Use** and click **Submit**.</span></span>
   
    ![LinkedIn - registrera appen](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)
5. <span data-ttu-id="0b9b1-114">Kopiera hello värdena för **klient-ID** och **Klienthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-114">Copy hello values of **Client ID** and **Client Secret**.</span></span> <span data-ttu-id="0b9b1-115">(Du hittar dem under **autentiseringsnycklar**.) Du måste båda tooconfigure LinkedIn som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-115">(You can find them under **Authentication Keys**.) You will need both of them tooconfigure LinkedIn as an identity provider in your tenant.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="0b9b1-116">**Klienthemlighet** är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-116">**Client Secret** is an important security credential.</span></span>
   > 
   > 
6. <span data-ttu-id="0b9b1-117">Ange `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i hello **behörighet omdirigerings-URL: er** fält (under **OAuth 2.0**).</span><span class="sxs-lookup"><span data-stu-id="0b9b1-117">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Authorized Redirect URLs** field (under **OAuth 2.0**).</span></span> <span data-ttu-id="0b9b1-118">Ersätt **{klient}** med din klient namn (till exempel contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="0b9b1-118">Replace **{tenant}** with your tenant's name (for example, contoso.onmicrosoft.com).</span></span> <span data-ttu-id="0b9b1-119">Klicka på **Lägg till**, och klicka sedan på **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-119">Click **Add**, and then click **Update**.</span></span> <span data-ttu-id="0b9b1-120">Hej **{klient}** värdet är skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-120">hello **{tenant}** value is case-sensitive.</span></span>
   
    ![LinkedIn - installationsprogrammet för app](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="0b9b1-122">Konfigurera LinkedIn som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="0b9b1-122">Configure LinkedIn as an identity provider in your tenant</span></span>
1. <span data-ttu-id="0b9b1-123">Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-123">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="0b9b1-124">Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-124">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="0b9b1-125">Klicka på **+ Lägg till** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-125">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="0b9b1-126">Ange ett eget **namn** för hello identitet Providerkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-126">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="0b9b1-127">Ange till exempel ”LI”.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-127">For example, enter "LI".</span></span>
5. <span data-ttu-id="0b9b1-128">Klicka på **identitet providertyp**väljer **LinkedIn**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-128">Click **Identity provider type**, select **LinkedIn**, and click **OK**.</span></span>
6. <span data-ttu-id="0b9b1-129">Klicka på **ställa in den här identitetsleverantör** och ange hello klient-ID och klienten hemligheten för hello LinkedIn-program som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-129">Click **Set up this identity provider** and enter hello client ID and client secret of hello LinkedIn application that you created earlier.</span></span>
7. <span data-ttu-id="0b9b1-130">Klicka på **OK** och klicka sedan på **skapa** toosave LinkedIn-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0b9b1-130">Click **OK** and then click **Create** toosave your LinkedIn configuration.</span></span>

