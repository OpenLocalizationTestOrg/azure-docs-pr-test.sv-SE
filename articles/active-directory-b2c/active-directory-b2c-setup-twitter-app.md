---
title: 'Azure Active Directory B2C: Twitter-konfiguration | Microsoft Docs'
description: "Ange registrering och inloggning för konsumenter med Twitter-konton i dina program som skyddas av Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 579a6841-9329-45b8-a351-da4315a6634e
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/06/2017
ms.author: parakhj
ms.openlocfilehash: 82a001dd53cdddcf3b360090f3250af593c96fbb
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-twitter-accounts"></a><span data-ttu-id="9e681-103">Azure Active Directory B2C: Ange registrering och inloggning för konsumenter med Twitter-konton</span><span class="sxs-lookup"><span data-stu-id="9e681-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Twitter accounts</span></span>

> [!NOTE]
> <span data-ttu-id="9e681-104">Den här funktionen är i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="9e681-104">This feature is in preview.</span></span>
> 

## <a name="create-a-twitter-application"></a><span data-ttu-id="9e681-105">Skapa ett Twitter-program</span><span class="sxs-lookup"><span data-stu-id="9e681-105">Create a Twitter application</span></span>
<span data-ttu-id="9e681-106">Om du vill använda Twitter som en identitetsleverantör i Azure Active Directory (AD Azure) B2C måste du skapa ett Twitter-program och ange rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="9e681-106">To use Twitter as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Twitter application and supply it with the right parameters.</span></span> <span data-ttu-id="9e681-107">Du behöver ett Twitter-konto för utvecklare att göra detta.</span><span class="sxs-lookup"><span data-stu-id="9e681-107">You need a Twitter developer account to do this.</span></span> <span data-ttu-id="9e681-108">Om du inte har något du kan hämta den på [https://dev.twitter.com/](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="9e681-108">If you don’t have one, you can get it at [https://dev.twitter.com/](https://dev.twitter.com/).</span></span>

1. <span data-ttu-id="9e681-109">Gå till den [Twitter-utvecklare webbplats](https://dev.twitter.com/) och logga in med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="9e681-109">Go to the [Twitter developer's website](https://dev.twitter.com/) and sign in with your credentials.</span></span>
2. <span data-ttu-id="9e681-110">Klicka på **Mina appar** under **verktyg och Support** och klicka sedan på **Skapa ny App**.</span><span class="sxs-lookup"><span data-stu-id="9e681-110">Click **My apps** under **Tools & Support** and then click **Create New App**.</span></span> 
3. <span data-ttu-id="9e681-111">I formuläret anger du ett värde för den **namn**, **beskrivning**, och **webbplats**.</span><span class="sxs-lookup"><span data-stu-id="9e681-111">In the form, provide a value for the **Name**, **Description**, and **Website**.</span></span>
4. <span data-ttu-id="9e681-112">För den **motringning URL**, ange `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="9e681-112">For the **Callback URL**, enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`.</span></span> <span data-ttu-id="9e681-113">Ersätt **{klient}** med din klient namn (till exempel contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="9e681-113">Make sure to replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>
5. <span data-ttu-id="9e681-114">Markera kryssrutan om du accepterar den **Developer avtal** och på **skapa programmet Twitter**.</span><span class="sxs-lookup"><span data-stu-id="9e681-114">Check the box to agree to the **Developer Agreement** and click **Create your Twitter application**.</span></span>
6. <span data-ttu-id="9e681-115">När appen har skapats klickar du på **nycklar och åtkomst-token**.</span><span class="sxs-lookup"><span data-stu-id="9e681-115">Once the app is created, click **Keys and Access Tokens**.</span></span>
7. <span data-ttu-id="9e681-116">Kopiera värdet för **konsumenten nyckeln** och **konsumenthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="9e681-116">Copy the value of **Consumer Key** and **Consumer Secret**.</span></span> <span data-ttu-id="9e681-117">Du måste båda för att konfigurera Twitter som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="9e681-117">You will need both of them to configure Twitter as an identity provider in your tenant.</span></span>

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="9e681-118">Konfigurera Twitter som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="9e681-118">Configure Twitter as an identity provider in your tenant</span></span>
1. <span data-ttu-id="9e681-119">Följ dessa steg för att [gå till B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="9e681-119">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="9e681-120">Klicka på B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="9e681-120">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="9e681-121">Klicka på **+Lägg till** överst på bladet.</span><span class="sxs-lookup"><span data-stu-id="9e681-121">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="9e681-122">Ange ett eget **namn** för providerkonfigurationen identitet.</span><span class="sxs-lookup"><span data-stu-id="9e681-122">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="9e681-123">Ange till exempel ”Twitter”.</span><span class="sxs-lookup"><span data-stu-id="9e681-123">For example, enter "Twitter".</span></span>
5. <span data-ttu-id="9e681-124">Klicka på **identitet providertyp**väljer **Twitter**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9e681-124">Click **Identity provider type**, select **Twitter**, and click **OK**.</span></span>
6. <span data-ttu-id="9e681-125">Klicka på **ställa in den här identitetsleverantör** och ange Twitter **konsumentnyckel** för den **klient-id** och på Twitter **konsumenthemlighet** för den **klienthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="9e681-125">Click **Set up this identity provider** and enter the Twitter **Consumer Key** for the **Client id** and the Twitter **Consumer Secret** for the **Client secret**.</span></span>
7. <span data-ttu-id="9e681-126">Klicka på **OK**, och klicka sedan på **skapa** spara Twitter-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="9e681-126">Click **OK**, and then click **Create** to save your Twitter configuration.</span></span>

