---
title: 'Azure Active Directory B2C: Amazon-konfiguration | Microsoft Docs'
description: "Ange tooconsumers för registrering och inloggning med Amazon-konton i dina program som skyddas av Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 77c099bb-a005-4d75-87f9-f61e3de48725
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 60d7c4b76d9d3e86ed535765329abed07f1e5996
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-amazon-accounts"></a><span data-ttu-id="c6ebf-103">Azure Active Directory B2C: Ange tooconsumers för registrering och inloggning med Amazon-konton</span><span class="sxs-lookup"><span data-stu-id="c6ebf-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Amazon accounts</span></span>
## <a name="create-an-amazon-application"></a><span data-ttu-id="c6ebf-104">Skapa ett Amazon-program</span><span class="sxs-lookup"><span data-stu-id="c6ebf-104">Create an Amazon application</span></span>
<span data-ttu-id="c6ebf-105">toouse Amazon som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate ett Amazon-program och lämna hello rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-105">toouse Amazon as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate an Amazon application and supply it with hello right parameters.</span></span> <span data-ttu-id="c6ebf-106">Du behöver en Amazon-konto toodo detta.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-106">You need an Amazon account toodo this.</span></span> <span data-ttu-id="c6ebf-107">Om du inte har något du kan hämta den på [http://www.amazon.com/](http://www.amazon.com/).</span><span class="sxs-lookup"><span data-stu-id="c6ebf-107">If you don’t have one, you can get it at [http://www.amazon.com/](http://www.amazon.com/).</span></span>

1. <span data-ttu-id="c6ebf-108">Gå toohello [Amazon Developer Center](https://login.amazon.com/) och logga in med autentiseringsuppgifterna för ditt Amazon-konto.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-108">Go toohello [Amazon Developer Center](https://login.amazon.com/) and sign in with your Amazon account credentials.</span></span>
2. <span data-ttu-id="c6ebf-109">Om du inte redan har gjort det, klickar du på **registrera dig**gör hello developer registrering och acceptera hello princip.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-109">If you have not already done so, click **Sign Up**, follow hello developer registration steps, and accept hello policy.</span></span>
3. <span data-ttu-id="c6ebf-110">Klicka på **registrera nya program**.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-110">Click **Register new application**.</span></span>
   
    ![Registrera ett nytt program på hello Amazon-webbplatsen](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)
4. <span data-ttu-id="c6ebf-112">Ange programinformationen (**namn**, **beskrivning**, och **meddelande Sekretesswebbadress**) och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-112">Provide application information (**Name**, **Description**, and **Privacy Notice URL**) and click **Save**.</span></span>
   
    ![Tillhandahåller information om programmet för att registrera en ny App på Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)
5. <span data-ttu-id="c6ebf-114">I hello **webbinställningar** avsnittet Kopiera hello värdena för **klient-ID** och **Klienthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-114">In hello **Web Settings** section, copy hello values of **Client ID** and **Client Secret**.</span></span> <span data-ttu-id="c6ebf-115">(Du behöver tooclick hello **visa hemlighet** knappen toosee detta.) Du måste båda tooconfigure Amazon som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-115">(You need tooclick hello **Show Secret** button toosee this.) You need both of them tooconfigure Amazon as an identity provider in your tenant.</span></span> <span data-ttu-id="c6ebf-116">Klicka på **redigera** längst hello hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-116">Click **Edit** at hello bottom of hello section.</span></span> <span data-ttu-id="c6ebf-117">**Klienthemlighet** är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-117">**Client Secret** is an important security credential.</span></span>
   
    ![Med klient-ID och Klienthemlighet för det nya programmet på Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)
6. <span data-ttu-id="c6ebf-119">Ange `https://login.microsoftonline.com` i hello **tillåtna JavaScript ursprung** fält och `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i hello **tillåtna returnerar URL: er** fältet.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-119">Enter `https://login.microsoftonline.com` in hello **Allowed JavaScript Origins** field and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Allowed Return URLs** field.</span></span> <span data-ttu-id="c6ebf-120">Ersätt **{klient}** med din klient namn (till exempel contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="c6ebf-120">Replace **{tenant}** with your tenant's name (for example, contoso.onmicrosoft.com).</span></span> <span data-ttu-id="c6ebf-121">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-121">Click **Save**.</span></span> <span data-ttu-id="c6ebf-122">Hej **{klient}** värdet är skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-122">hello **{tenant}** value is case-sensitive.</span></span>
   
    ![Med JavaScript ursprung och returnera URL: er för det nya programmet på Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="c6ebf-124">Konfigurera Amazon som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="c6ebf-124">Configure Amazon as an identity provider in your tenant</span></span>
1. <span data-ttu-id="c6ebf-125">Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-125">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="c6ebf-126">Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-126">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="c6ebf-127">Klicka på **+ Lägg till** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-127">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="c6ebf-128">Ange ett eget **namn** för hello identitet Providerkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-128">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="c6ebf-129">Till exempel ange ”Amzn”.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-129">For example, enter "Amzn".</span></span>
5. <span data-ttu-id="c6ebf-130">Klicka på **identitet providertyp**väljer **Amazon**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-130">Click **Identity provider type**, select **Amazon**, and click **OK**.</span></span>
6. <span data-ttu-id="c6ebf-131">Klicka på **ställa in den här identitetsleverantör** och ange hello klient-ID och klienten hemligheten för hello Amazon-program som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-131">Click **Set up this identity provider** and enter hello client ID and client secret of hello Amazon application that you created earlier.</span></span>
7. <span data-ttu-id="c6ebf-132">Klicka på **OK** och klicka sedan på **skapa** toosave Amazon-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="c6ebf-132">Click **OK** and then click **Create** toosave your Amazon configuration.</span></span>

