---
title: "Generera och exporterar certifikat för plats-till-plats: PowerShell: Azure | Microsoft Docs"
description: "Den här artikeln innehåller steg toocreate ett självsignerat rotcertifikat, exportera hello offentliga nyckel och generera klientcertifikat med hjälp av PowerShell på Windows 10."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 27b99f7c-50dc-4f88-8a6e-d60080819a43
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: 11dda015368cda5ce9799fcc4f01d7c542b84fe8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-powershell-on-windows-10"></a><span data-ttu-id="838f5-103">Generera och exporterar certifikat för plats-till-plats-anslutningar med hjälp av PowerShell på Windows 10</span><span class="sxs-lookup"><span data-stu-id="838f5-103">Generate and export certificates for Point-to-Site connections using PowerShell on Windows 10</span></span>

<span data-ttu-id="838f5-104">Punkt-till-plats-anslutningar använder certifikat tooauthenticate.</span><span class="sxs-lookup"><span data-stu-id="838f5-104">Point-to-Site connections use certificates tooauthenticate.</span></span> <span data-ttu-id="838f5-105">Den här artikeln visar hur rot toocreate ett självsignerat certifikat och generera klientcertifikat med hjälp av PowerShell på Windows 10.</span><span class="sxs-lookup"><span data-stu-id="838f5-105">This article shows you how toocreate a self-signed root certificate and generate client certificates using PowerShell on Windows 10.</span></span> <span data-ttu-id="838f5-106">Om du letar efter konfigurationssteg för punkt-till-plats, t.ex. hur tooupload rotcertifikat, Välj ett av hello ' Configure punkt-till-plats-artiklar från hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="838f5-106">If you are looking for Point-to-Site configuration steps, such as how tooupload root certificates, select one of hello 'Configure Point-to-Site' articles from hello following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="838f5-107">Skapa självsignerat certifikat - PowerShell</span><span class="sxs-lookup"><span data-stu-id="838f5-107">Create self-signed certificates - PowerShell</span></span>](vpn-gateway-certificates-point-to-site.md)
> * [<span data-ttu-id="838f5-108">Skapa självsignerat certifikat - MakeCert</span><span class="sxs-lookup"><span data-stu-id="838f5-108">Create self-signed certificates - MakeCert</span></span>](vpn-gateway-certificates-point-to-site-makecert.md)
> * [<span data-ttu-id="838f5-109">Konfigurera punkt-till-plats - Resource Manager - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="838f5-109">Configure Point-to-Site - Resource Manager - Azure portal</span></span>](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="838f5-110">Konfigurera punkt-till-plats - Resource Manager - PowerShell</span><span class="sxs-lookup"><span data-stu-id="838f5-110">Configure Point-to-Site - Resource Manager - PowerShell</span></span>](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [<span data-ttu-id="838f5-111">Konfigurera punkt-till-plats - klassisk - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="838f5-111">Configure Point-to-Site - Classic - Azure portal</span></span>](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 


<span data-ttu-id="838f5-112">Du måste utföra hello stegen i den här artikeln på en dator som kör Windows 10.</span><span class="sxs-lookup"><span data-stu-id="838f5-112">You must perform hello steps in this article on a computer running Windows 10.</span></span> <span data-ttu-id="838f5-113">hello PowerShell-cmdletar för att du använder toogenerate certifikat är en del av hello Windows 10-operativsystem och fungerar inte på andra versioner av Windows.</span><span class="sxs-lookup"><span data-stu-id="838f5-113">hello PowerShell cmdlets that you use toogenerate certificates are part of hello Windows 10 operating system and do not work on other versions of Windows.</span></span> <span data-ttu-id="838f5-114">hello Windows 10-dator är endast nödvändiga toogenerate hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="838f5-114">hello Windows 10 computer is only needed toogenerate hello certificates.</span></span> <span data-ttu-id="838f5-115">När hello certifikat skapas bör du överför dem. eller installera dem på alla operativsystem stöds för klientdatorer.</span><span class="sxs-lookup"><span data-stu-id="838f5-115">Once hello certificates are generated, you can upload them, or install them on any supported client operating system.</span></span> 

<span data-ttu-id="838f5-116">Om du inte har åtkomst tooa Windows 10-dator kan du använda [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) toogenerate certifikat.</span><span class="sxs-lookup"><span data-stu-id="838f5-116">If you do not have access tooa Windows 10 computer, you can use [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) toogenerate certificates.</span></span> <span data-ttu-id="838f5-117">hello certifikaten som du skapar med någon av metoderna kan installeras på någon [stöds](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) klientoperativsystem.</span><span class="sxs-lookup"><span data-stu-id="838f5-117">hello certificates that you generate using either method can be installed on any [supported](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) client operating system.</span></span>

## <span data-ttu-id="838f5-118"><a name="rootcert"></a>Skapa ett självsignerat rotcertifikat</span><span class="sxs-lookup"><span data-stu-id="838f5-118"><a name="rootcert"></a>Create a self-signed root certificate</span></span>

<span data-ttu-id="838f5-119">Använd hello ny SelfSignedCertificate cmdlet toocreate ett självsignerat rotcertifikat.</span><span class="sxs-lookup"><span data-stu-id="838f5-119">Use hello New-SelfSignedCertificate cmdlet toocreate a self-signed root certificate.</span></span> <span data-ttu-id="838f5-120">Ytterligare parameterinformation finns [ny SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="838f5-120">For additional parameter information, see [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span></span>

1. <span data-ttu-id="838f5-121">Öppna en Windows PowerShell-konsol med utökade privilegier från en dator som kör Windows 10.</span><span class="sxs-lookup"><span data-stu-id="838f5-121">From a computer running Windows 10, open a Windows PowerShell console with elevated privileges.</span></span>
2. <span data-ttu-id="838f5-122">Använd följande exempel toocreate hello självsignerade rotcertifikat hello.</span><span class="sxs-lookup"><span data-stu-id="838f5-122">Use hello following example toocreate hello self-signed root certificate.</span></span> <span data-ttu-id="838f5-123">hello skapas följande exempel ett självsignerat rotcertifikat med namnet 'P2SRootCert' som installeras automatiskt i 'Certifikat-aktuell User\Personal\Certificates'.</span><span class="sxs-lookup"><span data-stu-id="838f5-123">hello following example creates a self-signed root certificate named 'P2SRootCert' that is automatically installed in 'Certificates-Current User\Personal\Certificates'.</span></span> <span data-ttu-id="838f5-124">Du kan visa hello certifikat genom att öppna *certmgr.msc*, eller *Hantera användarcertifikat*.</span><span class="sxs-lookup"><span data-stu-id="838f5-124">You can view hello certificate by opening *certmgr.msc*, or *Manage User Certificates*.</span></span>

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

### <span data-ttu-id="838f5-125"><a name="cer"></a>Exportera hello offentlig nyckel (.cer)</span><span class="sxs-lookup"><span data-stu-id="838f5-125"><a name="cer"></a>Export hello public key (.cer)</span></span>

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

<span data-ttu-id="838f5-126">Hej exported.cer filen måste vara överförda tooAzure.</span><span class="sxs-lookup"><span data-stu-id="838f5-126">hello exported.cer file must be uploaded tooAzure.</span></span> <span data-ttu-id="838f5-127">Instruktioner finns i [konfigurerar en punkt-till-plats-anslutning](vpn-gateway-howto-point-to-site-rm-ps.md#upload).</span><span class="sxs-lookup"><span data-stu-id="838f5-127">For instructions, see [Configure a Point-to-Site connection](vpn-gateway-howto-point-to-site-rm-ps.md#upload).</span></span> <span data-ttu-id="838f5-128">tooadd ett ytterligare betrodda rotcertifikat [i det här avsnittet](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) i hello artikel.</span><span class="sxs-lookup"><span data-stu-id="838f5-128">tooadd an additional trusted root certificate, [this section](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) of hello article.</span></span>

### <a name="export-hello-self-signed-root-certificate-and-public-key-toostore-it-optional"></a><span data-ttu-id="838f5-129">Exportera hello självsignerade rotcertifikat och offentliga nyckel toostore den (valfritt)</span><span class="sxs-lookup"><span data-stu-id="838f5-129">Export hello self-signed root certificate and public key toostore it (optional)</span></span>

<span data-ttu-id="838f5-130">Du kanske vill tooexport hello självsignerade rotcertifikat och lagra den på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="838f5-130">You may want tooexport hello self-signed root certificate and store it safely.</span></span> <span data-ttu-id="838f5-131">Om behövs bör du senare kan installera den på en annan dator och generera flera klientcertifikat eller exportera en annan .cer-fil.</span><span class="sxs-lookup"><span data-stu-id="838f5-131">If need be, you can later install it on another computer and generate more client certificates, or export another .cer file.</span></span> <span data-ttu-id="838f5-132">tooexport hello självsignerade rotcertifikat som en .pfx, Välj hello rotcertifikat och använda hello samma steg som beskrivs i [exportera ett certifikat för](#clientexport).</span><span class="sxs-lookup"><span data-stu-id="838f5-132">tooexport hello self-signed root certificate as a .pfx, select hello root certificate and use hello same steps as described in [Export a client certificate](#clientexport).</span></span>

## <span data-ttu-id="838f5-133"><a name="clientcert"></a>Generera ett klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="838f5-133"><a name="clientcert"></a>Generate a client certificate</span></span>

<span data-ttu-id="838f5-134">Varje klientdator som ansluter tooa VNet med punkt-till-plats måste ha ett certifikat installerat.</span><span class="sxs-lookup"><span data-stu-id="838f5-134">Each client computer that connects tooa VNet using Point-to-Site must have a client certificate installed.</span></span> <span data-ttu-id="838f5-135">Du generera ett klientcertifikat från hello självsignerat rotcertifikatet, och sedan exportera och installera hello klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="838f5-135">You generate a client certificate from hello self-signed root certificate, and then export and install hello client certificate.</span></span> <span data-ttu-id="838f5-136">Om hello klientcertifikatet inte är installerad, misslyckas autentiseringen.</span><span class="sxs-lookup"><span data-stu-id="838f5-136">If hello client certificate is not installed, authentication fails.</span></span> 

<span data-ttu-id="838f5-137">hello följande steg beskriver hur du genererar ett klientcertifikat från ett självsignerat rotcertifikat.</span><span class="sxs-lookup"><span data-stu-id="838f5-137">hello following steps walk you through generating a client certificate from a self-signed root certificate.</span></span> <span data-ttu-id="838f5-138">Du kan skapa flera klientcertifikat från hello samma rotcertifikat.</span><span class="sxs-lookup"><span data-stu-id="838f5-138">You may generate multiple client certificates from hello same root certificate.</span></span> <span data-ttu-id="838f5-139">När du skapar klientcertifikat med hjälp av hello stegen nedan installeras hello klientcertifikat automatiskt på hello dator som du använde toogenerate hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="838f5-139">When you generate client certificates using hello steps below, hello client certificate is automatically installed on hello computer that you used toogenerate hello certificate.</span></span> <span data-ttu-id="838f5-140">Om du vill tooinstall ett klientcertifikat på en annan dator, kan du exportera hello certifikat.</span><span class="sxs-lookup"><span data-stu-id="838f5-140">If you want tooinstall a client certificate on another client computer, you can export hello certificate.</span></span>

<span data-ttu-id="838f5-141">hello exemplen använder hello ny SelfSignedCertificate cmdlet toogenerate ett klientcertifikat som upphör att gälla i ett år.</span><span class="sxs-lookup"><span data-stu-id="838f5-141">hello examples use hello New-SelfSignedCertificate cmdlet toogenerate a client certificate that expires in one year.</span></span> <span data-ttu-id="838f5-142">Parametern för ytterligare information, till exempel inställningsvärde olika giltighetstid för hello klientcertifikatet finns [ny SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="838f5-142">For additional parameter information, such as setting a different expiration value for hello client certificate, see [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span></span>

### <a name="example-1"></a><span data-ttu-id="838f5-143">Exempel 1</span><span class="sxs-lookup"><span data-stu-id="838f5-143">Example 1</span></span>

<span data-ttu-id="838f5-144">Det här exemplet använder hello deklarerats '$cert' variabeln från hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="838f5-144">This example uses hello declared '$cert' variable from hello previous section.</span></span> <span data-ttu-id="838f5-145">Om du har stängt hello PowerShell-konsolen efter att skapa hello självsignerade rotcertifikat eller skapar ytterligare certifikat i en ny PowerShell-konsolsession använder hello stegen i [exempel 2](#ex2).</span><span class="sxs-lookup"><span data-stu-id="838f5-145">If you closed hello PowerShell console after creating hello self-signed root certificate, or are creating additional client certificates in a new PowerShell console session, use hello steps in [Example 2](#ex2).</span></span>

<span data-ttu-id="838f5-146">Ändra och köra hello exempel toogenerate ett klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="838f5-146">Modify and run hello example toogenerate a client certificate.</span></span> <span data-ttu-id="838f5-147">Om du kör följande exempel utan att ändra den hello är hello resultatet ett klientcertifikat med namnet 'P2SChildCert'.</span><span class="sxs-lookup"><span data-stu-id="838f5-147">If you run hello following example without modifying it, hello result is a client certificate named 'P2SChildCert'.</span></span>  <span data-ttu-id="838f5-148">Om du vill ha tooname hello underordnade certifikat något annat ändra hello CN-värdet.</span><span class="sxs-lookup"><span data-stu-id="838f5-148">If you want tooname hello child certificate something else, modify hello CN value.</span></span> <span data-ttu-id="838f5-149">Ändra inte hello TextExtension när du kör det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="838f5-149">Do not change hello TextExtension when running this example.</span></span> <span data-ttu-id="838f5-150">hello-klientcertifikat som du skapar installeras automatiskt i ”certifikat - aktuell User\Personal\Certificates” på datorn.</span><span class="sxs-lookup"><span data-stu-id="838f5-150">hello client certificate that you generate is automatically installed in 'Certificates - Current User\Personal\Certificates' on your computer.</span></span>

```powershell
New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <span data-ttu-id="838f5-151"><a name="ex2"></a>Exempel 2</span><span class="sxs-lookup"><span data-stu-id="838f5-151"><a name="ex2"></a>Example 2</span></span>

<span data-ttu-id="838f5-152">Om du skapar ytterligare certifikat, eller är inte använder hello samma PowerShell-session som du använde toocreate din självsignerat rotcertifikatet, Använd hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="838f5-152">If you are creating additional client certificates, or are not using hello same PowerShell session that you used toocreate your self-signed root certificate, use hello following steps:</span></span>

1. <span data-ttu-id="838f5-153">Identifiera hello självsignerade rotcertifikat som är installerad på hello-dator.</span><span class="sxs-lookup"><span data-stu-id="838f5-153">Identify hello self-signed root certificate that is installed on hello computer.</span></span> <span data-ttu-id="838f5-154">Denna cmdlet returnerar en lista över certifikat som är installerade på datorn.</span><span class="sxs-lookup"><span data-stu-id="838f5-154">This cmdlet returns a list of certificates that are installed on your computer.</span></span>

  ```powershell
  Get-ChildItem -Path “Cert:\CurrentUser\My”
  ```
2. <span data-ttu-id="838f5-155">Leta upp hello ämnesnamnet från hello returnerade lista och sedan kopiera hello tumavtryck som är placerad nästa tooit tooa text filen.</span><span class="sxs-lookup"><span data-stu-id="838f5-155">Locate hello subject name from hello returned list, then copy hello thumbprint that is located next tooit tooa text file.</span></span> <span data-ttu-id="838f5-156">I följande exempel hello, finns det två certifikat.</span><span class="sxs-lookup"><span data-stu-id="838f5-156">In hello following example, there are two certificates.</span></span> <span data-ttu-id="838f5-157">hello CN-namn är hello namn hello självsignerade rotcertifikat som du vill toogenerate ett underordnat certifikat.</span><span class="sxs-lookup"><span data-stu-id="838f5-157">hello CN name is hello name of hello self-signed root certificate from which you want toogenerate a child certificate.</span></span> <span data-ttu-id="838f5-158">I det här fallet 'P2SRootCert'.</span><span class="sxs-lookup"><span data-stu-id="838f5-158">In this case, 'P2SRootCert'.</span></span>

  ```
  Thumbprint                                Subject
  
  AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
  7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert
  ```
3. <span data-ttu-id="838f5-159">Deklarera en variabel för hello rotcertifikat använder hello tumavtryck hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="838f5-159">Declare a variable for hello root certificate using hello thumbprint from hello previous step.</span></span> <span data-ttu-id="838f5-160">Ersätt TUMAVTRYCKET med hello tumavtryck hello rotcertifikatet som du vill toogenerate ett underordnat certifikat.</span><span class="sxs-lookup"><span data-stu-id="838f5-160">Replace THUMBPRINT with hello thumbprint of hello root certificate from which you want toogenerate a child certificate.</span></span>

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
  ```

  <span data-ttu-id="838f5-161">Till exempel använder hello tumavtrycket för P2SRootCert i hello föregående steg, hello variabeln ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="838f5-161">For example, using hello thumbprint for P2SRootCert in hello previous step, hello variable looks like this:</span></span>

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
  ```
4.  <span data-ttu-id="838f5-162">Ändra och köra hello exempel toogenerate ett klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="838f5-162">Modify and run hello example toogenerate a client certificate.</span></span> <span data-ttu-id="838f5-163">Om du kör följande exempel utan att ändra den hello är hello resultatet ett klientcertifikat med namnet 'P2SChildCert'.</span><span class="sxs-lookup"><span data-stu-id="838f5-163">If you run hello following example without modifying it, hello result is a client certificate named 'P2SChildCert'.</span></span> <span data-ttu-id="838f5-164">Om du vill ha tooname hello underordnade certifikat något annat ändra hello CN-värdet.</span><span class="sxs-lookup"><span data-stu-id="838f5-164">If you want tooname hello child certificate something else, modify hello CN value.</span></span> <span data-ttu-id="838f5-165">Ändra inte hello TextExtension när du kör det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="838f5-165">Do not change hello TextExtension when running this example.</span></span> <span data-ttu-id="838f5-166">hello-klientcertifikat som du skapar installeras automatiskt i ”certifikat - aktuell User\Personal\Certificates” på datorn.</span><span class="sxs-lookup"><span data-stu-id="838f5-166">hello client certificate that you generate is automatically installed in 'Certificates - Current User\Personal\Certificates' on your computer.</span></span>

  ```powershell
  New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <span data-ttu-id="838f5-167"><a name="clientexport"></a>Exportera ett certifikat</span><span class="sxs-lookup"><span data-stu-id="838f5-167"><a name="clientexport"></a>Export a client certificate</span></span>   

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

## <span data-ttu-id="838f5-168"><a name="install"></a>Installera en exporterade klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="838f5-168"><a name="install"></a>Install an exported client certificate</span></span>

[!INCLUDE [Install client certificate](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="next-steps"></a><span data-ttu-id="838f5-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="838f5-169">Next steps</span></span>

<span data-ttu-id="838f5-170">Vill du fortsätta med konfigurationen punkt-till-plats.</span><span class="sxs-lookup"><span data-stu-id="838f5-170">Continue with your Point-to-Site configuration.</span></span> 

* <span data-ttu-id="838f5-171">För **Resource Manager** modell distributionsstegen, se [konfigurerar en punkt-till-plats-anslutning tooa VNet](vpn-gateway-howto-point-to-site-resource-manager-portal.md).</span><span class="sxs-lookup"><span data-stu-id="838f5-171">For **Resource Manager** deployment model steps, see [Configure a Point-to-Site connection tooa VNet](vpn-gateway-howto-point-to-site-resource-manager-portal.md).</span></span> 
* <span data-ttu-id="838f5-172">För **klassiska** modell distributionsstegen, se [konfigurerar en punkt-till-plats VPN-anslutningen tooa virtuella nätverk (klassiska)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="838f5-172">For **classic** deployment model steps, see [Configure a Point-to-Site VPN connection tooa VNet (classic)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).</span></span>
