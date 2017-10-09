---
title: problem med aaaTroubleshoot Azure punkt-till-plats-anslutning | Microsoft Docs
description: "Lär dig hur tootroubleshoot punkt-till-plats-anslutningsproblem."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/23/2017
ms.author: genli
ms.openlocfilehash: 98d66074be62ad8c7153a903f69cb0d01f988cd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-azure-point-to-site-connection-problems"></a><span data-ttu-id="48c10-103">Felsökning: Anslutningsproblem med Azure punkt-till-plats</span><span class="sxs-lookup"><span data-stu-id="48c10-103">Troubleshooting: Azure point-to-site connection problems</span></span>

<span data-ttu-id="48c10-104">Den här artikeln innehåller vanliga anslutningsproblem som kan uppstå i punkt-till-plats.</span><span class="sxs-lookup"><span data-stu-id="48c10-104">This article lists common point-to-site connection problems that you might experience.</span></span> <span data-ttu-id="48c10-105">Här beskrivs också möjliga orsaker och lösningar på problemen.</span><span class="sxs-lookup"><span data-stu-id="48c10-105">It also discusses possible causes and solutions for these problems.</span></span>

## <a name="vpn-client-error-a-certificate-could-not-be-found"></a><span data-ttu-id="48c10-106">VPN-klientfel: Det gick inte att hitta ett certifikat</span><span class="sxs-lookup"><span data-stu-id="48c10-106">VPN client error: A certificate could not be found</span></span>

### <a name="symptom"></a><span data-ttu-id="48c10-107">Symtom</span><span class="sxs-lookup"><span data-stu-id="48c10-107">Symptom</span></span>

<span data-ttu-id="48c10-108">När du försöker tooconnect tooan virtuella Azure-nätverket med hjälp av hello VPN-klienten får du hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="48c10-108">When you try tooconnect tooan Azure virtual network by using hello VPN client, you receive hello following error message:</span></span>

<span data-ttu-id="48c10-109">**Det gick inte att hitta ett certifikat som kan användas med Extensible Authentication Protocol. (Fel 798)**</span><span class="sxs-lookup"><span data-stu-id="48c10-109">**A certificate could not be found that can be used with this Extensible Authentication Protocol. (Error 798)**</span></span>

### <a name="cause"></a><span data-ttu-id="48c10-110">Orsak</span><span class="sxs-lookup"><span data-stu-id="48c10-110">Cause</span></span>

<span data-ttu-id="48c10-111">Det här problemet uppstår om hello Klientcertifikatet saknas från **certifikat - aktuell User\Personal\Certificates**.</span><span class="sxs-lookup"><span data-stu-id="48c10-111">This problem occurs if hello client certificate is missing from **Certificates - Current User\Personal\Certificates**.</span></span>

### <a name="solution"></a><span data-ttu-id="48c10-112">Lösning</span><span class="sxs-lookup"><span data-stu-id="48c10-112">Solution</span></span>

<span data-ttu-id="48c10-113">Kontrollera att hello Klientcertifikatet har installerats i hello följande lagringsplats för hello certifikat (Certmgr.msc):</span><span class="sxs-lookup"><span data-stu-id="48c10-113">Make sure that hello client certificate is installed in hello following location of hello Certificates store (Certmgr.msc):</span></span>
 
<span data-ttu-id="48c10-114">**Certifikat - aktuell User\Personal\Certificates**</span><span class="sxs-lookup"><span data-stu-id="48c10-114">**Certificates - Current User\Personal\Certificates**</span></span>

<span data-ttu-id="48c10-115">Mer information om hur tooinstall hello klientcertifikat finns [generera och exportera certifikat för plats-till-plats-anslutningar](vpn-gateway-certificates-point-to-site.md).</span><span class="sxs-lookup"><span data-stu-id="48c10-115">For more information about how tooinstall hello client certificate, see [Generate and export certificates for point-to-site connections](vpn-gateway-certificates-point-to-site.md).</span></span>

> [!NOTE]
> <span data-ttu-id="48c10-116">När du importerar hello klientcertifikatet inte väljer hello **aktivera starkt skydd av den privata nyckeln** alternativet.</span><span class="sxs-lookup"><span data-stu-id="48c10-116">When you import hello client certificate, do not select hello **Enable strong private key protection** option.</span></span>

## <a name="vpn-client-error-hello-message-received-was-unexpected-or-badly-formatted"></a><span data-ttu-id="48c10-117">VPN-klientfel: hello-meddelande togs emot var oväntad eller felaktigt formaterat</span><span class="sxs-lookup"><span data-stu-id="48c10-117">VPN client error: hello message received was unexpected or badly formatted</span></span>

### <a name="symptom"></a><span data-ttu-id="48c10-118">Symtom</span><span class="sxs-lookup"><span data-stu-id="48c10-118">Symptom</span></span>

<span data-ttu-id="48c10-119">När du försöker tooconnect tooan virtuella Azure-nätverket med hjälp av hello VPN-klienten får du hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="48c10-119">When you try tooconnect tooan Azure virtual network by using hello VPN client, you receive hello following error message:</span></span>

<span data-ttu-id="48c10-120">**hello-meddelande togs emot var oväntat eller felaktigt formaterat. (Fel 0x80090326)**</span><span class="sxs-lookup"><span data-stu-id="48c10-120">**hello message received was unexpected or badly formatted. (Error 0x80090326)**</span></span>

### <a name="cause"></a><span data-ttu-id="48c10-121">Orsak</span><span class="sxs-lookup"><span data-stu-id="48c10-121">Cause</span></span>

<span data-ttu-id="48c10-122">Det här problemet uppstår om hello rot certifikatets offentliga nyckel inte har överförts till hello Azure VPN-gateway.</span><span class="sxs-lookup"><span data-stu-id="48c10-122">This problem occurs if hello root certificate public key is not uploaded into hello Azure VPN gateway.</span></span> <span data-ttu-id="48c10-123">Det kan också inträffa om hello nyckeln är skadad eller upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="48c10-123">It can also occur if hello key is corrupted or expired.</span></span>

### <a name="solution"></a><span data-ttu-id="48c10-124">Lösning</span><span class="sxs-lookup"><span data-stu-id="48c10-124">Solution</span></span>

<span data-ttu-id="48c10-125">tooresolve problemet hello statusen för hello root certificate i hello Azure portal toosee om den har återkallats.</span><span class="sxs-lookup"><span data-stu-id="48c10-125">tooresolve this problem, check hello status of hello root certificate in hello Azure portal toosee whether it was revoked.</span></span> <span data-ttu-id="48c10-126">Om det inte har återkallats försök toodelete hello-rotcertifikat och reupload.</span><span class="sxs-lookup"><span data-stu-id="48c10-126">If it is not revoked, try toodelete hello root certificate and reupload.</span></span> <span data-ttu-id="48c10-127">Mer information finns i [skapa certifikat](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).</span><span class="sxs-lookup"><span data-stu-id="48c10-127">For more information, see [Create certificates](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).</span></span>

## <a name="vpn-client-error-a-certificate-chain-processed-but-terminated"></a><span data-ttu-id="48c10-128">VPN-klientfel: en certifikatkedja bearbetas men avslutades</span><span class="sxs-lookup"><span data-stu-id="48c10-128">VPN client error: A certificate chain processed but terminated</span></span> 

### <a name="symptom"></a><span data-ttu-id="48c10-129">Symtom</span><span class="sxs-lookup"><span data-stu-id="48c10-129">Symptom</span></span> 

<span data-ttu-id="48c10-130">När du försöker tooconnect tooan virtuella Azure-nätverket med hjälp av hello VPN-klienten får du hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="48c10-130">When you try tooconnect tooan Azure virtual network by using hello VPN client, you receive hello following error message:</span></span>

<span data-ttu-id="48c10-131">**En certifikatkedja bearbetas men avslutades med ett rotcertifikat som inte är betrodd av hello förtroende.**</span><span class="sxs-lookup"><span data-stu-id="48c10-131">**A certificate chain processed but terminated in a root certificate which is not trusted by hello trust provider.**</span></span>

### <a name="solution"></a><span data-ttu-id="48c10-132">Lösning</span><span class="sxs-lookup"><span data-stu-id="48c10-132">Solution</span></span>

1. <span data-ttu-id="48c10-133">Kontrollera att hello följande certifikat i hello rätt plats:</span><span class="sxs-lookup"><span data-stu-id="48c10-133">Make sure that hello following certificates are in hello correct location:</span></span>

    | <span data-ttu-id="48c10-134">Certifikat</span><span class="sxs-lookup"><span data-stu-id="48c10-134">Certificate</span></span> | <span data-ttu-id="48c10-135">Plats</span><span class="sxs-lookup"><span data-stu-id="48c10-135">Location</span></span> |
    | ------------- | ------------- |
    | <span data-ttu-id="48c10-136">AzureClient.pfx</span><span class="sxs-lookup"><span data-stu-id="48c10-136">AzureClient.pfx</span></span>  | <span data-ttu-id="48c10-137">Aktuella User\Personal\Certificates</span><span class="sxs-lookup"><span data-stu-id="48c10-137">Current User\Personal\Certificates</span></span> |
    | <span data-ttu-id="48c10-138">Azuregateway -*GUID*. cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="48c10-138">Azuregateway-*GUID*.cloudapp.net</span></span>  | <span data-ttu-id="48c10-139">Aktuella User\Trusted rotcertifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="48c10-139">Current User\Trusted Root Certification Authorities</span></span>|
    | <span data-ttu-id="48c10-140">AzureGateway -*GUID*. cloudapp.net AzureRoot.cer</span><span class="sxs-lookup"><span data-stu-id="48c10-140">AzureGateway-*GUID*.cloudapp.net, AzureRoot.cer</span></span>    | <span data-ttu-id="48c10-141">Lokal dator\Betrodda certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="48c10-141">Local Computer\Trusted Root Certification Authorities</span></span>|

2. <span data-ttu-id="48c10-142">Om hello certifikat finns redan i hello plats, försök toodelete hello certifikat och installera om dem.</span><span class="sxs-lookup"><span data-stu-id="48c10-142">If hello certificates are already in hello location, try toodelete hello certificates and reinstall them.</span></span> <span data-ttu-id="48c10-143">Hej  **azuregateway -*GUID*. cloudapp.net** certifikatet finns i hello VPN-klienten med konfigurationspaket som du hämtade från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="48c10-143">hello **azuregateway-*GUID*.cloudapp.net** certificate is in hello VPN client configuration package that you downloaded from hello Azure portal.</span></span> <span data-ttu-id="48c10-144">Du kan använda archivers tooextract hello-filer från hello-paketet.</span><span class="sxs-lookup"><span data-stu-id="48c10-144">You can use file archivers tooextract hello files from hello package.</span></span>

## <a name="file-download-error-target-uri-is-not-specified"></a><span data-ttu-id="48c10-145">Fel vid hämtning av filen: mål-URI har inte angetts</span><span class="sxs-lookup"><span data-stu-id="48c10-145">File download error: Target URI is not specified</span></span>

### <a name="symptom"></a><span data-ttu-id="48c10-146">Symtom</span><span class="sxs-lookup"><span data-stu-id="48c10-146">Symptom</span></span>

<span data-ttu-id="48c10-147">Hello följande felmeddelande visas:</span><span class="sxs-lookup"><span data-stu-id="48c10-147">You receive hello following error message:</span></span>

<span data-ttu-id="48c10-148">**Fel vid hämtning av filen. Mål-URI har inte angetts.**</span><span class="sxs-lookup"><span data-stu-id="48c10-148">**File download error. Target URI is not specified.**</span></span>

### <a name="cause"></a><span data-ttu-id="48c10-149">Orsak</span><span class="sxs-lookup"><span data-stu-id="48c10-149">Cause</span></span> 

<span data-ttu-id="48c10-150">Det här problemet uppstår på grund av en felaktig gateway-typen.</span><span class="sxs-lookup"><span data-stu-id="48c10-150">This problem occurs because of an incorrect gateway type.</span></span> 

### <a name="solution"></a><span data-ttu-id="48c10-151">Lösning</span><span class="sxs-lookup"><span data-stu-id="48c10-151">Solution</span></span>

<span data-ttu-id="48c10-152">hello VPN gateway-typ måste vara **VPN**, och hello VPN-typ måste vara **RouteBased**.</span><span class="sxs-lookup"><span data-stu-id="48c10-152">hello VPN gateway type must be **VPN**, and hello VPN type must be **RouteBased**.</span></span>

## <a name="vpn-client-error-azure-vpn-custom-script-failed"></a><span data-ttu-id="48c10-153">VPN-klientfel: Azure VPN-anpassade skript misslyckades</span><span class="sxs-lookup"><span data-stu-id="48c10-153">VPN client error: Azure VPN custom script failed</span></span> 

### <a name="symptom"></a><span data-ttu-id="48c10-154">Symtom</span><span class="sxs-lookup"><span data-stu-id="48c10-154">Symptom</span></span>

<span data-ttu-id="48c10-155">När du försöker tooconnect tooan virtuella Azure-nätverket med hjälp av hello VPN-klienten får du hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="48c10-155">When you try tooconnect tooan Azure virtual network by using hello VPN client, you receive hello following error message:</span></span>

<span data-ttu-id="48c10-156">**Anpassat skript (tooupdate din routning tabell) misslyckades. (Fel 8007026f)**</span><span class="sxs-lookup"><span data-stu-id="48c10-156">**Custom script (tooupdate your routing table) failed. (Error 8007026f)**</span></span>

### <a name="cause"></a><span data-ttu-id="48c10-157">Orsak</span><span class="sxs-lookup"><span data-stu-id="48c10-157">Cause</span></span>

<span data-ttu-id="48c10-158">Det här problemet kan inträffa om du försöker tooopen hello plats-till-punkt VPN-anslutning med hjälp av en genväg.</span><span class="sxs-lookup"><span data-stu-id="48c10-158">This problem might occur if you are trying tooopen hello site-to-point VPN connection by using a shortcut.</span></span>

### <a name="solution"></a><span data-ttu-id="48c10-159">Lösning</span><span class="sxs-lookup"><span data-stu-id="48c10-159">Solution</span></span> 

<span data-ttu-id="48c10-160">Öppna hello VPN-paketet direkt i stället för att öppna den från hello genväg.</span><span class="sxs-lookup"><span data-stu-id="48c10-160">Open hello VPN package directly instead of opening it from hello shortcut.</span></span>

## <a name="cannot-install-hello-vpn-client"></a><span data-ttu-id="48c10-161">Det går inte att installera hello VPN-klienten</span><span class="sxs-lookup"><span data-stu-id="48c10-161">Cannot install hello VPN client</span></span>

### <a name="cause"></a><span data-ttu-id="48c10-162">Orsak</span><span class="sxs-lookup"><span data-stu-id="48c10-162">Cause</span></span> 

<span data-ttu-id="48c10-163">Ett ytterligare certifikat är obligatoriska tootrust hello VPN-gateway för det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="48c10-163">An additional certificate is required tootrust hello VPN gateway for your virtual network.</span></span> <span data-ttu-id="48c10-164">hello certifikatet ingår i hello VPN-klienten med konfigurationspaket som genereras från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="48c10-164">hello certificate is included in hello VPN client configuration package that is generated from hello Azure portal.</span></span>

### <a name="solution"></a><span data-ttu-id="48c10-165">Lösning</span><span class="sxs-lookup"><span data-stu-id="48c10-165">Solution</span></span>

<span data-ttu-id="48c10-166">Extrahera hello VPN-klientpaketet konfiguration och hitta hello .cer-fil.</span><span class="sxs-lookup"><span data-stu-id="48c10-166">Extract hello VPN client configuration package, and find hello .cer file.</span></span> <span data-ttu-id="48c10-167">tooinstall hello certifikat, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="48c10-167">tooinstall hello certificate, follow these steps:</span></span>

1. <span data-ttu-id="48c10-168">Öppna mmc.exe.</span><span class="sxs-lookup"><span data-stu-id="48c10-168">Open mmc.exe.</span></span>
2. <span data-ttu-id="48c10-169">Lägg till hello **certifikat** snapin-modulen.</span><span class="sxs-lookup"><span data-stu-id="48c10-169">Add hello **Certificates** snap-in.</span></span>
3. <span data-ttu-id="48c10-170">Välj hello **datorn** konto för hello lokal dator.</span><span class="sxs-lookup"><span data-stu-id="48c10-170">Select hello **Computer** account for hello local computer.</span></span>
4. <span data-ttu-id="48c10-171">Högerklicka på hello **betrodda rotcertifikatutfärdare** nod.</span><span class="sxs-lookup"><span data-stu-id="48c10-171">Right-click hello **Trusted Root Certification Authorities** node.</span></span> <span data-ttu-id="48c10-172">Klicka på **All aktivitet** > **Import**, och bläddra toohello .cer-fil som du har extraherat från hello VPN-klientpaketet konfiguration.</span><span class="sxs-lookup"><span data-stu-id="48c10-172">Click **All-Task** > **Import**, and browse toohello .cer file you extracted from hello VPN client configuration package.</span></span>
5. <span data-ttu-id="48c10-173">Starta om datorn hello.</span><span class="sxs-lookup"><span data-stu-id="48c10-173">Restart hello computer.</span></span> 
6. <span data-ttu-id="48c10-174">Försök tooinstall hello VPN-klienten.</span><span class="sxs-lookup"><span data-stu-id="48c10-174">Try tooinstall hello VPN client.</span></span>

## <a name="azure-portal-error-failed-toosave-hello-vpn-gateway-and-hello-data-is-invalid"></a><span data-ttu-id="48c10-175">Azure portal fel: kunde inte toosave hello VPN-gateway och hello data är ogiltiga</span><span class="sxs-lookup"><span data-stu-id="48c10-175">Azure portal error: Failed toosave hello VPN gateway, and hello data is invalid</span></span>

### <a name="symptom"></a><span data-ttu-id="48c10-176">Symtom</span><span class="sxs-lookup"><span data-stu-id="48c10-176">Symptom</span></span>

<span data-ttu-id="48c10-177">När du försöker toosave hello ändringar för hello VPN-gateway i hello Azure-portalen får du hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="48c10-177">When you try toosave hello changes for hello VPN gateway in hello Azure portal, you receive hello following error message:</span></span>

<span data-ttu-id="48c10-178">**Misslyckade toosave virtuell nätverksgateway &lt;* gatewaynamnet*&gt;.</span><span class="sxs-lookup"><span data-stu-id="48c10-178">**Failed toosave virtual network gateway &lt;*gateway name*&gt;.</span></span> <span data-ttu-id="48c10-179">Data för certifikatet &lt; *certifikat ID* &gt; är invalid.* *</span><span class="sxs-lookup"><span data-stu-id="48c10-179">Data for certificate &lt;*certificate ID*&gt; is invalid.**</span></span>

### <a name="cause"></a><span data-ttu-id="48c10-180">Orsak</span><span class="sxs-lookup"><span data-stu-id="48c10-180">Cause</span></span> 

<span data-ttu-id="48c10-181">Det här problemet kan inträffa om hello rot certifikatets offentliga nyckel som du överfört innehåller ett ogiltigt tecken, till exempel ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="48c10-181">This problem might occur if hello root certificate public key that you uploaded contains an invalid character, such as a space.</span></span>

### <a name="solution"></a><span data-ttu-id="48c10-182">Lösning</span><span class="sxs-lookup"><span data-stu-id="48c10-182">Solution</span></span>

<span data-ttu-id="48c10-183">Kontrollera att hello data i hello certifikatet inte innehåller ogiltiga tecken, till exempel radbrytningar (vagnreturer).</span><span class="sxs-lookup"><span data-stu-id="48c10-183">Make sure that hello data in hello certificate does not contain invalid characters, such as line breaks (carriage returns).</span></span> <span data-ttu-id="48c10-184">hello hela värdet bör vara en lång rad.</span><span class="sxs-lookup"><span data-stu-id="48c10-184">hello entire value should be one long line.</span></span> <span data-ttu-id="48c10-185">hello efter texten är ett exempel på hello certifikat:</span><span class="sxs-lookup"><span data-stu-id="48c10-185">hello following text is a sample of hello certificate:</span></span>

    -----BEGIN CERTIFICATE-----
    MIIC5zCCAc+gAwIBAgIQFSwsLuUrCIdHwI3hzJbdBjANBgkqhkiG9w0BAQsFADAW
    MRQwEgYDVQQDDAtQMlNSb290Q2VydDAeFw0xNzA2MTUwMjU4NDZaFw0xODA2MTUw
    MzE4NDZaMBYxFDASBgNVBAMMC1AyU1Jvb3RDZXJ0MIIBIjANBgkqhkiG9w0BAQEF
    AAOCAQ8AMIIBCgKCAQEAz8QUCWCxxxTrxF5yc5uUpL/bzwC5zZ804ltB1NpPa/PI
    sa5uwLw/YFb8XG/JCWxUJpUzS/kHUKFluqkY80U+fAmRmTEMq5wcaMhp3wRfeq+1
    G9OPBNTyqpnHe+i54QAnj1DjsHXXNL4AL1N8/TSzYTm7dkiq+EAIyRRMrZlYwije
    407ChxIp0stB84MtMShhyoSm2hgl+3zfwuaGXoJQwWiXh715kMHVTSj9zFechYd7
    5OLltoRRDyyxsf0qweTFKIgFj13Hn/bq/UJG3AcyQNvlCv1HwQnXO+hckVBB29wE
    sF8QSYk2MMGimPDYYt4ZM5tmYLxxxvGmrGhc+HWXzMeQIDAQABozEwLzAOBgNVHQ8B
    Af8EBAMCAgQwHQYDVR0OBBYEFBE9zZWhQftVLBQNATC/LHLvMb0OMA0GCSqGSIb3
    DQEBCwUAA4IBAQB7k0ySFUQu72sfj3BdNxrXSyOT4L2rADLhxxxiK0U6gHUF6eWz
    /0h6y4mNkg3NgLT3j/WclqzHXZruhWAXSF+VbAGkwcKA99xGWOcUJ+vKVYL/kDja
    gaZrxHlhTYVVmwn4F7DWhteFqhzZ89/W9Mv6p180AimF96qDU8Ez8t860HQaFkU6
    2Nw9ZMsGkvLePZZi78yVBDCWMogBMhrRVXG/xQkBajgvL5syLwFBo2kWGdC+wyWY
    U/Z+EK9UuHnn3Hkq/vXEzRVsYuaxchta0X2UNRzRq+o706l+iyLTpe6fnvW6ilOi
    e8Jcej7mzunzyjz4chN0/WVF94MtxbUkLkqP
    -----END CERTIFICATE-----

## <a name="azure-portal-error-failed-toosave-hello-vpn-gateway-and-hello-resource-name-is-invalid"></a><span data-ttu-id="48c10-186">Azure portal fel: kunde inte toosave hello VPN-gateway och hello resursnamnet är ogiltigt</span><span class="sxs-lookup"><span data-stu-id="48c10-186">Azure portal error: Failed toosave hello VPN gateway, and hello resource name is invalid</span></span>

### <a name="symptom"></a><span data-ttu-id="48c10-187">Symtom</span><span class="sxs-lookup"><span data-stu-id="48c10-187">Symptom</span></span>

<span data-ttu-id="48c10-188">När du försöker toosave hello ändringar för hello VPN-gateway i hello Azure-portalen får du hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="48c10-188">When you try toosave hello changes for hello VPN gateway in hello Azure portal, you receive hello following error message:</span></span> 

<span data-ttu-id="48c10-189">**Misslyckade toosave virtuell nätverksgateway &lt;* gatewaynamnet*&gt;.</span><span class="sxs-lookup"><span data-stu-id="48c10-189">**Failed toosave virtual network gateway &lt;*gateway name*&gt;.</span></span> <span data-ttu-id="48c10-190">Resursnamnet &lt; *certifikatnamn försök tooupload* &gt; är ogiltig **.</span><span class="sxs-lookup"><span data-stu-id="48c10-190">Resource name &lt;*certificate name you try tooupload*&gt; is invalid**.</span></span>

### <a name="cause"></a><span data-ttu-id="48c10-191">Orsak</span><span class="sxs-lookup"><span data-stu-id="48c10-191">Cause</span></span>

<span data-ttu-id="48c10-192">Det här problemet uppstår eftersom hello namnet på hello certifikat innehåller ett ogiltigt tecken, till exempel ett blanksteg.</span><span class="sxs-lookup"><span data-stu-id="48c10-192">This problem occurs because hello name of hello certificate contains an invalid character, such as a space.</span></span> 

## <a name="azure-portal-error-vpn-package-file-download-error-503"></a><span data-ttu-id="48c10-193">Azure portal fel: VPN-paketet filen fel vid hämtning av 503</span><span class="sxs-lookup"><span data-stu-id="48c10-193">Azure portal error: VPN package file download error 503</span></span>

### <a name="symptom"></a><span data-ttu-id="48c10-194">Symtom</span><span class="sxs-lookup"><span data-stu-id="48c10-194">Symptom</span></span>

<span data-ttu-id="48c10-195">När konfigurationen för toodownload hello VPN-klientpaketet felmeddelandet hello följande felmeddelande:</span><span class="sxs-lookup"><span data-stu-id="48c10-195">When you try toodownload hello VPN client configuration package, you receive hello following error message:</span></span>

<span data-ttu-id="48c10-196">**Det gick inte toodownload hello-filen. Information om felet: fel 503. hello-servern är upptagen.**</span><span class="sxs-lookup"><span data-stu-id="48c10-196">**Failed toodownload hello file. Error details: error 503. hello server is busy.**</span></span>
 
### <a name="solution"></a><span data-ttu-id="48c10-197">Lösning</span><span class="sxs-lookup"><span data-stu-id="48c10-197">Solution</span></span>

<span data-ttu-id="48c10-198">Det här felet kan bero på tillfälliga nätverksproblem.</span><span class="sxs-lookup"><span data-stu-id="48c10-198">This error can be caused by a temporary network problem.</span></span> <span data-ttu-id="48c10-199">Försök toodownload hello VPN-paketet igen efter några minuter.</span><span class="sxs-lookup"><span data-stu-id="48c10-199">Try toodownload hello VPN package again after a few minutes.</span></span>

## <a name="azure-vpn-gateway-upgrade-all-p2s-clients-are-unable-tooconnect"></a><span data-ttu-id="48c10-200">Uppgraderingen av Azure VPN-Gateway: alla P2S-klienter är tooconnect</span><span class="sxs-lookup"><span data-stu-id="48c10-200">Azure VPN Gateway upgrade: All P2S clients are unable tooconnect</span></span>

### <a name="cause"></a><span data-ttu-id="48c10-201">Orsak</span><span class="sxs-lookup"><span data-stu-id="48c10-201">Cause</span></span>

<span data-ttu-id="48c10-202">Om hello certifikat är mer än 50 procent via dess livslängd hello certifikatet förnyas.</span><span class="sxs-lookup"><span data-stu-id="48c10-202">If hello certificate is more than 50 percent through its lifetime, hello certificate is rolled over.</span></span>

### <a name="solution"></a><span data-ttu-id="48c10-203">Lösning</span><span class="sxs-lookup"><span data-stu-id="48c10-203">Solution</span></span>

<span data-ttu-id="48c10-204">tooresolve problemet, skapa och distribuera nya certifikat toohello VPN-klienter.</span><span class="sxs-lookup"><span data-stu-id="48c10-204">tooresolve this problem, create and redistribute new certificates toohello VPN clients.</span></span> 

## <a name="too-many-vpn-clients-connected-at-once"></a><span data-ttu-id="48c10-205">För många VPN-klienter anslutna samtidigt</span><span class="sxs-lookup"><span data-stu-id="48c10-205">Too many VPN clients connected at once</span></span>

<span data-ttu-id="48c10-206">För varje VPN-gateway är hello maximalt antal tillåtna anslutningar 128.</span><span class="sxs-lookup"><span data-stu-id="48c10-206">For each VPN gateway, hello maximum number of allowable connections is 128.</span></span> <span data-ttu-id="48c10-207">Du kan se hello Totalt antal anslutna klienter i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="48c10-207">You can see hello total number of connected clients in hello Azure portal.</span></span>

## <a name="point-to-site-vpn-incorrectly-adds-a-route-for-100008-toohello-route-table"></a><span data-ttu-id="48c10-208">Punkt-till-plats VPN felaktigt lägger till en väg för 10.0.0.0/8 toohello routningstabellen</span><span class="sxs-lookup"><span data-stu-id="48c10-208">Point-to-site VPN incorrectly adds a route for 10.0.0.0/8 toohello route table</span></span>

### <a name="symptom"></a><span data-ttu-id="48c10-209">Symtom</span><span class="sxs-lookup"><span data-stu-id="48c10-209">Symptom</span></span>

<span data-ttu-id="48c10-210">När du ringer hello VPN-anslutning på hello punkt-till-plats klienten hello VPN-klienten ska lägga till en väg mot hello virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="48c10-210">When you dial hello VPN connection on hello point-to-site client, hello VPN client should add a route toward hello Azure virtual network.</span></span> <span data-ttu-id="48c10-211">hello IP helper-tjänsten bör du lägga till en väg för undernätet hello hello VPN-klienter.</span><span class="sxs-lookup"><span data-stu-id="48c10-211">hello IP helper service should add a route for hello subnet of hello VPN clients.</span></span> 

<span data-ttu-id="48c10-212">hello VPN-klienten intervallet tillhör tooa mindre undernätet 10.0.0.0/8, till exempel 10.0.12.0/24.</span><span class="sxs-lookup"><span data-stu-id="48c10-212">hello VPN client range belongs tooa smaller subnet of 10.0.0.0/8, such as 10.0.12.0/24.</span></span> <span data-ttu-id="48c10-213">I stället för en väg för 10.0.12.0/24 läggs en väg för 10.0.0.0/8 som har högre prioritet.</span><span class="sxs-lookup"><span data-stu-id="48c10-213">Instead of a route for 10.0.12.0/24, a route for 10.0.0.0/8 is added that has higher priority.</span></span> 

<span data-ttu-id="48c10-214">Felaktig vägen bryts anslutningen till andra lokala nätverk som tillhör tooanother undernät inom intervallet för hello 10.0.0.0/8, till exempel 10.50.0.0/24, som inte har en specifik väg som har definierats.</span><span class="sxs-lookup"><span data-stu-id="48c10-214">This incorrect route breaks connectivity with other on-premises networks that might belong tooanother subnet within hello 10.0.0.0/8 range, such as 10.50.0.0/24, that don't have a specific route defined.</span></span> 

### <a name="cause"></a><span data-ttu-id="48c10-215">Orsak</span><span class="sxs-lookup"><span data-stu-id="48c10-215">Cause</span></span>

<span data-ttu-id="48c10-216">Det här beteendet är avsiktligt för Windows-klienter.</span><span class="sxs-lookup"><span data-stu-id="48c10-216">This behavior is by design for Windows clients.</span></span> <span data-ttu-id="48c10-217">När hello klienten använder hello PPP IPCP-protokollet, hämtar hello IP-adress för hello tunnelgränssnitt från hello server (hello VPN-gateway i det här fallet).</span><span class="sxs-lookup"><span data-stu-id="48c10-217">When hello client uses hello PPP IPCP protocol, it obtains hello IP address for hello tunnel interface from hello server (hello VPN gateway in this case).</span></span> <span data-ttu-id="48c10-218">Men på grund av en begränsning i hello protokollet hello klienten har inte hello nätmask.</span><span class="sxs-lookup"><span data-stu-id="48c10-218">However, because of a limitation in hello protocol, hello client does not have hello subnet mask.</span></span> <span data-ttu-id="48c10-219">Eftersom det inte finns några andra sätt tooget, hello klienten försöker programmet tooguess hello nätmask som baseras på klassen hello hello tunnel gränssnittet IP-adress.</span><span class="sxs-lookup"><span data-stu-id="48c10-219">Because there is no other way tooget it, hello client tries tooguess hello subnet mask based on hello class of hello tunnel interface IP address.</span></span> 

<span data-ttu-id="48c10-220">Därför läggs en väg baserat på hello följande avbildningar:</span><span class="sxs-lookup"><span data-stu-id="48c10-220">Therefore, a route is added based on hello following static mapping:</span></span> 

<span data-ttu-id="48c10-221">Om adressen hör tooclass A gäller/8 som för--></span><span class="sxs-lookup"><span data-stu-id="48c10-221">If address belongs tooclass A --> apply /8</span></span>

<span data-ttu-id="48c10-222">Om adressen hör gäller tooclass B--> /16</span><span class="sxs-lookup"><span data-stu-id="48c10-222">If address belongs tooclass B --> apply /16</span></span>

<span data-ttu-id="48c10-223">Om adressen hör gäller tooclass C--> /24</span><span class="sxs-lookup"><span data-stu-id="48c10-223">If address belongs tooclass C --> apply /24</span></span>

## <a name="vpn-client-cannot-access-network-file-shares"></a><span data-ttu-id="48c10-224">VPN-klienten kan inte komma åt filresurser över nätverket</span><span class="sxs-lookup"><span data-stu-id="48c10-224">VPN client cannot access network file shares</span></span>

### <a name="symptom"></a><span data-ttu-id="48c10-225">Symtom</span><span class="sxs-lookup"><span data-stu-id="48c10-225">Symptom</span></span>

<span data-ttu-id="48c10-226">hello VPN-klient har anslutit toohello virtuella Azure-nätverket.</span><span class="sxs-lookup"><span data-stu-id="48c10-226">hello VPN client has connected toohello Azure virtual network.</span></span> <span data-ttu-id="48c10-227">Dock hello-klienten kan inte komma åt nätverksresurser.</span><span class="sxs-lookup"><span data-stu-id="48c10-227">However, hello client cannot access network shares.</span></span>

### <a name="cause"></a><span data-ttu-id="48c10-228">Orsak</span><span class="sxs-lookup"><span data-stu-id="48c10-228">Cause</span></span>

<span data-ttu-id="48c10-229">hello SMB-protokollet används för åtkomst till resursen för filen.</span><span class="sxs-lookup"><span data-stu-id="48c10-229">hello SMB protocol is used for file share access.</span></span> <span data-ttu-id="48c10-230">När hello anslutningen initieras hello VPN-klienten lägger till hello session autentiseringsuppgifter och hello fel inträffar.</span><span class="sxs-lookup"><span data-stu-id="48c10-230">When hello connection is initiated, hello VPN client adds hello session credentials and hello failure occurs.</span></span> <span data-ttu-id="48c10-231">Efter hello anslutningen har upprättats tvingas hello klienten toouse hello cache autentiseringsuppgifter för Kerberos-autentisering.</span><span class="sxs-lookup"><span data-stu-id="48c10-231">After hello connection is established, hello client is forced toouse hello cache credentials for Kerberos authentication.</span></span> <span data-ttu-id="48c10-232">Den här processen initierar frågor toohello Key Distribution Center (domänkontrollanten) tooget en token.</span><span class="sxs-lookup"><span data-stu-id="48c10-232">This process initiates queries toohello Key Distribution Center (a domain controller) tooget a token.</span></span> <span data-ttu-id="48c10-233">Eftersom hello klienten ansluter från hello Internet, kanske inte kan tooreach hello-domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="48c10-233">Because hello client connects from hello Internet, it might not be able tooreach hello domain controller.</span></span> <span data-ttu-id="48c10-234">Därför inte kan hello-klienten växla över från Kerberos tooNTLM.</span><span class="sxs-lookup"><span data-stu-id="48c10-234">Therefore, hello client cannot fail over from Kerberos tooNTLM.</span></span> 

<span data-ttu-id="48c10-235">hello endast tid uppmanas att hello-klienten för en autentiseringsuppgift är när den har ett giltigt certifikat (med SAN = UPN) utfärdat av hello domän toowhich som den är ansluten.</span><span class="sxs-lookup"><span data-stu-id="48c10-235">hello only time that hello client is prompted for a credential is when it has a valid certificate (with SAN=UPN) issued by hello domain toowhich it is joined.</span></span> <span data-ttu-id="48c10-236">hello-klienten måste också vara fysiskt ansluten toohello domännätverk.</span><span class="sxs-lookup"><span data-stu-id="48c10-236">hello client also must be physically connected toohello domain network.</span></span> <span data-ttu-id="48c10-237">I det här fallet hello klienten försöker toouse hello certifikat och når ut toohello domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="48c10-237">In this case, hello client tries toouse hello certificate and reaches out toohello domain controller.</span></span> <span data-ttu-id="48c10-238">Hello Key Distribution Center returnerar ett ”KDC_ERR_C_PRINCIPAL_UNKNOWN”-fel.</span><span class="sxs-lookup"><span data-stu-id="48c10-238">Then hello Key Distribution Center returns a "KDC_ERR_C_PRINCIPAL_UNKNOWN" error.</span></span> <span data-ttu-id="48c10-239">hello-klienten är framtvingad toofail över tooNTLM.</span><span class="sxs-lookup"><span data-stu-id="48c10-239">hello client is forced toofail over tooNTLM.</span></span> 

### <a name="solution"></a><span data-ttu-id="48c10-240">Lösning</span><span class="sxs-lookup"><span data-stu-id="48c10-240">Solution</span></span>

<span data-ttu-id="48c10-241">toowork runt hello problemet, inaktivera hello cachelagring av autentiseringsuppgifter för domänen från hello följande registerundernyckel:</span><span class="sxs-lookup"><span data-stu-id="48c10-241">toowork around hello problem, disable hello caching of domain credentials from hello following registry subkey:</span></span> 

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\DisableDomainCreds - Set hello value too1 


## <a name="cannot-find-hello-point-to-site-vpn-connection-in-windows-after-reinstalling-hello-vpn-client"></a><span data-ttu-id="48c10-242">Det går inte att hitta hello punkt-till-plats VPN-anslutning i Windows efter ominstallationen hello VPN-klienten</span><span class="sxs-lookup"><span data-stu-id="48c10-242">Cannot find hello point-to-site VPN connection in Windows after reinstalling hello VPN client</span></span>

### <a name="symptom"></a><span data-ttu-id="48c10-243">Symtom</span><span class="sxs-lookup"><span data-stu-id="48c10-243">Symptom</span></span>

<span data-ttu-id="48c10-244">Du tar bort hello punkt-till-plats VPN-anslutning och sedan installera om hello VPN-klienten.</span><span class="sxs-lookup"><span data-stu-id="48c10-244">You remove hello point-to-site VPN connection and then reinstall hello VPN client.</span></span> <span data-ttu-id="48c10-245">I den här situationen har hello VPN-anslutningen inte konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="48c10-245">In this situation, hello VPN connection is not configured successfully.</span></span> <span data-ttu-id="48c10-246">Du ser inte hello VPN-anslutningen i hello **nätverksanslutningar** inställningar i Windows.</span><span class="sxs-lookup"><span data-stu-id="48c10-246">You do not see hello VPN connection in hello **Network connections** settings in Windows.</span></span>

### <a name="solution"></a><span data-ttu-id="48c10-247">Lösning</span><span class="sxs-lookup"><span data-stu-id="48c10-247">Solution</span></span>

<span data-ttu-id="48c10-248">tooresolve hello problem, ta bort hello gamla VPN-klientkonfigurationsfiler från **C:\Users\TheUserName\AppData\Roaming\Microsoft\Network\Connections**, och kör sedan hello VPN klientinstallationsprogrammet igen.</span><span class="sxs-lookup"><span data-stu-id="48c10-248">tooresolve hello problem, delete hello old VPN client configuration files from **C:\Users\TheUserName\AppData\Roaming\Microsoft\Network\Connections**, and then run hello VPN client installer again.</span></span>
