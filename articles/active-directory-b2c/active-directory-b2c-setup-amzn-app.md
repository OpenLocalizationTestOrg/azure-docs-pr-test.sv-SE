---
title: 'Azure Active Directory B2C: Amazon-konfiguration | Microsoft Docs'
description: "Ange registrering och inloggning för konsumenter med Amazon-konton i dina program som skyddas av Azure Active Directory B2C."
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
ms.openlocfilehash: dcc97e1b7f6287bd7692c52bf068950065a26572
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a><span data-ttu-id="459db-103">Azure Active Directory B2C: Ange registrering och inloggning för konsumenter med Amazon-konton</span><span class="sxs-lookup"><span data-stu-id="459db-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Amazon accounts</span></span>
## <a name="create-an-amazon-application"></a><span data-ttu-id="459db-104">Skapa ett Amazon-program</span><span class="sxs-lookup"><span data-stu-id="459db-104">Create an Amazon application</span></span>
<span data-ttu-id="459db-105">Om du vill använda Amazon som en identitetsleverantör i Azure Active Directory (AD Azure) B2C måste du skapa ett Amazon-program och ange rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="459db-105">To use Amazon as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create an Amazon application and supply it with the right parameters.</span></span> <span data-ttu-id="459db-106">Du behöver ett Amazon-konto för att göra detta.</span><span class="sxs-lookup"><span data-stu-id="459db-106">You need an Amazon account to do this.</span></span> <span data-ttu-id="459db-107">Om du inte har något du kan hämta den på [http://www.amazon.com/](http://www.amazon.com/).</span><span class="sxs-lookup"><span data-stu-id="459db-107">If you don’t have one, you can get it at [http://www.amazon.com/](http://www.amazon.com/).</span></span>

1. <span data-ttu-id="459db-108">Gå till den [Amazon Developer Center](https://login.amazon.com/) och logga in med autentiseringsuppgifterna för ditt Amazon-konto.</span><span class="sxs-lookup"><span data-stu-id="459db-108">Go to the [Amazon Developer Center](https://login.amazon.com/) and sign in with your Amazon account credentials.</span></span>
2. <span data-ttu-id="459db-109">Om du inte redan har gjort det, klickar du på **registrera dig**gör developer registrering och acceptera principen.</span><span class="sxs-lookup"><span data-stu-id="459db-109">If you have not already done so, click **Sign Up**, follow the developer registration steps, and accept the policy.</span></span>
3. <span data-ttu-id="459db-110">Klicka på **registrera nya program**.</span><span class="sxs-lookup"><span data-stu-id="459db-110">Click **Register new application**.</span></span>
   
    ![Registrera en ny App på Amazon-webbplatsen](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)
4. <span data-ttu-id="459db-112">Ange programinformationen (**namn**, **beskrivning**, och **meddelande Sekretesswebbadress**) och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="459db-112">Provide application information (**Name**, **Description**, and **Privacy Notice URL**) and click **Save**.</span></span>
   
    ![Tillhandahåller information om programmet för att registrera en ny App på Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)
5. <span data-ttu-id="459db-114">I den **webbinställningar** avsnittet, kopiera värdena för **klient-ID** och **Klienthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="459db-114">In the **Web Settings** section, copy the values of **Client ID** and **Client Secret**.</span></span> <span data-ttu-id="459db-115">(Du måste klicka på den **visa hemlighet** för att visa detta.) Du måste båda för att konfigurera Amazon som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="459db-115">(You need to click the **Show Secret** button to see this.) You need both of them to configure Amazon as an identity provider in your tenant.</span></span> <span data-ttu-id="459db-116">Klicka på **redigera** längst ned i avsnittet.</span><span class="sxs-lookup"><span data-stu-id="459db-116">Click **Edit** at the bottom of the section.</span></span> <span data-ttu-id="459db-117">**Klienthemlighet** är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="459db-117">**Client Secret** is an important security credential.</span></span>
   
    ![Med klient-ID och Klienthemlighet för det nya programmet på Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)
6. <span data-ttu-id="459db-119">Ange `https://login.microsoftonline.com` i den **tillåtna JavaScript ursprung** fält och `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i den **tillåtna returnerar URL: er** fältet.</span><span class="sxs-lookup"><span data-stu-id="459db-119">Enter `https://login.microsoftonline.com` in the **Allowed JavaScript Origins** field and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Allowed Return URLs** field.</span></span> <span data-ttu-id="459db-120">Ersätt **{klient}** med din klient namn (till exempel contoso.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="459db-120">Replace **{tenant}** with your tenant's name (for example, contoso.onmicrosoft.com).</span></span> <span data-ttu-id="459db-121">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="459db-121">Click **Save**.</span></span> <span data-ttu-id="459db-122">Den **{klient}** värdet är skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="459db-122">The **{tenant}** value is case-sensitive.</span></span>
   
    ![Med JavaScript ursprung och returnera URL: er för det nya programmet på Amazon](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="459db-124">Konfigurera Amazon som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="459db-124">Configure Amazon as an identity provider in your tenant</span></span>
1. <span data-ttu-id="459db-125">Följ dessa steg för att [gå till B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="459db-125">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="459db-126">Klicka på B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="459db-126">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="459db-127">Klicka på **+Lägg till** överst på bladet.</span><span class="sxs-lookup"><span data-stu-id="459db-127">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="459db-128">Ange ett eget **namn** för providerkonfigurationen identitet.</span><span class="sxs-lookup"><span data-stu-id="459db-128">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="459db-129">Till exempel ange ”Amzn”.</span><span class="sxs-lookup"><span data-stu-id="459db-129">For example, enter "Amzn".</span></span>
5. <span data-ttu-id="459db-130">Klicka på **identitet providertyp**väljer **Amazon**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="459db-130">Click **Identity provider type**, select **Amazon**, and click **OK**.</span></span>
6. <span data-ttu-id="459db-131">Klicka på **ställa in den här identitetsleverantör** och ange klient-ID och klienthemlighet för Amazon-program som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="459db-131">Click **Set up this identity provider** and enter the client ID and client secret of the Amazon application that you created earlier.</span></span>
7. <span data-ttu-id="459db-132">Klicka på **OK** och klicka sedan på **skapa** spara Amazon-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="459db-132">Click **OK** and then click **Create** to save your Amazon configuration.</span></span>

