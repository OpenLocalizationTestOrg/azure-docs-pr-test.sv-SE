---
title: "Anpassa ett gränssnitt med hjälp av anpassade principer - Azure AD B2C | Microsoft Docs"
description: "Lär dig mer om hur du anpassar ett användargränssnitt (UI) samtidigt som du använder anpassade principer i Azure AD B2C."
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/04/2017
ms.author: saeedakhter-msft
ms.openlocfilehash: 6f00995e54c9f9ef27cc51e38f3de07cd5817cc1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-configure-ui-customization-in-a-custom-policy"></a><span data-ttu-id="fe019-103">Azure Active Directory B2C: Konfigurera anpassningar i en anpassad princip</span><span class="sxs-lookup"><span data-stu-id="fe019-103">Azure Active Directory B2C: Configure UI customization in a custom policy</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="fe019-104">När du har slutfört den här artikeln har du en anpassad princip för registrering och inloggning med märke och utseende.</span><span class="sxs-lookup"><span data-stu-id="fe019-104">After you complete this article, you will have a sign-up and sign-in custom policy with your brand and appearance.</span></span> <span data-ttu-id="fe019-105">Med Azure Active Directory B2C (Azure AD B2C), du får nästan full kontroll över hello HTML- och CSS-innehåll som har presenterat toousers.</span><span class="sxs-lookup"><span data-stu-id="fe019-105">With Azure Active Directory B2C (Azure AD B2C), you get nearly full control of hello HTML and CSS content that's presented toousers.</span></span> <span data-ttu-id="fe019-106">När du använder en anpassad princip kan konfigurera du anpassning av Användargränssnittet i XML i stället för att använda kontroller i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fe019-106">When you use a custom policy, you configure UI customization in XML instead of using controls in hello Azure portal.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="fe019-107">Krav</span><span class="sxs-lookup"><span data-stu-id="fe019-107">Prerequisites</span></span>

<span data-ttu-id="fe019-108">Innan du kan slutföra [komma igång med anpassade principer](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="fe019-108">Before you begin, complete [Getting started with custom policies](active-directory-b2c-get-started-custom.md).</span></span> <span data-ttu-id="fe019-109">Du bör ha en fungerande anpassad princip för registrering och inloggning med lokala konton.</span><span class="sxs-lookup"><span data-stu-id="fe019-109">You should have a working custom policy for sign-up and sign-in with local accounts.</span></span>

## <a name="page-ui-customization"></a><span data-ttu-id="fe019-110">Anpassning av Page UI</span><span class="sxs-lookup"><span data-stu-id="fe019-110">Page UI customization</span></span>

<span data-ttu-id="fe019-111">Med funktionen hello sidan UI anpassning kan anpassa du hello utseende och känslan av en anpassad princip.</span><span class="sxs-lookup"><span data-stu-id="fe019-111">By using hello page UI customization feature, you can customize hello look and feel of any custom policy.</span></span> <span data-ttu-id="fe019-112">Du kan också upprätthålla märke och visual konsekvensen mellan dina program och Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="fe019-112">You can also maintain brand and visual consistency between your application and Azure AD B2C.</span></span>

<span data-ttu-id="fe019-113">Här är hur det fungerar: Azure AD B2C Kör koden i kundens webbläsare och använder en modern metod som kallas [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="fe019-113">Here's how it works: Azure AD B2C runs code in your customer's browser and uses a modern approach called [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/).</span></span> <span data-ttu-id="fe019-114">Först måste ange du en URL i hello anpassad princip med anpassade HTML-innehåll.</span><span class="sxs-lookup"><span data-stu-id="fe019-114">First, you specify a URL in hello custom policy with customized HTML content.</span></span> <span data-ttu-id="fe019-115">Azure AD B2C sammanfogas UI-element med hello HTML-innehåll som har lästs in från URL: en och sedan visar hello sidan toohello kunden.</span><span class="sxs-lookup"><span data-stu-id="fe019-115">Azure AD B2C merges UI elements with hello HTML content that's loaded from your URL and then displays hello page toohello customer.</span></span>

## <a name="create-your-html5-content"></a><span data-ttu-id="fe019-116">Skapa din HTML5 innehåll</span><span class="sxs-lookup"><span data-stu-id="fe019-116">Create your HTML5 content</span></span>

<span data-ttu-id="fe019-117">Skapa HTML innehåll med varumärken Produktnamn i hello rubrik.</span><span class="sxs-lookup"><span data-stu-id="fe019-117">Create HTML content with your product's brand name in hello title.</span></span>

1. <span data-ttu-id="fe019-118">Kopiera hello följande HTML-fragment.</span><span class="sxs-lookup"><span data-stu-id="fe019-118">Copy hello following HTML snippet.</span></span> <span data-ttu-id="fe019-119">Det är korrekt strukturerad HTML5 med ett tomt element kallas  *\<div id = ”api”\>\</div\>*  i hello  *\<brödtext\>*  taggar.</span><span class="sxs-lookup"><span data-stu-id="fe019-119">It is well-formed HTML5 with an empty element called *\<div id="api"\>\</div\>* located within hello *\<body\>* tags.</span></span> <span data-ttu-id="fe019-120">Det här elementet anger där Azure AD B2C innehåll är toobe infogas.</span><span class="sxs-lookup"><span data-stu-id="fe019-120">This element indicates where Azure AD B2C content is toobe inserted.</span></span>

   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>My Product Brand Name</title>
   </head>
   <body>
       <div id="api"></div>
   </body>
   </html>
   ```

   >[!NOTE]
   ><span data-ttu-id="fe019-121">Av säkerhetsskäl är hello användning av JavaScript blockerad för anpassning.</span><span class="sxs-lookup"><span data-stu-id="fe019-121">For security reasons, hello use of JavaScript is currently blocked for customization.</span></span>

2. <span data-ttu-id="fe019-122">Klistra in hello kopieras fragment i en textredigerare och sedan spara hello som *anpassa ui.html*.</span><span class="sxs-lookup"><span data-stu-id="fe019-122">Paste hello copied snippet in a text editor, and then save hello file as *customize-ui.html*.</span></span>

## <a name="create-an-azure-blob-storage-account"></a><span data-ttu-id="fe019-123">Skapa ett Azure Blob storage-konto</span><span class="sxs-lookup"><span data-stu-id="fe019-123">Create an Azure Blob storage account</span></span>

>[!NOTE]
> <span data-ttu-id="fe019-124">I den här artikeln använder vi Azure Blob storage toohost innehållet.</span><span class="sxs-lookup"><span data-stu-id="fe019-124">In this article, we use Azure Blob storage toohost our content.</span></span> <span data-ttu-id="fe019-125">Du kan välja toohost ditt innehåll på en webbserver, men du måste [aktivera CORS på din webbserver](https://enable-cors.org/server.html).</span><span class="sxs-lookup"><span data-stu-id="fe019-125">You can choose toohost your content on a web server, but you must [enable CORS on your web server](https://enable-cors.org/server.html).</span></span>

<span data-ttu-id="fe019-126">toohost den här HTML-innehåll i Blob storage hello följande:</span><span class="sxs-lookup"><span data-stu-id="fe019-126">toohost this HTML content in Blob storage, do hello following:</span></span>

1. <span data-ttu-id="fe019-127">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fe019-127">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="fe019-128">På hello **hubb** väljer du **ny** > **lagring** > **lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="fe019-128">On hello **Hub** menu, select **New** > **Storage** > **Storage account**.</span></span>
3. <span data-ttu-id="fe019-129">Ange ett unikt **namn** för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="fe019-129">Enter a unique **Name** for your storage account.</span></span>
4. <span data-ttu-id="fe019-130">**Distributionsmodell** kan vara **Resource Manager**.</span><span class="sxs-lookup"><span data-stu-id="fe019-130">**Deployment model** can remain **Resource Manager**.</span></span>
5. <span data-ttu-id="fe019-131">Ändra **konto typ** för**Blob storage**.</span><span class="sxs-lookup"><span data-stu-id="fe019-131">Change **Account Kind** too**Blob storage**.</span></span>
6. <span data-ttu-id="fe019-132">**Prestanda** kan vara **Standard**.</span><span class="sxs-lookup"><span data-stu-id="fe019-132">**Performance** can remain **Standard**.</span></span>
7. <span data-ttu-id="fe019-133">**Replikering** kan vara **RA-GRS**.</span><span class="sxs-lookup"><span data-stu-id="fe019-133">**Replication** can remain **RA-GRS**.</span></span>
8. <span data-ttu-id="fe019-134">**Åtkomstnivå** kan vara **frekvent**.</span><span class="sxs-lookup"><span data-stu-id="fe019-134">**Access tier** can remain **Hot**.</span></span>
9. <span data-ttu-id="fe019-135">**Lagringstjänstens kryptering** kan vara **inaktiverade**.</span><span class="sxs-lookup"><span data-stu-id="fe019-135">**Storage service encryption** can remain **Disabled**.</span></span>
10. <span data-ttu-id="fe019-136">Välj en **prenumeration** för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="fe019-136">Select a **Subscription** for your storage account.</span></span>
11. <span data-ttu-id="fe019-137">Skapa en **resursgruppen** eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="fe019-137">Create a **Resource group** or select an existing one.</span></span>
12. <span data-ttu-id="fe019-138">Välj hello **geografisk plats** för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="fe019-138">Select hello **Geographic location** for your storage account.</span></span>
13. <span data-ttu-id="fe019-139">Klicka på **skapa** toocreate hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="fe019-139">Click **Create** toocreate hello storage account.</span></span>  
    <span data-ttu-id="fe019-140">När hello distributionen är klar hello **lagringskonto** blad öppnas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="fe019-140">After hello deployment is completed, hello **Storage account** blade opens automatically.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="fe019-141">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="fe019-141">Create a container</span></span>

<span data-ttu-id="fe019-142">toocreate en offentlig behållare i Blob storage hello följande:</span><span class="sxs-lookup"><span data-stu-id="fe019-142">toocreate a public container in Blob storage, do hello following:</span></span>

1. <span data-ttu-id="fe019-143">Klicka på hello **översikt** fliken.</span><span class="sxs-lookup"><span data-stu-id="fe019-143">Click hello **Overview** tab.</span></span>
2. <span data-ttu-id="fe019-144">Klicka på **behållare**.</span><span class="sxs-lookup"><span data-stu-id="fe019-144">Click **Container**.</span></span>
3. <span data-ttu-id="fe019-145">För **namn**, typen **$root**.</span><span class="sxs-lookup"><span data-stu-id="fe019-145">For **Name**, type **$root**.</span></span>
4. <span data-ttu-id="fe019-146">Ange **komma åt typen** för**Blob**.</span><span class="sxs-lookup"><span data-stu-id="fe019-146">Set **Access type** too**Blob**.</span></span>
5. <span data-ttu-id="fe019-147">Klicka på **$root** tooopen hello ny behållare.</span><span class="sxs-lookup"><span data-stu-id="fe019-147">Click **$root** tooopen hello new container.</span></span>
6. <span data-ttu-id="fe019-148">Klicka på **Överför**.</span><span class="sxs-lookup"><span data-stu-id="fe019-148">Click **Upload**.</span></span>
7. <span data-ttu-id="fe019-149">Klicka på hello mappikon visas bredvid för**Välj en fil**.</span><span class="sxs-lookup"><span data-stu-id="fe019-149">Click hello folder icon next too**Select a file**.</span></span>
8. <span data-ttu-id="fe019-150">Gå för**anpassa ui.html**, som du skapade tidigare i hello [Page UI anpassning](#the-page-ui-customization-feature) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fe019-150">Go too**customize-ui.html**, which you created earlier in hello [Page UI customization](#the-page-ui-customization-feature) section.</span></span>
9. <span data-ttu-id="fe019-151">Klicka på **Överför**.</span><span class="sxs-lookup"><span data-stu-id="fe019-151">Click **Upload**.</span></span>
10. <span data-ttu-id="fe019-152">Välj hello anpassa ui.html blob som du har överfört.</span><span class="sxs-lookup"><span data-stu-id="fe019-152">Select hello customize-ui.html blob that you uploaded.</span></span>
11. <span data-ttu-id="fe019-153">Nästa för**URL**, klickar du på **kopiera**.</span><span class="sxs-lookup"><span data-stu-id="fe019-153">Next too**URL**, click **Copy**.</span></span>
12. <span data-ttu-id="fe019-154">Klistra in hello kopieras URL i en webbläsare och gå toohello plats.</span><span class="sxs-lookup"><span data-stu-id="fe019-154">In a browser, paste hello copied URL, and go toohello site.</span></span> <span data-ttu-id="fe019-155">Om hello platsen är tillgänglig, kontrollera hello behållartypen åtkomst anges för**blob**.</span><span class="sxs-lookup"><span data-stu-id="fe019-155">If hello site is inaccessible, make sure hello container access type is set too**blob**.</span></span>

## <a name="configure-cors"></a><span data-ttu-id="fe019-156">Konfigurera CORS</span><span class="sxs-lookup"><span data-stu-id="fe019-156">Configure CORS</span></span>

<span data-ttu-id="fe019-157">Konfigurera Blob storage för Cross-Origin Resource Sharing hello följande:</span><span class="sxs-lookup"><span data-stu-id="fe019-157">Configure Blob storage for Cross-Origin Resource Sharing by doing hello following:</span></span>

>[!NOTE]
><span data-ttu-id="fe019-158">Vill tootry ut hello gränssnittsfunktionen anpassning med hjälp av vår HTML- och CSS innehåll?</span><span class="sxs-lookup"><span data-stu-id="fe019-158">Want tootry out hello UI customization feature by using our sample HTML and CSS content?</span></span> <span data-ttu-id="fe019-159">Vi har samlat [ett enkelt kommandoradsverktyg](active-directory-b2c-reference-ui-customization-helper-tool.md) som överför och konfigurerar våra exemplen på Blob storage-konto.</span><span class="sxs-lookup"><span data-stu-id="fe019-159">We've provided [a simple helper tool](active-directory-b2c-reference-ui-customization-helper-tool.md) that uploads and configures our sample content on your Blob storage account.</span></span> <span data-ttu-id="fe019-160">Om du använder hello-verktyget kan du gå vidare för[ändra en anpassad princip för registrering eller inloggning](#modify-your-sign-up-or-sign-in-custom-policy).</span><span class="sxs-lookup"><span data-stu-id="fe019-160">If you use hello tool, skip ahead too[Modify your sign-up or sign-in custom policy](#modify-your-sign-up-or-sign-in-custom-policy).</span></span>

1. <span data-ttu-id="fe019-161">På hello **lagring** bladet under **inställningar**öppnar **CORS**.</span><span class="sxs-lookup"><span data-stu-id="fe019-161">On hello **Storage** blade, under **Settings**, open **CORS**.</span></span>
2. <span data-ttu-id="fe019-162">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="fe019-162">Click **Add**.</span></span>
3. <span data-ttu-id="fe019-163">För **tillåtna ursprung**, skriver du en asterisk (\*).</span><span class="sxs-lookup"><span data-stu-id="fe019-163">For **Allowed origins**, type an asterisk (\*).</span></span>
4. <span data-ttu-id="fe019-164">I hello **tillåtna verb** listrutan, Välj **hämta** och **alternativ**.</span><span class="sxs-lookup"><span data-stu-id="fe019-164">In hello **Allowed verbs** drop-down list, select both **GET** and **OPTIONS**.</span></span>
5. <span data-ttu-id="fe019-165">För **tillåtna huvuden**, skriver du en asterisk (\*).</span><span class="sxs-lookup"><span data-stu-id="fe019-165">For **Allowed headers**, type an asterisk (\*).</span></span>
6. <span data-ttu-id="fe019-166">För **exponeras huvuden**, skriver du en asterisk (\*).</span><span class="sxs-lookup"><span data-stu-id="fe019-166">For **Exposed headers**, type an asterisk (\*).</span></span>
7. <span data-ttu-id="fe019-167">För **högsta ålder (sekunder)**, typen **200**.</span><span class="sxs-lookup"><span data-stu-id="fe019-167">For **Maximum age (seconds)**, type **200**.</span></span>
8. <span data-ttu-id="fe019-168">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="fe019-168">Click **Add**.</span></span>

## <a name="test-cors"></a><span data-ttu-id="fe019-169">Testa CORS</span><span class="sxs-lookup"><span data-stu-id="fe019-169">Test CORS</span></span>

<span data-ttu-id="fe019-170">Verifiera att du är redo hello följande:</span><span class="sxs-lookup"><span data-stu-id="fe019-170">Validate that you're ready by doing hello following:</span></span>

1. <span data-ttu-id="fe019-171">Gå toohello [test cors.org](http://test-cors.org/) webbplats och klistra in hello URL i hello **fjärr-URL** rutan.</span><span class="sxs-lookup"><span data-stu-id="fe019-171">Go toohello [test-cors.org](http://test-cors.org/) website, and then paste hello URL in hello **Remote URL** box.</span></span>
2. <span data-ttu-id="fe019-172">Klicka på **skicka begäran**.</span><span class="sxs-lookup"><span data-stu-id="fe019-172">Click **Send Request**.</span></span>  
    <span data-ttu-id="fe019-173">Om du får ett felmeddelande, kontrollerar du att din [CORS-inställningarna](#configure-cors) är korrekta.</span><span class="sxs-lookup"><span data-stu-id="fe019-173">If you receive an error, make sure that your [CORS settings](#configure-cors) are correct.</span></span> <span data-ttu-id="fe019-174">Du kanske också måste tooclear webbläsarens cache eller öppna ett privat sessionen genom att trycka på Ctrl + Skift + P.</span><span class="sxs-lookup"><span data-stu-id="fe019-174">You might also need tooclear your browser cache or open an in-private browsing session by pressing Ctrl+Shift+P.</span></span>

## <a name="modify-your-sign-up-or-sign-in-custom-policy"></a><span data-ttu-id="fe019-175">Ändra en anpassad princip för registrering eller inloggning</span><span class="sxs-lookup"><span data-stu-id="fe019-175">Modify your sign-up or sign-in custom policy</span></span>

<span data-ttu-id="fe019-176">Under hello översta  *\<TrustFrameworkPolicy\>*  tagg, bör du hitta  *\<BuildingBlocks\>*  tagg.</span><span class="sxs-lookup"><span data-stu-id="fe019-176">Under hello top-level *\<TrustFrameworkPolicy\>* tag, you should find *\<BuildingBlocks\>* tag.</span></span> <span data-ttu-id="fe019-177">Inom hello  *\<BuildingBlocks\>*  taggar, lägga till en  *\<ContentDefinitions\>*  taggen genom att kopiera hello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="fe019-177">Within hello *\<BuildingBlocks\>* tags, add a *\<ContentDefinitions\>* tag by copying hello following example.</span></span> <span data-ttu-id="fe019-178">Ersätt *your_storage_account* med hello namnet på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="fe019-178">Replace *your_storage_account* with hello name of your storage account.</span></span>

  ```xml
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.idpselections">
        <LoadUri>https://{your_storage_account}.blob.core.windows.net/customize-ui.html</LoadUri>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>
  ```

## <a name="upload-your-updated-custom-policy"></a><span data-ttu-id="fe019-179">Ladda upp din uppdaterade anpassad princip</span><span class="sxs-lookup"><span data-stu-id="fe019-179">Upload your updated custom policy</span></span>

1. <span data-ttu-id="fe019-180">I hello [Azure-portalen](https://portal.azure.com), [växla till hello kontext för din Azure AD B2C-klient](active-directory-b2c-navigate-to-b2c-context.md), och sedan öppna hello **Azure AD B2C** bladet.</span><span class="sxs-lookup"><span data-stu-id="fe019-180">In hello [Azure portal](https://portal.azure.com), [switch into hello context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and then open hello **Azure AD B2C** blade.</span></span>
2. <span data-ttu-id="fe019-181">Klicka på **alla principer**.</span><span class="sxs-lookup"><span data-stu-id="fe019-181">Click **All Policies**.</span></span>
3. <span data-ttu-id="fe019-182">Klicka på **överföra princip**.</span><span class="sxs-lookup"><span data-stu-id="fe019-182">Click **Upload Policy**.</span></span>
4. <span data-ttu-id="fe019-183">Överför `SignUpOrSignin.xml` med hello  *\<ContentDefinitions\>*  tagg som du har lagt till tidigare.</span><span class="sxs-lookup"><span data-stu-id="fe019-183">Upload `SignUpOrSignin.xml` with hello *\<ContentDefinitions\>* tag that you added previously.</span></span>

## <a name="test-hello-custom-policy-by-using-run-now"></a><span data-ttu-id="fe019-184">Testa hello anpassad princip med **kör nu**</span><span class="sxs-lookup"><span data-stu-id="fe019-184">Test hello custom policy by using **Run now**</span></span>

1. <span data-ttu-id="fe019-185">På hello **Azure AD B2C** bladet går för**alla principer**.</span><span class="sxs-lookup"><span data-stu-id="fe019-185">On hello **Azure AD B2C** blade, go too**All polices**.</span></span>
2. <span data-ttu-id="fe019-186">Välj hello anpassad princip som du överfört och klicka på hello **kör nu** knappen.</span><span class="sxs-lookup"><span data-stu-id="fe019-186">Select hello custom policy that you uploaded, and click hello **Run now** button.</span></span>
3. <span data-ttu-id="fe019-187">Du ska kunna toosign upp med hjälp av en e-postadress.</span><span class="sxs-lookup"><span data-stu-id="fe019-187">You should be able toosign up by using an email address.</span></span>

## <a name="reference"></a><span data-ttu-id="fe019-188">Referens</span><span class="sxs-lookup"><span data-stu-id="fe019-188">Reference</span></span>

<span data-ttu-id="fe019-189">Du kan hitta exempelmallarna för anpassning av Användargränssnittet här:</span><span class="sxs-lookup"><span data-stu-id="fe019-189">You can find sample templates for UI customization here:</span></span>

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

<span data-ttu-id="fe019-190">Hej sample_templates/wingtip mappen innehåller hello följande HTML-filer:</span><span class="sxs-lookup"><span data-stu-id="fe019-190">hello sample_templates/wingtip folder contains hello following HTML files:</span></span>

| <span data-ttu-id="fe019-191">HTML5-mall</span><span class="sxs-lookup"><span data-stu-id="fe019-191">HTML5 template</span></span> | <span data-ttu-id="fe019-192">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe019-192">Description</span></span> |
|----------------|-------------|
| <span data-ttu-id="fe019-193">*phonefactor.HTML*</span><span class="sxs-lookup"><span data-stu-id="fe019-193">*phonefactor.html*</span></span> | <span data-ttu-id="fe019-194">Använd den här filen som en mall för en multifaktorautentiseringssidan.</span><span class="sxs-lookup"><span data-stu-id="fe019-194">Use this file as a template for a multi-factor authentication page.</span></span> |
| <span data-ttu-id="fe019-195">*ResetPassword.HTML*</span><span class="sxs-lookup"><span data-stu-id="fe019-195">*resetpassword.html*</span></span> | <span data-ttu-id="fe019-196">Använd den här filen som en mall för en glömt lösenord.</span><span class="sxs-lookup"><span data-stu-id="fe019-196">Use this file as a template for a forgot password page.</span></span> |
| <span data-ttu-id="fe019-197">*selfasserted.HTML*</span><span class="sxs-lookup"><span data-stu-id="fe019-197">*selfasserted.html*</span></span> | <span data-ttu-id="fe019-198">Använd den här filen som en mall för en sociala konto registreringssidan, registreringssidan för lokalt konto eller ett lokalt konto-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="fe019-198">Use this file as a template for a social account sign-up page, a local account sign-up page, or a local account sign-in page.</span></span> |
| <span data-ttu-id="fe019-199">*Unified.HTML*</span><span class="sxs-lookup"><span data-stu-id="fe019-199">*unified.html*</span></span> | <span data-ttu-id="fe019-200">Använd den här filen som en mall för ett enhetlig registrering eller inloggning.</span><span class="sxs-lookup"><span data-stu-id="fe019-200">Use this file as a template for a unified sign-up or sign-in page.</span></span> |
| <span data-ttu-id="fe019-201">*updateprofile.HTML*</span><span class="sxs-lookup"><span data-stu-id="fe019-201">*updateprofile.html*</span></span> | <span data-ttu-id="fe019-202">Använd den här filen som en mall för en uppdatering profilsida.</span><span class="sxs-lookup"><span data-stu-id="fe019-202">Use this file as a template for a profile update page.</span></span> |

<span data-ttu-id="fe019-203">I hello [ändra anpassad princip för registrering eller inloggning-avsnittet](#modify-your-sign-up-or-sign-in-custom-policy), du har konfigurerat hello innehållsdefinitionen för `api.idpselections`.</span><span class="sxs-lookup"><span data-stu-id="fe019-203">In hello [Modify your sign-up or sign-in custom policy section](#modify-your-sign-up-or-sign-in-custom-policy), you configured hello content definition for `api.idpselections`.</span></span> <span data-ttu-id="fe019-204">hello fullständig uppsättning innehållsdefinitionen ID: N som identifieras av hello Azure AD B2C identitet upplevelse framework och deras beskrivningar finns i hello följande tabell:</span><span class="sxs-lookup"><span data-stu-id="fe019-204">hello full set of content definition IDs that are recognized by hello Azure AD B2C identity experience framework and their descriptions are in hello following table:</span></span>

| <span data-ttu-id="fe019-205">Innehålls-ID: N</span><span class="sxs-lookup"><span data-stu-id="fe019-205">Content definition ID</span></span> | <span data-ttu-id="fe019-206">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fe019-206">Description</span></span> | 
|-----------------------|-------------|
| <span data-ttu-id="fe019-207">*API.Error*</span><span class="sxs-lookup"><span data-stu-id="fe019-207">*api.error*</span></span> | <span data-ttu-id="fe019-208">**Felsidan**.</span><span class="sxs-lookup"><span data-stu-id="fe019-208">**Error page**.</span></span> <span data-ttu-id="fe019-209">Den här sidan visas när ett undantagsfel eller ett fel har påträffats.</span><span class="sxs-lookup"><span data-stu-id="fe019-209">This page is displayed when an exception or an error is encountered.</span></span> |
| <span data-ttu-id="fe019-210">*API.idpselections*</span><span class="sxs-lookup"><span data-stu-id="fe019-210">*api.idpselections*</span></span> | <span data-ttu-id="fe019-211">**Identity-providern på sidan**.</span><span class="sxs-lookup"><span data-stu-id="fe019-211">**Identity provider selection page**.</span></span> <span data-ttu-id="fe019-212">Den här sidan innehåller en lista över providers som hello användaren kan välja bland under inloggning identitet.</span><span class="sxs-lookup"><span data-stu-id="fe019-212">This page contains a list of identity providers that hello user can choose from during sign-in.</span></span> <span data-ttu-id="fe019-213">Dessa alternativ är enterprise identitetsleverantörer, sociala identitetsleverantörer, till exempel Facebook och Google + eller lokala konton.</span><span class="sxs-lookup"><span data-stu-id="fe019-213">These options are either enterprise identity providers, social identity providers such as Facebook and Google+, or local accounts.</span></span> |
| <span data-ttu-id="fe019-214">*API.idpselections.Signup*</span><span class="sxs-lookup"><span data-stu-id="fe019-214">*api.idpselections.signup*</span></span> | <span data-ttu-id="fe019-215">**Identitet providern val för registrering**.</span><span class="sxs-lookup"><span data-stu-id="fe019-215">**Identity provider selection for sign-up**.</span></span> <span data-ttu-id="fe019-216">Den här sidan innehåller en lista över providers som hello användaren kan välja bland under registreringen identitet.</span><span class="sxs-lookup"><span data-stu-id="fe019-216">This page contains a list of identity providers that hello user can choose from during sign-up.</span></span> <span data-ttu-id="fe019-217">Dessa alternativ är enterprise identitetsleverantörer, sociala identitetsleverantörer, till exempel Facebook och Google + eller lokala konton.</span><span class="sxs-lookup"><span data-stu-id="fe019-217">These options are either enterprise identity providers, social identity providers such as Facebook and Google+, or local accounts.</span></span> |
| <span data-ttu-id="fe019-218">*API.localaccountpasswordreset*</span><span class="sxs-lookup"><span data-stu-id="fe019-218">*api.localaccountpasswordreset*</span></span> | <span data-ttu-id="fe019-219">**Har du glömt lösenordssidan**.</span><span class="sxs-lookup"><span data-stu-id="fe019-219">**Forgot password page**.</span></span> <span data-ttu-id="fe019-220">Den här sidan innehåller ett formulär hello användaren måste slutföra tooinitiate en lösenordsåterställning.</span><span class="sxs-lookup"><span data-stu-id="fe019-220">This page contains a form that hello user must complete tooinitiate a password reset.</span></span>  |
| <span data-ttu-id="fe019-221">*API.localaccountsignin*</span><span class="sxs-lookup"><span data-stu-id="fe019-221">*api.localaccountsignin*</span></span> | <span data-ttu-id="fe019-222">**Lokalt konto inloggningssidan**.</span><span class="sxs-lookup"><span data-stu-id="fe019-222">**Local account sign-in page**.</span></span> <span data-ttu-id="fe019-223">Den här sidan innehåller ett formulär för att logga in med ett lokalt konto som baseras på en e-postadress eller ett användarnamn.</span><span class="sxs-lookup"><span data-stu-id="fe019-223">This page contains a sign-in form for signing in with a local account that is based on an email address or a user name.</span></span> <span data-ttu-id="fe019-224">hello formulär kan innehålla en inkommande textruta och inmatningsfält för lösenord.</span><span class="sxs-lookup"><span data-stu-id="fe019-224">hello form can contain a text input box and password entry box.</span></span> |
| <span data-ttu-id="fe019-225">*API.localaccountsignup*</span><span class="sxs-lookup"><span data-stu-id="fe019-225">*api.localaccountsignup*</span></span> | <span data-ttu-id="fe019-226">**Lokalt konto registreringssidan**.</span><span class="sxs-lookup"><span data-stu-id="fe019-226">**Local account sign-up page**.</span></span> <span data-ttu-id="fe019-227">Den här sidan innehåller en registreringsformuläret för att registrera dig för ett lokalt konto som baseras på en e-postadress eller ett användarnamn.</span><span class="sxs-lookup"><span data-stu-id="fe019-227">This page contains a sign-up form for signing up for a local account that is based on an email address or a user name.</span></span> <span data-ttu-id="fe019-228">hello formulär kan innehålla olika inkommande kontroller, till exempel en textruta, inmatningsfält för lösenord, en alternativknapp, enkelval listrutorna och välja flera kryssrutor.</span><span class="sxs-lookup"><span data-stu-id="fe019-228">hello form can contain various input controls, such as a text input box, a password entry box, a radio button, single-select drop-down boxes, and multi-select check boxes.</span></span> |
| <span data-ttu-id="fe019-229">*API.phonefactor*</span><span class="sxs-lookup"><span data-stu-id="fe019-229">*api.phonefactor*</span></span> | <span data-ttu-id="fe019-230">**Multifaktorautentiseringssidan**.</span><span class="sxs-lookup"><span data-stu-id="fe019-230">**Multi-factor authentication page**.</span></span> <span data-ttu-id="fe019-231">På den här sidan verifiera användare sina telefonnummer (med hjälp av text- eller röst) under registrering eller inloggning.</span><span class="sxs-lookup"><span data-stu-id="fe019-231">On this page, users can verify their phone numbers (by using text or voice) during sign-up or sign-in.</span></span> |
| <span data-ttu-id="fe019-232">*API.selfasserted*</span><span class="sxs-lookup"><span data-stu-id="fe019-232">*api.selfasserted*</span></span> | <span data-ttu-id="fe019-233">**Sociala konto registreringssidan**.</span><span class="sxs-lookup"><span data-stu-id="fe019-233">**Social account sign-up page**.</span></span> <span data-ttu-id="fe019-234">Den här sidan innehåller en registreringsformuläret som användare måste slutföra när de loggar med ett befintligt konto från en sociala identitetsleverantören, till exempel Facebook eller Google +.</span><span class="sxs-lookup"><span data-stu-id="fe019-234">This page contains a sign-up form that users must complete when they sign up by using an existing account from a social identity provider such as Facebook or Google+.</span></span> <span data-ttu-id="fe019-235">Den här sidan är liknande toohello föregående sociala konto registreringssidan, förutom hello lösenordsfält.</span><span class="sxs-lookup"><span data-stu-id="fe019-235">This page is similar toohello preceding social account sign-up page, except for hello password entry fields.</span></span> |
| <span data-ttu-id="fe019-236">*API.selfasserted.profileupdate*</span><span class="sxs-lookup"><span data-stu-id="fe019-236">*api.selfasserted.profileupdate*</span></span> | <span data-ttu-id="fe019-237">**Uppdatera profilsida**.</span><span class="sxs-lookup"><span data-stu-id="fe019-237">**Profile update page**.</span></span> <span data-ttu-id="fe019-238">Den här sidan innehåller ett formulär som användare kan använda tooupdate sin profil.</span><span class="sxs-lookup"><span data-stu-id="fe019-238">This page contains a form that users can use tooupdate their profile.</span></span> <span data-ttu-id="fe019-239">Den här sidan är liknande toohello sociala konto registreringssidan, förutom hello lösenordsfält.</span><span class="sxs-lookup"><span data-stu-id="fe019-239">This page is similar toohello social account sign-up page, except for hello password entry fields.</span></span> |
| <span data-ttu-id="fe019-240">*API.signuporsignin*</span><span class="sxs-lookup"><span data-stu-id="fe019-240">*api.signuporsignin*</span></span> | <span data-ttu-id="fe019-241">**Enhetlig registrering eller inloggning sidan**.</span><span class="sxs-lookup"><span data-stu-id="fe019-241">**Unified sign-up or sign-in page**.</span></span> <span data-ttu-id="fe019-242">Den här sidan hanterar både hello registrering och inloggning av användare som kan använda enterprise identitetsleverantörer, sociala identitetsleverantörer, till exempel Facebook eller Google + eller lokala konton.</span><span class="sxs-lookup"><span data-stu-id="fe019-242">This page handles both hello sign-up and sign-in of users, who can use enterprise identity providers, social identity providers such as Facebook or Google+, or local accounts.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="fe019-243">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fe019-243">Next steps</span></span>

<span data-ttu-id="fe019-244">Mer information om UI-element som kan anpassas finns [referenshandbok för anpassning av Användargränssnittet för inbyggda principer](active-directory-b2c-reference-ui-customization.md).</span><span class="sxs-lookup"><span data-stu-id="fe019-244">For additional information about UI elements that can be customized, see [reference guide for UI customization for built-in policies](active-directory-b2c-reference-ui-customization.md).</span></span>
