---
title: 'Azure Active Directory B2C: Facebook-konfiguration | Microsoft Docs'
description: "Ange tooconsumers för registrering och inloggning med Facebook-konton i dina program som skyddas av Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: sromeroz
manager: krassk
editor: sromeroz
ms.assetid: b875f235-a1d2-4abb-b9f0-b89beac38a32
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/7/2017
ms.author: sromeroz
ms.openlocfilehash: 4c3b79c7248bd1e789bf33fc467abb27d0170edd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-facebook-accounts"></a><span data-ttu-id="7faae-103">Azure Active Directory B2C: Ange tooconsumers för registrering och inloggning med Facebook-konton</span><span class="sxs-lookup"><span data-stu-id="7faae-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Facebook accounts</span></span>
## <a name="create-a-facebook-application"></a><span data-ttu-id="7faae-104">Skapa ett Facebook-program</span><span class="sxs-lookup"><span data-stu-id="7faae-104">Create a Facebook application</span></span>
<span data-ttu-id="7faae-105">toouse Facebook som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate Facebook-programmet och lämna hello rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="7faae-105">toouse Facebook as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Facebook application and supply it with hello right parameters.</span></span> <span data-ttu-id="7faae-106">Du behöver en Facebook konto toodo detta.</span><span class="sxs-lookup"><span data-stu-id="7faae-106">You need a Facebook account toodo this.</span></span> <span data-ttu-id="7faae-107">Om du inte har något du kan hämta den på [https://www.facebook.com/](https://www.facebook.com/).</span><span class="sxs-lookup"><span data-stu-id="7faae-107">If you don’t have one, you can get it at [https://www.facebook.com/](https://www.facebook.com/).</span></span>

1. <span data-ttu-id="7faae-108">Gå toohello [Facebook för utvecklare](https://developers.facebook.com/) webbplatsen och logga in med dina Facebook kontoautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="7faae-108">Go toohello [Facebook for developers](https://developers.facebook.com/) website and sign in with your Facebook account credentials.</span></span>
2. <span data-ttu-id="7faae-109">Om du inte redan har gjort det måste tooregister som utvecklare Facebook.</span><span class="sxs-lookup"><span data-stu-id="7faae-109">If you have not already done so, you need tooregister as a Facebook developer.</span></span> <span data-ttu-id="7faae-110">toodo, genom att välja **registrera** (på hello övre högra hörnet på sidan hello) godkänner Facebooks principer och utföra hello registrering steg.</span><span class="sxs-lookup"><span data-stu-id="7faae-110">toodo this, click **Register** (on hello upper-right corner of hello page), accept Facebook's policies, and complete hello registration steps.</span></span>
3. <span data-ttu-id="7faae-111">Klicka på **Mina appar** och klicka sedan på **lägga till en ny App**.</span><span class="sxs-lookup"><span data-stu-id="7faae-111">Click **My Apps** and then click **Add a New App**.</span></span> 
4. <span data-ttu-id="7faae-112">Hello formuläret och ange en **visningsnamn** och en giltig **kontakta e-post**.</span><span class="sxs-lookup"><span data-stu-id="7faae-112">In hello form, provide a **Display Name** and a valid **Contact Email**.</span></span>
5. <span data-ttu-id="7faae-113">Klicka på **skapa App-ID**.</span><span class="sxs-lookup"><span data-stu-id="7faae-113">Click **Create App ID**.</span></span> <span data-ttu-id="7faae-114">Detta kan kräva tooaccept Facebook-plattformen principer och slutföra en kontroll av online-säkerhet.</span><span class="sxs-lookup"><span data-stu-id="7faae-114">This may require you tooaccept Facebook platform policies and complete an online security check.</span></span>
6. <span data-ttu-id="7faae-115">Klicka på hello vänstra kolumnen **inställningar** och välj sedan **grundläggande** om du inte redan har markerats.</span><span class="sxs-lookup"><span data-stu-id="7faae-115">In hello left column, click **Settings** and then select **Basic** if not selected already.</span></span>
7. <span data-ttu-id="7faae-116">Välj en **kategori**.</span><span class="sxs-lookup"><span data-stu-id="7faae-116">Select a **Category**.</span></span> 
8. <span data-ttu-id="7faae-117">Klicka på **+ Lägg till plattformen** och välj **webbplats**.</span><span class="sxs-lookup"><span data-stu-id="7faae-117">Click **+ Add Platform** and select **Website**.</span></span>
   
    ![Facebook - inställningar](./media/active-directory-b2c-setup-fb-app/fb-settings.png)
   
    ![Facebook - inställningar - webbplats](./media/active-directory-b2c-setup-fb-app/fb-website.png)
9. <span data-ttu-id="7faae-120">Ange `https://login.microsoftonline.com/` i hello **Webbadress** fältet och klickar sedan på **spara ändringar** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="7faae-120">Enter `https://login.microsoftonline.com/` in hello **Site URL** field and then click **Save Changes** at hello bottom of hello page.</span></span>
   
    ![Facebook - Webbadress](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

10. <span data-ttu-id="7faae-122">Kopiera hello värdet för **App-ID**.</span><span class="sxs-lookup"><span data-stu-id="7faae-122">Copy hello value of **App ID**.</span></span> <span data-ttu-id="7faae-123">Klicka på **visa** och kopiera hello värdet för **App hemlighet**.</span><span class="sxs-lookup"><span data-stu-id="7faae-123">Click **Show** and copy hello value of **App Secret**.</span></span> <span data-ttu-id="7faae-124">Du måste båda tooconfigure Facebook som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="7faae-124">You will need both of them tooconfigure Facebook as an identity provider in your tenant.</span></span> <span data-ttu-id="7faae-125">**Appen hemlighet** är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="7faae-125">**App Secret** is an important security credential.</span></span>
   
    ![Facebook - App-ID & App hemlighet](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)
11. <span data-ttu-id="7faae-127">Klicka på **+ Lägg till produkten** hello vänstra navigeringsfönstret och sedan hello **konfigurera** knappen för **Facebook inloggningen**.</span><span class="sxs-lookup"><span data-stu-id="7faae-127">Click **+ Add Product** on hello left navigation and then hello **Set Up** button for **Facebook Login**.</span></span>
   
    ![Facebook - Facebook-inloggning](./media/active-directory-b2c-setup-fb-app/fb-login.png)
12. <span data-ttu-id="7faae-129">Klicka på **inställningar** på hello rätt nav under **Facebook-inloggning**</span><span class="sxs-lookup"><span data-stu-id="7faae-129">Click **Settings** on hello right nav under **Facebook Login**</span></span>

    ![Facebook - Facebook inloggningsinställningar](./media/active-directory-b2c-setup-fb-app/fb-login-settings.png)
13. <span data-ttu-id="7faae-131">Ange `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i hello **omdirigerings-URI: er för giltig OAuth** i hello **OAuth klientinställningar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7faae-131">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Valid OAuth redirect URIs** field in hello **Client OAuth Settings** section.</span></span> <span data-ttu-id="7faae-132">Ersätt **{klient}** med din klient namn (till exempel contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="7faae-132">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="7faae-133">Klicka på **spara ändringar** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="7faae-133">Click **Save Changes** at hello bottom of hello page.</span></span>
    
    ![Facebook - OAuth omdirigerings-URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)
14. <span data-ttu-id="7faae-135">toomake Facebook-programmet kan användas av Azure AD B2C måste toomake den allmänt tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="7faae-135">toomake your Facebook application usable by Azure AD B2C, you need toomake it publicly available.</span></span> <span data-ttu-id="7faae-136">Du kan göra detta genom att klicka på **App granska** på hello vänstra navigeringsrutan och genom att stänga hello växla hello överst på hello sidan för**Ja** och klicka på **Bekräfta**.</span><span class="sxs-lookup"><span data-stu-id="7faae-136">You can do this by clicking **App Review** on hello left navigation and by turning hello switch at hello top of hello page too**YES** and clicking **Confirm**.</span></span>
    
    ![Facebook - App offentliga](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="7faae-138">Konfigurera Facebook som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="7faae-138">Configure Facebook as an identity provider in your tenant</span></span>
1. <span data-ttu-id="7faae-139">Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7faae-139">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="7faae-140">Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="7faae-140">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="7faae-141">Klicka på **+ Lägg till** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="7faae-141">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="7faae-142">Ange ett eget **namn** för hello identitet Providerkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="7faae-142">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="7faae-143">Till exempel ange ”Facebook”.</span><span class="sxs-lookup"><span data-stu-id="7faae-143">For example, enter "Facebook".</span></span>
5. <span data-ttu-id="7faae-144">Klicka på **identitet providertyp**väljer **Facebook**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7faae-144">Click **Identity provider type**, select **Facebook**, and click **OK**.</span></span>
6. <span data-ttu-id="7faae-145">Klicka på **ställa in den här identitetsleverantör** och ange hello app-ID och app hemligheten (för hello Facebook-programmet som du skapade tidigare) i hello **klient-ID** och **klienthemlighet**respektive fält.</span><span class="sxs-lookup"><span data-stu-id="7faae-145">Click **Set up this identity provider** and enter hello app ID and app secret (of hello Facebook application that you created earlier) in hello **Client ID** and **Client secret** fields respectively.</span></span>
7. <span data-ttu-id="7faae-146">Klicka på **OK**, och klicka sedan på **skapa** toosave Facebook-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="7faae-146">Click **OK**, and then click **Create** toosave your Facebook configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="7faae-147">Lägga till en **identitetsleverantör** tooyour klient inte ändrar de befintliga principerna.</span><span class="sxs-lookup"><span data-stu-id="7faae-147">Adding an **Identity provider** tooyour tenant does not modify your existing policies.</span></span> <span data-ttu-id="7faae-148">Kom ihåg tooupdate dina principer genom att inkludera hello identitetsleverantör som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="7faae-148">Remember tooupdate your policies by including hello identity provider you just created.</span></span>
>