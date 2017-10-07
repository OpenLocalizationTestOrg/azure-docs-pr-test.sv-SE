---
title: "Logic Apps B2B lista över fel och lösningar: Azure App Service | Microsoft Docs"
description: "Logic Apps B2B lista över fel och lösningar"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: 
ms.assetid: cf44af18-1fe5-41d5-9e06-cc57a968207c
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/02/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 0340e2979f1972ba631354e206c93969e55946e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-b2b-list-of-errors-and-solutions"></a><span data-ttu-id="dfb34-103">Logic Apps B2B lista över fel och lösningar</span><span class="sxs-lookup"><span data-stu-id="dfb34-103">Logic Apps B2B list of errors and solutions</span></span>  
<span data-ttu-id="dfb34-104">Den här artikeln hjälper dig att felsöka problem som kan inträffa i Logic Apps B2B-scenarier och ger förslag på åtgärder för att korrigera felen.</span><span class="sxs-lookup"><span data-stu-id="dfb34-104">This article helps you troubleshoot errors that might happen in Logic Apps B2B scenarios and suggests appropriate actions for correcting those errors.</span></span>


## <a name="agreement-resolution"></a><span data-ttu-id="dfb34-105">Avtalet upplösning</span><span class="sxs-lookup"><span data-stu-id="dfb34-105">Agreement Resolution</span></span>

### <a name="no-agreement-found"></a><span data-ttu-id="dfb34-106">* Inget avtal hittades</span><span class="sxs-lookup"><span data-stu-id="dfb34-106">*No agreement found</span></span> 

|   |   |  
|---|---|
| <span data-ttu-id="dfb34-107">Felbeskrivning</span><span class="sxs-lookup"><span data-stu-id="dfb34-107">Error description</span></span> | <span data-ttu-id="dfb34-108">Inget avtal med avtal upplösning parametrar hittades</span><span class="sxs-lookup"><span data-stu-id="dfb34-108">No agreement found with Agreement Resolution Parameters</span></span>|    
| <span data-ttu-id="dfb34-109">Användaråtgärd</span><span class="sxs-lookup"><span data-stu-id="dfb34-109">User action</span></span> | <span data-ttu-id="dfb34-110">hello avtal bör läggas toohello integrering konto med överenskomna business identiteter.</span><span class="sxs-lookup"><span data-stu-id="dfb34-110">hello agreement should be added toohello integration account with agreed business identities.</span></span></br> <span data-ttu-id="dfb34-111">hello business identiteter ska matcha toohello meddelande-ID: n</span><span class="sxs-lookup"><span data-stu-id="dfb34-111">hello business identities should match toohello input message ids</span></span>|  
|   |   |

### <a name="-no-agreement-found-with-identities"></a><span data-ttu-id="dfb34-112">* Inget avtal hittades med identiteter</span><span class="sxs-lookup"><span data-stu-id="dfb34-112">* No agreement found with identities</span></span>

|   |   | 
|---|---|
| <span data-ttu-id="dfb34-113">Felbeskrivning</span><span class="sxs-lookup"><span data-stu-id="dfb34-113">Error description</span></span> | <span data-ttu-id="dfb34-114">Inget avtal med identiteter hittades: 'AS2Identity':: 'Partner1' och 'AS2Identity':: 'Partner3'</span><span class="sxs-lookup"><span data-stu-id="dfb34-114">No agreement found with identities: 'AS2Identity'::'Partner1' and'AS2Identity'::'Partner3'</span></span>| 
| <span data-ttu-id="dfb34-115">Användaråtgärd</span><span class="sxs-lookup"><span data-stu-id="dfb34-115">User action</span></span> | <span data-ttu-id="dfb34-116">Ogiltig AS2-från eller AS2-tooconfigured för avtalet.</span><span class="sxs-lookup"><span data-stu-id="dfb34-116">Invalid AS2-From or AS2-tooconfigured for agreement.</span></span> </br> <span data-ttu-id="dfb34-117">Rätt AS2 meddelandet AS2-från eller AS2-tooheaders eller avtal toomatch AS2-ID: n i AS2-meddelandet huvuden med avtal konfigurationer</span><span class="sxs-lookup"><span data-stu-id="dfb34-117">Correct AS2 message AS2-From or AS2-tooheaders or agreement toomatch AS2 ids in AS2 message headers with agreement configurations</span></span> |
|   |   |     

## <a name="as2"></a><span data-ttu-id="dfb34-118">AS2</span><span class="sxs-lookup"><span data-stu-id="dfb34-118">AS2</span></span>

### <a name="-missing-as2-message-headers"></a><span data-ttu-id="dfb34-119">* Saknas AS2-meddelandehuvuden</span><span class="sxs-lookup"><span data-stu-id="dfb34-119">* Missing AS2 message headers</span></span>  

|   |   |  
|---|---|
| <span data-ttu-id="dfb34-120">Felbeskrivning</span><span class="sxs-lookup"><span data-stu-id="dfb34-120">Error description</span></span>| <span data-ttu-id="dfb34-121">Ogiltig AS2-huvuden.</span><span class="sxs-lookup"><span data-stu-id="dfb34-121">Invalid AS2 headers.</span></span> <span data-ttu-id="dfb34-122">En av ' AS2-att ' eller ' AS2-från-huvuden är tom</span><span class="sxs-lookup"><span data-stu-id="dfb34-122">One of 'AS2-To' or 'AS2-From' headers are empty</span></span>| 
| <span data-ttu-id="dfb34-123">Användaråtgärd</span><span class="sxs-lookup"><span data-stu-id="dfb34-123">User action</span></span> | <span data-ttu-id="dfb34-124">Ett AS2-meddelande togs emot som inte innehåller hello AS2-från eller AS2-tooor båda huvuden.</span><span class="sxs-lookup"><span data-stu-id="dfb34-124">An AS2 message was received that did not contain hello AS2-From or AS2-tooor both headers.</span></span> </br> <span data-ttu-id="dfb34-125">Kontrollera AS2-meddelande AS2-från och AS2-tooheaders och åtgärda dem baserat på konfigurationen av avtalet</span><span class="sxs-lookup"><span data-stu-id="dfb34-125">Check AS2 message AS2-From and AS2-tooheaders and correct them based on agreement configuration</span></span> |
|  |  | 


### <a name="-missing-as2-message-body-and-headers"></a><span data-ttu-id="dfb34-126">* Saknas AS2 meddelandetexten och rubriker</span><span class="sxs-lookup"><span data-stu-id="dfb34-126">* Missing AS2 message body and headers</span></span>    

|   |   |  
|---|---|
| <span data-ttu-id="dfb34-127">Felbeskrivning</span><span class="sxs-lookup"><span data-stu-id="dfb34-127">Error description</span></span>| <span data-ttu-id="dfb34-128">hello begär innehåll är null eller tomt</span><span class="sxs-lookup"><span data-stu-id="dfb34-128">hello request content is null or empty</span></span> | 
| <span data-ttu-id="dfb34-129">Användaråtgärd</span><span class="sxs-lookup"><span data-stu-id="dfb34-129">User action</span></span> | <span data-ttu-id="dfb34-130">Ett AS2-meddelande togs emot som inte innehåller hello meddelandetexten</span><span class="sxs-lookup"><span data-stu-id="dfb34-130">An AS2 message was received that did not contain hello message body</span></span> |
|  |  | 

### <a name="-as2-message-decryption-failure"></a><span data-ttu-id="dfb34-131">* Krypteringsfel för AS2-meddelande</span><span class="sxs-lookup"><span data-stu-id="dfb34-131">* AS2 message decryption failure</span></span>

|   |   | 
|---|---|
| <span data-ttu-id="dfb34-132">Felbeskrivning</span><span class="sxs-lookup"><span data-stu-id="dfb34-132">Error description</span></span> |  <span data-ttu-id="dfb34-133">[bearbetas/fel: dekryptering misslyckades]</span><span class="sxs-lookup"><span data-stu-id="dfb34-133">[processed/Error: decryption-failed]</span></span> | 
| <span data-ttu-id="dfb34-134">Användaråtgärd</span><span class="sxs-lookup"><span data-stu-id="dfb34-134">User action</span></span> | <span data-ttu-id="dfb34-135">Lägg till @base64ToBinary tooAS2Message innan du skickar toopartner</span><span class="sxs-lookup"><span data-stu-id="dfb34-135">Add @base64ToBinary tooAS2Message before sending toopartner</span></span> 
```java
            "HTTP": {
                "inputs": {
                    "body": "@base64ToBinary(body('Encode_to_AS2_message')?['AS2Message']?['Content'])",
                    "headers": "@body('Encode_to_AS2_message')?['AS2Message']?['OutboundHeaders']",
                    "method": "POST",
                    "uri": "xxxxx.xxx"
                },
                
``` 

### <a name="-mdn-decryption-failure"></a><span data-ttu-id="dfb34-136">* MDN krypteringsfel</span><span class="sxs-lookup"><span data-stu-id="dfb34-136">* MDN decryption failure</span></span>

|   |   | 
|---|---|
| <span data-ttu-id="dfb34-137">Felbeskrivning</span><span class="sxs-lookup"><span data-stu-id="dfb34-137">Error description</span></span> |  <span data-ttu-id="dfb34-138">[bearbetas/fel: dekryptering misslyckades]</span><span class="sxs-lookup"><span data-stu-id="dfb34-138">[processed/Error: decryption-failed]</span></span> | 
| <span data-ttu-id="dfb34-139">Användaråtgärd</span><span class="sxs-lookup"><span data-stu-id="dfb34-139">User action</span></span> | <span data-ttu-id="dfb34-140">Lägg till @base64ToBinary tooMDN innan du skickar toopartner</span><span class="sxs-lookup"><span data-stu-id="dfb34-140">Add @base64ToBinary tooMDN before sending toopartner</span></span> 
```java
            "Response": {
                "inputs": {
                    "body": "@base64ToBinary(body('Decode_AS2_message')?['OutgoingMDN']?['Content'])",
                    "headers": "@body('Decode_AS2_message')?['OutgoingMDN']?['OutboundHeaders']",
                    "statusCode": 200
                },
                
``` 

### <a name="-missing-signing-certificate"></a><span data-ttu-id="dfb34-141">* Saknas signeringscertifikat</span><span class="sxs-lookup"><span data-stu-id="dfb34-141">* Missing signing certificate</span></span>

|   |   |  
|---|---|
| <span data-ttu-id="dfb34-142">Felbeskrivning</span><span class="sxs-lookup"><span data-stu-id="dfb34-142">Error description</span></span>| <span data-ttu-id="dfb34-143">hello har signeringscertifikat inte konfigurerats för AS2 part.</span><span class="sxs-lookup"><span data-stu-id="dfb34-143">hello Signing Certificate has not been configured for AS2 party.</span></span> </br> <span data-ttu-id="dfb34-144">AS2-från: partner1 AS2-till: partner2</span><span class="sxs-lookup"><span data-stu-id="dfb34-144">AS2-From: partner1 AS2-To: partner2</span></span> | 
| <span data-ttu-id="dfb34-145">Användaråtgärd</span><span class="sxs-lookup"><span data-stu-id="dfb34-145">User action</span></span> | <span data-ttu-id="dfb34-146">Konfigurera inställningar för AS2-avtal med rätt certifikat för signatur</span><span class="sxs-lookup"><span data-stu-id="dfb34-146">Configure AS2 agreement settings with correct certificate for signature</span></span> |
|  |  | 

## <a name="x12-and-edifact"></a><span data-ttu-id="dfb34-147">X12 och EDIFACT</span><span class="sxs-lookup"><span data-stu-id="dfb34-147">X12 and EDIFACT</span></span>

### <a name="-leading-or-trailing-space-found"></a><span data-ttu-id="dfb34-148">* Inledande eller avslutande blanksteg hittades</span><span class="sxs-lookup"><span data-stu-id="dfb34-148">* Leading or trailing space found</span></span>    
    
|   |   | 
|---|---|
| <span data-ttu-id="dfb34-149">Felbeskrivning</span><span class="sxs-lookup"><span data-stu-id="dfb34-149">Error description</span></span> | <span data-ttu-id="dfb34-150">Ett fel uppstod vid parsning.</span><span class="sxs-lookup"><span data-stu-id="dfb34-150">Error encountered during parsing.</span></span> <span data-ttu-id="dfb34-151">Hej Edifact-transaktion med id ' 123456 'som finns i interchange (utan grupp) med id ' 987654', med avsändar-id 'Partner1', mottagaren id 'Partner2' pausas med följande fel: inledande avslutande avgränsare hittades</span><span class="sxs-lookup"><span data-stu-id="dfb34-151">hello Edifact transaction set with id '123456' contained in interchange (without group) with id '987654', with sender id 'Partner1', receiver id 'Partner2' is being suspended with following errors: Leading Trailing separator found</span></span> |
| <span data-ttu-id="dfb34-152">Användaråtgärd</span><span class="sxs-lookup"><span data-stu-id="dfb34-152">User action</span></span> | <span data-ttu-id="dfb34-153">hello avtal inställningar toobe konfigurerad tooallow inledande och avslutande blanksteg.</span><span class="sxs-lookup"><span data-stu-id="dfb34-153">hello agreement settings toobe configured tooallow leading and trailing space.</span></span> </br> <span data-ttu-id="dfb34-154">Redigera avtal inställningar tooallow inledande och avslutande blanksteg</span><span class="sxs-lookup"><span data-stu-id="dfb34-154">Edit agreement settings tooallow leading and trailing space</span></span> |
|   |   |

![ge utrymme](./media/logic-apps-enterprise-integration-b2b-list-errors-solutions/leadingandtrailing.png)

### <a name="-duplicate-check-has-enabled-in-hello-agreement"></a><span data-ttu-id="dfb34-156">* Kontrollen av dubblett har aktiverats i hello avtal</span><span class="sxs-lookup"><span data-stu-id="dfb34-156">* Duplicate check has enabled in hello agreement</span></span>

|   |   | 
|---|---| 
| <span data-ttu-id="dfb34-157">Felbeskrivning</span><span class="sxs-lookup"><span data-stu-id="dfb34-157">Error description</span></span> | <span data-ttu-id="dfb34-158">Duplicera kontroll tal</span><span class="sxs-lookup"><span data-stu-id="dfb34-158">Duplicate Control Number</span></span> |
| <span data-ttu-id="dfb34-159">Användaråtgärd</span><span class="sxs-lookup"><span data-stu-id="dfb34-159">User action</span></span> | <span data-ttu-id="dfb34-160">Det här felet indikerar emot hälsningsmeddelande har dubbla kontrollen siffror.</span><span class="sxs-lookup"><span data-stu-id="dfb34-160">This error indicates hello received message has duplicate control numbers.</span></span> </br> <span data-ttu-id="dfb34-161">Korrigera hello kontrollen antalet och skicka hello-meddelande</span><span class="sxs-lookup"><span data-stu-id="dfb34-161">Correct hello control number and resend hello message</span></span> |
|   |   |

### <a name="-missing-schema-in-hello-agreement"></a><span data-ttu-id="dfb34-162">* Saknas schemat i hello avtal</span><span class="sxs-lookup"><span data-stu-id="dfb34-162">* Missing schema in hello agreement</span></span>

|   |   | 
|---|---| 
| <span data-ttu-id="dfb34-163">Felbeskrivning</span><span class="sxs-lookup"><span data-stu-id="dfb34-163">Error description</span></span> | <span data-ttu-id="dfb34-164">Ett fel uppstod vid parsning.</span><span class="sxs-lookup"><span data-stu-id="dfb34-164">Error encountered during parsing.</span></span> <span data-ttu-id="dfb34-165">Hej X12 transaktion uppsättningen med id '564220001' finns i funktionella grupp med id '56422', i utbyte med id '000056422' med ' 12345678-avsändar-id', mottagaren id ' 87654321 ' pausas med följande fel ”hello-meddelande har ett okänt dokument ty PE och gick inte att tolka tooany hello befintliga scheman som konfigureras i hello avtalet ”</span><span class="sxs-lookup"><span data-stu-id="dfb34-165">hello X12 transaction set with id '564220001' contained in functional group with id '56422', in interchange with id '000056422', with sender id '12345678       ', receiver id '87654321       ' is being suspended with following errors "hello message has an unknown document type and did not resolve tooany of hello existing schemas configured in hello agreement"</span></span> |
| <span data-ttu-id="dfb34-166">Användaråtgärd</span><span class="sxs-lookup"><span data-stu-id="dfb34-166">User action</span></span> | <span data-ttu-id="dfb34-167">Konfigurera schemat i hello avtal inställningar</span><span class="sxs-lookup"><span data-stu-id="dfb34-167">Configure schema in hello agreement settings</span></span>  |
|   |   |

### <a name="-incorrect-schema-in-hello-agreement"></a><span data-ttu-id="dfb34-168">* Felaktigt schema i hello avtal</span><span class="sxs-lookup"><span data-stu-id="dfb34-168">* Incorrect schema in hello agreement</span></span>

|   |   | 
|---|---| 
| <span data-ttu-id="dfb34-169">Felbeskrivning</span><span class="sxs-lookup"><span data-stu-id="dfb34-169">Error description</span></span> | <span data-ttu-id="dfb34-170">hello-meddelande har en okänd dokumenttyp och gick inte att tolka tooany hello befintliga scheman som konfigureras i hello avtal.</span><span class="sxs-lookup"><span data-stu-id="dfb34-170">hello message has an unknown document type and did not resolve tooany of hello existing schemas configured in hello agreement.</span></span> |
| <span data-ttu-id="dfb34-171">Användaråtgärd</span><span class="sxs-lookup"><span data-stu-id="dfb34-171">User action</span></span> | <span data-ttu-id="dfb34-172">Konfigurera rätt schema i hello avtal inställningar</span><span class="sxs-lookup"><span data-stu-id="dfb34-172">Configure correct schema in hello agreement settings</span></span>  |
|   |   |

## <a name="flat-file"></a><span data-ttu-id="dfb34-173">Flat-fil</span><span class="sxs-lookup"><span data-stu-id="dfb34-173">Flat file</span></span>

### <a name="-input-message-with-no-body"></a><span data-ttu-id="dfb34-174">* Indatameddelande med ingen brödtext</span><span class="sxs-lookup"><span data-stu-id="dfb34-174">* Input message with no body</span></span>

|   |   | 
|---|---|
| <span data-ttu-id="dfb34-175">Felbeskrivning</span><span class="sxs-lookup"><span data-stu-id="dfb34-175">Error description</span></span> | <span data-ttu-id="dfb34-176">InvalidTemplate.</span><span class="sxs-lookup"><span data-stu-id="dfb34-176">InvalidTemplate.</span></span> <span data-ttu-id="dfb34-177">Det går inte tooprocess mallspråksuttryck i indata för åtgärden 'Flat_File_Decoding' på rad '1'- och '1902': ' krävs för egenskapen 'content-förväntar sig ett värde men erhöll null.</span><span class="sxs-lookup"><span data-stu-id="dfb34-177">Unable tooprocess template language expressions in action 'Flat_File_Decoding' inputs at line '1' and column '1902': 'Required property 'content' expects a value but got null.</span></span> <span data-ttu-id="dfb34-178">Sökvägen ''.'.</span><span class="sxs-lookup"><span data-stu-id="dfb34-178">Path ''.'.</span></span> |
| <span data-ttu-id="dfb34-179">Användaråtgärd</span><span class="sxs-lookup"><span data-stu-id="dfb34-179">User action</span></span> | <span data-ttu-id="dfb34-180">Det här felet indikerar hello inkommande meddelandet inte innehåller en text</span><span class="sxs-lookup"><span data-stu-id="dfb34-180">This error indicates hello input message does not contain a body</span></span> |
|   |   | 

## <a name="learn-more"></a><span data-ttu-id="dfb34-181">Läs mer</span><span class="sxs-lookup"><span data-stu-id="dfb34-181">Learn more</span></span>
[<span data-ttu-id="dfb34-182">Mer information om hello Enterprise-Integrationspaket</span><span class="sxs-lookup"><span data-stu-id="dfb34-182">Learn more about hello Enterprise Integration Pack</span></span>](logic-apps-enterprise-integration-overview.md)
