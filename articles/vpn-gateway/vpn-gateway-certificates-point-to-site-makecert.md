---
title: "Generera och exporterar certifikat för plats-till-plats: MakeCert: Azure | Microsoft Docs"
description: "Den här artikeln innehåller steg toocreate ett självsignerat rotcertifikat, exportera hello offentliga nyckel och generera klientcertifikat med hjälp av MakeCert."
services: vpn-gateway
documentationcenter: na
author: cherylmc
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/09/2017
ms.author: cherylmc
ms.openlocfilehash: 0b795ccf74f1f97fbd3de8a0dc339f9cb0b48183
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="generate-and-export-certificates-for-point-to-site-connections-using-makecert"></a>Generera och exporterar certifikat för plats-till-plats-anslutningar med hjälp av MakeCert

Punkt-till-plats-anslutningar använder certifikat tooauthenticate. Den här artikeln visar hur rot toocreate ett självsignerat certifikat och generera klientcertifikat med hjälp av MakeCert. Om du letar efter konfigurationssteg för punkt-till-plats, t.ex. hur tooupload rotcertifikat, Välj ett av hello ' Configure punkt-till-plats-artiklar från hello följande lista:

> [!div class="op_single_selector"]
> * [Skapa självsignerat certifikat - PowerShell](vpn-gateway-certificates-point-to-site.md)
> * [Skapa självsignerat certifikat - MakeCert](vpn-gateway-certificates-point-to-site-makecert.md)
> * [Konfigurera punkt-till-plats - Resource Manager - Azure-portalen](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
> * [Konfigurera punkt-till-plats - Resource Manager - PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md)
> * [Konfigurera punkt-till-plats - klassisk - Azure-portalen](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
> 
> 

Medan vi rekommenderar att du använder hello [Windows 10 PowerShell steg](vpn-gateway-certificates-point-to-site.md) toocreate certifikaten, vi tillhandahåller dessa MakeCert-instruktioner som en valfri metod. hello-certifikat som du skapar med någon av metoderna kan installeras på [alla stöds klientoperativsystem](vpn-gateway-howto-point-to-site-resource-manager-portal.md#faq). MakeCert har dock hello följande begränsningar:

* MakeCert är föråldrad. Det innebär att det här verktyget kan tas bort när som helst. Certifikat som du redan har skapats med hjälp av MakeCert påverkas inte när MakeCert är inte längre tillgänglig. MakeCert är inte bara använda toogenerate hello certifikat som verifierar mekanism.

## <a name="rootcert"></a>Skapa ett självsignerat rotcertifikat

hello följande steg visar hur toocreate ett självsignerat certifikat med hjälp av MakeCert. De här stegen är inte specifik modellen för distribution. De är giltiga för både Resource Manager och klassisk.

1. Hämta och installera [MakeCert](https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx).
2. Efter installationen hittar oftast hello makecert.exe verktyget under den här sökvägen: ' C:\Program Files (x86) \Windows Kits\10\bin\<arkitektur >'. Även om det är möjligt att det var installerade tooanother plats. Öppna en kommandotolk som administratör och navigera toohello platsen för hello MakeCert-verktyget. Du kan använda hello följande exempel, justera för hello rätt plats:

  ```cmd
  cd C:\Program Files (x86)\Windows Kits\10\bin\x64
  ```
3. Skapa och installera ett certifikat i hello personliga certifikatarkivet på datorn. hello följande exempel skapas en motsvarande *.cer* filen som du laddar upp tooAzure när du konfigurerar P2S. Ersätt 'P2SRootCert' och 'P2SRootCert.cer' hello namn som du vill toouse för hello certifikatet. hello certifikat finns i ”certifikat - aktuell User\Personal\Certificates'.

  ```cmd
  makecert -sky exchange -r -n "CN=P2SRootCert" -pe -a sha256 -len 2048 -ss My
  ```

## <a name="cer"></a>Exportera hello offentlig nyckel (.cer)

[!INCLUDE [Export public key](../../includes/vpn-gateway-certificates-export-public-key-include.md)]

Hej exported.cer filen måste vara överförda tooAzure. Instruktioner finns i [konfigurerar en punkt-till-plats-anslutning](vpn-gateway-howto-point-to-site-resource-manager-portal.md#uploadfile). tooadd ett ytterligare betrodda rotcertifikat finns [i det här avsnittet](vpn-gateway-howto-point-to-site-resource-manager-portal.md#add) i hello artikel.

### <a name="export-hello-self-signed-certificate-and-private-key-toostore-it-optional"></a>Exportera hello självsignerat certifikat och privat nyckel toostore den (valfritt)

Du kanske vill tooexport hello självsignerade rotcertifikat och lagra den på ett säkert sätt. Om behövs bör du senare kan installera den på en annan dator och generera flera klientcertifikat eller exportera en annan .cer-fil. tooexport hello självsignerade rotcertifikat som en .pfx, Välj hello rotcertifikat och använda hello samma steg som beskrivs i [exportera ett certifikat för](#clientexport).

## <a name="create-and-install-client-certificates"></a>Skapa och installera klientcertifikat

Installera hello självsignerat certifikat inte direkt på hello-klientdator. Du måste toogenerate ett klientcertifikat från hello självsignerat certifikat. Exportera och installera hello certifikat toohello klienten klientdatorn. hello följande är inte specifik modellen för distribution. De är giltiga för både Resource Manager och klassisk.

### <a name="clientcert"></a>Generera ett klientcertifikat

Varje klientdator som ansluter tooa VNet med punkt-till-plats måste ha ett certifikat installerat. Du generera ett klientcertifikat från hello självsignerat rotcertifikatet, och sedan exportera och installera hello klientcertifikat. Om hello klientcertifikatet inte är installerad, misslyckas autentiseringen. 

hello följande steg beskriver hur du genererar ett klientcertifikat från ett självsignerat rotcertifikat. Du kan skapa flera klientcertifikat från hello samma rotcertifikat. När du skapar klientcertifikat med hjälp av hello stegen nedan installeras hello klientcertifikat automatiskt på hello dator som du använde toogenerate hello certifikat. Om du vill tooinstall ett klientcertifikat på en annan dator, kan du exportera hello certifikat.
 
1. På hello samma dator som du använde toocreate hello självsignerade certifikat, öppna en kommandotolk som administratör.
2. Ändra och köra hello exempel toogenerate ett klientcertifikat.
  * Ändra *”P2SRootCert”* toohello namnet för hello självsignerat rot som du genererar hello klientcertifikat från. Kontrollera att du använder hello namnet på hello rotcertifikatet, vilket är hello ”CN =' värdet var du angav när du skapade hello självsignerat rot.
  * Ändra *P2SChildCert* toohello namn som du vill toogenerate toobe en klient-certifikat.

  Om du kör följande exempel utan att ändra den hello är hello resultatet ett klientcertifikat med namnet P2SChildcert i ditt personliga certifikatarkiv som har genererats från rotcertifikatet P2SRootCert.

  ```cmd
  makecert.exe -n "CN=P2SChildCert" -pe -sky exchange -m 96 -ss My -in "P2SRootCert" -is my -a sha256
  ```

### <a name="clientexport"></a>Exportera ett certifikat

[!INCLUDE [Export client certificate](../../includes/vpn-gateway-certificates-export-client-cert-include.md)]

### <a name="install"></a>Installera en exporterade klientcertifikat

[!INCLUDE [Install client certificate](../../includes/vpn-gateway-certificates-install-client-cert-include.md)]

## <a name="next-steps"></a>Nästa steg

Vill du fortsätta med konfigurationen punkt-till-plats. 

* För **Resource Manager** modell distributionsstegen, se [konfigurerar en punkt-till-plats-anslutning tooa VNet](vpn-gateway-howto-point-to-site-resource-manager-portal.md).
* För **klassiska** modell distributionsstegen, se [konfigurerar en punkt-till-plats VPN-anslutningen tooa virtuella nätverk (klassiska)](vpn-gateway-howto-point-to-site-classic-azure-portal.md).
