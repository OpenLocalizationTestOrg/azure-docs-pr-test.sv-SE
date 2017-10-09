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
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-powershell-on-windows-10"></a>Generera och exporterar certifikat för plats-till-plats-anslutningar med hjälp av PowerShell på Windows 10

Punkt-till-plats-anslutningar använder certifikat tooauthenticate. Den här artikeln visar hur rot toocreate ett självsignerat certifikat och generera klientcertifikat med hjälp av PowerShell på Windows 10. Om du letar efter konfigurationssteg för punkt-till-plats, t.ex. hur tooupload rotcertifikat, Välj ett av hello ' Configure punkt-till-plats-artiklar från hello följande lista:

> [!div class="op_single_selector"]
> * [Skapa självsignerat certifikat - PowerShell](vpn-gateway-certificates-point-to-site.md)
> * [Skapa självsignerat certifikat - MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)
> * [Konfigurera punkt-till-plats - Resource Manager - Azure-portalen](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Konfigurera punkt-till-plats - Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Konfigurera punkt-till-plats - klassisk - Azure-portalen](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 


Du måste utföra hello stegen i den här artikeln på en dator som kör Windows 10. hello PowerShell-cmdletar för att du använder toogenerate certifikat är en del av hello Windows 10-operativsystem och fungerar inte på andra versioner av Windows. hello Windows 10-dator är endast nödvändiga toogenerate hello certifikat. När hello certifikat skapas bör du överför dem. eller installera dem på alla operativsystem stöds för klientdatorer. 

Om du inte har åtkomst tooa Windows 10-dator kan du använda [MakeCert](vpn-gateway-certificates-point-to-site-makecert.md) toogenerate certifikat. hello certifikaten som du skapar med någon av metoderna kan installeras på någon [stöds](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq) klientoperativsystem.

## <a name="rootcert"></a>Skapa ett självsignerat rotcertifikat

Använd hello ny SelfSignedCertificate cmdlet toocreate ett självsignerat rotcertifikat. Ytterligare parameterinformation finns [ny SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

1. Öppna en Windows PowerShell-konsol med utökade privilegier från en dator som kör Windows 10.
2. Använd följande exempel toocreate hello självsignerade rotcertifikat hello. hello skapas följande exempel ett självsignerat rotcertifikat med namnet 'P2SRootCert' som installeras automatiskt i 'Certifikat-aktuell User\Personal\Certificates'. Du kan visa hello certifikat genom att öppna *certmgr.msc*, eller *Hantera användarcertifikat*.

  ```powershell
  $cert = New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SRootCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" -KeyUsageProperty Sign -KeyUsage CertSign
  ```

### <a name="cer"></a>Exportera hello offentlig nyckel (.cer)

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

Hej exported.cer filen måste vara överförda tooAzure. Instruktioner finns i [konfigurerar en punkt-till-plats-anslutning](vpn-gateway-howto-point-to-site-rm-ps.md#upload). tooadd ett ytterligare betrodda rotcertifikat [i det här avsnittet](vpn-gateway-howto-point-to-site-rm-ps.md#addremovecert) i hello artikel.

### <a name="export-hello-self-signed-root-certificate-and-public-key-toostore-it-optional"></a>Exportera hello självsignerade rotcertifikat och offentliga nyckel toostore den (valfritt)

Du kanske vill tooexport hello självsignerade rotcertifikat och lagra den på ett säkert sätt. Om behövs bör du senare kan installera den på en annan dator och generera flera klientcertifikat eller exportera en annan .cer-fil. tooexport hello självsignerade rotcertifikat som en .pfx, Välj hello rotcertifikat och använda hello samma steg som beskrivs i [exportera ett certifikat för](#clientexport).

## <a name="clientcert"></a>Generera ett klientcertifikat

Varje klientdator som ansluter tooa VNet med punkt-till-plats måste ha ett certifikat installerat. Du generera ett klientcertifikat från hello självsignerat rotcertifikatet, och sedan exportera och installera hello klientcertifikat. Om hello klientcertifikatet inte är installerad, misslyckas autentiseringen. 

hello följande steg beskriver hur du genererar ett klientcertifikat från ett självsignerat rotcertifikat. Du kan skapa flera klientcertifikat från hello samma rotcertifikat. När du skapar klientcertifikat med hjälp av hello stegen nedan installeras hello klientcertifikat automatiskt på hello dator som du använde toogenerate hello certifikat. Om du vill tooinstall ett klientcertifikat på en annan dator, kan du exportera hello certifikat.

hello exemplen använder hello ny SelfSignedCertificate cmdlet toogenerate ett klientcertifikat som upphör att gälla i ett år. Parametern för ytterligare information, till exempel inställningsvärde olika giltighetstid för hello klientcertifikatet finns [ny SelfSignedCertificate](https://technet.microsoft.com/itpro/powershell/windows/pkiclient/new-selfsignedcertificate).

### <a name="example-1"></a>Exempel 1

Det här exemplet använder hello deklarerats '$cert' variabeln från hello föregående avsnitt. Om du har stängt hello PowerShell-konsolen efter att skapa hello självsignerade rotcertifikat eller skapar ytterligare certifikat i en ny PowerShell-konsolsession använder hello stegen i [exempel 2](#ex2).

Ändra och köra hello exempel toogenerate ett klientcertifikat. Om du kör följande exempel utan att ändra den hello är hello resultatet ett klientcertifikat med namnet 'P2SChildCert'.  Om du vill ha tooname hello underordnade certifikat något annat ändra hello CN-värdet. Ändra inte hello TextExtension när du kör det här exemplet. hello-klientcertifikat som du skapar installeras automatiskt i ”certifikat - aktuell User\Personal\Certificates” på datorn.

```powershell
New-SelfSignedCertificate -Type Custom -KeySpec Signature `
-Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
-HashAlgorithm sha256 -KeyLength 2048 `
-CertStoreLocation "Cert:\CurrentUser\My" `
-Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
```

### <a name="ex2"></a>Exempel 2

Om du skapar ytterligare certifikat, eller är inte använder hello samma PowerShell-session som du använde toocreate din självsignerat rotcertifikatet, Använd hello följande steg:

1. Identifiera hello självsignerade rotcertifikat som är installerad på hello-dator. Denna cmdlet returnerar en lista över certifikat som är installerade på datorn.

  ```powershell
  Get-ChildItem -Path “Cert:\CurrentUser\My”
  ```
2. Leta upp hello ämnesnamnet från hello returnerade lista och sedan kopiera hello tumavtryck som är placerad nästa tooit tooa text filen. I följande exempel hello, finns det två certifikat. hello CN-namn är hello namn hello självsignerade rotcertifikat som du vill toogenerate ett underordnat certifikat. I det här fallet 'P2SRootCert'.

  ```
  Thumbprint                                Subject
  
  AED812AD883826FF76B4D1D5A77B3C08EFA79F3F  CN=P2SChildCert4
  7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655  CN=P2SRootCert
  ```
3. Deklarera en variabel för hello rotcertifikat använder hello tumavtryck hello föregående steg. Ersätt TUMAVTRYCKET med hello tumavtryck hello rotcertifikatet som du vill toogenerate ett underordnat certifikat.

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\THUMBPRINT"
  ```

  Till exempel använder hello tumavtrycket för P2SRootCert i hello föregående steg, hello variabeln ser ut så här:

  ```powershell
  $cert = Get-ChildItem -Path "Cert:\CurrentUser\My\7181AA8C1B4D34EEDB2F3D3BEC5839F3FE52D655"
  ```
4.  Ändra och köra hello exempel toogenerate ett klientcertifikat. Om du kör följande exempel utan att ändra den hello är hello resultatet ett klientcertifikat med namnet 'P2SChildCert'. Om du vill ha tooname hello underordnade certifikat något annat ändra hello CN-värdet. Ändra inte hello TextExtension när du kör det här exemplet. hello-klientcertifikat som du skapar installeras automatiskt i ”certifikat - aktuell User\Personal\Certificates” på datorn.

  ```powershell
  New-SelfSignedCertificate -Type Custom -KeySpec Signature `
  -Subject "CN=P2SChildCert" -KeyExportPolicy Exportable `
  -HashAlgorithm sha256 -KeyLength 2048 `
  -CertStoreLocation "Cert:\CurrentUser\My" `
  -Signer $cert -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.2")
  ```

## <a name="clientexport"></a>Exportera ett certifikat   

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

## <a name="install"></a>Installera en exporterade klientcertifikat

[!INCLUDE [Install client certificate](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="next-steps"></a>Nästa steg

Vill du fortsätta med konfigurationen punkt-till-plats. 

* För **Resource Manager** modell distributionsstegen, se [konfigurerar en punkt-till-plats-anslutning tooa VNet](vpn-gateway-howto-point-to-site-resource-manager-portal.md). 
* För **klassiska** modell distributionsstegen, se [konfigurerar en punkt-till-plats VPN-anslutningen tooa virtuella nätverk (klassiska)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).
