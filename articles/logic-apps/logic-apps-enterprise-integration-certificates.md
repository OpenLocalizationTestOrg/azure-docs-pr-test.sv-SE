---
title: aaaUsing certifikat med Enterprise-Integrationspaket | Microsoft Docs
description: "Lär dig hur toouse certifikat med hello Enterprise-Integrationspaket | Azure Logikappar"
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
ms.openlocfilehash: 7ba9f597a03a852a9ba05d2af08fe4bc2d98aef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="learn-about-certificates-and-enterprise-integration-pack"></a><span data-ttu-id="e0888-103">Läs om certifikat och Enterprise-integreringspaket</span><span class="sxs-lookup"><span data-stu-id="e0888-103">Learn about certificates and Enterprise Integration Pack</span></span>
## <a name="overview"></a><span data-ttu-id="e0888-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="e0888-104">Overview</span></span>
<span data-ttu-id="e0888-105">Enterprise integration använder certifikat toosecure B2B-kommunikation.</span><span class="sxs-lookup"><span data-stu-id="e0888-105">Enterprise integration uses certificates toosecure B2B communications.</span></span> <span data-ttu-id="e0888-106">Du kan använda två typer av certifikat i dina enterprise integration-appar:</span><span class="sxs-lookup"><span data-stu-id="e0888-106">You can use two types of certificates in your enterprise integration apps:</span></span>

* <span data-ttu-id="e0888-107">Offentliga certifikat, som måste köpas från en certifikatutfärdare (CA).</span><span class="sxs-lookup"><span data-stu-id="e0888-107">Public certificates, which must be purchased from a certification authority (CA).</span></span>
* <span data-ttu-id="e0888-108">Privata certifikat kan du utfärda själv.</span><span class="sxs-lookup"><span data-stu-id="e0888-108">Private certificates, which you can issue yourself.</span></span> <span data-ttu-id="e0888-109">Dessa certifikat kan ibland hänvisade tooas självsignerade certifikat.</span><span class="sxs-lookup"><span data-stu-id="e0888-109">These certificates are sometimes referred tooas self-signed certificates.</span></span>

## <a name="what-are-certificates"></a><span data-ttu-id="e0888-110">Vad är certifikat?</span><span class="sxs-lookup"><span data-stu-id="e0888-110">What are certificates?</span></span>
<span data-ttu-id="e0888-111">Certifikat är digitalt dokument som verifierar hello hello deltagare i elektronisk kommunikation och som även säker elektronisk kommunikation.</span><span class="sxs-lookup"><span data-stu-id="e0888-111">Certificates are digital documents that verify hello identity of hello participants in electronic communications and that also secure electronic communications.</span></span>

## <a name="why-use-certificates"></a><span data-ttu-id="e0888-112">Varför använda certifikat?</span><span class="sxs-lookup"><span data-stu-id="e0888-112">Why use certificates?</span></span>
<span data-ttu-id="e0888-113">Ibland måste B2B-kommunikation vara konfidentiella.</span><span class="sxs-lookup"><span data-stu-id="e0888-113">Sometimes B2B communications must be kept confidential.</span></span> <span data-ttu-id="e0888-114">Enterprise integration använder certifikat toosecure dessa meddelanden på två sätt:</span><span class="sxs-lookup"><span data-stu-id="e0888-114">Enterprise integration uses certificates toosecure these communications in two ways:</span></span>

* <span data-ttu-id="e0888-115">Genom att kryptera hello innehållet i meddelanden</span><span class="sxs-lookup"><span data-stu-id="e0888-115">By encrypting hello contents of messages</span></span>
* <span data-ttu-id="e0888-116">Genom att signera meddelanden</span><span class="sxs-lookup"><span data-stu-id="e0888-116">By digitally signing messages</span></span>  

## <a name="upload-a-public-certificate"></a><span data-ttu-id="e0888-117">Ladda upp ett offentligt certifikat</span><span class="sxs-lookup"><span data-stu-id="e0888-117">Upload a public certificate</span></span>

<span data-ttu-id="e0888-118">toouse en *certifikat med offentlig* i dina logic apps med B2B-funktioner, måste du först tooupload hello certifikatet till ditt konto för integrering.</span><span class="sxs-lookup"><span data-stu-id="e0888-118">toouse a *public certificate* in your logic apps with B2B capabilities, you first need tooupload hello certificate into your integration account.</span></span>  

<span data-ttu-id="e0888-119">När du har överfört ett certifikat är det tillgängliga toohelp som du skyddar dina B2B-meddelanden när du definierar deras egenskaper i hello [avtal](logic-apps-enterprise-integration-agreements.md) som du skapar.</span><span class="sxs-lookup"><span data-stu-id="e0888-119">After you upload a certificate, it's available toohelp you secure your B2B messages when you define their properties in hello [agreements](logic-apps-enterprise-integration-agreements.md) that you create.</span></span>  

<span data-ttu-id="e0888-120">Här följer hello detaljerade anvisningar för uppladdning av din offentliga certifikat till kontot integration när du loggar in toohello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="e0888-120">Here are hello detailed steps for uploading your public certificates into your integration account after you sign in toohello Azure portal:</span></span>

1. <span data-ttu-id="e0888-121">Välj **fler tjänster** och ange **integrering** i sökrutan för hello filter.</span><span class="sxs-lookup"><span data-stu-id="e0888-121">Select **More services** and enter **integration** in hello filter search box.</span></span> <span data-ttu-id="e0888-122">Välj **Integrationskonton** från hello resultatlistan</span><span class="sxs-lookup"><span data-stu-id="e0888-122">Select **Integration Accounts** from hello results list</span></span>     
<span data-ttu-id="e0888-123">![Välj Bläddra](media/logic-apps-enterprise-integration-certificates/overview-1.png)</span><span class="sxs-lookup"><span data-stu-id="e0888-123">![Select Browse](media/logic-apps-enterprise-integration-certificates/overview-1.png)</span></span>  
2. <span data-ttu-id="e0888-124">Välj hello integration konto toowhich du vill ha tooadd hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="e0888-124">Select hello integration account toowhich you want tooadd hello certificate.</span></span>  
![Välj hello integration konto toowhich du vill ha tooadd hello certifikat](media/logic-apps-enterprise-integration-certificates/overview-3.png)  
3. <span data-ttu-id="e0888-126">Välj hello **certifikat** panelen.</span><span class="sxs-lookup"><span data-stu-id="e0888-126">Select hello **Certificates** tile.</span></span>  
<span data-ttu-id="e0888-127">![Välj hello certifikat panelen](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span><span class="sxs-lookup"><span data-stu-id="e0888-127">![Select hello Certificates tile](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span></span>
4. <span data-ttu-id="e0888-128">I hello **certifikat** bladet som öppnas väljer hello **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e0888-128">In hello **Certificates** blade that opens, select hello **Add** button.</span></span>   
<span data-ttu-id="e0888-129">![Välj knappen hello](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span><span class="sxs-lookup"><span data-stu-id="e0888-129">![Select hello Add button](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span></span>
5. <span data-ttu-id="e0888-130">Ange en **namn** för certifikatet och välj sedan hello certifikattyp som **offentliga** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="e0888-130">Enter a **Name** for your certificate, and then select hello certificate type as **public** from hello dropdown.</span></span>  
6. <span data-ttu-id="e0888-131">Välj hello mappikon hello höger på hello **certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="e0888-131">Select hello folder icon on hello right side of hello **Certificate** text box.</span></span> <span data-ttu-id="e0888-132">När hello filväljaren öppnas, söka efter och väljer hello certifikatfil som du vill tooupload tooyour integrering konto.</span><span class="sxs-lookup"><span data-stu-id="e0888-132">When hello file picker opens, find and select hello certificate file that you want tooupload tooyour integration account.</span></span>
7. <span data-ttu-id="e0888-133">Välj hello certifikatet och välj sedan **OK** i hello filväljaren.</span><span class="sxs-lookup"><span data-stu-id="e0888-133">Select hello certificate, and then select **OK** in hello file picker.</span></span> <span data-ttu-id="e0888-134">Detta verifierar och laddar upp hello certifikat tooyour integrering konto.</span><span class="sxs-lookup"><span data-stu-id="e0888-134">This validates and uploads hello certificate tooyour integration account.</span></span>
8. <span data-ttu-id="e0888-135">Slutligen tillbaka på hello **Lägg till certifikat** bladet, Välj hello **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="e0888-135">Finally, back on hello **Add certificate** blade, select hello **OK** button.</span></span>  
<span data-ttu-id="e0888-136">![Välj hello OK-knapp](media/logic-apps-enterprise-integration-certificates/certificate-3.png)</span><span class="sxs-lookup"><span data-stu-id="e0888-136">![Select hello OK button](media/logic-apps-enterprise-integration-certificates/certificate-3.png)</span></span>  
9. <span data-ttu-id="e0888-137">Välj hello **certifikat** panelen.</span><span class="sxs-lookup"><span data-stu-id="e0888-137">Select hello **Certificates** tile.</span></span> <span data-ttu-id="e0888-138">Du bör se hello nyligen tillagda certifikatet.</span><span class="sxs-lookup"><span data-stu-id="e0888-138">You should see hello newly added certificate.</span></span>  
<span data-ttu-id="e0888-139">![Se hello nya certifikat](media/logic-apps-enterprise-integration-certificates/certificate-4.png)</span><span class="sxs-lookup"><span data-stu-id="e0888-139">![See hello new certificate](media/logic-apps-enterprise-integration-certificates/certificate-4.png)</span></span>  

## <a name="upload-a-private-certificate"></a><span data-ttu-id="e0888-140">Ladda upp ett privat certifikat</span><span class="sxs-lookup"><span data-stu-id="e0888-140">Upload a private certificate</span></span>

<span data-ttu-id="e0888-141">toouse en *privat certifikat* i dina logic apps med B2B-funktioner, kan du överföra ett privat certifikat tooyour integrering konto av tar hello följande</span><span class="sxs-lookup"><span data-stu-id="e0888-141">toouse a *private certificate* in your logic apps with B2B capabilities, You can upload a private certificate tooyour integration account by taking hello following steps</span></span>

1. <span data-ttu-id="e0888-142">[Ladda upp din privata nyckel tooKey valvet](../key-vault/key-vault-get-started.md "Lär dig mer om Key Vault") och ger en **nyckelnamn**</span><span class="sxs-lookup"><span data-stu-id="e0888-142">[Upload your private key tooKey Vault](../key-vault/key-vault-get-started.md "Learn about Key Vault") and provide a **Key Name**</span></span> 
   
   > [!TIP]
   > <span data-ttu-id="e0888-143">Du måste auktorisera Logic Apps tooperform åtgärder på Nyckelvalvet.</span><span class="sxs-lookup"><span data-stu-id="e0888-143">You must authorize Logic Apps tooperform operations on Key Vault.</span></span> <span data-ttu-id="e0888-144">Du kan bevilja åtkomst toohello Logic Apps tjänstens huvudnamn med hjälp av hello följande PowerShell-kommando:`Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`</span><span class="sxs-lookup"><span data-stu-id="e0888-144">You can grant access toohello Logic Apps service principal by using hello following PowerShell command: `Set-AzureRmKeyVaultAccessPolicy -VaultName 'TestcertKeyVault' -ServicePrincipalName '7cd684f4-8a78-49b0-91ec-6a35d38739ba' -PermissionsToKeys decrypt, sign, get, list`</span></span>  
   > 
   > 

<span data-ttu-id="e0888-145">Lägg till ett privat certifikat toointegration konto när du har gjort hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="e0888-145">After you've taken hello previous step, add a private certificate toointegration account.</span></span>

<span data-ttu-id="e0888-146">Följande är hello detaljerade anvisningar för uppladdning av dina privata certifikat till kontot integration när du loggar in toohello Azure-portalen:</span><span class="sxs-lookup"><span data-stu-id="e0888-146">Following are hello detailed steps for uploading your private certificates into your integration account after you sign in toohello Azure portal:</span></span>  
 
1. <span data-ttu-id="e0888-147">Välj hello integration konto toowhich tooadd hello certifikat och markera hello **certifikat** panelen.</span><span class="sxs-lookup"><span data-stu-id="e0888-147">Select hello integration account toowhich you want tooadd hello certificate and select hello **Certificates** tile.</span></span>  
<span data-ttu-id="e0888-148">![Välj hello certifikat panelen](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span><span class="sxs-lookup"><span data-stu-id="e0888-148">![Select hello Certificates tile](media/logic-apps-enterprise-integration-certificates/certificate-1.png)</span></span>  
2. <span data-ttu-id="e0888-149">I hello **certifikat** bladet som öppnas väljer hello **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e0888-149">In hello **Certificates** blade that opens, select hello **Add** button.</span></span>   
<span data-ttu-id="e0888-150">![Välj knappen hello](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span><span class="sxs-lookup"><span data-stu-id="e0888-150">![Select hello Add button](media/logic-apps-enterprise-integration-certificates/certificate-2.png)</span></span>
3. <span data-ttu-id="e0888-151">Ange en **namn** för certifikatet och välj hello certifikattyp som **privata** hello listrutan.</span><span class="sxs-lookup"><span data-stu-id="e0888-151">Enter a **Name** for your certificate, and select hello certificate type as **private** from hello dropdown.</span></span>   
4. <span data-ttu-id="e0888-152">Välj hello mappikon hello höger på hello **certifikat** textruta.</span><span class="sxs-lookup"><span data-stu-id="e0888-152">select hello folder icon on hello right side of hello **Certificate** text box.</span></span> <span data-ttu-id="e0888-153">Hitta hello motsvarande offentliga certifikat som du vill tooupload tooyour integrering konto när hello filväljaren öppnas.</span><span class="sxs-lookup"><span data-stu-id="e0888-153">When hello file picker opens, find hello corresponding public certificate that you want tooupload tooyour integration account.</span></span>   
   
   > [!Note]
   > <span data-ttu-id="e0888-154">När du lägger till ett privat certifikat är det viktigt tooadd motsvarande offentliga certifikat tooshow i [AS2-avtal](logic-apps-enterprise-integration-as2.md) ta emot och skicka inställningarna för signering och kryptering hälsningsmeddelande.</span><span class="sxs-lookup"><span data-stu-id="e0888-154">While adding a private certificate it is important tooadd corresponding public certificate tooshow in [AS2 agreement](logic-apps-enterprise-integration-as2.md) receive and send settings for signing and encrypting hello messages.</span></span>
   > 
   >   

5. <span data-ttu-id="e0888-155">Välj hello **resursgruppen**, **Nyckelvalvet**, **nyckelnamn** och välj hello **OK** knappen.</span><span class="sxs-lookup"><span data-stu-id="e0888-155">Select hello **Resource Group**, **Key Vault**, **Key Name** and select hello **OK** button.</span></span>  
<span data-ttu-id="e0888-156">![Lägg till certifikat](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)</span><span class="sxs-lookup"><span data-stu-id="e0888-156">![Add certificate](media/logic-apps-enterprise-integration-certificates/privatecertificate-1.png)</span></span>  
6. <span data-ttu-id="e0888-157">Välj hello **certifikat** panelen.</span><span class="sxs-lookup"><span data-stu-id="e0888-157">Select hello **Certificates** tile.</span></span> <span data-ttu-id="e0888-158">Du bör se hello nyligen tillagda certifikatet.</span><span class="sxs-lookup"><span data-stu-id="e0888-158">You should see hello newly added certificate.</span></span>
<span data-ttu-id="e0888-159">![Se hello nya certifikat](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)</span><span class="sxs-lookup"><span data-stu-id="e0888-159">![See hello new certificate](media/logic-apps-enterprise-integration-certificates/privatecertificate-2.png)</span></span>  



* [<span data-ttu-id="e0888-160">Skapa en B2B-avtal</span><span class="sxs-lookup"><span data-stu-id="e0888-160">Create a B2B agreement</span></span>](logic-apps-enterprise-integration-agreements.md)  
* [<span data-ttu-id="e0888-161">Mer information om Key Vault</span><span class="sxs-lookup"><span data-stu-id="e0888-161">Learn more about Key Vault</span></span>](../key-vault/key-vault-get-started.md "Lär dig mer om Key Vault")  

