---
title: "<span data-ttu-id=\"a0ccf-101\">Använder certifikat med Enterprise-Integrationspaket | Microsoft Docs</span><span class=\"sxs-lookup\"><span data-stu-id=\"a0ccf-101\">Using certificates with Enterprise Integration Pack | Microsoft Docs</span></span>"
description: "<span data-ttu-id=\"a0ccf-102\">Lär dig hur du använder certifikat med Enterprise-Integrationspaket | Azure Logikappar</span><span class=\"sxs-lookup\"><span data-stu-id=\"a0ccf-102\">Learn how to use certificates with the Enterprise Integration Pack | Azure Logic Apps</span></span>"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: cgronlun
ms.assetid: 4cbffd85-fe8d-4dde-aa5b-24108a7caa7d
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 0570aab14283b38f9efcc50636f0c0c1c8e3ed13
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="learn-about-certificates-and-enterprise-integration-pack"></a><span data-ttu-id="a0ccf-103">Läs om certifikat och Enterprise-integreringspaket</span><span class="sxs-lookup"><span data-stu-id="a0ccf-103">Learn about certificates and Enterprise Integration Pack</span></span>
## <a name="overview"></a><span data-ttu-id="a0ccf-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="a0ccf-104">Overview</span></span>
<span data-ttu-id="a0ccf-105">Enterprise integration använder certifikat för säker B2B-kommunikation.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-105">Enterprise integration uses certificates to secure B2B communications.</span></span> <span data-ttu-id="a0ccf-106">Du kan använda två typer av certifikat i dina enterprise integration-appar:</span><span class="sxs-lookup"><span data-stu-id="a0ccf-106">You can use two types of certificates in your enterprise integration apps:</span></span>

* <span data-ttu-id="a0ccf-107">Offentliga certifikat, som måste köpas från en certifikatutfärdare (CA).</span><span class="sxs-lookup"><span data-stu-id="a0ccf-107">Public certificates, which must be purchased from a certification authority (CA).</span></span>
* <span data-ttu-id="a0ccf-108">Privata certifikat kan du utfärda själv.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-108">Private certificates, which you can issue yourself.</span></span> <span data-ttu-id="a0ccf-109">Dessa certifikat kallas ibland självsignerade certifikat.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-109">These certificates are sometimes referred to as self-signed certificates.</span></span>

## <a name="what-are-certificates"></a><span data-ttu-id="a0ccf-110">Vad är certifikat?</span><span class="sxs-lookup"><span data-stu-id="a0ccf-110">What are certificates?</span></span>
<span data-ttu-id="a0ccf-111">Certifikat är digitalt dokument som verifieringen av identiteten för deltagarna i elektronisk kommunikation och som även säker elektronisk kommunikation.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-111">Certificates are digital documents that verify the identity of the participants in electronic communications and that also secure electronic communications.</span></span>

## <a name="why-use-certificates"></a><span data-ttu-id="a0ccf-112">Varför använda certifikat?</span><span class="sxs-lookup"><span data-stu-id="a0ccf-112">Why use certificates?</span></span>
<span data-ttu-id="a0ccf-113">Ibland måste B2B-kommunikation vara konfidentiella.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-113">Sometimes B2B communications must be kept confidential.</span></span> <span data-ttu-id="a0ccf-114">Enterprise integration använder certifikat för att skydda dessa meddelanden på två sätt:</span><span class="sxs-lookup"><span data-stu-id="a0ccf-114">Enterprise integration uses certificates to secure these communications in two ways:</span></span>

* <span data-ttu-id="a0ccf-115">Genom att kryptera innehållet i meddelanden</span><span class="sxs-lookup"><span data-stu-id="a0ccf-115">By encrypting the contents of messages</span></span>
* <span data-ttu-id="a0ccf-116">Genom att signera meddelanden</span><span class="sxs-lookup"><span data-stu-id="a0ccf-116">By digitally signing messages</span></span>  

## <a name="upload-a-public-certificate"></a><span data-ttu-id="a0ccf-117">Ladda upp ett offentligt certifikat</span><span class="sxs-lookup"><span data-stu-id="a0ccf-117">Upload a public certificate</span></span>

<span data-ttu-id="a0ccf-118">Att använda en *certifikat med offentlig* i dina logic apps med B2B-funktioner, måste du först överföra certifikatet till ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-118">To use a *public certificate* in your logic apps with B2B capabilities, you first need to upload the certificate into your integration account.</span></span>  

<span data-ttu-id="a0ccf-119">När du har överfört ett certifikat är tillgängligt för att skydda dina B2B-meddelanden när du definierar deras egenskaper i den [avtal](logic-apps-enterprise-integration-agreements.md) som du skapar.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-119">After you upload a certificate, it's available to help you secure your B2B messages when you define their properties in the [agreements](logic-apps-enterprise-integration-agreements.md) that you create.</span></span>  

<span data-ttu-id="a0ccf-120">Här följer detaljerade anvisningar för uppladdning av din offentliga certifikat till kontot integration när du loggar in på Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="a0ccf-120">Here are the detailed steps for uploading your public certificates into your integration account after you sign in to the Azure portal:</span></span>

1. <span data-ttu-id="a0ccf-121">Välj **fler tjänster** och ange **integrering** i sökrutan filtrera.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-121">Select **More services** and enter **integration** in the filter search box.</span></span> <span data-ttu-id="a0ccf-122">Välj **Integrationskonton** från resultatlistan över</span><span class="sxs-lookup"><span data-stu-id="a0ccf-122">Select **Integration Accounts** from the results list</span></span>     
<span data-ttu-id="a0ccf-123">![Välj Bläddra](media/logic-apps-enterprise-integration-certificates/overview-1.png)</span><span class="sxs-lookup"><span data-stu-id="a0ccf-123">![Select Browse](media/logic-apps-enterprise-integration-certificates/overview-1.png)</span></span>  
2. <span data-ttu-id="a0ccf-124">Välj integration kontot som du vill lägga till certifikatet.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-124">Select the integration account to which you want to add the certificate.</span></span>  
![Välj integration kontot som du vill lägga till certifikatet](media/logic-apps-enterprise-integration-certificates/overview-3.png)  
3. <span data-ttu-id="a0ccf-126">Välj den **certifikat** panelen.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-126">Select the **Certificates** tile.</span></span>  
<span data-ttu-id="a0ccf-127">![Välj panelen certifikat](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span><span class="sxs-lookup"><span data-stu-id="a0ccf-127">![Select the Certificates tile](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span></span>
4. <span data-ttu-id="a0ccf-128">I den **certifikat** bladet som öppnas väljer du den **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-128">In the **Certificates** blade that opens, select the **Add** button.</span></span>   
<span data-ttu-id="a0ccf-129">![Klicka på knappen Lägg till](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span><span class="sxs-lookup"><span data-stu-id="a0ccf-129">![Select the Add button](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span></span>
5. <span data-ttu-id="a0ccf-130">Ange en **namn** för certifikatet och välj sedan certifikatet skriver som **offentliga** i listrutan.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-130">Enter a **Name** for your certificate, and then select the certificate type as **public** from the dropdown.</span></span>  
6. <span data-ttu-id="a0ccf-131">Välj mappikonen på höger sida av den **certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-131">Select the folder icon on the right side of the **Certificate** text box.</span></span> <span data-ttu-id="a0ccf-132">När filväljaren öppnas, söka efter och väljer den certifikatfil som du vill överföra till ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-132">When the file picker opens, find and select the certificate file that you want to upload to your integration account.</span></span>
7. <span data-ttu-id="a0ccf-133">Välj certifikatet och välj sedan **OK** i filväljaren.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-133">Select the certificate, and then select **OK** in the file picker.</span></span> <span data-ttu-id="a0ccf-134">Detta verifierar och laddar upp certifikatet till ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-134">This validates and uploads the certificate to your integration account.</span></span>
8. <span data-ttu-id="a0ccf-135">Slutligen tillbaka på den **Lägg till certifikat** bladet väljer den **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-135">Finally, back on the **Add certificate** blade, select the **OK** button.</span></span>  
<span data-ttu-id="a0ccf-136">![Klicka på knappen OK](media/logic-apps-enterprise-integration-certificates/certificate-3.png)</span><span class="sxs-lookup"><span data-stu-id="a0ccf-136">![Select the OK button](media/logic-apps-enterprise-integration-certificates/certificate-3.png)</span></span>  
9. <span data-ttu-id="a0ccf-137">Välj den **certifikat** panelen.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-137">Select the **Certificates** tile.</span></span> <span data-ttu-id="a0ccf-138">Du bör se det nya certifikatet.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-138">You should see the newly added certificate.</span></span>  
<span data-ttu-id="a0ccf-139">![Finns det nya certifikatet](media/logic-apps-enterprise-integration-certificates/certificate-4.png)</span><span class="sxs-lookup"><span data-stu-id="a0ccf-139">![See the new certificate](media/logic-apps-enterprise-integration-certificates/certificate-4.png)</span></span>  

## <a name="upload-a-private-certificate"></a><span data-ttu-id="a0ccf-140">Ladda upp ett privat certifikat</span><span class="sxs-lookup"><span data-stu-id="a0ccf-140">Upload a private certificate</span></span>

<span data-ttu-id="a0ccf-141">Att använda en *privat certifikat* i dina logic apps med B2B-funktioner, du kan ladda upp ett privat certifikat till ditt konto integration genom att utföra följande åtgärder</span><span class="sxs-lookup"><span data-stu-id="a0ccf-141">To use a *private certificate* in your logic apps with B2B capabilities, You can upload a private certificate to your integration account by taking the following steps</span></span>

1. <span data-ttu-id="a0ccf-142">[Ladda upp din privata nyckel till Nyckelvalvet](../key-vault/key-vault-get-started.md "Lär dig mer om Key Vault") och ger en **nyckelnamn**</span><span class="sxs-lookup"><span data-stu-id="a0ccf-142">[Upload your private key to Key Vault](../key-vault/key-vault-get-started.md "Learn about Key Vault") and provide a **Key Name**</span></span> 
   
   > [!TIP]
   > <span data-ttu-id="a0ccf-143">Du måste auktorisera Logic Apps att utföra åtgärder på Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-143">You must authorize Logic Apps to perform operations on Key Vault.</span></span> <span data-ttu-id="a0ccf-144">Du kan ge åtkomst till Logic Apps tjänstens huvudnamn med hjälp av följande PowerShell-kommando:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`</span><span class="sxs-lookup"><span data-stu-id="a0ccf-144">You can grant access to the Logic Apps service principal by using the following PowerShell command: `Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`</span></span>  
   > 
   > 

<span data-ttu-id="a0ccf-145">När du har gjort det föregående steget, lägger du till ett privat certifikat i integration konto.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-145">After you've taken the previous step, add a private certificate to integration account.</span></span>

<span data-ttu-id="a0ccf-146">Nedan följer detaljerade anvisningar för uppladdning av dina privata certifikat till kontot integration när du loggar in på Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="a0ccf-146">Following are the detailed steps for uploading your private certificates into your integration account after you sign in to the Azure portal:</span></span>  
 
1. <span data-ttu-id="a0ccf-147">Välj integration kontot som du vill lägga till certifikatet och välj den **certifikat** panelen.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-147">Select the integration account to which you want to add the certificate and select the **Certificates** tile.</span></span>  
<span data-ttu-id="a0ccf-148">![Välj panelen certifikat](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span><span class="sxs-lookup"><span data-stu-id="a0ccf-148">![Select the Certificates tile](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span></span>  
2. <span data-ttu-id="a0ccf-149">I den **certifikat** bladet som öppnas väljer du den **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-149">In the **Certificates** blade that opens, select the **Add** button.</span></span>   
<span data-ttu-id="a0ccf-150">![Klicka på knappen Lägg till](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span><span class="sxs-lookup"><span data-stu-id="a0ccf-150">![Select the Add button](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span></span>
3. <span data-ttu-id="a0ccf-151">Ange en **namn** för certifikatet och välj certifikatet skriver som **privata** i listrutan.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-151">Enter a **Name** for your certificate, and select the certificate type as **private** from the dropdown.</span></span>   
4. <span data-ttu-id="a0ccf-152">Välj mappikonen på höger sida av den **certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-152">select the folder icon on the right side of the **Certificate** text box.</span></span> <span data-ttu-id="a0ccf-153">Hitta motsvarande offentliga certifikat som du vill överföra till ditt konto integration när filväljaren öppnas.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-153">When the file picker opens, find the corresponding public certificate that you want to upload to your integration account.</span></span>   
   
   > [!Note]
   > <span data-ttu-id="a0ccf-154">När du lägger till ett privat certifikat är det viktigt att lägga till motsvarande offentliga certifikat ska visas i [AS2-avtal](logic-apps-enterprise-integration-as2.md) ta emot och skicka inställningarna för signering och kryptering av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-154">While adding a private certificate it is important to add corresponding public certificate to show in [AS2 agreement](logic-apps-enterprise-integration-as2.md) receive and send settings for signing and encrypting the messages.</span></span>
   > 
   >   

5. <span data-ttu-id="a0ccf-155">Välj den **resursgruppen**, **Nyckelvalvet**, **nyckelnamn** och välj den **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-155">Select the **Resource Group**, **Key Vault**, **Key Name** and select the **OK** button.</span></span>  
<span data-ttu-id="a0ccf-156">![Lägg till certifikat](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)</span><span class="sxs-lookup"><span data-stu-id="a0ccf-156">![Add certificate](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)</span></span>  
6. <span data-ttu-id="a0ccf-157">Välj den **certifikat** panelen.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-157">Select the **Certificates** tile.</span></span> <span data-ttu-id="a0ccf-158">Du bör se det nya certifikatet.</span><span class="sxs-lookup"><span data-stu-id="a0ccf-158">You should see the newly added certificate.</span></span>
<span data-ttu-id="a0ccf-159">![Finns det nya certifikatet](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)</span><span class="sxs-lookup"><span data-stu-id="a0ccf-159">![See the new certificate](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)</span></span>  



* [<span data-ttu-id="a0ccf-160">Skapa en B2B-avtal</span><span class="sxs-lookup"><span data-stu-id="a0ccf-160">Create a B2B agreement</span></span>](logic-apps-enterprise-integration-agreements.md)  
* [<span data-ttu-id="a0ccf-161">Mer information om Key Vault</span><span class="sxs-lookup"><span data-stu-id="a0ccf-161">Learn more about Key Vault</span></span>](../key-vault/key-vault-get-started.md "Lär dig mer om Key Vault")  

