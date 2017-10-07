---
title: 'Azure Active Directory B2C: Twitter-konfiguration | Microsoft Docs'
description: "Ange tooconsumers för registrering och logga in med Twitter-konton i dina program som skyddas av Azure Active Directory B2C."
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
ms.openlocfilehash: 275c5c73fd5e8e5075e77fee942cbc1b5e1586cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-twitter-accounts"></a><span data-ttu-id="153f1-103">Azure Active Directory B2C: Ange tooconsumers för registrering och logga in med Twitter-konton</span><span class="sxs-lookup"><span data-stu-id="153f1-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Twitter accounts</span></span>

> [!NOTE]
> <span data-ttu-id="153f1-104">Den här funktionen är i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="153f1-104">This feature is in preview.</span></span>
> 

## <a name="create-a-twitter-application"></a><span data-ttu-id="153f1-105">Skapa ett Twitter-program</span><span class="sxs-lookup"><span data-stu-id="153f1-105">Create a Twitter application</span></span>
<span data-ttu-id="153f1-106">toouse Twitter som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate ett Twitter-program och lämna hello rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="153f1-106">toouse Twitter as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Twitter application and supply it with hello right parameters.</span></span> <span data-ttu-id="153f1-107">Du behöver en Twitter developer konto toodo detta.</span><span class="sxs-lookup"><span data-stu-id="153f1-107">You need a Twitter developer account toodo this.</span></span> <span data-ttu-id="153f1-108">Om du inte har något du kan hämta den på [https://dev.twitter.com/](https://dev.twitter.com/).</span><span class="sxs-lookup"><span data-stu-id="153f1-108">If you don’t have one, you can get it at [https://dev.twitter.com/](https://dev.twitter.com/).</span></span>

1. <span data-ttu-id="153f1-109">Gå toohello [Twitter-utvecklare webbplats](https://dev.twitter.com/) och logga in med dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="153f1-109">Go toohello [Twitter developer's website](https://dev.twitter.com/) and sign in with your credentials.</span></span>
2. <span data-ttu-id="153f1-110">Klicka på **Mina appar** under **verktyg och Support** och klicka sedan på **Skapa ny App**.</span><span class="sxs-lookup"><span data-stu-id="153f1-110">Click **My apps** under **Tools & Support** and then click **Create New App**.</span></span> 
3. <span data-ttu-id="153f1-111">Ange ett värde i hello form för hello **namn**, **beskrivning**, och **webbplats**.</span><span class="sxs-lookup"><span data-stu-id="153f1-111">In hello form, provide a value for hello **Name**, **Description**, and **Website**.</span></span>
4. <span data-ttu-id="153f1-112">För hello **motringning URL**, ange `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="153f1-112">For hello **Callback URL**, enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`.</span></span> <span data-ttu-id="153f1-113">Se till att tooreplace **{klient}** med din klient namn (till exempel contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="153f1-113">Make sure tooreplace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>
5. <span data-ttu-id="153f1-114">Kontrollera hello rutan tooagree toohello **Developer avtal** och på **skapa programmet Twitter**.</span><span class="sxs-lookup"><span data-stu-id="153f1-114">Check hello box tooagree toohello **Developer Agreement** and click **Create your Twitter application**.</span></span>
6. <span data-ttu-id="153f1-115">När hello app har skapats klickar du på **nycklar och åtkomst-token**.</span><span class="sxs-lookup"><span data-stu-id="153f1-115">Once hello app is created, click **Keys and Access Tokens**.</span></span>
7. <span data-ttu-id="153f1-116">Kopiera hello värdet för **konsumenten nyckeln** och **konsumenthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="153f1-116">Copy hello value of **Consumer Key** and **Consumer Secret**.</span></span> <span data-ttu-id="153f1-117">Du måste båda tooconfigure Twitter som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="153f1-117">You will need both of them tooconfigure Twitter as an identity provider in your tenant.</span></span>

## <a name="configure-twitter-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="153f1-118">Konfigurera Twitter som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="153f1-118">Configure Twitter as an identity provider in your tenant</span></span>
1. <span data-ttu-id="153f1-119">Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="153f1-119">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="153f1-120">Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="153f1-120">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="153f1-121">Klicka på **+ Lägg till** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="153f1-121">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="153f1-122">Ange ett eget **namn** för hello identitet Providerkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="153f1-122">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="153f1-123">Ange till exempel ”Twitter”.</span><span class="sxs-lookup"><span data-stu-id="153f1-123">For example, enter "Twitter".</span></span>
5. <span data-ttu-id="153f1-124">Klicka på **identitet providertyp**väljer **Twitter**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="153f1-124">Click **Identity provider type**, select **Twitter**, and click **OK**.</span></span>
6. <span data-ttu-id="153f1-125">Klicka på **ställa in den här identitetsleverantör** och ange hello Twitter **konsumenten nyckeln** för hello **klient-id** och hello Twitter **konsumenthemlighet**för hello **klienthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="153f1-125">Click **Set up this identity provider** and enter hello Twitter **Consumer Key** for hello **Client id** and hello Twitter **Consumer Secret** for hello **Client secret**.</span></span>
7. <span data-ttu-id="153f1-126">Klicka på **OK**, och klicka sedan på **skapa** toosave Twitter-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="153f1-126">Click **OK**, and then click **Create** toosave your Twitter configuration.</span></span>

