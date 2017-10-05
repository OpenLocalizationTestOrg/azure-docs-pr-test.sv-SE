---
title: 'Azure Active Directory B2C: Facebook-konfiguration | Microsoft Docs'
description: "Ange registrering och inloggning för konsumenter med Facebook-konton i dina program som skyddas av Azure Active Directory B2C."
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
ms.openlocfilehash: 8c2154fcf33537358b549395d15b4ba937371cd0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a><span data-ttu-id="7c373-103">Azure Active Directory B2C: Ange registrering och inloggning för konsumenter med Facebook-konton</span><span class="sxs-lookup"><span data-stu-id="7c373-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Facebook accounts</span></span>
## <a name="create-a-facebook-application"></a><span data-ttu-id="7c373-104">Skapa ett Facebook-program</span><span class="sxs-lookup"><span data-stu-id="7c373-104">Create a Facebook application</span></span>
<span data-ttu-id="7c373-105">Om du vill använda Facebook som en identitetsleverantör i Azure Active Directory (AD Azure) B2C måste du skapa ett Facebook-program och ange rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="7c373-105">To use Facebook as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Facebook application and supply it with the right parameters.</span></span> <span data-ttu-id="7c373-106">Du behöver en Facebook-konto för att göra detta.</span><span class="sxs-lookup"><span data-stu-id="7c373-106">You need a Facebook account to do this.</span></span> <span data-ttu-id="7c373-107">Om du inte har något du kan hämta den på [https://www.facebook.com/](https://www.facebook.com/).</span><span class="sxs-lookup"><span data-stu-id="7c373-107">If you don’t have one, you can get it at [https://www.facebook.com/](https://www.facebook.com/).</span></span>

1. <span data-ttu-id="7c373-108">Gå till den [Facebook för utvecklare](https://developers.facebook.com/) webbplatsen och logga in med dina Facebook kontoautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="7c373-108">Go to the [Facebook for developers](https://developers.facebook.com/) website and sign in with your Facebook account credentials.</span></span>
2. <span data-ttu-id="7c373-109">Om du inte redan har gjort det så, behöver du registrera som en Facebook-utvecklare.</span><span class="sxs-lookup"><span data-stu-id="7c373-109">If you have not already done so, you need to register as a Facebook developer.</span></span> <span data-ttu-id="7c373-110">Gör detta genom att klicka på **registrera** (i det övre högra hörnet på sidan) och acceptera Facebooks principer och slutföra registreringen stegen.</span><span class="sxs-lookup"><span data-stu-id="7c373-110">To do this, click **Register** (on the upper-right corner of the page), accept Facebook's policies, and complete the registration steps.</span></span>
3. <span data-ttu-id="7c373-111">Klicka på **Mina appar** och klicka sedan på **lägga till en ny App**.</span><span class="sxs-lookup"><span data-stu-id="7c373-111">Click **My Apps** and then click **Add a New App**.</span></span> 
4. <span data-ttu-id="7c373-112">I formuläret kan tillhandahålla en **visningsnamn** och en giltig **kontakta e-post**.</span><span class="sxs-lookup"><span data-stu-id="7c373-112">In the form, provide a **Display Name** and a valid **Contact Email**.</span></span>
5. <span data-ttu-id="7c373-113">Klicka på **skapa App-ID**.</span><span class="sxs-lookup"><span data-stu-id="7c373-113">Click **Create App ID**.</span></span> <span data-ttu-id="7c373-114">Du kan behöva du acceptera Facebook-plattformen principer och slutföra en kontroll av online-säkerhet.</span><span class="sxs-lookup"><span data-stu-id="7c373-114">This may require you to accept Facebook platform policies and complete an online security check.</span></span>
6. <span data-ttu-id="7c373-115">Klicka i den vänstra kolumnen **inställningar** och välj sedan **grundläggande** om du inte redan har markerats.</span><span class="sxs-lookup"><span data-stu-id="7c373-115">In the left column, click **Settings** and then select **Basic** if not selected already.</span></span>
7. <span data-ttu-id="7c373-116">Välj en **kategori**.</span><span class="sxs-lookup"><span data-stu-id="7c373-116">Select a **Category**.</span></span> 
8. <span data-ttu-id="7c373-117">Klicka på **+ Lägg till plattformen** och välj **webbplats**.</span><span class="sxs-lookup"><span data-stu-id="7c373-117">Click **+ Add Platform** and select **Website**.</span></span>
   
    ![Facebook - inställningar](./media/active-directory-b2c-setup-fb-app/fb-settings.png)
   
    ![Facebook - inställningar - webbplats](./media/active-directory-b2c-setup-fb-app/fb-website.png)
9. <span data-ttu-id="7c373-120">Ange `https://login.microsoftonline.com/` i den **Webbadress** fältet och klickar sedan på **spara ändringar** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="7c373-120">Enter `https://login.microsoftonline.com/` in the **Site URL** field and then click **Save Changes** at the bottom of the page.</span></span>
   
    ![Facebook - Webbadress](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

10. <span data-ttu-id="7c373-122">Kopiera värdet för **App-ID**.</span><span class="sxs-lookup"><span data-stu-id="7c373-122">Copy the value of **App ID**.</span></span> <span data-ttu-id="7c373-123">Klicka på **visa** och kopiera värdet för **App hemlighet**.</span><span class="sxs-lookup"><span data-stu-id="7c373-123">Click **Show** and copy the value of **App Secret**.</span></span> <span data-ttu-id="7c373-124">Du måste båda för att konfigurera Facebook som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="7c373-124">You will need both of them to configure Facebook as an identity provider in your tenant.</span></span> <span data-ttu-id="7c373-125">**Appen hemlighet** är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="7c373-125">**App Secret** is an important security credential.</span></span>
   
    ![Facebook - App-ID & App hemlighet](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)
11. <span data-ttu-id="7c373-127">Klicka på **+ Lägg till produkten** i navigeringen till vänster och sedan den **konfigurera** knappen för **Facebook inloggningen**.</span><span class="sxs-lookup"><span data-stu-id="7c373-127">Click **+ Add Product** on the left navigation and then the **Set Up** button for **Facebook Login**.</span></span>
   
    ![Facebook - Facebook-inloggning](./media/active-directory-b2c-setup-fb-app/fb-login.png)
12. <span data-ttu-id="7c373-129">Klicka på **inställningar** på rätt nav under **Facebook-inloggning**</span><span class="sxs-lookup"><span data-stu-id="7c373-129">Click **Settings** on the right nav under **Facebook Login**</span></span>

    ![Facebook - Facebook inloggningsinställningar](./media/active-directory-b2c-setup-fb-app/fb-login-settings.png)
13. <span data-ttu-id="7c373-131">Ange `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i den **omdirigerings-URI: er för giltig OAuth** i den **OAuth klientinställningar** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7c373-131">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Valid OAuth redirect URIs** field in the **Client OAuth Settings** section.</span></span> <span data-ttu-id="7c373-132">Ersätt **{klient}** med din klient namn (till exempel contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="7c373-132">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="7c373-133">Klicka på **spara ändringar** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="7c373-133">Click **Save Changes** at the bottom of the page.</span></span>
    
    ![Facebook - OAuth omdirigerings-URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)
14. <span data-ttu-id="7c373-135">Om du vill göra Facebook-programmet kan användas av Azure AD B2C som du behöver göra den tillgänglig för allmänheten.</span><span class="sxs-lookup"><span data-stu-id="7c373-135">To make your Facebook application usable by Azure AD B2C, you need to make it publicly available.</span></span> <span data-ttu-id="7c373-136">Du kan göra detta genom att klicka på **App granska** i navigeringen till vänster och genom att aktivera växeln längst upp på sidan för att **Ja** och klicka på **Bekräfta**.</span><span class="sxs-lookup"><span data-stu-id="7c373-136">You can do this by clicking **App Review** on the left navigation and by turning the switch at the top of the page to **YES** and clicking **Confirm**.</span></span>
    
    ![Facebook - App offentliga](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="7c373-138">Konfigurera Facebook som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="7c373-138">Configure Facebook as an identity provider in your tenant</span></span>
1. <span data-ttu-id="7c373-139">Följ dessa steg för att [gå till B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7c373-139">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="7c373-140">Klicka på B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="7c373-140">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="7c373-141">Klicka på **+Lägg till** överst på bladet.</span><span class="sxs-lookup"><span data-stu-id="7c373-141">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="7c373-142">Ange ett eget **namn** för providerkonfigurationen identitet.</span><span class="sxs-lookup"><span data-stu-id="7c373-142">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="7c373-143">Till exempel ange ”Facebook”.</span><span class="sxs-lookup"><span data-stu-id="7c373-143">For example, enter "Facebook".</span></span>
5. <span data-ttu-id="7c373-144">Klicka på **identitet providertyp**väljer **Facebook**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7c373-144">Click **Identity provider type**, select **Facebook**, and click **OK**.</span></span>
6. <span data-ttu-id="7c373-145">Klicka på **ställa in den här identitetsleverantör** och ange den app hemligheten (för Facebook-programmet som du skapade tidigare) och app-ID i den **klient-ID** och **klienthemlighet** respektive fält.</span><span class="sxs-lookup"><span data-stu-id="7c373-145">Click **Set up this identity provider** and enter the app ID and app secret (of the Facebook application that you created earlier) in the **Client ID** and **Client secret** fields respectively.</span></span>
7. <span data-ttu-id="7c373-146">Klicka på **OK**, och klicka sedan på **skapa** spara Facebook-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="7c373-146">Click **OK**, and then click **Create** to save your Facebook configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="7c373-147">Lägga till en **identitetsleverantör** till din klient inte ändra din befintliga principer.</span><span class="sxs-lookup"><span data-stu-id="7c373-147">Adding an **Identity provider** to your tenant does not modify your existing policies.</span></span> <span data-ttu-id="7c373-148">Kom ihåg att uppdatera dina principer genom att inkludera den identitetsleverantör som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="7c373-148">Remember to update your policies by including the identity provider you just created.</span></span>
>