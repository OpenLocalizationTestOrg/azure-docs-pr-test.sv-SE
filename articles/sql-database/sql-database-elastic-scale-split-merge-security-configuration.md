---
title: "aaaSplit merge säkerhetskonfiguration | Microsoft Docs"
description: "Ställ in x409 certifikat för kryptering"
metakeywords: Elastic Database certificates security
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: f9e89c57-61a0-484f-b787-82dae2349cb6
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: 511c04be0598d8a0889aa3e3fcf02be0bf0e96cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="split-merge-security-configuration"></a><span data-ttu-id="d5307-103">Dela dokument säkerhetskonfiguration</span><span class="sxs-lookup"><span data-stu-id="d5307-103">Split-merge security configuration</span></span>
<span data-ttu-id="d5307-104">toouse hello dela/kopplingstjänsten, måste du ska konfigurera säkerhet.</span><span class="sxs-lookup"><span data-stu-id="d5307-104">toouse hello Split/Merge service, you must correctly configure security.</span></span> <span data-ttu-id="d5307-105">hello service är en del av hello elastisk skalbarhet funktion i Microsoft Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="d5307-105">hello service is part of hello Elastic Scale feature of Microsoft Azure SQL Database.</span></span> <span data-ttu-id="d5307-106">Mer information finns i [elastisk skala dela och sammanfoga Service kursen](sql-database-elastic-scale-configure-deploy-split-and-merge.md).</span><span class="sxs-lookup"><span data-stu-id="d5307-106">For more information, see [Elastic Scale Split and Merge Service Tutorial](sql-database-elastic-scale-configure-deploy-split-and-merge.md).</span></span>

## <a name="configuring-certificates"></a><span data-ttu-id="d5307-107">Konfigurera certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-107">Configuring certificates</span></span>
<span data-ttu-id="d5307-108">Certifikat är konfigurerade på två sätt.</span><span class="sxs-lookup"><span data-stu-id="d5307-108">Certificates are configured in two ways.</span></span> 

1. [<span data-ttu-id="d5307-109">tooConfigure hello SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-109">tooConfigure hello SSL Certificate</span></span>](#to-configure-the-ssl-certificate)
2. [<span data-ttu-id="d5307-110">tooConfigure klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-110">tooConfigure Client Certificates</span></span>](#to-configure-client-certificates) 

## <a name="tooobtain-certificates"></a><span data-ttu-id="d5307-111">tooobtain certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-111">tooobtain certificates</span></span>
<span data-ttu-id="d5307-112">Certifikat kan hämtas från offentlig certifikatutfärdare (CA) eller från hello [Windows certifikattjänst](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx).</span><span class="sxs-lookup"><span data-stu-id="d5307-112">Certificates can be obtained from public Certificate Authorities (CAs) or from hello [Windows Certificate Service](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx).</span></span> <span data-ttu-id="d5307-113">Är detta hello önskad metoder tooobtain certifikat.</span><span class="sxs-lookup"><span data-stu-id="d5307-113">These are hello preferred methods tooobtain certificates.</span></span>

<span data-ttu-id="d5307-114">Om dessa alternativ inte är tillgängliga kan du generera **självsignerade certifikat**.</span><span class="sxs-lookup"><span data-stu-id="d5307-114">If those options are not available, you can generate **self-signed certificates**.</span></span>

## <a name="tools-toogenerate-certificates"></a><span data-ttu-id="d5307-115">Verktyg toogenerate certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-115">Tools toogenerate certificates</span></span>
* [<span data-ttu-id="d5307-116">MakeCert.exe</span><span class="sxs-lookup"><span data-stu-id="d5307-116">makecert.exe</span></span>](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [<span data-ttu-id="d5307-117">pvk2pfx.exe</span><span class="sxs-lookup"><span data-stu-id="d5307-117">pvk2pfx.exe</span></span>](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="toorun-hello-tools"></a><span data-ttu-id="d5307-118">toorun hello-verktyg</span><span class="sxs-lookup"><span data-stu-id="d5307-118">toorun hello tools</span></span>
* <span data-ttu-id="d5307-119">Från en utvecklare kommandotolk för Visual Studios finns [Kommandotolken Visual Studio](http://msdn.microsoft.com/library/ms229859.aspx)</span><span class="sxs-lookup"><span data-stu-id="d5307-119">From a Developer Command Prompt for Visual Studios, see [Visual Studio Command Prompt](http://msdn.microsoft.com/library/ms229859.aspx)</span></span> 
  
    <span data-ttu-id="d5307-120">Om installerat, gå till:</span><span class="sxs-lookup"><span data-stu-id="d5307-120">If installed, go to:</span></span>
  
        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 
* <span data-ttu-id="d5307-121">Hämta hello WDK från [Windows 8.1: Hämta paket och verktyg](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)</span><span class="sxs-lookup"><span data-stu-id="d5307-121">Get hello WDK from [Windows 8.1: Download kits and tools](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)</span></span>

## <a name="tooconfigure-hello-ssl-certificate"></a><span data-ttu-id="d5307-122">tooconfigure hello SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-122">tooconfigure hello SSL certificate</span></span>
<span data-ttu-id="d5307-123">Ett SSL-certifikat är obligatoriska tooencrypt hello kommunikation och autentisera hello-servern.</span><span class="sxs-lookup"><span data-stu-id="d5307-123">A SSL certificate is required tooencrypt hello communication and authenticate hello server.</span></span> <span data-ttu-id="d5307-124">Välj hello som är mest användbart hello tre scenarier nedan och kör alla steg:</span><span class="sxs-lookup"><span data-stu-id="d5307-124">Choose hello most applicable of hello three scenarios below, and execute all its steps:</span></span>

### <a name="create-a-new-self-signed-certificate"></a><span data-ttu-id="d5307-125">Skapa ett nytt självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-125">Create a new self-signed certificate</span></span>
1. [<span data-ttu-id="d5307-126">Skapa ett självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-126">Create a Self-Signed Certificate</span></span>](#create-a-self-signed-certificate)
2. [<span data-ttu-id="d5307-127">Skapa PFX-fil för ett självsignerat SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-127">Create PFX file for Self-Signed SSL Certificate</span></span>](#create-pfx-file-for-self-signed-ssl-certificate)
3. [<span data-ttu-id="d5307-128">Överför SSL-certifikat tooCloud Service</span><span class="sxs-lookup"><span data-stu-id="d5307-128">Upload SSL Certificate tooCloud Service</span></span>](#upload-ssl-certificate-to-cloud-service)
4. [<span data-ttu-id="d5307-129">Uppdatera SSL-certifikat i Tjänstkonfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="d5307-129">Update SSL Certificate in Service Configuration File</span></span>](#update-ssl-certificate-in-service-configuration-file)
5. [<span data-ttu-id="d5307-130">Importera SSL-certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="d5307-130">Import SSL Certification Authority</span></span>](#import-ssl-certification-authority)

### <a name="toouse-an-existing-certificate-from-hello-certificate-store"></a><span data-ttu-id="d5307-131">toouse ett befintligt certifikat från hello certifikat lagra</span><span class="sxs-lookup"><span data-stu-id="d5307-131">toouse an existing certificate from hello certificate store</span></span>
1. [<span data-ttu-id="d5307-132">Exportera SSL-certifikatet från certifikatarkivet</span><span class="sxs-lookup"><span data-stu-id="d5307-132">Export SSL Certificate From Certificate Store</span></span>](#export-ssl-certificate-from-certificate-store)
2. [<span data-ttu-id="d5307-133">Överför SSL-certifikat tooCloud Service</span><span class="sxs-lookup"><span data-stu-id="d5307-133">Upload SSL Certificate tooCloud Service</span></span>](#upload-ssl-certificate-to-cloud-service)
3. [<span data-ttu-id="d5307-134">Uppdatera SSL-certifikat i Tjänstkonfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="d5307-134">Update SSL Certificate in Service Configuration File</span></span>](#update-ssl-certificate-in-service-configuration-file)

### <a name="toouse-an-existing-certificate-in-a-pfx-file"></a><span data-ttu-id="d5307-135">toouse ett befintligt certifikat i en PFX-fil</span><span class="sxs-lookup"><span data-stu-id="d5307-135">toouse an existing certificate in a PFX file</span></span>
1. [<span data-ttu-id="d5307-136">Överför SSL-certifikat tooCloud Service</span><span class="sxs-lookup"><span data-stu-id="d5307-136">Upload SSL Certificate tooCloud Service</span></span>](#upload-ssl-certificate-to-cloud-service)
2. [<span data-ttu-id="d5307-137">Uppdatera SSL-certifikat i Tjänstkonfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="d5307-137">Update SSL Certificate in Service Configuration File</span></span>](#update-ssl-certificate-in-service-configuration-file)

## <a name="tooconfigure-client-certificates"></a><span data-ttu-id="d5307-138">tooconfigure klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-138">tooconfigure client certificates</span></span>
<span data-ttu-id="d5307-139">Klientcertifikat krävs i ordning tooauthenticate begäranden toohello service.</span><span class="sxs-lookup"><span data-stu-id="d5307-139">Client certificates are required in order tooauthenticate requests toohello service.</span></span> <span data-ttu-id="d5307-140">Välj hello som är mest användbart hello tre scenarier nedan och kör alla steg:</span><span class="sxs-lookup"><span data-stu-id="d5307-140">Choose hello most applicable of hello three scenarios below, and execute all its steps:</span></span>

### <a name="turn-off-client-certificates"></a><span data-ttu-id="d5307-141">Stäng av klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-141">Turn off client certificates</span></span>
1. [<span data-ttu-id="d5307-142">Inaktivera certifikatbaserad klientautentisering</span><span class="sxs-lookup"><span data-stu-id="d5307-142">Turn Off Client Certificate-Based Authentication</span></span>](#turn-off-client-certificate-based-authentication)

### <a name="issue-new-self-signed-client-certificates"></a><span data-ttu-id="d5307-143">Utfärda nya självsignerade certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-143">Issue new self-signed client certificates</span></span>
1. [<span data-ttu-id="d5307-144">Skapa en självsignerat certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="d5307-144">Create a Self-Signed Certification Authority</span></span>](#create-a-self-signed-certification-authority)
2. [<span data-ttu-id="d5307-145">Ladda upp CA-certifikat tooCloud Service</span><span class="sxs-lookup"><span data-stu-id="d5307-145">Upload CA Certificate tooCloud Service</span></span>](#upload-ca-certificate-to-cloud-service)
3. [<span data-ttu-id="d5307-146">Uppdatera CA-certifikat i Tjänstkonfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="d5307-146">Update CA Certificate in Service Configuration File</span></span>](#update-ca-certificate-in-service-configuration-file)
4. [<span data-ttu-id="d5307-147">Utfärda klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-147">Issue Client Certificates</span></span>](#issue-client-certificates)
5. [<span data-ttu-id="d5307-148">Skapa PFX-filer för klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-148">Create PFX files for Client Certificates</span></span>](#create-pfx-files-for-client-certificates)
6. [<span data-ttu-id="d5307-149">Importera klientcertifikatet</span><span class="sxs-lookup"><span data-stu-id="d5307-149">Import Client Certificate</span></span>](#Import-Client-Certificate)
7. [<span data-ttu-id="d5307-150">Kopiera Certifikattumavtryck för klienten</span><span class="sxs-lookup"><span data-stu-id="d5307-150">Copy Client Certificate Thumbprints</span></span>](#copy-client-certificate-thumbprints)
8. [<span data-ttu-id="d5307-151">Konfigurera tillåtna klienter i hello-Tjänstkonfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="d5307-151">Configure Allowed Clients in hello Service Configuration File</span></span>](#configure-allowed-clients-in-the-service-configuration-file)

### <a name="use-existing-client-certificates"></a><span data-ttu-id="d5307-152">Använd befintlig klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-152">Use existing client certificates</span></span>
1. [<span data-ttu-id="d5307-153">Hitta Certifikatutfärdarens offentliga nyckel</span><span class="sxs-lookup"><span data-stu-id="d5307-153">Find CA Public Key</span></span>](#find-ca-public-key)
2. [<span data-ttu-id="d5307-154">Ladda upp CA-certifikat tooCloud Service</span><span class="sxs-lookup"><span data-stu-id="d5307-154">Upload CA Certificate tooCloud Service</span></span>](#Upload-CA-certificate-to-cloud-service)
3. [<span data-ttu-id="d5307-155">Uppdatera CA-certifikat i Tjänstkonfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="d5307-155">Update CA Certificate in Service Configuration File</span></span>](#Update-CA-Certificate-in-Service-Configuration-File)
4. [<span data-ttu-id="d5307-156">Kopiera Certifikattumavtryck för klienten</span><span class="sxs-lookup"><span data-stu-id="d5307-156">Copy Client Certificate Thumbprints</span></span>](#Copy-Client-Certificate-Thumbprints)
5. [<span data-ttu-id="d5307-157">Konfigurera tillåtna klienter i hello-Tjänstkonfigurationsfil</span><span class="sxs-lookup"><span data-stu-id="d5307-157">Configure Allowed Clients in hello Service Configuration File</span></span>](#configure-allowed-clients-in-the-service-configuration-file)
6. [<span data-ttu-id="d5307-158">Konfigurera klienten Återkallningskontrollen för lövcertifikatet</span><span class="sxs-lookup"><span data-stu-id="d5307-158">Configure Client Certificate Revocation Check</span></span>](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a><span data-ttu-id="d5307-159">Tillåtna IP-adresser</span><span class="sxs-lookup"><span data-stu-id="d5307-159">Allowed IP addresses</span></span>
<span data-ttu-id="d5307-160">Åtkomst toohello slutpunkter kan vara begränsad toospecific intervall med IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="d5307-160">Access toohello service endpoints can be restricted toospecific ranges of IP addresses.</span></span>

## <a name="tooconfigure-encryption-for-hello-store"></a><span data-ttu-id="d5307-161">tooconfigure kryptering för hello store</span><span class="sxs-lookup"><span data-stu-id="d5307-161">tooconfigure encryption for hello store</span></span>
<span data-ttu-id="d5307-162">Ett certifikat är obligatoriska tooencrypt hello autentiseringsuppgifter som lagras i hello metadata store.</span><span class="sxs-lookup"><span data-stu-id="d5307-162">A certificate is required tooencrypt hello credentials that are stored in hello metadata store.</span></span> <span data-ttu-id="d5307-163">Välj hello som är mest användbart hello tre scenarier nedan och kör alla steg:</span><span class="sxs-lookup"><span data-stu-id="d5307-163">Choose hello most applicable of hello three scenarios below, and execute all its steps:</span></span>

### <a name="use-a-new-self-signed-certificate"></a><span data-ttu-id="d5307-164">Använda ett nytt självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-164">Use a new self-signed certificate</span></span>
1. [<span data-ttu-id="d5307-165">Skapa ett självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-165">Create a Self-Signed Certificate</span></span>](#create-a-self-signed-certificate)
2. [<span data-ttu-id="d5307-166">Skapa PFX-fil för ett självsignerat certifikat för kryptering</span><span class="sxs-lookup"><span data-stu-id="d5307-166">Create PFX file for Self-Signed Encryption Certificate</span></span>](#create-pfx-file-for-self-signed-ssl-certificate)
3. [<span data-ttu-id="d5307-167">Överför krypteringscertifikat tooCloud Service</span><span class="sxs-lookup"><span data-stu-id="d5307-167">Upload Encryption Certificate tooCloud Service</span></span>](#upload-encryption-certificate-to-cloud-service)
4. [<span data-ttu-id="d5307-168">Uppdatera certifikat för kryptering i konfigurationsfilen för tjänsten</span><span class="sxs-lookup"><span data-stu-id="d5307-168">Update Encryption Certificate in Service Configuration File</span></span>](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-from-hello-certificate-store"></a><span data-ttu-id="d5307-169">Använda ett befintligt certifikat från certifikatarkivet hello</span><span class="sxs-lookup"><span data-stu-id="d5307-169">Use an existing certificate from hello certificate store</span></span>
1. [<span data-ttu-id="d5307-170">Exportera krypteringscertifikat från certifikatarkivet</span><span class="sxs-lookup"><span data-stu-id="d5307-170">Export Encryption Certificate From Certificate Store</span></span>](#export-encryption-certificate-from-certificate-store)
2. [<span data-ttu-id="d5307-171">Överför krypteringscertifikat tooCloud Service</span><span class="sxs-lookup"><span data-stu-id="d5307-171">Upload Encryption Certificate tooCloud Service</span></span>](#upload-encryption-certificate-to-cloud-service)
3. [<span data-ttu-id="d5307-172">Uppdatera certifikat för kryptering i konfigurationsfilen för tjänsten</span><span class="sxs-lookup"><span data-stu-id="d5307-172">Update Encryption Certificate in Service Configuration File</span></span>](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a><span data-ttu-id="d5307-173">Använd ett befintligt certifikat i en PFX-fil</span><span class="sxs-lookup"><span data-stu-id="d5307-173">Use an existing certificate in a PFX file</span></span>
1. [<span data-ttu-id="d5307-174">Överför krypteringscertifikat tooCloud Service</span><span class="sxs-lookup"><span data-stu-id="d5307-174">Upload Encryption Certificate tooCloud Service</span></span>](#upload-encryption-certificate-to-cloud-service)
2. [<span data-ttu-id="d5307-175">Uppdatera certifikat för kryptering i konfigurationsfilen för tjänsten</span><span class="sxs-lookup"><span data-stu-id="d5307-175">Update Encryption Certificate in Service Configuration File</span></span>](#update-encryption-certificate-in-service-configuration-file)

## <a name="hello-default-configuration"></a><span data-ttu-id="d5307-176">hello standardkonfigurationen</span><span class="sxs-lookup"><span data-stu-id="d5307-176">hello default configuration</span></span>
<span data-ttu-id="d5307-177">hello standardkonfigurationen nekar alla åtkomst toohello HTTP-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="d5307-177">hello default configuration denies all access toohello HTTP endpoint.</span></span> <span data-ttu-id="d5307-178">Detta är hello rekommenderad inställning, eftersom hello begäranden toothese slutpunkter kan utföra känslig information, till exempel autentiseringsuppgifter på databasen.</span><span class="sxs-lookup"><span data-stu-id="d5307-178">This is hello recommended setting, since hello requests toothese endpoints may carry sensitive information like database credentials.</span></span>
<span data-ttu-id="d5307-179">hello standardkonfigurationen kan alla åtkomst toohello HTTPS-slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="d5307-179">hello default configuration allows all access toohello HTTPS endpoint.</span></span> <span data-ttu-id="d5307-180">Den här inställningen kan begränsas ytterligare.</span><span class="sxs-lookup"><span data-stu-id="d5307-180">This setting may be restricted further.</span></span>

### <a name="changing-hello-configuration"></a><span data-ttu-id="d5307-181">Ändra hello konfiguration</span><span class="sxs-lookup"><span data-stu-id="d5307-181">Changing hello Configuration</span></span>
<span data-ttu-id="d5307-182">hello grupp av regler för åtkomstkontroll som gäller tooand slutpunkt har konfigurerats i hello  **<EndpointAcls>**  avsnitt i hello **tjänstkonfigurationsfilen**.</span><span class="sxs-lookup"><span data-stu-id="d5307-182">hello group of access control rules that apply tooand endpoint are configured in hello **<EndpointAcls>** section in hello **service configuration file**.</span></span>

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

<span data-ttu-id="d5307-183">hello regler i en grupp för kontroll av åtkomst som konfigureras i en <AccessControl name=""> i hello service konfigurationsfil.</span><span class="sxs-lookup"><span data-stu-id="d5307-183">hello rules in an access control group are configured in a <AccessControl name=""> section of hello service configuration file.</span></span> 

<span data-ttu-id="d5307-184">hello format beskrivs i dokumentationen för nätverket ACL-listor.</span><span class="sxs-lookup"><span data-stu-id="d5307-184">hello format is explained in Network Access Control Lists documentation.</span></span>
<span data-ttu-id="d5307-185">Till exempel tooallow endast IP-adresser i hello intervallet 100.100.0.0 too100.100.255.255 tooaccess hello HTTPS-slutpunkt, hello regler skulle se ut så här:</span><span class="sxs-lookup"><span data-stu-id="d5307-185">For example, tooallow only IPs in hello range 100.100.0.0 too100.100.255.255 tooaccess hello HTTPS endpoint, hello rules would look like this:</span></span>

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a><span data-ttu-id="d5307-186">För tjänsten förebyggande</span><span class="sxs-lookup"><span data-stu-id="d5307-186">Denial of service prevention</span></span>
<span data-ttu-id="d5307-187">Det finns två olika metoder stöds toodetect och förhindra Denial of Service-attacker:</span><span class="sxs-lookup"><span data-stu-id="d5307-187">There are two different mechanisms supported toodetect and prevent Denial of Service attacks:</span></span>

* <span data-ttu-id="d5307-188">Begränsa antalet samtidiga förfrågningar per fjärrvärden (inaktiverat som standard)</span><span class="sxs-lookup"><span data-stu-id="d5307-188">Restrict number of concurrent requests per remote host (off by default)</span></span>
* <span data-ttu-id="d5307-189">Begränsa antalet åtkomst per fjärrvärden (på som standard)</span><span class="sxs-lookup"><span data-stu-id="d5307-189">Restrict rate of access per remote host (on by default)</span></span>

<span data-ttu-id="d5307-190">Dessa är baserade på hello funktioner ytterligare dokumenteras i den dynamiska IP-säkerhet i IIS.</span><span class="sxs-lookup"><span data-stu-id="d5307-190">These are based on hello features further documented in Dynamic IP Security in IIS.</span></span> <span data-ttu-id="d5307-191">När du ändrar den här konfigurationen varning för hello följande faktorer:</span><span class="sxs-lookup"><span data-stu-id="d5307-191">When changing this configuration beware of hello following factors:</span></span>

* <span data-ttu-id="d5307-192">hello beteendet för proxyservrar och Network Address Translation enheter över hello fjärrvärden information</span><span class="sxs-lookup"><span data-stu-id="d5307-192">hello behavior of proxies and Network Address Translation devices over hello remote host information</span></span>
* <span data-ttu-id="d5307-193">Varje begäran tooany resurs i hello webbroll anses (t.ex. läser in skript, bilder, osv)</span><span class="sxs-lookup"><span data-stu-id="d5307-193">Each request tooany resource in hello web role is considered (e.g. loading scripts, images, etc)</span></span>

## <a name="restricting-number-of-concurrent-accesses"></a><span data-ttu-id="d5307-194">Begränsa antalet samtidiga användningar</span><span class="sxs-lookup"><span data-stu-id="d5307-194">Restricting number of concurrent accesses</span></span>
<span data-ttu-id="d5307-195">hello-inställningar som konfigurerar det här beteendet är:</span><span class="sxs-lookup"><span data-stu-id="d5307-195">hello settings that configure this behavior are:</span></span>

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

<span data-ttu-id="d5307-196">Ändra DynamicIpRestrictionDenyByConcurrentRequests tootrue tooenable skyddet.</span><span class="sxs-lookup"><span data-stu-id="d5307-196">Change DynamicIpRestrictionDenyByConcurrentRequests tootrue tooenable this protection.</span></span>

## <a name="restricting-rate-of-access"></a><span data-ttu-id="d5307-197">Begränsa antalet åtkomst</span><span class="sxs-lookup"><span data-stu-id="d5307-197">Restricting rate of access</span></span>
<span data-ttu-id="d5307-198">hello-inställningar som konfigurerar det här beteendet är:</span><span class="sxs-lookup"><span data-stu-id="d5307-198">hello settings that configure this behavior are:</span></span>

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-hello-response-tooa-denied-request"></a><span data-ttu-id="d5307-199">Konfigurera hello svar tooa nekade begäran</span><span class="sxs-lookup"><span data-stu-id="d5307-199">Configuring hello response tooa denied request</span></span>
<span data-ttu-id="d5307-200">hello konfigurerar följande inställning hello svar tooa nekade begäran:</span><span class="sxs-lookup"><span data-stu-id="d5307-200">hello following setting configures hello response tooa denied request:</span></span>

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
<span data-ttu-id="d5307-201">Läs toohello dokumentationen för den dynamiska IP-säkerhet i IIS för andra värden som stöds.</span><span class="sxs-lookup"><span data-stu-id="d5307-201">Refer toohello documentation for Dynamic IP Security in IIS for other supported values.</span></span>

## <a name="operations-for-configuring-service-certificates"></a><span data-ttu-id="d5307-202">Åtgärder för att konfigurera tjänstcertifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-202">Operations for configuring service certificates</span></span>
<span data-ttu-id="d5307-203">Det här avsnittet är endast för referens.</span><span class="sxs-lookup"><span data-stu-id="d5307-203">This topic is for reference only.</span></span> <span data-ttu-id="d5307-204">Följ konfigurationsstegen för hello i:</span><span class="sxs-lookup"><span data-stu-id="d5307-204">Please follow hello configuration steps outlined in:</span></span>

* <span data-ttu-id="d5307-205">Konfigurera hello SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-205">Configure hello SSL certificate</span></span>
* <span data-ttu-id="d5307-206">Konfigurera klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-206">Configure client certificates</span></span>

## <a name="create-a-self-signed-certificate"></a><span data-ttu-id="d5307-207">Skapa ett självsignerat certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-207">Create a self-signed certificate</span></span>
<span data-ttu-id="d5307-208">Kör:</span><span class="sxs-lookup"><span data-stu-id="d5307-208">Execute:</span></span>

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

<span data-ttu-id="d5307-209">toocustomize:</span><span class="sxs-lookup"><span data-stu-id="d5307-209">toocustomize:</span></span>

* <span data-ttu-id="d5307-210">-n med hello tjänst-URL.</span><span class="sxs-lookup"><span data-stu-id="d5307-210">-n with hello service URL.</span></span> <span data-ttu-id="d5307-211">Jokertecken (”CN = * .cloudapp .net”) och alternativa namn (”CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net”) stöds.</span><span class="sxs-lookup"><span data-stu-id="d5307-211">Wildcards ("CN=*.cloudapp.net") and alternative names ("CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net") are supported.</span></span>
* <span data-ttu-id="d5307-212">e - med hello certifikatets förfallodatum skapa ett starkt lösenord och ange den när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="d5307-212">-e with hello certificate expiration date Create a strong password and specify it when prompted.</span></span>

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a><span data-ttu-id="d5307-213">Skapa PFX-filen för självsignerade SSL-certifikatet</span><span class="sxs-lookup"><span data-stu-id="d5307-213">Create PFX file for self-signed SSL certificate</span></span>
<span data-ttu-id="d5307-214">Kör:</span><span class="sxs-lookup"><span data-stu-id="d5307-214">Execute:</span></span>

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

<span data-ttu-id="d5307-215">Ange lösenordet och sedan exportera certifikat med dessa alternativ:</span><span class="sxs-lookup"><span data-stu-id="d5307-215">Enter password and then export certificate with these options:</span></span>

* <span data-ttu-id="d5307-216">Ja, exportera hello privat nyckel</span><span class="sxs-lookup"><span data-stu-id="d5307-216">Yes, export hello private key</span></span>
* <span data-ttu-id="d5307-217">Exportera alla utökade egenskaper</span><span class="sxs-lookup"><span data-stu-id="d5307-217">Export all extended properties</span></span>

## <a name="export-ssl-certificate-from-certificate-store"></a><span data-ttu-id="d5307-218">Exportera SSL-certifikatet från certifikatarkivet</span><span class="sxs-lookup"><span data-stu-id="d5307-218">Export SSL certificate from certificate store</span></span>
* <span data-ttu-id="d5307-219">Hitta certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-219">Find certificate</span></span>
* <span data-ttu-id="d5307-220">Klicka på Åtgärder -> alla uppgifter -> Exportera...</span><span class="sxs-lookup"><span data-stu-id="d5307-220">Click Actions -> All tasks -> Export…</span></span>
* <span data-ttu-id="d5307-221">Exportera certifikatet till en. PFX-filen med dessa alternativ:</span><span class="sxs-lookup"><span data-stu-id="d5307-221">Export certificate into a .PFX file with these options:</span></span>
  * <span data-ttu-id="d5307-222">Ja, exportera hello privat nyckel</span><span class="sxs-lookup"><span data-stu-id="d5307-222">Yes, export hello private key</span></span>
  * <span data-ttu-id="d5307-223">Inkludera om möjligt alla certifikat i certifieringssökvägen hello * exportera alla utökade egenskaper</span><span class="sxs-lookup"><span data-stu-id="d5307-223">Include all certificates in hello certification path if possible *Export all extended properties</span></span>

## <a name="upload-ssl-certificate-toocloud-service"></a><span data-ttu-id="d5307-224">Överför SSL-certifikat toocloud-tjänsten</span><span class="sxs-lookup"><span data-stu-id="d5307-224">Upload SSL certificate toocloud service</span></span>
<span data-ttu-id="d5307-225">Överför certifikatet med hello befintliga eller genereras. PFX-filen med hello SSL-nyckelpar:</span><span class="sxs-lookup"><span data-stu-id="d5307-225">Upload certificate with hello existing or generated .PFX file with hello SSL key pair:</span></span>

* <span data-ttu-id="d5307-226">Ange hello lösenord som skyddar hello information om privat nyckel</span><span class="sxs-lookup"><span data-stu-id="d5307-226">Enter hello password protecting hello private key information</span></span>

## <a name="update-ssl-certificate-in-service-configuration-file"></a><span data-ttu-id="d5307-227">Uppdatera SSL-certifikat i tjänstkonfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="d5307-227">Update SSL certificate in service configuration file</span></span>
<span data-ttu-id="d5307-228">Hello tumavtrycksvärde på hello följande inställning i hello tjänstkonfigurationsfil med hello hello certifikatets tumavtryck har överförts toohello Molntjänsten:</span><span class="sxs-lookup"><span data-stu-id="d5307-228">Update hello thumbprint value of hello following setting in hello service configuration file with hello thumbprint of hello certificate uploaded toohello cloud service:</span></span>

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a><span data-ttu-id="d5307-229">Importera SSL-certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="d5307-229">Import SSL certification authority</span></span>
<span data-ttu-id="d5307-230">Följ dessa steg på alla konto/datorn som kommunicerar med hello-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="d5307-230">Follow these steps in all account/machine that will communicate with hello service:</span></span>

* <span data-ttu-id="d5307-231">Dubbelklicka på hello. CER-filen i Utforskaren i Windows</span><span class="sxs-lookup"><span data-stu-id="d5307-231">Double-click hello .CER file in Windows Explorer</span></span>
* <span data-ttu-id="d5307-232">Klicka på Installera certifikat i dialogrutan för hello certifikat...</span><span class="sxs-lookup"><span data-stu-id="d5307-232">In hello Certificate dialog, click Install Certificate…</span></span>
* <span data-ttu-id="d5307-233">Importera certifikatet till hello betrodda rotcertifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="d5307-233">Import certificate into hello Trusted Root Certification Authorities store</span></span>

## <a name="turn-off-client-certificate-based-authentication"></a><span data-ttu-id="d5307-234">Inaktivera certifikatbaserad klientautentisering</span><span class="sxs-lookup"><span data-stu-id="d5307-234">Turn off client certificate-based authentication</span></span>
<span data-ttu-id="d5307-235">Endast certifikatbaserad klientautentisering stöds och inaktiverar den tillåter för allmän åtkomst toohello slutpunkter, såvida inte andra mekanismer finns på plats (t.ex. Microsoft Azure Virtual Network).</span><span class="sxs-lookup"><span data-stu-id="d5307-235">Only client certificate-based authentication is supported and disabling it will allow for public access toohello service endpoints, unless other mechanisms are in place (e.g. Microsoft Azure Virtual Network).</span></span>

<span data-ttu-id="d5307-236">Ändra dessa inställningar toofalse i hello service configuration tooturn hello funktionen:</span><span class="sxs-lookup"><span data-stu-id="d5307-236">Change these settings toofalse in hello service configuration file tooturn hello feature off:</span></span>

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

<span data-ttu-id="d5307-237">Kopiera sedan hello samma tumavtryck som hello SSL-certifikatet i inställningen för hello CA-certifikat:</span><span class="sxs-lookup"><span data-stu-id="d5307-237">Then, copy hello same thumbprint as hello SSL certificate in hello CA certificate setting:</span></span>

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a><span data-ttu-id="d5307-238">Skapa en självsignerat certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="d5307-238">Create a self-signed certification authority</span></span>
<span data-ttu-id="d5307-239">Kör hello följande steg toocreate ett självsignerat certifikat tooact som en certifikatutfärdare:</span><span class="sxs-lookup"><span data-stu-id="d5307-239">Execute hello following steps toocreate a self-signed certificate tooact as a Certification Authority:</span></span>

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

<span data-ttu-id="d5307-240">toocustomize den</span><span class="sxs-lookup"><span data-stu-id="d5307-240">toocustomize it</span></span>

* <span data-ttu-id="d5307-241">-e med hello utgångsdatum för certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="d5307-241">-e with hello certification expiration date</span></span>

## <a name="find-ca-public-key"></a><span data-ttu-id="d5307-242">Hitta Certifikatutfärdarens offentliga nyckel</span><span class="sxs-lookup"><span data-stu-id="d5307-242">Find CA public key</span></span>
<span data-ttu-id="d5307-243">Alla certifikat måste har utfärdats av en certifikatutfärdare som betrodd av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d5307-243">All client certificates must have been issued by a Certification Authority trusted by hello service.</span></span> <span data-ttu-id="d5307-244">Hitta hello offentliga nyckel toohello certifikatutfärdare som utfärdade hello klientcertifikat som ska toobe användes för autentisering i ordning tooupload toohello Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="d5307-244">Find hello public key toohello Certification Authority that issued hello client certificates that are going toobe used for authentication in order tooupload it toohello cloud service.</span></span>

<span data-ttu-id="d5307-245">Om hello-filen med hello offentliga nyckel inte är tillgänglig, kan du exportera det från certifikatarkivet hello:</span><span class="sxs-lookup"><span data-stu-id="d5307-245">If hello file with hello public key is not available, export it from hello certificate store:</span></span>

* <span data-ttu-id="d5307-246">Hitta certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-246">Find certificate</span></span>
  * <span data-ttu-id="d5307-247">Sök efter ett klientcertifikat utfärdat av hello samma certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="d5307-247">Search for a client certificate issued by hello same Certification Authority</span></span>
* <span data-ttu-id="d5307-248">Dubbelklicka på hello certifikatet.</span><span class="sxs-lookup"><span data-stu-id="d5307-248">Double-click hello certificate.</span></span>
* <span data-ttu-id="d5307-249">Välj hello certifieringssökvägen fliken i dialogrutan för hello-certifikat.</span><span class="sxs-lookup"><span data-stu-id="d5307-249">Select hello Certification Path tab in hello Certificate dialog.</span></span>
* <span data-ttu-id="d5307-250">Dubbelklicka på hello CA post i hello sökväg.</span><span class="sxs-lookup"><span data-stu-id="d5307-250">Double-click hello CA entry in hello path.</span></span>
* <span data-ttu-id="d5307-251">Anteckna av hello certifikategenskaper.</span><span class="sxs-lookup"><span data-stu-id="d5307-251">Take notes of hello certificate properties.</span></span>
* <span data-ttu-id="d5307-252">Stäng hello **certifikat** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d5307-252">Close hello **Certificate** dialog.</span></span>
* <span data-ttu-id="d5307-253">Hitta certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-253">Find certificate</span></span>
  * <span data-ttu-id="d5307-254">Sök efter hello Certifikatutfärdare som nämns ovan.</span><span class="sxs-lookup"><span data-stu-id="d5307-254">Search for hello CA noted above.</span></span>
* <span data-ttu-id="d5307-255">Klicka på Åtgärder -> alla uppgifter -> Exportera...</span><span class="sxs-lookup"><span data-stu-id="d5307-255">Click Actions -> All tasks -> Export…</span></span>
* <span data-ttu-id="d5307-256">Exportera certifikatet till en. CER med dessa alternativ:</span><span class="sxs-lookup"><span data-stu-id="d5307-256">Export certificate into a .CER with these options:</span></span>
  * <span data-ttu-id="d5307-257">**Nej, exportera inte privat nyckel för hello**</span><span class="sxs-lookup"><span data-stu-id="d5307-257">**No, do not export hello private key**</span></span>
  * <span data-ttu-id="d5307-258">Inkludera om möjligt alla certifikat i certifieringssökvägen hello.</span><span class="sxs-lookup"><span data-stu-id="d5307-258">Include all certificates in hello certification path if possible.</span></span>
  * <span data-ttu-id="d5307-259">Exportera alla utökade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="d5307-259">Export all extended properties.</span></span>

## <a name="upload-ca-certificate-toocloud-service"></a><span data-ttu-id="d5307-260">Ladda upp CA-certifikat toocloud-tjänsten</span><span class="sxs-lookup"><span data-stu-id="d5307-260">Upload CA certificate toocloud service</span></span>
<span data-ttu-id="d5307-261">Överför certifikatet med hello befintliga eller genereras. CER-filen med hello CA offentlig nyckel.</span><span class="sxs-lookup"><span data-stu-id="d5307-261">Upload certificate with hello existing or generated .CER file with hello CA public key.</span></span>

## <a name="update-ca-certificate-in-service-configuration-file"></a><span data-ttu-id="d5307-262">Uppdatera CA-certifikat i tjänstkonfigurationsfilen</span><span class="sxs-lookup"><span data-stu-id="d5307-262">Update CA certificate in service configuration file</span></span>
<span data-ttu-id="d5307-263">Hello tumavtrycksvärde på hello följande inställning i hello tjänstkonfigurationsfil med hello hello certifikatets tumavtryck har överförts toohello Molntjänsten:</span><span class="sxs-lookup"><span data-stu-id="d5307-263">Update hello thumbprint value of hello following setting in hello service configuration file with hello thumbprint of hello certificate uploaded toohello cloud service:</span></span>

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

<span data-ttu-id="d5307-264">Uppdatera hello värdet för hello följande inställning med hello samma tumavtryck:</span><span class="sxs-lookup"><span data-stu-id="d5307-264">Update hello value of hello following setting with hello same thumbprint:</span></span>

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a><span data-ttu-id="d5307-265">Utfärda klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-265">Issue client certificates</span></span>
<span data-ttu-id="d5307-266">Varje enskild behöriga tooaccess hello-tjänst bör ha ett klientcertifikat utfärdat för his/hers exklusiv använder och bör välja his/hers äger starkt lösenord tooprotect dess privata nyckel.</span><span class="sxs-lookup"><span data-stu-id="d5307-266">Each individual authorized tooaccess hello service should have a client certificate issued for his/hers exclusive use and should choose his/hers own strong password tooprotect its private key.</span></span> 

<span data-ttu-id="d5307-267">hello följande steg måste utföras i hello samma dator där hello självsignerade certifikat har genererats och lagras:</span><span class="sxs-lookup"><span data-stu-id="d5307-267">hello following steps must be executed in hello same machine where hello self-signed CA certificate was generated and stored:</span></span>

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

<span data-ttu-id="d5307-268">Anpassa:</span><span class="sxs-lookup"><span data-stu-id="d5307-268">Customizing:</span></span>

* <span data-ttu-id="d5307-269">-n med ett ID för toohello-klient som autentiseras med det här certifikatet</span><span class="sxs-lookup"><span data-stu-id="d5307-269">-n with an ID for toohello client that will be authenticated with this certificate</span></span>
* <span data-ttu-id="d5307-270">e - med hello certifikatets förfallodatum</span><span class="sxs-lookup"><span data-stu-id="d5307-270">-e with hello certificate expiration date</span></span>
* <span data-ttu-id="d5307-271">MyID.pvk och MyID.cer med unika filnamn för det här klientcertifikatet</span><span class="sxs-lookup"><span data-stu-id="d5307-271">MyID.pvk and MyID.cer with unique filenames for this client certificate</span></span>

<span data-ttu-id="d5307-272">Det här kommandot ombeds du att ange ett lösenord toobe skapas och sedan används en gång.</span><span class="sxs-lookup"><span data-stu-id="d5307-272">This command will prompt for a password toobe created and then used once.</span></span> <span data-ttu-id="d5307-273">Använd ett starkt lösenord.</span><span class="sxs-lookup"><span data-stu-id="d5307-273">Use a strong password.</span></span>

## <a name="create-pfx-files-for-client-certificates"></a><span data-ttu-id="d5307-274">Skapa PFX-filer för klienten certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-274">Create PFX files for client certificates</span></span>
<span data-ttu-id="d5307-275">För varje genererade klientcertifikat, kör du:</span><span class="sxs-lookup"><span data-stu-id="d5307-275">For each generated client certificate, execute:</span></span>

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

<span data-ttu-id="d5307-276">Anpassa:</span><span class="sxs-lookup"><span data-stu-id="d5307-276">Customizing:</span></span>

    MyID.pvk and MyID.cer with hello filename for hello client certificate

<span data-ttu-id="d5307-277">Ange lösenordet och sedan exportera certifikat med dessa alternativ:</span><span class="sxs-lookup"><span data-stu-id="d5307-277">Enter password and then export certificate with these options:</span></span>

* <span data-ttu-id="d5307-278">Ja, exportera hello privat nyckel</span><span class="sxs-lookup"><span data-stu-id="d5307-278">Yes, export hello private key</span></span>
* <span data-ttu-id="d5307-279">Exportera alla utökade egenskaper</span><span class="sxs-lookup"><span data-stu-id="d5307-279">Export all extended properties</span></span>
* <span data-ttu-id="d5307-280">hello enskilda toowhom detta certifikat utfärdas ska välja hello export lösenord</span><span class="sxs-lookup"><span data-stu-id="d5307-280">hello individual toowhom this certificate is being issued should choose hello export password</span></span>

## <a name="import-client-certificate"></a><span data-ttu-id="d5307-281">Importera klientcertifikatet</span><span class="sxs-lookup"><span data-stu-id="d5307-281">Import client certificate</span></span>
<span data-ttu-id="d5307-282">Varje enskild person som ett certifikat har utfärdats ska importera hello nyckelpar i hello datorer han eller hon använda toocommunicate med hello-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="d5307-282">Each individual for whom a client certificate has been issued should import hello key pair in hello machines he/she will use toocommunicate with hello service:</span></span>

* <span data-ttu-id="d5307-283">Dubbelklicka på hello. PFX-filen i Utforskaren i Windows</span><span class="sxs-lookup"><span data-stu-id="d5307-283">Double-click hello .PFX file in Windows Explorer</span></span>
* <span data-ttu-id="d5307-284">Importera certifikatet till hello personligt arkiv med minst det här alternativet:</span><span class="sxs-lookup"><span data-stu-id="d5307-284">Import certificate into hello Personal store with at least this option:</span></span>
  * <span data-ttu-id="d5307-285">Inkludera alla utökade egenskaper markerat</span><span class="sxs-lookup"><span data-stu-id="d5307-285">Include all extended properties checked</span></span>

## <a name="copy-client-certificate-thumbprints"></a><span data-ttu-id="d5307-286">Kopiera certifikattumavtryck för klienten</span><span class="sxs-lookup"><span data-stu-id="d5307-286">Copy client certificate thumbprints</span></span>
<span data-ttu-id="d5307-287">Varje enskild person som ett certifikat har utfärdats måste följa stegen i ordning tooobtain hello tumavtryck his/hers certifikat som kommer att läggas till toohello tjänstkonfigurationsfilen:</span><span class="sxs-lookup"><span data-stu-id="d5307-287">Each individual for whom a client certificate has been issued must follow these steps in order tooobtain hello thumbprint of his/hers certificate which will be added toohello service configuration file:</span></span>

* <span data-ttu-id="d5307-288">Kör certmgr.exe</span><span class="sxs-lookup"><span data-stu-id="d5307-288">Run certmgr.exe</span></span>
* <span data-ttu-id="d5307-289">Välj fliken för hello personlig</span><span class="sxs-lookup"><span data-stu-id="d5307-289">Select hello Personal tab</span></span>
* <span data-ttu-id="d5307-290">Dubbelklicka på hello klientcertifikat toobe används för autentisering</span><span class="sxs-lookup"><span data-stu-id="d5307-290">Double-click hello client certificate toobe used for authentication</span></span>
* <span data-ttu-id="d5307-291">I hello certifikat dialogruta som öppnas väljer du fliken för hello-information</span><span class="sxs-lookup"><span data-stu-id="d5307-291">In hello Certificate dialog that opens, select hello Details tab</span></span>
* <span data-ttu-id="d5307-292">Se till att visa visas alla</span><span class="sxs-lookup"><span data-stu-id="d5307-292">Make sure Show is displaying All</span></span>
* <span data-ttu-id="d5307-293">Välj hello fält med namnet tumavtrycket i hello lista</span><span class="sxs-lookup"><span data-stu-id="d5307-293">Select hello field named Thumbprint in hello list</span></span>
* <span data-ttu-id="d5307-294">Kopiera hello värdet för hello tumavtrycket ** ta bort icke-synliga Unicode-tecken framför hello första siffran ** ta bort alla blanksteg</span><span class="sxs-lookup"><span data-stu-id="d5307-294">Copy hello value of hello thumbprint ** Delete non-visible Unicode characters in front of hello first digit ** Delete all spaces</span></span>

## <a name="configure-allowed-clients-in-hello-service-configuration-file"></a><span data-ttu-id="d5307-295">Konfigurera tillåtna klienter i konfigurationsfilen för hello-tjänsten</span><span class="sxs-lookup"><span data-stu-id="d5307-295">Configure Allowed clients in hello service configuration file</span></span>
<span data-ttu-id="d5307-296">Uppdatera hello värdet för hello följande inställning i hello tjänstkonfigurationsfil med en kommaavgränsad lista över hello tumavtryck av hello klientcertifikat tillåten toohello-tjänsten för dataåtkomst:</span><span class="sxs-lookup"><span data-stu-id="d5307-296">Update hello value of hello following setting in hello service configuration file with a comma-separated list of hello thumbprints of hello client certificates allowed access toohello service:</span></span>

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a><span data-ttu-id="d5307-297">Konfigurera klienten Återkallningskontrollen för lövcertifikatet</span><span class="sxs-lookup"><span data-stu-id="d5307-297">Configure client certificate revocation check</span></span>
<span data-ttu-id="d5307-298">hello standardinställningen kontrollerar inte med hello certifikatutfärdare för certifikatåterkallningsstatus för klienten.</span><span class="sxs-lookup"><span data-stu-id="d5307-298">hello default setting does not check with hello Certification Authority for client certificate revocation status.</span></span> <span data-ttu-id="d5307-299">tooturn på hello kontrollerar om hello certifikatutfärdare som utfärdade hello klientcertifikat har stöd för dessa kontroller, ändra hello följande inställning med något av hello värden som definierats i hello X509RevocationMode uppräkningen:</span><span class="sxs-lookup"><span data-stu-id="d5307-299">tooturn on hello checks, if hello Certification Authority which issued hello client certificates supports such checks, change hello following setting with one of hello values defined in hello X509RevocationMode Enumeration:</span></span>

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a><span data-ttu-id="d5307-300">Skapa PFX-filen för självsignerade krypteringscertifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-300">Create PFX file for self-signed encryption certificates</span></span>
<span data-ttu-id="d5307-301">För ett certifikat för kryptering, kör du:</span><span class="sxs-lookup"><span data-stu-id="d5307-301">For an encryption certificate, execute:</span></span>

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

<span data-ttu-id="d5307-302">Anpassa:</span><span class="sxs-lookup"><span data-stu-id="d5307-302">Customizing:</span></span>

    MyID.pvk and MyID.cer with hello filename for hello encryption certificate

<span data-ttu-id="d5307-303">Ange lösenordet och sedan exportera certifikat med dessa alternativ:</span><span class="sxs-lookup"><span data-stu-id="d5307-303">Enter password and then export certificate with these options:</span></span>

* <span data-ttu-id="d5307-304">Ja, exportera hello privat nyckel</span><span class="sxs-lookup"><span data-stu-id="d5307-304">Yes, export hello private key</span></span>
* <span data-ttu-id="d5307-305">Exportera alla utökade egenskaper</span><span class="sxs-lookup"><span data-stu-id="d5307-305">Export all extended properties</span></span>
* <span data-ttu-id="d5307-306">Du behöver hello lösenord när du överför hello certifikat toohello Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="d5307-306">You will need hello password when uploading hello certificate toohello cloud service.</span></span>

## <a name="export-encryption-certificate-from-certificate-store"></a><span data-ttu-id="d5307-307">Exportera krypteringscertifikat från certifikatarkivet</span><span class="sxs-lookup"><span data-stu-id="d5307-307">Export encryption certificate from certificate store</span></span>
* <span data-ttu-id="d5307-308">Hitta certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-308">Find certificate</span></span>
* <span data-ttu-id="d5307-309">Klicka på Åtgärder -> alla uppgifter -> Exportera...</span><span class="sxs-lookup"><span data-stu-id="d5307-309">Click Actions -> All tasks -> Export…</span></span>
* <span data-ttu-id="d5307-310">Exportera certifikatet till en. PFX-filen med dessa alternativ:</span><span class="sxs-lookup"><span data-stu-id="d5307-310">Export certificate into a .PFX file with these options:</span></span> 
  * <span data-ttu-id="d5307-311">Ja, exportera hello privat nyckel</span><span class="sxs-lookup"><span data-stu-id="d5307-311">Yes, export hello private key</span></span>
  * <span data-ttu-id="d5307-312">Inkludera om möjligt alla certifikat i certifieringssökvägen hello</span><span class="sxs-lookup"><span data-stu-id="d5307-312">Include all certificates in hello certification path if possible</span></span> 
* <span data-ttu-id="d5307-313">Exportera alla utökade egenskaper</span><span class="sxs-lookup"><span data-stu-id="d5307-313">Export all extended properties</span></span>

## <a name="upload-encryption-certificate-toocloud-service"></a><span data-ttu-id="d5307-314">Överför krypteringstjänsten certifikat toocloud</span><span class="sxs-lookup"><span data-stu-id="d5307-314">Upload encryption certificate toocloud service</span></span>
<span data-ttu-id="d5307-315">Överför certifikatet med hello befintliga eller genereras. PFX-filen med hello krypteringsnyckelpar:</span><span class="sxs-lookup"><span data-stu-id="d5307-315">Upload certificate with hello existing or generated .PFX file with hello encryption key pair:</span></span>

* <span data-ttu-id="d5307-316">Ange hello lösenord som skyddar hello information om privat nyckel</span><span class="sxs-lookup"><span data-stu-id="d5307-316">Enter hello password protecting hello private key information</span></span>

## <a name="update-encryption-certificate-in-service-configuration-file"></a><span data-ttu-id="d5307-317">Uppdatera certifikat för kryptering i konfigurationsfilen för tjänsten</span><span class="sxs-lookup"><span data-stu-id="d5307-317">Update encryption certificate in service configuration file</span></span>
<span data-ttu-id="d5307-318">Hello tumavtrycksvärde på hello följande inställningar i hello tjänstkonfigurationsfil med hello hello certifikatets tumavtryck har överförts toohello Molntjänsten:</span><span class="sxs-lookup"><span data-stu-id="d5307-318">Update hello thumbprint value of hello following settings in hello service configuration file with hello thumbprint of hello certificate uploaded toohello cloud service:</span></span>

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a><span data-ttu-id="d5307-319">Vanliga certifikatåtgärder</span><span class="sxs-lookup"><span data-stu-id="d5307-319">Common certificate operations</span></span>
* <span data-ttu-id="d5307-320">Konfigurera hello SSL-certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-320">Configure hello SSL certificate</span></span>
* <span data-ttu-id="d5307-321">Konfigurera klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-321">Configure client certificates</span></span>

## <a name="find-certificate"></a><span data-ttu-id="d5307-322">Hitta certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-322">Find certificate</span></span>
<span data-ttu-id="d5307-323">Följ de här stegen:</span><span class="sxs-lookup"><span data-stu-id="d5307-323">Follow these steps:</span></span>

1. <span data-ttu-id="d5307-324">Kör mmc.exe.</span><span class="sxs-lookup"><span data-stu-id="d5307-324">Run mmc.exe.</span></span>
2. <span data-ttu-id="d5307-325">Arkiv -> Lägg till/ta bort snapin-modulen...</span><span class="sxs-lookup"><span data-stu-id="d5307-325">File -> Add/Remove Snap-in…</span></span>
3. <span data-ttu-id="d5307-326">Välj **certifikat**.</span><span class="sxs-lookup"><span data-stu-id="d5307-326">Select **Certificates**.</span></span>
4. <span data-ttu-id="d5307-327">Klicka på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="d5307-327">Click **Add**.</span></span>
5. <span data-ttu-id="d5307-328">Välj lagringsplats för hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="d5307-328">Choose hello certificate store location.</span></span>
6. <span data-ttu-id="d5307-329">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="d5307-329">Click **Finish**.</span></span>
7. <span data-ttu-id="d5307-330">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d5307-330">Click **OK**.</span></span>
8. <span data-ttu-id="d5307-331">Expandera **certifikat**.</span><span class="sxs-lookup"><span data-stu-id="d5307-331">Expand **Certificates**.</span></span>
9. <span data-ttu-id="d5307-332">Expandera hello certificate store nod.</span><span class="sxs-lookup"><span data-stu-id="d5307-332">Expand hello certificate store node.</span></span>
10. <span data-ttu-id="d5307-333">Expandera hello certifikat underordnad nod.</span><span class="sxs-lookup"><span data-stu-id="d5307-333">Expand hello Certificate child node.</span></span>
11. <span data-ttu-id="d5307-334">Välj ett certifikat i hello-listan.</span><span class="sxs-lookup"><span data-stu-id="d5307-334">Select a certificate in hello list.</span></span>

## <a name="export-certificate"></a><span data-ttu-id="d5307-335">Exportera certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-335">Export certificate</span></span>
<span data-ttu-id="d5307-336">I hello **guiden Exportera certifikat**:</span><span class="sxs-lookup"><span data-stu-id="d5307-336">In hello **Certificate Export Wizard**:</span></span>

1. <span data-ttu-id="d5307-337">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="d5307-337">Click **Next**.</span></span>
2. <span data-ttu-id="d5307-338">Välj **Ja**, sedan **Export hello privata nyckeln**.</span><span class="sxs-lookup"><span data-stu-id="d5307-338">Select **Yes**, then **Export hello private key**.</span></span>
3. <span data-ttu-id="d5307-339">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="d5307-339">Click **Next**.</span></span>
4. <span data-ttu-id="d5307-340">Välj filformat för hello önskade utdata.</span><span class="sxs-lookup"><span data-stu-id="d5307-340">Select hello desired output file format.</span></span>
5. <span data-ttu-id="d5307-341">Kontrollera hello önskat alternativ.</span><span class="sxs-lookup"><span data-stu-id="d5307-341">Check hello desired options.</span></span>
6. <span data-ttu-id="d5307-342">Kontrollera **lösenord**.</span><span class="sxs-lookup"><span data-stu-id="d5307-342">Check **Password**.</span></span>
7. <span data-ttu-id="d5307-343">Ange ett starkt lösenord och bekräfta.</span><span class="sxs-lookup"><span data-stu-id="d5307-343">Enter a strong password and confirm it.</span></span>
8. <span data-ttu-id="d5307-344">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="d5307-344">Click **Next**.</span></span>
9. <span data-ttu-id="d5307-345">Skriv eller bläddra fram ett filnamn där toostore hello certifikat (använder en. Filnamnstillägget PFX).</span><span class="sxs-lookup"><span data-stu-id="d5307-345">Type or browse a filename where toostore hello certificate (use a .PFX extension).</span></span>
10. <span data-ttu-id="d5307-346">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="d5307-346">Click **Next**.</span></span>
11. <span data-ttu-id="d5307-347">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="d5307-347">Click **Finish**.</span></span>
12. <span data-ttu-id="d5307-348">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d5307-348">Click **OK**.</span></span>

## <a name="import-certificate"></a><span data-ttu-id="d5307-349">Importera certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-349">Import certificate</span></span>
<span data-ttu-id="d5307-350">I hello guiden Importera certifikat:</span><span class="sxs-lookup"><span data-stu-id="d5307-350">In hello Certificate Import Wizard:</span></span>

1. <span data-ttu-id="d5307-351">Välj hello lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="d5307-351">Select hello store location.</span></span>
   
   * <span data-ttu-id="d5307-352">Välj **aktuell användare** om endast processer som körs under aktuell användare kommer åt hello-tjänsten</span><span class="sxs-lookup"><span data-stu-id="d5307-352">Select **Current User** if only processes running under current user will access hello service</span></span>
   * <span data-ttu-id="d5307-353">Välj **lokal dator** om andra processer i den här datorn ansluter till hello-tjänsten</span><span class="sxs-lookup"><span data-stu-id="d5307-353">Select **Local Machine** if other processes in this computer will access hello service</span></span>
2. <span data-ttu-id="d5307-354">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="d5307-354">Click **Next**.</span></span>
3. <span data-ttu-id="d5307-355">Om du importerar från en fil, bekräfta hello filsökväg.</span><span class="sxs-lookup"><span data-stu-id="d5307-355">If importing from a file, confirm hello file path.</span></span>
4. <span data-ttu-id="d5307-356">Om du importerar en. PFX-filen:</span><span class="sxs-lookup"><span data-stu-id="d5307-356">If importing a .PFX file:</span></span>
   1. <span data-ttu-id="d5307-357">Ange hello lösenord som skyddar hello privat nyckel</span><span class="sxs-lookup"><span data-stu-id="d5307-357">Enter hello password protecting hello private key</span></span>
   2. <span data-ttu-id="d5307-358">Välj alternativ för import</span><span class="sxs-lookup"><span data-stu-id="d5307-358">Select import options</span></span>
5. <span data-ttu-id="d5307-359">Välj ”plats”-certifikat i hello följande store</span><span class="sxs-lookup"><span data-stu-id="d5307-359">Select "Place" certificates in hello following store</span></span>
6. <span data-ttu-id="d5307-360">Klicka på **Browse** (Bläddra).</span><span class="sxs-lookup"><span data-stu-id="d5307-360">Click **Browse**.</span></span>
7. <span data-ttu-id="d5307-361">Välj önskad hello-arkivet.</span><span class="sxs-lookup"><span data-stu-id="d5307-361">Select hello desired store.</span></span>
8. <span data-ttu-id="d5307-362">Klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="d5307-362">Click **Finish**.</span></span>
   
   * <span data-ttu-id="d5307-363">Om hello betrodda rotcertifikatutfärdare har valts klickar du på **Ja**.</span><span class="sxs-lookup"><span data-stu-id="d5307-363">If hello Trusted Root Certification Authority store was chosen, click **Yes**.</span></span>
9. <span data-ttu-id="d5307-364">Klicka på **OK** på alla fönster för dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d5307-364">Click **OK** on all dialog windows.</span></span>

## <a name="upload-certificate"></a><span data-ttu-id="d5307-365">Överför certifikat</span><span class="sxs-lookup"><span data-stu-id="d5307-365">Upload certificate</span></span>
<span data-ttu-id="d5307-366">I hello [Azure-portalen](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="d5307-366">In hello [Azure Portal](https://portal.azure.com/)</span></span>

1. <span data-ttu-id="d5307-367">Välj **molntjänster**.</span><span class="sxs-lookup"><span data-stu-id="d5307-367">Select **Cloud Services**.</span></span>
2. <span data-ttu-id="d5307-368">Välj hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="d5307-368">Select hello cloud service.</span></span>
3. <span data-ttu-id="d5307-369">På hello översta menyn **certifikat**.</span><span class="sxs-lookup"><span data-stu-id="d5307-369">On hello top menu, click **Certificates**.</span></span>
4. <span data-ttu-id="d5307-370">Klicka på hello nedre **överför**.</span><span class="sxs-lookup"><span data-stu-id="d5307-370">On hello bottom bar, click **Upload**.</span></span>
5. <span data-ttu-id="d5307-371">Välj hello certifikatfil.</span><span class="sxs-lookup"><span data-stu-id="d5307-371">Select hello certificate file.</span></span>
6. <span data-ttu-id="d5307-372">Om det är en. PFX-fil, ange hello lösenord för privat nyckel för hello.</span><span class="sxs-lookup"><span data-stu-id="d5307-372">If it is a .PFX file, enter hello password for hello private key.</span></span>
7. <span data-ttu-id="d5307-373">Kopiera hello certifikatets tumavtryck från hello ny post i listan hello när klar.</span><span class="sxs-lookup"><span data-stu-id="d5307-373">Once completed, copy hello certificate thumbprint from hello new entry in hello list.</span></span>

## <a name="other-security-considerations"></a><span data-ttu-id="d5307-374">Andra säkerhetsaspekter</span><span class="sxs-lookup"><span data-stu-id="d5307-374">Other security considerations</span></span>
<span data-ttu-id="d5307-375">hello SSL-inställningarna som beskrivs i det här dokumentet kryptera kommunikationen mellan hello service och dess klienter när hello HTTPS-slutpunkt används.</span><span class="sxs-lookup"><span data-stu-id="d5307-375">hello SSL settings described in this document encrypt communication between hello service and its clients when hello HTTPS endpoint is used.</span></span> <span data-ttu-id="d5307-376">Detta är viktigt eftersom autentiseringsuppgifter för åtkomst till databasen och eventuellt andra känslig information finns i hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="d5307-376">This is important since credentials for database access and potentially other sensitive information are contained in hello communication.</span></span> <span data-ttu-id="d5307-377">Observera dock att tjänsten hello kvarstår interna status, inklusive inloggningsuppgifter, i dess interna tabeller i hello Microsoft Azure SQL-databas som du har angett för lagring av metadata i din Microsoft Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="d5307-377">Note, however, that hello service persists internal status, including credentials, in its internal tables in hello Microsoft Azure SQL database that you have provided for metadata storage in your Microsoft Azure subscription.</span></span> <span data-ttu-id="d5307-378">Databasen har definierats som en del av hello följande inställning i din tjänstekonfigurationsfil (. CSCFG-fil):</span><span class="sxs-lookup"><span data-stu-id="d5307-378">That database was defined as part of hello following setting in your service configuration file (.CSCFG file):</span></span> 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

<span data-ttu-id="d5307-379">Autentiseringsuppgifterna som lagras i den här databasen krypteras.</span><span class="sxs-lookup"><span data-stu-id="d5307-379">Credentials stored in this database are encrypted.</span></span> <span data-ttu-id="d5307-380">Dock som bästa praxis bör du kontrollera att både webb- och arbetsroller roller för dina tjänstdistributioner hålls in toodate och säker som de har båda åtkomst toohello metadata-databasen och hello certifikatet som används för kryptering och dekryptering av lagrade autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="d5307-380">However, as a best practice, ensure that both web and worker roles of your service deployments are kept up toodate and secure as they both have access toohello metadata database and hello certificate used for encryption and decryption of stored credentials.</span></span> 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

