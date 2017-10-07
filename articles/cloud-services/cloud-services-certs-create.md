---
title: "aaaCloud tjänster och hantering av certifikat | Microsoft Docs"
description: "Lär dig hur toocreate och använda certifikat med Microsoft Azure"
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: fc70d00d-899b-4771-855f-44574dc4bfc6
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: adegeo
ms.openlocfilehash: 69cb5467ece58a91dae06b4120954aeb2826bde1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="certificates-overview-for-azure-cloud-services"></a>Översikt över certifikat för Azure-molntjänster
Certifikat används i Azure för molntjänster ([tjänsten certifikat](#what-are-service-certificates)) och för att autentisera med hello management API ([hanteringscertifikat](#what-are-management-certificates) när med hello klassiska Azure-portalen och inte hello-klassisk Azure-portalen). Det här avsnittet ger en allmän översikt över båda typer av certifikat, hur för[skapa](#create) och [distribuera](#deploy) dem tooAzure.

Certifikat som används i Azure är x.509 v3-certifikat och kan vara signerat av en annan betrodda certifikat eller så kan de vara självsignerade. Ett självsignerat certifikat som är signerat av en egen skapare, därför inte är betrodd som standard. De flesta webbläsare kan ignorera det här problemet. Du bör endast använda självsignerade certifikat när du utvecklar och testar dina molntjänster. 

Certifikat som används av Azure kan innehålla en privat eller offentlig nyckel. Certifikatet har ett tumavtryck som ger en innebär tooidentify dem på ett entydigt sätt. Det här tumavtrycket används i hello Azure [konfigurationsfilen](cloud-services-configure-ssl-certificate.md) tooidentify som en tjänst i molnet av certifikat ska använda. 

## <a name="what-are-service-certificates"></a>Vad är service-certifikat?
Tjänstcertifikat som är bifogade toocloud tjänster och aktivera säker kommunikation tooand från hello-tjänsten. Till exempel om du har distribuerat en webbroll kan toosupply ett certifikat som kan autentisera en exponerade HTTPS-slutpunkt. Tjänstcertifikat som definierats i din tjänstdefinitionen är automatiskt distribuerade toohello virtuell dator som kör en instans av din roll. 

Du kan ladda upp tjänsten certifikat tooAzure klassisk portal antingen med hjälp av hello Azure klassiska portal eller genom att använda hello klassiska distributionsmodellen. Tjänstcertifikat som är associerade med en specifik molnbaserad tjänst. De är tilldelade tooa distribution i hello tjänstdefinitionsfilen.

Tjänstcertifikat kan hanteras separat från dina tjänster och kan hanteras av olika personer. En utvecklare kan till exempel överföra ett servicepaket som refererar tooa certifikat att en IT-chef tidigare har överförts tooAzure. En IT-chef kan hantera och förnya certifikatet (ändra hello konfigurering av hello-tjänst) utan att behöva tooupload ett nytt tjänstepaket. Det är möjligt att uppdatera utan ett nytt tjänstepaket eftersom hello logiskt namn och store-namn och placering för hello certifikatet tjänstdefinitionsfilen hello och när hello certifikatets tumavtryck har angetts i konfigurationsfilen för hello-tjänsten. tooupdate hello certifikat, är det endast nödvändiga tooupload ett nytt certifikat och ändra hello tumavtrycket värdet i konfigurationsfilen för hello-tjänsten.

>[!Note]
>Hej [Cloud Services vanliga frågor och svar](cloud-services-faq.md) artikeln innehåller användbar information om certifikat.

## <a name="what-are-management-certificates"></a>Vad är certifikat?
Certifikat kan du tooauthenticate med hello klassiska distributionsmodellen. Många program och verktyg (till exempel Visual Studio eller hello Azure SDK) använda dessa certifikat tooautomate konfiguration och distribution av olika Azure-tjänster. Dessa är inte verkligen relaterade toocloud tjänster. 

> [!WARNING]
> Var försiktig! Dessa typer av certifikat att alla som autentiserar med dem toomanage hello prenumeration de hör till. 
> 
> 

### <a name="limitations"></a>Begränsningar
Det finns en gräns på 100 hanteringscertifikat per prenumeration. Det finns en gräns på 100 hanteringscertifikat för alla prenumerationer under en viss tjänstadministratör användar-ID. Om hello användar-ID för hello kontoadministratör redan har använt tooadd 100 hanteringscertifikat och det finns flera certifikat måste du lägga till en medadministratör tooadd hello ytterligare certifikat. 

Innan du lägger till fler än 100 certifikat finns i om du kan återanvända ett befintligt certifikat. Med hjälp av medadministratörer lägger till potentiellt onödig komplexitet tooyour process för hantering av certifikat.

<a name="create"></a>
## <a name="create-a-new-self-signed-certificate"></a>Skapa ett nytt självsignerat certifikat
Du kan använda alla tillgängliga toocreate för verktyget ett självsignerat certifikat, så länge de följer toothese inställningar:

* Ett X.509-certifikat.
* Innehåller en privat nyckel.
* Skapa för utbyte av nycklar (.pfx-fil).
* Ämnesnamn måste matcha hello domän används tooaccess hello-Molntjänsten.

    > Du kan inte skaffa ett SSL-certifikat för hello cloudapp.net (eller något Azure-relaterade) domän. hello certifikatets ämnesnamn måste matcha hello domänen namnet som används för tooaccess ditt program. Till exempel **contoso.net**, inte **contoso.cloudapp.net**.

* Minst 2048-bitars kryptering.
* **Tjänsten certifikat endast**: klientsidan certifikatet måste finnas i hello *personliga* certifikatarkiv.

Det finns två sätt toocreate ett certifikat i Windows, med hello `makecert.exe` verktyg eller IIS.

### <a name="makecertexe"></a>MakeCert.exe
Det här verktyget är inaktuell och inte längre dokumenteras här. Mer information finns i [MSDN-artikel](https://msdn.microsoft.com/library/windows/desktop/aa386968).

### <a name="powershell"></a>PowerShell
```powershell
$cert = New-SelfSignedCertificate -DnsName yourdomain.cloudapp.net -CertStoreLocation "cert:\LocalMachine\My"
$password = ConvertTo-SecureString -String "your-password" -Force -AsPlainText
Export-PfxCertificate -Cert $cert -FilePath ".\my-cert-file.pfx" -Password $password
```

> [!NOTE]
> Om du vill toouse hello certifikat med en IP-adress i stället för en domän, kan du använda hello IP-adress i hello - DnsName-parametern.


Om du vill toouse [certifikat med hanteringsportalen hello](../azure-api-management-certs.md), exportera den tooa **.cer** fil:

```powershell
Export-Certificate -Type CERT -Cert $cert -FilePath .\my-cert-file.cer
```

### <a name="internet-information-services-iis"></a>Internet Information Services (IIS)
Det finns många sidor på hello internet som beskriver hur toodo detta med IIS. [Här](https://www.sslshopper.com/article-how-to-create-a-self-signed-certificate-in-iis-7.html) är en bra jag hittade som tror förklarar det bra. 

### <a name="java"></a>Java
Du kan använda Java för[skapa ett certifikat](../app-service-web/java-create-azure-website-using-java-sdk.md#create-a-certificate).

### <a name="linux"></a>Linux
[Detta](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) artikeln beskriver hur toocreate certifikat med SSH.

## <a name="next-steps"></a>Nästa steg
[Ladda upp din tjänst certifikat toohello klassiska Azure-portalen](cloud-services-configure-ssl-certificate.md) (eller hello [Azure-portalen](cloud-services-configure-ssl-certificate-portal.md)).

Överför en [hanteringscertifikat API](../azure-api-management-certs.md) toohello klassiska Azure-portalen. hello Azure-portalen använder inte certifikat för autentisering.

