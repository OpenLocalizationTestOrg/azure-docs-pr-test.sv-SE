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
ms.openlocfilehash: d5a3c0a323b31696d39e3d2b36317dec3a2337d7
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-b2c-configure-ui-customization-in-a-custom-policy"></a><span data-ttu-id="d60e4-103">Azure Active Directory B2C: Konfigurera anpassningar i en anpassad princip</span><span class="sxs-lookup"><span data-stu-id="d60e4-103">Azure Active Directory B2C: Configure UI customization in a custom policy</span></span>

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

<span data-ttu-id="d60e4-104">När du har slutfört den här artikeln har du en anpassad princip för registrering och inloggning med märke och utseende.</span><span class="sxs-lookup"><span data-stu-id="d60e4-104">After you complete this article, you will have a sign-up and sign-in custom policy with your brand and appearance.</span></span> <span data-ttu-id="d60e4-105">Med Azure Active Directory B2C (Azure AD B2C), du får nästan full kontroll över HTML- och CSS-innehåll som visas för användarna.</span><span class="sxs-lookup"><span data-stu-id="d60e4-105">With Azure Active Directory B2C (Azure AD B2C), you get nearly full control of the HTML and CSS content that's presented to users.</span></span> <span data-ttu-id="d60e4-106">När du använder en anpassad princip kan konfigurera du anpassning av Användargränssnittet i XML istället för att använda kontroller i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d60e4-106">When you use a custom policy, you configure UI customization in XML instead of using controls in the Azure portal.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="d60e4-107">Krav</span><span class="sxs-lookup"><span data-stu-id="d60e4-107">Prerequisites</span></span>

<span data-ttu-id="d60e4-108">Innan du kan slutföra [komma igång med anpassade principer](active-directory-b2c-get-started-custom.md).</span><span class="sxs-lookup"><span data-stu-id="d60e4-108">Before you begin, complete [Getting started with custom policies](active-directory-b2c-get-started-custom.md).</span></span> <span data-ttu-id="d60e4-109">Du bör ha en fungerande anpassad princip för registrering och inloggning med lokala konton.</span><span class="sxs-lookup"><span data-stu-id="d60e4-109">You should have a working custom policy for sign-up and sign-in with local accounts.</span></span>

## <a name="page-ui-customization"></a><span data-ttu-id="d60e4-110">Anpassning av Page UI</span><span class="sxs-lookup"><span data-stu-id="d60e4-110">Page UI customization</span></span>

<span data-ttu-id="d60e4-111">Du kan anpassa utseendet och känslan av en anpassad princip med hjälp av sidan anpassning gränssnittsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="d60e4-111">By using the page UI customization feature, you can customize the look and feel of any custom policy.</span></span> <span data-ttu-id="d60e4-112">Du kan också upprätthålla märke och visual konsekvensen mellan dina program och Azure AD B2C.</span><span class="sxs-lookup"><span data-stu-id="d60e4-112">You can also maintain brand and visual consistency between your application and Azure AD B2C.</span></span>

<span data-ttu-id="d60e4-113">Här är hur det fungerar: Azure AD B2C Kör koden i kundens webbläsare och använder en modern metod som kallas [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/).</span><span class="sxs-lookup"><span data-stu-id="d60e4-113">Here's how it works: Azure AD B2C runs code in your customer's browser and uses a modern approach called [Cross-Origin Resource Sharing (CORS)](http://www.w3.org/TR/cors/).</span></span> <span data-ttu-id="d60e4-114">Först måste ange du en URL i en anpassad princip med anpassade HTML-innehåll.</span><span class="sxs-lookup"><span data-stu-id="d60e4-114">First, you specify a URL in the custom policy with customized HTML content.</span></span> <span data-ttu-id="d60e4-115">Azure AD B2C sammanfogas UI-element med HTML-innehåll som har lästs in från URL: en och visar sidan till kunden.</span><span class="sxs-lookup"><span data-stu-id="d60e4-115">Azure AD B2C merges UI elements with the HTML content that's loaded from your URL and then displays the page to the customer.</span></span>

## <a name="create-your-html5-content"></a><span data-ttu-id="d60e4-116">Skapa din HTML5 innehåll</span><span class="sxs-lookup"><span data-stu-id="d60e4-116">Create your HTML5 content</span></span>

<span data-ttu-id="d60e4-117">Skapa HTML innehåll med varumärken produktnamnet i rubriken.</span><span class="sxs-lookup"><span data-stu-id="d60e4-117">Create HTML content with your product's brand name in the title.</span></span>

1. <span data-ttu-id="d60e4-118">Kopiera följande HTML-fragment.</span><span class="sxs-lookup"><span data-stu-id="d60e4-118">Copy the following HTML snippet.</span></span> <span data-ttu-id="d60e4-119">Det är korrekt HTML5 med ett tomt element kallas  *\<div id = ”api”\>\</div\>*  i den  *\<brödtext\>*  taggar.</span><span class="sxs-lookup"><span data-stu-id="d60e4-119">It is well-formed HTML5 with an empty element called *\<div id="api"\>\</div\>* located within the *\<body\>* tags.</span></span> <span data-ttu-id="d60e4-120">Det här elementet anger där Azure AD B2C-innehåll som ska infogas.</span><span class="sxs-lookup"><span data-stu-id="d60e4-120">This element indicates where Azure AD B2C content is to be inserted.</span></span>

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
   ><span data-ttu-id="d60e4-121">Av säkerhetsskäl bör blockerat användningen av JavaScript för närvarande för anpassning.</span><span class="sxs-lookup"><span data-stu-id="d60e4-121">For security reasons, the use of JavaScript is currently blocked for customization.</span></span>

2. <span data-ttu-id="d60e4-122">Klistra in den kopierade fragment i en textredigerare och spara filen som *anpassa ui.html*.</span><span class="sxs-lookup"><span data-stu-id="d60e4-122">Paste the copied snippet in a text editor, and then save the file as *customize-ui.html*.</span></span>

## <a name="create-an-azure-blob-storage-account"></a><span data-ttu-id="d60e4-123">Skapa ett Azure Blob storage-konto</span><span class="sxs-lookup"><span data-stu-id="d60e4-123">Create an Azure Blob storage account</span></span>

>[!NOTE]
> <span data-ttu-id="d60e4-124">I den här artikeln används Azure Blob storage som värd för innehållet.</span><span class="sxs-lookup"><span data-stu-id="d60e4-124">In this article, we use Azure Blob storage to host our content.</span></span> <span data-ttu-id="d60e4-125">Du kan välja att vara värd för ditt innehåll på en webbserver, men du måste [aktivera CORS på din webbserver](https://enable-cors.org/server.html).</span><span class="sxs-lookup"><span data-stu-id="d60e4-125">You can choose to host your content on a web server, but you must [enable CORS on your web server](https://enable-cors.org/server.html).</span></span>

<span data-ttu-id="d60e4-126">Om du vill vara värd för den här HTML-innehåll i Blob storage, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="d60e4-126">To host this HTML content in Blob storage, do the following:</span></span>

1. <span data-ttu-id="d60e4-127">Logga in på [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d60e4-127">Sign in to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="d60e4-128">På den **hubb** väljer du **ny** > **lagring** > **lagringskonto**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-128">On the **Hub** menu, select **New** > **Storage** > **Storage account**.</span></span>
3. <span data-ttu-id="d60e4-129">Ange ett unikt **namn** för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d60e4-129">Enter a unique **Name** for your storage account.</span></span>
4. <span data-ttu-id="d60e4-130">**Distributionsmodell** kan vara **Resource Manager**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-130">**Deployment model** can remain **Resource Manager**.</span></span>
5. <span data-ttu-id="d60e4-131">Ändra **konto typ** till **Blob storage**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-131">Change **Account Kind** to **Blob storage**.</span></span>
6. <span data-ttu-id="d60e4-132">**Prestanda** kan vara **Standard**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-132">**Performance** can remain **Standard**.</span></span>
7. <span data-ttu-id="d60e4-133">**Replikering** kan vara **RA-GRS**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-133">**Replication** can remain **RA-GRS**.</span></span>
8. <span data-ttu-id="d60e4-134">**Åtkomstnivå** kan vara **frekvent**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-134">**Access tier** can remain **Hot**.</span></span>
9. <span data-ttu-id="d60e4-135">**Lagringstjänstens kryptering** kan vara **inaktiverade**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-135">**Storage service encryption** can remain **Disabled**.</span></span>
10. <span data-ttu-id="d60e4-136">Välj en **prenumeration** för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d60e4-136">Select a **Subscription** for your storage account.</span></span>
11. <span data-ttu-id="d60e4-137">Skapa en **resursgruppen** eller välj en befintlig.</span><span class="sxs-lookup"><span data-stu-id="d60e4-137">Create a **Resource group** or select an existing one.</span></span>
12. <span data-ttu-id="d60e4-138">Välj den **geografisk plats** för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d60e4-138">Select the **Geographic location** for your storage account.</span></span>
13. <span data-ttu-id="d60e4-139">Skapa lagringskontot genom att klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-139">Click **Create** to create the storage account.</span></span>  
    <span data-ttu-id="d60e4-140">När distributionen är klar i **lagringskonto** blad öppnas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d60e4-140">After the deployment is completed, the **Storage account** blade opens automatically.</span></span>

## <a name="create-a-container"></a><span data-ttu-id="d60e4-141">Skapa en behållare</span><span class="sxs-lookup"><span data-stu-id="d60e4-141">Create a container</span></span>

<span data-ttu-id="d60e4-142">Om du vill skapa en offentlig behållare i Blob storage, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="d60e4-142">To create a public container in Blob storage, do the following:</span></span>

1. <span data-ttu-id="d60e4-143">Klicka på den **översikt** fliken.</span><span class="sxs-lookup"><span data-stu-id="d60e4-143">Click the **Overview** tab.</span></span>
2. <span data-ttu-id="d60e4-144">Klicka på **behållare**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-144">Click **Container**.</span></span>
3. <span data-ttu-id="d60e4-145">För **namn**, typen **$root**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-145">For **Name**, type **$root**.</span></span>
4. <span data-ttu-id="d60e4-146">Ange **komma åt typen** till **Blob**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-146">Set **Access type** to **Blob**.</span></span>
5. <span data-ttu-id="d60e4-147">Klicka på **$root** att öppna den nya behållaren.</span><span class="sxs-lookup"><span data-stu-id="d60e4-147">Click **$root** to open the new container.</span></span>
6. <span data-ttu-id="d60e4-148">Klicka på **Överför**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-148">Click **Upload**.</span></span>
7. <span data-ttu-id="d60e4-149">Klicka på mappikonen bredvid **Välj en fil**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-149">Click the folder icon next to **Select a file**.</span></span>
8. <span data-ttu-id="d60e4-150">Gå till **anpassa ui.html**, som du skapade tidigare i den [Page UI anpassning](#the-page-ui-customization-feature) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d60e4-150">Go to **customize-ui.html**, which you created earlier in the [Page UI customization](#the-page-ui-customization-feature) section.</span></span>
9. <span data-ttu-id="d60e4-151">Klicka på **Överför**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-151">Click **Upload**.</span></span>
10. <span data-ttu-id="d60e4-152">Välj Anpassa ui.html blob som du överfört.</span><span class="sxs-lookup"><span data-stu-id="d60e4-152">Select the customize-ui.html blob that you uploaded.</span></span>
11. <span data-ttu-id="d60e4-153">Bredvid **URL**, klickar du på **kopiera**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-153">Next to **URL**, click **Copy**.</span></span>
12. <span data-ttu-id="d60e4-154">Klistra in den kopierade Webbadressen i en webbläsare och gå till webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="d60e4-154">In a browser, paste the copied URL, and go to the site.</span></span> <span data-ttu-id="d60e4-155">Om platsen är tillgänglig, kontrollera åtkomst behållartypen anges till **blob**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-155">If the site is inaccessible, make sure the container access type is set to **blob**.</span></span>

## <a name="configure-cors"></a><span data-ttu-id="d60e4-156">Konfigurera CORS</span><span class="sxs-lookup"><span data-stu-id="d60e4-156">Configure CORS</span></span>

<span data-ttu-id="d60e4-157">Konfigurera Blob storage för Cross-Origin Resource Sharing genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="d60e4-157">Configure Blob storage for Cross-Origin Resource Sharing by doing the following:</span></span>

>[!NOTE]
><span data-ttu-id="d60e4-158">Om du vill testa funktionen för anpassning av Användargränssnittet med hjälp av våra exempel HTML och CSS innehåll?</span><span class="sxs-lookup"><span data-stu-id="d60e4-158">Want to try out the UI customization feature by using our sample HTML and CSS content?</span></span> <span data-ttu-id="d60e4-159">Vi har samlat [ett enkelt kommandoradsverktyg](active-directory-b2c-reference-ui-customization-helper-tool.md) som överför och konfigurerar våra exemplen på Blob storage-konto.</span><span class="sxs-lookup"><span data-stu-id="d60e4-159">We've provided [a simple helper tool](active-directory-b2c-reference-ui-customization-helper-tool.md) that uploads and configures our sample content on your Blob storage account.</span></span> <span data-ttu-id="d60e4-160">Om du använder verktyget kan du gå vidare till [ändra en anpassad princip för registrering eller inloggning](#modify-your-sign-up-or-sign-in-custom-policy).</span><span class="sxs-lookup"><span data-stu-id="d60e4-160">If you use the tool, skip ahead to [Modify your sign-up or sign-in custom policy](#modify-your-sign-up-or-sign-in-custom-policy).</span></span>

1. <span data-ttu-id="d60e4-161">På den **lagring** bladet under **inställningar**öppnar **CORS**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-161">On the **Storage** blade, under **Settings**, open **CORS**.</span></span>
2. <span data-ttu-id="d60e4-162">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-162">Click **Add**.</span></span>
3. <span data-ttu-id="d60e4-163">För **tillåtna ursprung**, skriver du en asterisk (\*).</span><span class="sxs-lookup"><span data-stu-id="d60e4-163">For **Allowed origins**, type an asterisk (\*).</span></span>
4. <span data-ttu-id="d60e4-164">I den **tillåtna verb** listrutan, Välj **hämta** och **alternativ**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-164">In the **Allowed verbs** drop-down list, select both **GET** and **OPTIONS**.</span></span>
5. <span data-ttu-id="d60e4-165">För **tillåtna huvuden**, skriver du en asterisk (\*).</span><span class="sxs-lookup"><span data-stu-id="d60e4-165">For **Allowed headers**, type an asterisk (\*).</span></span>
6. <span data-ttu-id="d60e4-166">För **exponeras huvuden**, skriver du en asterisk (\*).</span><span class="sxs-lookup"><span data-stu-id="d60e4-166">For **Exposed headers**, type an asterisk (\*).</span></span>
7. <span data-ttu-id="d60e4-167">För **högsta ålder (sekunder)**, typen **200**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-167">For **Maximum age (seconds)**, type **200**.</span></span>
8. <span data-ttu-id="d60e4-168">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-168">Click **Add**.</span></span>

## <a name="test-cors"></a><span data-ttu-id="d60e4-169">Testa CORS</span><span class="sxs-lookup"><span data-stu-id="d60e4-169">Test CORS</span></span>

<span data-ttu-id="d60e4-170">Verifiera att du är redo genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="d60e4-170">Validate that you're ready by doing the following:</span></span>

1. <span data-ttu-id="d60e4-171">Gå till den [test cors.org](http://test-cors.org/) webbplats, och klistra in Webbadressen i den **fjärr-URL** rutan.</span><span class="sxs-lookup"><span data-stu-id="d60e4-171">Go to the [test-cors.org](http://test-cors.org/) website, and then paste the URL in the **Remote URL** box.</span></span>
2. <span data-ttu-id="d60e4-172">Klicka på **skicka begäran**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-172">Click **Send Request**.</span></span>  
    <span data-ttu-id="d60e4-173">Om du får ett felmeddelande, kontrollerar du att din [CORS-inställningarna](#configure-cors) är korrekta.</span><span class="sxs-lookup"><span data-stu-id="d60e4-173">If you receive an error, make sure that your [CORS settings](#configure-cors) are correct.</span></span> <span data-ttu-id="d60e4-174">Du kan också behöva rensa webbläsarens cacheminne eller öppna ett privat sessionen genom att trycka på Ctrl + Skift + P.</span><span class="sxs-lookup"><span data-stu-id="d60e4-174">You might also need to clear your browser cache or open an in-private browsing session by pressing Ctrl+Shift+P.</span></span>

## <a name="modify-your-sign-up-or-sign-in-custom-policy"></a><span data-ttu-id="d60e4-175">Ändra en anpassad princip för registrering eller inloggning</span><span class="sxs-lookup"><span data-stu-id="d60e4-175">Modify your sign-up or sign-in custom policy</span></span>

<span data-ttu-id="d60e4-176">Under den översta  *\<TrustFrameworkPolicy\>*  tagg, bör du hitta  *\<BuildingBlocks\>*  tagg.</span><span class="sxs-lookup"><span data-stu-id="d60e4-176">Under the top-level *\<TrustFrameworkPolicy\>* tag, you should find *\<BuildingBlocks\>* tag.</span></span> <span data-ttu-id="d60e4-177">I den  *\<BuildingBlocks\>*  taggar, lägga till en  *\<ContentDefinitions\>*  taggen genom att kopiera följande exempel.</span><span class="sxs-lookup"><span data-stu-id="d60e4-177">Within the *\<BuildingBlocks\>* tags, add a *\<ContentDefinitions\>* tag by copying the following example.</span></span> <span data-ttu-id="d60e4-178">Ersätt *your_storage_account* med namnet på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d60e4-178">Replace *your_storage_account* with the name of your storage account.</span></span>

  ```xml
  <BuildingBlocks>
    <ContentDefinitions>
      <ContentDefinition Id="api.idpselections">
        <LoadUri>https://{your_storage_account}.blob.core.windows.net/customize-ui.html</LoadUri>
      </ContentDefinition>
    </ContentDefinitions>
  </BuildingBlocks>
  ```

## <a name="upload-your-updated-custom-policy"></a><span data-ttu-id="d60e4-179">Ladda upp din uppdaterade anpassad princip</span><span class="sxs-lookup"><span data-stu-id="d60e4-179">Upload your updated custom policy</span></span>

1. <span data-ttu-id="d60e4-180">I den [Azure-portalen](https://portal.azure.com), [växla i samband med din Azure AD B2C-klient](active-directory-b2c-navigate-to-b2c-context.md), och sedan öppna den **Azure AD B2C** bladet.</span><span class="sxs-lookup"><span data-stu-id="d60e4-180">In the [Azure portal](https://portal.azure.com), [switch into the context of your Azure AD B2C tenant](active-directory-b2c-navigate-to-b2c-context.md), and then open the **Azure AD B2C** blade.</span></span>
2. <span data-ttu-id="d60e4-181">Klicka på **alla principer**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-181">Click **All Policies**.</span></span>
3. <span data-ttu-id="d60e4-182">Klicka på **överföra princip**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-182">Click **Upload Policy**.</span></span>
4. <span data-ttu-id="d60e4-183">Överför `SignUpOrSignin.xml` med den  *\<ContentDefinitions\>*  tagg som du har lagt till tidigare.</span><span class="sxs-lookup"><span data-stu-id="d60e4-183">Upload `SignUpOrSignin.xml` with the *\<ContentDefinitions\>* tag that you added previously.</span></span>

## <a name="test-the-custom-policy-by-using-run-now"></a><span data-ttu-id="d60e4-184">Testa den anpassade principen med hjälp av **kör nu**</span><span class="sxs-lookup"><span data-stu-id="d60e4-184">Test the custom policy by using **Run now**</span></span>

1. <span data-ttu-id="d60e4-185">På den **Azure AD B2C** gå till bladet **alla principer**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-185">On the **Azure AD B2C** blade, go to **All polices**.</span></span>
2. <span data-ttu-id="d60e4-186">Välj den anpassade principen som du överfört och klicka på den **kör nu** knappen.</span><span class="sxs-lookup"><span data-stu-id="d60e4-186">Select the custom policy that you uploaded, and click the **Run now** button.</span></span>
3. <span data-ttu-id="d60e4-187">Du ska kunna logga med hjälp av en e-postadress.</span><span class="sxs-lookup"><span data-stu-id="d60e4-187">You should be able to sign up by using an email address.</span></span>

## <a name="reference"></a><span data-ttu-id="d60e4-188">Referens</span><span class="sxs-lookup"><span data-stu-id="d60e4-188">Reference</span></span>

<span data-ttu-id="d60e4-189">Du kan hitta exempelmallarna för anpassning av Användargränssnittet här:</span><span class="sxs-lookup"><span data-stu-id="d60e4-189">You can find sample templates for UI customization here:</span></span>

```
git clone https://github.com/azureadquickstarts/b2c-azureblobstorage-client
```

<span data-ttu-id="d60e4-190">Mappen sample_templates/wingtip innehåller följande HTML-filer:</span><span class="sxs-lookup"><span data-stu-id="d60e4-190">The sample_templates/wingtip folder contains the following HTML files:</span></span>

| <span data-ttu-id="d60e4-191">HTML5-mall</span><span class="sxs-lookup"><span data-stu-id="d60e4-191">HTML5 template</span></span> | <span data-ttu-id="d60e4-192">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d60e4-192">Description</span></span> |
|----------------|-------------|
| <span data-ttu-id="d60e4-193">*phonefactor.HTML*</span><span class="sxs-lookup"><span data-stu-id="d60e4-193">*phonefactor.html*</span></span> | <span data-ttu-id="d60e4-194">Använd den här filen som en mall för en multifaktorautentiseringssidan.</span><span class="sxs-lookup"><span data-stu-id="d60e4-194">Use this file as a template for a multi-factor authentication page.</span></span> |
| <span data-ttu-id="d60e4-195">*ResetPassword.HTML*</span><span class="sxs-lookup"><span data-stu-id="d60e4-195">*resetpassword.html*</span></span> | <span data-ttu-id="d60e4-196">Använd den här filen som en mall för en glömt lösenord.</span><span class="sxs-lookup"><span data-stu-id="d60e4-196">Use this file as a template for a forgot password page.</span></span> |
| <span data-ttu-id="d60e4-197">*selfasserted.HTML*</span><span class="sxs-lookup"><span data-stu-id="d60e4-197">*selfasserted.html*</span></span> | <span data-ttu-id="d60e4-198">Använd den här filen som en mall för en sociala konto registreringssidan, registreringssidan för lokalt konto eller ett lokalt konto-inloggningssida.</span><span class="sxs-lookup"><span data-stu-id="d60e4-198">Use this file as a template for a social account sign-up page, a local account sign-up page, or a local account sign-in page.</span></span> |
| <span data-ttu-id="d60e4-199">*Unified.HTML*</span><span class="sxs-lookup"><span data-stu-id="d60e4-199">*unified.html*</span></span> | <span data-ttu-id="d60e4-200">Använd den här filen som en mall för ett enhetlig registrering eller inloggning.</span><span class="sxs-lookup"><span data-stu-id="d60e4-200">Use this file as a template for a unified sign-up or sign-in page.</span></span> |
| <span data-ttu-id="d60e4-201">*updateprofile.HTML*</span><span class="sxs-lookup"><span data-stu-id="d60e4-201">*updateprofile.html*</span></span> | <span data-ttu-id="d60e4-202">Använd den här filen som en mall för en uppdatering profilsida.</span><span class="sxs-lookup"><span data-stu-id="d60e4-202">Use this file as a template for a profile update page.</span></span> |

<span data-ttu-id="d60e4-203">I den [ändra anpassad princip för registrering eller inloggning-avsnittet](#modify-your-sign-up-or-sign-in-custom-policy), du har konfigurerat innehållsdefinitionen för `api.idpselections`.</span><span class="sxs-lookup"><span data-stu-id="d60e4-203">In the [Modify your sign-up or sign-in custom policy section](#modify-your-sign-up-or-sign-in-custom-policy), you configured the content definition for `api.idpselections`.</span></span> <span data-ttu-id="d60e4-204">En fullständig uppsättning innehåll Definitions-ID som identifieras av Azure AD B2C identitet upplevelse framework och deras beskrivningar finns i följande tabell:</span><span class="sxs-lookup"><span data-stu-id="d60e4-204">The full set of content definition IDs that are recognized by the Azure AD B2C identity experience framework and their descriptions are in the following table:</span></span>

| <span data-ttu-id="d60e4-205">Innehålls-ID: N</span><span class="sxs-lookup"><span data-stu-id="d60e4-205">Content definition ID</span></span> | <span data-ttu-id="d60e4-206">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d60e4-206">Description</span></span> | 
|-----------------------|-------------|
| <span data-ttu-id="d60e4-207">*API.Error*</span><span class="sxs-lookup"><span data-stu-id="d60e4-207">*api.error*</span></span> | <span data-ttu-id="d60e4-208">**Felsidan**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-208">**Error page**.</span></span> <span data-ttu-id="d60e4-209">Den här sidan visas när ett undantagsfel eller ett fel har påträffats.</span><span class="sxs-lookup"><span data-stu-id="d60e4-209">This page is displayed when an exception or an error is encountered.</span></span> |
| <span data-ttu-id="d60e4-210">*API.idpselections*</span><span class="sxs-lookup"><span data-stu-id="d60e4-210">*api.idpselections*</span></span> | <span data-ttu-id="d60e4-211">**Identity-providern på sidan**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-211">**Identity provider selection page**.</span></span> <span data-ttu-id="d60e4-212">Den här sidan innehåller en lista över identitetsleverantörer som användaren kan välja från under inloggningen.</span><span class="sxs-lookup"><span data-stu-id="d60e4-212">This page contains a list of identity providers that the user can choose from during sign-in.</span></span> <span data-ttu-id="d60e4-213">Dessa alternativ är enterprise identitetsleverantörer, sociala identitetsleverantörer, till exempel Facebook och Google + eller lokala konton.</span><span class="sxs-lookup"><span data-stu-id="d60e4-213">These options are either enterprise identity providers, social identity providers such as Facebook and Google+, or local accounts.</span></span> |
| <span data-ttu-id="d60e4-214">*API.idpselections.Signup*</span><span class="sxs-lookup"><span data-stu-id="d60e4-214">*api.idpselections.signup*</span></span> | <span data-ttu-id="d60e4-215">**Identitet providern val för registrering**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-215">**Identity provider selection for sign-up**.</span></span> <span data-ttu-id="d60e4-216">Den här sidan innehåller en lista över identitetsleverantörer som användaren kan välja bland under registreringen.</span><span class="sxs-lookup"><span data-stu-id="d60e4-216">This page contains a list of identity providers that the user can choose from during sign-up.</span></span> <span data-ttu-id="d60e4-217">Dessa alternativ är enterprise identitetsleverantörer, sociala identitetsleverantörer, till exempel Facebook och Google + eller lokala konton.</span><span class="sxs-lookup"><span data-stu-id="d60e4-217">These options are either enterprise identity providers, social identity providers such as Facebook and Google+, or local accounts.</span></span> |
| <span data-ttu-id="d60e4-218">*API.localaccountpasswordreset*</span><span class="sxs-lookup"><span data-stu-id="d60e4-218">*api.localaccountpasswordreset*</span></span> | <span data-ttu-id="d60e4-219">**Har du glömt lösenordssidan**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-219">**Forgot password page**.</span></span> <span data-ttu-id="d60e4-220">Den här sidan innehåller ett formulär som användaren måste slutföra för att initiera en återställning av lösenord.</span><span class="sxs-lookup"><span data-stu-id="d60e4-220">This page contains a form that the user must complete to initiate a password reset.</span></span>  |
| <span data-ttu-id="d60e4-221">*API.localaccountsignin*</span><span class="sxs-lookup"><span data-stu-id="d60e4-221">*api.localaccountsignin*</span></span> | <span data-ttu-id="d60e4-222">**Lokalt konto inloggningssidan**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-222">**Local account sign-in page**.</span></span> <span data-ttu-id="d60e4-223">Den här sidan innehåller ett formulär för att logga in med ett lokalt konto som baseras på en e-postadress eller ett användarnamn.</span><span class="sxs-lookup"><span data-stu-id="d60e4-223">This page contains a sign-in form for signing in with a local account that is based on an email address or a user name.</span></span> <span data-ttu-id="d60e4-224">Formuläret kan innehålla en textruta och inmatningsfält för lösenord.</span><span class="sxs-lookup"><span data-stu-id="d60e4-224">The form can contain a text input box and password entry box.</span></span> |
| <span data-ttu-id="d60e4-225">*API.localaccountsignup*</span><span class="sxs-lookup"><span data-stu-id="d60e4-225">*api.localaccountsignup*</span></span> | <span data-ttu-id="d60e4-226">**Lokalt konto registreringssidan**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-226">**Local account sign-up page**.</span></span> <span data-ttu-id="d60e4-227">Den här sidan innehåller en registreringsformuläret för att registrera dig för ett lokalt konto som baseras på en e-postadress eller ett användarnamn.</span><span class="sxs-lookup"><span data-stu-id="d60e4-227">This page contains a sign-up form for signing up for a local account that is based on an email address or a user name.</span></span> <span data-ttu-id="d60e4-228">Formuläret kan innehålla olika inkommande kontroller, till exempel en textruta, inmatningsfält för lösenord, en alternativknapp, enkelval listrutorna och välja flera kryssrutor.</span><span class="sxs-lookup"><span data-stu-id="d60e4-228">The form can contain various input controls, such as a text input box, a password entry box, a radio button, single-select drop-down boxes, and multi-select check boxes.</span></span> |
| <span data-ttu-id="d60e4-229">*API.phonefactor*</span><span class="sxs-lookup"><span data-stu-id="d60e4-229">*api.phonefactor*</span></span> | <span data-ttu-id="d60e4-230">**Multifaktorautentiseringssidan**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-230">**Multi-factor authentication page**.</span></span> <span data-ttu-id="d60e4-231">På den här sidan verifiera användare sina telefonnummer (med hjälp av text- eller röst) under registrering eller inloggning.</span><span class="sxs-lookup"><span data-stu-id="d60e4-231">On this page, users can verify their phone numbers (by using text or voice) during sign-up or sign-in.</span></span> |
| <span data-ttu-id="d60e4-232">*API.selfasserted*</span><span class="sxs-lookup"><span data-stu-id="d60e4-232">*api.selfasserted*</span></span> | <span data-ttu-id="d60e4-233">**Sociala konto registreringssidan**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-233">**Social account sign-up page**.</span></span> <span data-ttu-id="d60e4-234">Den här sidan innehåller en registreringsformuläret som användare måste slutföra när de loggar med ett befintligt konto från en sociala identitetsleverantören, till exempel Facebook eller Google +.</span><span class="sxs-lookup"><span data-stu-id="d60e4-234">This page contains a sign-up form that users must complete when they sign up by using an existing account from a social identity provider such as Facebook or Google+.</span></span> <span data-ttu-id="d60e4-235">Den här sidan liknar föregående sociala konto registreringssidan, förutom fälten lösenord post.</span><span class="sxs-lookup"><span data-stu-id="d60e4-235">This page is similar to the preceding social account sign-up page, except for the password entry fields.</span></span> |
| <span data-ttu-id="d60e4-236">*API.selfasserted.profileupdate*</span><span class="sxs-lookup"><span data-stu-id="d60e4-236">*api.selfasserted.profileupdate*</span></span> | <span data-ttu-id="d60e4-237">**Uppdatera profilsida**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-237">**Profile update page**.</span></span> <span data-ttu-id="d60e4-238">Den här sidan innehåller ett formulär som användarna kan använda för att uppdatera sin profil.</span><span class="sxs-lookup"><span data-stu-id="d60e4-238">This page contains a form that users can use to update their profile.</span></span> <span data-ttu-id="d60e4-239">Den här sidan liknar sociala konto registreringssidan, förutom fälten lösenord post.</span><span class="sxs-lookup"><span data-stu-id="d60e4-239">This page is similar to the social account sign-up page, except for the password entry fields.</span></span> |
| <span data-ttu-id="d60e4-240">*API.signuporsignin*</span><span class="sxs-lookup"><span data-stu-id="d60e4-240">*api.signuporsignin*</span></span> | <span data-ttu-id="d60e4-241">**Enhetlig registrering eller inloggning sidan**.</span><span class="sxs-lookup"><span data-stu-id="d60e4-241">**Unified sign-up or sign-in page**.</span></span> <span data-ttu-id="d60e4-242">Den här sidan hanterar både registrering och inloggning av användare som kan använda enterprise identitetsleverantörer, sociala identitetsleverantörer, till exempel Facebook eller Google + eller lokala konton.</span><span class="sxs-lookup"><span data-stu-id="d60e4-242">This page handles both the sign-up and sign-in of users, who can use enterprise identity providers, social identity providers such as Facebook or Google+, or local accounts.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="d60e4-243">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d60e4-243">Next steps</span></span>

<span data-ttu-id="d60e4-244">Mer information om UI-element som kan anpassas finns [referenshandbok för anpassning av Användargränssnittet för inbyggda principer](active-directory-b2c-reference-ui-customization.md).</span><span class="sxs-lookup"><span data-stu-id="d60e4-244">For additional information about UI elements that can be customized, see [reference guide for UI customization for built-in policies](active-directory-b2c-reference-ui-customization.md).</span></span>
