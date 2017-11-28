---
title: "Generera och exporterar certifikat för plats-till-plats: PowerShell: Azure | Microsoft Docs"
description: "Den här artikeln innehåller steg för att skapa ett självsignerat rotcertifikat, exportera den offentliga nyckeln och generera klientcertifikat med hjälp av PowerShell på Windows 10."
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
ms.openlocfilehash: f96b9b212b9322d0677e49ff95184d0feccca2df
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-powershell-on-windows-10"></a><span data-ttu-id="c521d-103">Generera och exporterar certifikat för plats-till-plats-anslutningar med hjälp av PowerShell på Windows 10</span><span class="sxs-lookup"><span data-stu-id="c521d-103">Generate and export certificates for Point-to-Site connections using PowerShell on Windows 10</span></span>

<span data-ttu-id="c521d-104">Punkt-till-plats-anslutningar använder certifikat för autentisering.</span><span class="sxs-lookup"><span data-stu-id="c521d-104">Point-to-Site connections use certificates to authenticate.</span></span> <span data-ttu-id="c521d-105">Den här artikeln visar hur du skapar ett självsignerat rotcertifikat och generera klientcertifikat med hjälp av PowerShell på Windows 10.</span><span class="sxs-lookup"><span data-stu-id="c521d-105">This article shows you how to create a self-signed root certificate and generate client certificates using PowerShell on Windows 10.</span></span> <span data-ttu-id="c521d-106">Om du letar efter konfigurationssteg för punkt-till-plats, till exempel hur du överför rotcertifikat, väljer du en av artiklarna Configure punkt-till-platsen från listan nedan:</span><span class="sxs-lookup"><span data-stu-id="c521d-106">If you are looking for Point-to-Site configuration steps, such as how to upload root certificates, select one of the 'Configure Point-to-Site' articles from the following list:</span></span>

> [!div class="op_single_selector"]
> * [<span data-ttu-id="c521d-107">Skapa självsignerat certifikat - PowerShell</span><span class="sxs-lookup"><span data-stu-id="c521d-107">Create self-signed certificates - PowerShell</span></span>](vpn-gateway-certificates-point-to-site.md)
> * [<span data-ttu-id="c521d-108">Skapa självsignerat certifikat - MakeCert</span><span class="sxs-lookup"><span data-stu-id="c521d-108">Create self-signed certificates - MakeCert</span></span>](vpn-gateway-certificates-point-to-site-makecert.md)
> * [<span data-ttu-id="c521d-109">Konfigurera punkt-till-plats - Resource Manager - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c521d-109">Configure Point-to-Site - Resource Manager - Azure portal</span></span>](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [<span data-ttu-id="c521d-110">Konfigurera punkt-till-plats - Resource Manager - PowerShell</span><span class="sxs-lookup"><span data-stu-id="c521d-110">Configure Point-to-Site - Resource Manager - PowerShell</span></span>](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [<span data-ttu-id="c521d-111">Konfigurera punkt-till-plats - klassisk - Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="c521d-111">Configure Point-to-Site - Classic - Azure portal</span></span>](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 


<span data-ttu-id="c521d-112">Du måste utföra stegen i den här artikeln på en dator som kör Windows 10.</span><span class="sxs-lookup"><span data-stu-id="c521d-112">You must perform the steps in this article on a computer running Windows 10.</span></span> <span data-ttu-id="c521d-113">PowerShell-cmdlets som används för att generera certifikat är en del av operativsystemet Windows 10 och fungerar inte på andra versioner av Windows.</span><span class="sxs-lookup"><span data-stu-id="c521d-113">The PowerShell cmdlets that you use to generate certificates are part of the Windows 10 operating system and do not work on other versions of Windows.</span></span> <span data-ttu-id="c521d-114">Windows 10-dator krävs endast för att generera certifikat.</span><span class="sxs-lookup"><span data-stu-id="c521d-114">The Windows 10 computer is only needed to generate the certificates.</span></span> <span data-ttu-id="c521d-115">När certifikat som genereras kan du överför dem. eller installera dem på alla operativsystem stöds för klientdatorer.</span><span class="sxs-lookup"><span data-stu-id="c521d-115">Once the certificates are generated, you can upload them, or install them on any supported client operating system.</span></span> 

<span data-ttu-id="c521d-116">Om du inte har tillgång till en Windows 10-dator kan du använda [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) att generera certifikat.</span><span class="sxs-lookup"><span data-stu-id="c521d-116">If you do not have access to a Windows 10 computer, you can use [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) to generate certificates.</span></span> <span data-ttu-id="c521d-117">De certifikat som du skapar med någon av metoderna kan installeras på något [stöds](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) klientoperativsystem.</span><span class="sxs-lookup"><span data-stu-id="c521d-117">The certificates that you generate using either method can be installed on any [supported](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) client operating system.</span></span>

## <span data-ttu-id="c521d-118"><a name="rootcert"></a>Skapa ett självsignerat rotcertifikat</span><span class="sxs-lookup"><span data-stu-id="c521d-118"><a name="rootcert"></a>Create a self-signed root certificate</span></span>

<span data-ttu-id="c521d-119">Använd cmdleten New-SelfSignedCertificate för att skapa ett självsignerat rotcertifikat.</span><span class="sxs-lookup"><span data-stu-id="c521d-119">Use the New-SelfSignedCertificate cmdlet to create a self-signed root certificate.</span></span> <span data-ttu-id="c521d-120">Ytterligare parameterinformation finns [ny SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="c521d-120">For additional parameter information, see [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span></span>

1. <span data-ttu-id="c521d-121">Öppna en Windows PowerShell-konsol med utökade privilegier från en dator som kör Windows 10.</span><span class="sxs-lookup"><span data-stu-id="c521d-121">From a computer running Windows 10, open a Windows PowerShell console with elevated privileges.</span></span>
2. <span data-ttu-id="c521d-122">Använd följande exempel för att skapa självsignerat rotcertifikatet.</span><span class="sxs-lookup"><span data-stu-id="c521d-122">Use the following example to create the self-signed root certificate.</span></span> <span data-ttu-id="c521d-123">I följande exempel skapas ett självsignerat rotcertifikat med namnet 'P2SRootCert' som installeras automatiskt i 'Certifikat-aktuell User\Personal\Certificates'.</span><span class="sxs-lookup"><span data-stu-id="c521d-123">The following example creates a self-signed root certificate named 'P2SRootCert' that is automatically installed in 'Certificates-Current User\Personal\Certificates'.</span></span> <span data-ttu-id="c521d-124">Du kan visa certifikatet genom att öppna *certmgr.msc*, eller *Hantera användarcertifikat*.</span><span class="sxs-lookup"><span data-stu-id="c521d-124">You can view the certificate by opening *certmgr.msc*, or *Manage User Certificates*.</span></span>

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

### <span data-ttu-id="c521d-125"><a name="cer"></a>Exportera den offentliga nyckeln (.cer)</span><span class="sxs-lookup"><span data-stu-id="c521d-125"><a name="cer"></a>Export the public key (.cer)</span></span>

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

<span data-ttu-id="c521d-126">Filen exported.cer måste laddas upp till Azure.</span><span class="sxs-lookup"><span data-stu-id="c521d-126">The exported.cer file must be uploaded to Azure.</span></span> <span data-ttu-id="c521d-127">Instruktioner finns i [konfigurerar en punkt-till-plats-anslutning](vpn-gateway-howto-point-to-site-rm-ps.md#upload).</span><span class="sxs-lookup"><span data-stu-id="c521d-127">For instructions, see [Configure a Point-to-Site connection](vpn-gateway-howto-point-to-site-rm-ps.md#upload).</span></span> <span data-ttu-id="c521d-128">Att lägga till ett ytterligare betrodda rotcertifikat [i det här avsnittet](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) av artikeln.</span><span class="sxs-lookup"><span data-stu-id="c521d-128">To add an additional trusted root certificate, [this section](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) of the article.</span></span>

### <a name="export-the-self-signed-root-certificate-and-public-key-to-store-it-optional"></a><span data-ttu-id="c521d-129">Exportera självsignerade rotcertifikat och offentliga nyckel för att lagra den (valfritt)</span><span class="sxs-lookup"><span data-stu-id="c521d-129">Export the self-signed root certificate and public key to store it (optional)</span></span>

<span data-ttu-id="c521d-130">Du kanske vill exportera självsignerade rotcertifikat och lagra den på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="c521d-130">You may want to export the self-signed root certificate and store it safely.</span></span> <span data-ttu-id="c521d-131">Om behövs bör du senare kan installera den på en annan dator och generera flera klientcertifikat eller exportera en annan .cer-fil.</span><span class="sxs-lookup"><span data-stu-id="c521d-131">If need be, you can later install it on another computer and generate more client certificates, or export another .cer file.</span></span> <span data-ttu-id="c521d-132">Om du vill exportera självsignerade rotcertifikat som en PFX-fil, Välj rotcertifikatet och använda samma steg som beskrivs i [exportera ett certifikat för](#clientexport).</span><span class="sxs-lookup"><span data-stu-id="c521d-132">To export the self-signed root certificate as a .pfx, select the root certificate and use the same steps as described in [Export a client certificate](#clientexport).</span></span>

## <span data-ttu-id="c521d-133"><a name="clientcert"></a>Generera ett klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="c521d-133"><a name="clientcert"></a>Generate a client certificate</span></span>

<span data-ttu-id="c521d-134">Varje klientdator som ansluter till ett virtuellt nätverk med punkt-till-plats måste ha ett klientcertifikat installerat.</span><span class="sxs-lookup"><span data-stu-id="c521d-134">Each client computer that connects to a VNet using Point-to-Site must have a client certificate installed.</span></span> <span data-ttu-id="c521d-135">Du genererar ett klientcertifikat från självsignerade rotcertifikatet, och sedan exportera och installera klientcertifikatet.</span><span class="sxs-lookup"><span data-stu-id="c521d-135">You generate a client certificate from the self-signed root certificate, and then export and install the client certificate.</span></span> <span data-ttu-id="c521d-136">Om klientcertifikatet inte är installerad, misslyckas autentiseringen.</span><span class="sxs-lookup"><span data-stu-id="c521d-136">If the client certificate is not installed, authentication fails.</span></span> 

<span data-ttu-id="c521d-137">Följande steg vägleder dig genom att generera ett certifikat från ett självsignerat rotcertifikat.</span><span class="sxs-lookup"><span data-stu-id="c521d-137">The following steps walk you through generating a client certificate from a self-signed root certificate.</span></span> <span data-ttu-id="c521d-138">Du kan skapa flera klientcertifikat från samma rotcertifikat.</span><span class="sxs-lookup"><span data-stu-id="c521d-138">You may generate multiple client certificates from the same root certificate.</span></span> <span data-ttu-id="c521d-139">När du skapar klientcertifikat med nedanstående steg installeras automatiskt klientcertifikatet på den dator som du använde för att generera certifikatet.</span><span class="sxs-lookup"><span data-stu-id="c521d-139">When you generate client certificates using the steps below, the client certificate is automatically installed on the computer that you used to generate the certificate.</span></span> <span data-ttu-id="c521d-140">Om du vill installera ett klientcertifikat på en annan dator, kan du exportera certifikatet.</span><span class="sxs-lookup"><span data-stu-id="c521d-140">If you want to install a client certificate on another client computer, you can export the certificate.</span></span>

<span data-ttu-id="c521d-141">Exemplen använder cmdleten New-SelfSignedCertificate för att generera ett certifikat som upphör att gälla i ett år.</span><span class="sxs-lookup"><span data-stu-id="c521d-141">The examples use the New-SelfSignedCertificate cmdlet to generate a client certificate that expires in one year.</span></span> <span data-ttu-id="c521d-142">Parametern för ytterligare information, till exempel inställningsvärde olika giltighetstid för klientcertifikat, se [ny SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span><span class="sxs-lookup"><span data-stu-id="c521d-142">For additional parameter information, such as setting a different expiration value for the client certificate, see [New-SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).</span></span>

### <a name="example-1"></a><span data-ttu-id="c521d-143">Exempel 1</span><span class="sxs-lookup"><span data-stu-id="c521d-143">Example 1</span></span>

<span data-ttu-id="c521d-144">Det här exemplet använder variabeln deklarerade '$cert' från föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="c521d-144">This example uses the declared '$cert' variable from the previous section.</span></span> <span data-ttu-id="c521d-145">Om du stängt PowerShell-konsolen när du har skapat det självsignerade rotcertifikatet eller skapar ytterligare certifikat i en ny PowerShell-konsolsession, Följ stegen i [exempel 2](#ex2).</span><span class="sxs-lookup"><span data-stu-id="c521d-145">If you closed the PowerShell console after creating the self-signed root certificate, or are creating additional client certificates in a new PowerShell console session, use the steps in [Example 2](#ex2).</span></span>

<span data-ttu-id="c521d-146">Ändra och köra exemplet för att generera ett klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="c521d-146">Modify and run the example to generate a client certificate.</span></span> <span data-ttu-id="c521d-147">Om du kör följande exempel utan att ändra den är resultatet ett klientcertifikat med namnet 'P2SChildCert'.</span><span class="sxs-lookup"><span data-stu-id="c521d-147">If you run the following example without modifying it, the result is a client certificate named 'P2SChildCert'.</span></span>  <span data-ttu-id="c521d-148">Om du vill att namnge certifikatet underordnade något annat, ändra värdet för Nätverksnamnet.</span><span class="sxs-lookup"><span data-stu-id="c521d-148">If you want to name the child certificate something else, modify the CN value.</span></span> <span data-ttu-id="c521d-149">Ändra inte TextExtension när du kör det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="c521d-149">Do not change the TextExtension when running this example.</span></span> <span data-ttu-id="c521d-150">Det klientcertifikat som du genererar installeras automatiskt i ”certifikat - aktuell User\Personal\Certificates” på datorn.</span><span class="sxs-lookup"><span data-stu-id="c521d-150">The client certificate that you generate is automatically installed in 'Certificates - Current User\Personal\Certificates' on your computer.</span></span>

```powershell
New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <span data-ttu-id="c521d-151"><a name="ex2"></a>Exempel 2</span><span class="sxs-lookup"><span data-stu-id="c521d-151"><a name="ex2"></a>Example 2</span></span>

<span data-ttu-id="c521d-152">Om du skapar ytterligare klientcertifikat eller använder inte samma PowerShell-session som du använde för att skapa din självsignerade rotcertifikat, använder du följande steg:</span><span class="sxs-lookup"><span data-stu-id="c521d-152">If you are creating additional client certificates, or are not using the same PowerShell session that you used to create your self-signed root certificate, use the following steps:</span></span>

1. <span data-ttu-id="c521d-153">Identifiera det självsignerade rotcertifikat som är installerad på datorn.</span><span class="sxs-lookup"><span data-stu-id="c521d-153">Identify the self-signed root certificate that is installed on the computer.</span></span> <span data-ttu-id="c521d-154">Denna cmdlet returnerar en lista över certifikat som är installerade på datorn.</span><span class="sxs-lookup"><span data-stu-id="c521d-154">This cmdlet returns a list of certificates that are installed on your computer.</span></span>

  ```powershell
  Get-ChildItem -Path “Cert:\CurrentUser\My”
  ```
2. <span data-ttu-id="c521d-155">Hitta ämnesnamnet från listan returneras och sedan kopiera tumavtrycket bredvid den till en textfil.</span><span class="sxs-lookup"><span data-stu-id="c521d-155">Locate the subject name from the returned list, then copy the thumbprint that is located next to it to a text file.</span></span> <span data-ttu-id="c521d-156">I följande exempel finns två certifikat.</span><span class="sxs-lookup"><span data-stu-id="c521d-156">In the following example, there are two certificates.</span></span> <span data-ttu-id="c521d-157">CN-namn är namnet på självsignerat rotcertifikatet som du vill skapa ett underordnat certifikat.</span><span class="sxs-lookup"><span data-stu-id="c521d-157">The CN name is the name of the self-signed root certificate from which you want to generate a child certificate.</span></span> <span data-ttu-id="c521d-158">I det här fallet 'P2SRootCert'.</span><span class="sxs-lookup"><span data-stu-id="c521d-158">In this case, 'P2SRootCert'.</span></span>

  ```
  Thumbprint                                Subject
  
  AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
  7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert
  ```
3. <span data-ttu-id="c521d-159">Deklarera en variabel för rotcertifikatet använder certifikatets tumavtryck från föregående steg.</span><span class="sxs-lookup"><span data-stu-id="c521d-159">Declare a variable for the root certificate using the thumbprint from the previous step.</span></span> <span data-ttu-id="c521d-160">Ersätt TUMAVTRYCKET med tumavtrycket för rotcertifikatet som du vill skapa ett underordnat certifikat.</span><span class="sxs-lookup"><span data-stu-id="c521d-160">Replace THUMBPRINT with the thumbprint of the root certificate from which you want to generate a child certificate.</span></span>

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
  ```

  <span data-ttu-id="c521d-161">Till exempel använder certifikatets tumavtryck för P2SRootCert i föregående steg, variabeln ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="c521d-161">For example, using the thumbprint for P2SRootCert in the previous step, the variable looks like this:</span></span>

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
  ```
4.  <span data-ttu-id="c521d-162">Ändra och köra exemplet för att generera ett klientcertifikat.</span><span class="sxs-lookup"><span data-stu-id="c521d-162">Modify and run the example to generate a client certificate.</span></span> <span data-ttu-id="c521d-163">Om du kör följande exempel utan att ändra den är resultatet ett klientcertifikat med namnet 'P2SChildCert'.</span><span class="sxs-lookup"><span data-stu-id="c521d-163">If you run the following example without modifying it, the result is a client certificate named 'P2SChildCert'.</span></span> <span data-ttu-id="c521d-164">Om du vill att namnge certifikatet underordnade något annat, ändra värdet för Nätverksnamnet.</span><span class="sxs-lookup"><span data-stu-id="c521d-164">If you want to name the child certificate something else, modify the CN value.</span></span> <span data-ttu-id="c521d-165">Ändra inte TextExtension när du kör det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="c521d-165">Do not change the TextExtension when running this example.</span></span> <span data-ttu-id="c521d-166">Det klientcertifikat som du genererar installeras automatiskt i ”certifikat - aktuell User\Personal\Certificates” på datorn.</span><span class="sxs-lookup"><span data-stu-id="c521d-166">The client certificate that you generate is automatically installed in 'Certificates - Current User\Personal\Certificates' on your computer.</span></span>

  ```powershell
  New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <span data-ttu-id="c521d-167"><a name="clientexport"></a>Exportera ett certifikat</span><span class="sxs-lookup"><span data-stu-id="c521d-167"><a name="clientexport"></a>Export a client certificate</span></span>   

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

## <span data-ttu-id="c521d-168"><a name="install"></a>Installera en exporterade klientcertifikat</span><span class="sxs-lookup"><span data-stu-id="c521d-168"><a name="install"></a>Install an exported client certificate</span></span>

[!INCLUDE [Install client certificate](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="next-steps"></a><span data-ttu-id="c521d-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c521d-169">Next steps</span></span>

<span data-ttu-id="c521d-170">Vill du fortsätta med konfigurationen punkt-till-plats.</span><span class="sxs-lookup"><span data-stu-id="c521d-170">Continue with your Point-to-Site configuration.</span></span> 

* <span data-ttu-id="c521d-171">För **Resource Manager** modell distributionsstegen, se [konfigurerar en punkt-till-plats-anslutning till ett virtuellt nätverk](vpn-gateway-howto-point-to-site-resource-manager-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c521d-171">For **Resource Manager** deployment model steps, see [Configure a Point-to-Site connection to a VNet](vpn-gateway-howto-point-to-site-resource-manager-portal.md).</span></span> 
* <span data-ttu-id="c521d-172">För **klassiska** modell distributionsstegen, se [konfigurerar en punkt-till-plats VPN-anslutning till ett virtuellt nätverk (klassiska)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="c521d-172">For **classic** deployment model steps, see [Configure a Point-to-Site VPN connection to a VNet (classic)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).</span></span>