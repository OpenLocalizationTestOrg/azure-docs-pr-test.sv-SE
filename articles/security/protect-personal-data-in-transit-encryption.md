---
title: "aaaProtect personliga data under överföring med kryptering i Azure | Microsoft Docs"
description: "Med hjälp av kryptering i Azure tooprotect personliga data"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 218ad3f49326e8dec299a6d92b18116da65eae71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-in-transit-with-encryption"></a>Azure krypteringstekniker: skydda personliga data under överföringen med kryptering

Den här artikeln hjälper dig att förstå och använda Azure kryptering tekniker toosecure data under överföringen. 

Skydda hello sekretess personliga data är över nätverket hello en viktig del av en flera lager skydd på djupet säkerhetsstrategi. Kryptering under överföring är utformad tooprevent en angripare fångar upp överföringar från att kunna tooview eller Använd hello-data.

## <a name="scenario"></a>Scenario

Ett stort kryssning företag, sitt säte i hello USA utökar dess operations toooffer färdvägar i hello Medelhavet, Adriatiska havet, baltiska havet samt hello brittiska staterna. toosupport dessa ansträngningar den genererade flera mindre kryssning rader i Italien Tyskland, Danmark och hello Storbritannien 

hello företag använder Microsoft Azure toostore företagsdata i hello molnet. Detta inkluderar personligt identifierbar information, till exempel namn, adresser, telefonnummer och kreditkortsinformation av dess globala kundbas. Den innehåller också traditionella personal information, till exempel adresser, telefonnummer, skatt identifikationsnummer och medicinska information om företagets anställda på alla platser. hello kryssning rad har också en stor databas med medlemmar av trafik och förmåner som innehåller personuppgifter tootrack relationer med aktuella och tidigare kunder.

Personliga data av kunder har angetts i hello databasen från hello företagets fjärranslutna kontor och resa agenter finns runt hello world. Dokument som innehåller kundinformation överförs via hello tooAzure nätverkslagring.

## <a name="problem-statement"></a>Problembeskrivning

hello företag måste skydda hello sekretessen för kunders och anställdas personliga data när den är i överföringen tooand från Azure-tjänster.

## <a name="company-goal"></a>Företagets mål

Hej företagets mål tooensure personliga data som är krypterade när av disken. Om obehöriga avlyssna hello av disk personliga data, måste den vara i ett formulär som kommer att visas inte kan läsas. Tillämpa kryptering ska vara enkelt eller helt transparent för användare och administratörer.

## <a name="solutions"></a>Lösningar

Azure tillhandahåller flera verktyg och tekniker toohelp du skydda personliga data under överföringen.

### <a name="azure-storage"></a>Azure Storage

Data som lagras i molnet hello färdas från hello-klienten, vilket kan vara fysiskt finns någonstans i hälsningsmeddelande toohello Azure-datacenter. När dessa data hämtas av användare, de överförs igen i hello motsatt riktning. Data som är under överföring över hello offentliga Internet är alltid risk för avlyssning av angripare. Det är viktigt tooprotect hello sekretess personliga data med hjälp av kryptering på transportnivå toosecure som den flyttas mellan platser.

hello HTTPS-protokollet ger en säker och krypterad kommunikationskanal över hello Internet. HTTPS ska använda tooaccess objekt i Azure Storage och vid anrop av REST API: er. Du framtvinga användningen av hello HTTPS-protokollet när du använder [signaturer för delad åtkomst](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) toodelegate åtkomst tooAzure lagringsobjekt. Det finns två typer av SAS: tjänst-SAS och konto-SAS.

#### <a name="how-do-i-construct-a-service-sas"></a>Hur jag för att skapa en tjänst-SAS?

En tjänst-SAS delegater åtkomst tooa resurs i en av hello lagringstjänster (blob, kö-, tabell- eller filen service). tooconstruct en tjänst-SAS hello följande:

1. Ange hello signerat versionsfältet

2. Ange hello signerat resurs (Blob och endast filen Service)

3. Ange frågeparametrar tooOverride svarshuvuden (Blob-tjänsten och endast filen Service)

4. Ange hello tabellnamn (endast tabellen Service)

5. Ange hello åtkomstprincipen

6. Ange hello giltighetsintervall för signatur

8. Ange behörigheter

9. Ange IP-adress eller IP-intervall

10. Ange hello HTTP-protokoll

11. Ange intervall för åtkomst av tabell

12. Ange hello signerat identifierare

13. Ange hello signatur

Mer instruktioner finns [hur du skapar en tjänst-SAS](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).

#### <a name="how-do-i-construct-an-account-sas"></a>Hur jag för att skapa ett konto-SAS?

En konto-SAS delegerar åtkomst tooresources i en eller flera av hello lagringstjänster. Du kan också delegera åtkomst tooread-, Skriv- och delete-åtgärder på blob-behållare, tabeller, köer och filresurser som inte tillåts med en tjänst-SAS. Skapa ett konto-SAS är liknande toothat av en tjänst-SAS. Detaljerade instruktioner finns [hur du skapar ett konto-SAS.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)

#### <a name="how-do-i-enforce-https-when-calling-rest-apis"></a>Hur behöver jag använda HTTPS vid anrop av REST API: er?

tooenforce hello HTTPS används vid anrop av REST API: er tooaccess objekt i storage-konton, du kan aktivera säker överföring krävs för hello storage-konto. 

1. Markera i hello Azure-portalen, **skapa Lagringskonto**, eller ett befintligt lagringskonto, Välj **inställningar** och sedan **Configuration**.

2. Under **säker överföring krävs**väljer **aktiverad**.

![Skapa ett lagringskonto](media/protect-personal-data-in-transit-encryption/create-storage-account.png)

Detaljerade instruktioner, inklusive hur tooenable säker överföring krävs programmässigt, se [kräver säker överföring](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).

#### <a name="how-do-i-encrypt-data-in-azure-file-storage"></a>Hur jag för att kryptera data i Azure File Storage?

tooencrypt data under överföring med [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), du kan använda SMB 3.x med Windows 8, 8.1 och 10 och Windows Server 2012 R2 och Windows Server 2016. När du använder hello Azure Files tjänsten misslyckas en anslutning utan kryptering när ”säker överföring krävs” är aktiverat. Detta inkluderar scenarier med hjälp av SMB 2.1 och SMB 3.0 utan kryptering vissa varianter av hello Linux SMB-klienten.

#### <a name="azure-client-side-encryption"></a>Azure klientsidan kryptering

Ett annat alternativ för att skydda personliga data medan de överförs mellan ett klientprogram och Azure Storage är [Client side Encryption](https://docs.microsoft.com/azure/storage/storage-client-side-encryption). hello data krypteras innan de överförs till Azure Storage och när du hämtar hello data från Azure Storage hello data dekrypteras när den tas emot på hello på klientsidan.

### <a name="azure-site-to-site-vpn"></a>Azure VPN för plats-till-plats

Ett effektivt sätt tooprotect personliga data som överförs mellan ett företagsnätverk eller användare och hello virtuella Azure-nätverket är toouse en [plats-till-plats](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) eller [punkt-till-plats](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) virtuella privata nätverk (VPN). En VPN-anslutning skapar en säker krypterad tunnel via hello Internet.

#### <a name="how-do-i-create-a-site-to-site-vpn-connection"></a>Hur skapar jag en plats-till-plats VPN-anslutning

En plats-till-plats-VPN ansluter flera användare på hello företagsnätverket tooAzure. toocreate en plats-till-plats-anslutning i hello Azure-portalen hello följande:

1. Skapa ett virtuellt nätverk.

2. Ange en DNS-server.

3. Skapa hello gateway-undernätet.

4. Skapa hello VPN-gateway. 

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-01.png)

5. Skapa hello lokal nätverksgateway.

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-02.png)

6. Konfigurera din VPN-enhet.

7. Skapa hello VPN-anslutning.

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-03.png)

8. Kontrollera hello VPN-anslutning.

Detaljerade instruktioner om hur toocreate plats-till-plats-anslutning i hello Azure portal, se [skapa en plats-till-plats-anslutning i hello Azure-portalen.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)

#### <a name="how-do-i-create-a-point-to-site-vpn-connection"></a>Hur skapar jag en punkt-till-plats VPN-anslutning

En punkt-till-plats-VPN skapar en säker anslutning från en enskild klient tooa virtuella datornätverk. Detta är användbart när du vill tooconnect tooAzure från en fjärrdator som hemma eller ett hotell eller konferens Center. toocreate en punkt-till-plats-anslutning i hello Azure-portalen

1. Skapa ett virtuellt nätverk.

2. Lägg till en gateway-undernätet.

3. Ange en DNS-server. (valfritt)

4. Skapa en virtuell nätverksgateway.

5. Generera certifikat.

6. Lägg till hello klient-adresspool.

7. Överföra hello root certifikat certifikatets offentliga data.

8. Generera och installera hello VPN-klientpaketet konfiguration.

9. Installera ett exporterade klientcertifikat.

10. Ansluta tooAzure.

11. Verifiera anslutningen.

Mer instruktioner finns [konfigurerar en punkt-till-plats-anslutning tooa VNet använder certifikatautentisering för: Azure-portalen.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)

### <a name="ssltls"></a>SSL/TLS

Microsoft rekommenderar att du alltid använder SSL/TLS-protokollen tooexchange data mellan olika platser. Organisationer som väljer toouse [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) toomove stora datamängder dedikerade snabba WAN-länk kan också kryptera hello data i hello på programnivå med hjälp av SSL/TLS- eller andra protokoll för extra skydd.

### <a name="encryption-by-default"></a>Kryptering som standard

Microsoft använder kryptering tooprotect data som överförs mellan kunder och Azure-molntjänster. Om du interagerar med Azure Storage via hello Azure-portalen, alla transaktioner ska ske via HTTPS.

[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) är Microsoft-datacenter försöker toonegotiate med klientdatorer som ansluter till molntjänster tooMicrosoft hello-protokollet. TLS innehåller stark autentisering, meddelandesekretess, och integritet (aktiveras identifiering av meddelandet manipulation avlyssning och förfalskning), samverkan, flexibla algoritmer, enkel distribution och användning.

[Perfekt Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (Secrecy) används också så att varje anslutning mellan Microsofts molntjänster och kunders klientsystem använda unika nycklar. Anslutningar tooMicrosoft molntjänster också dra nytta av RSA baserat 2 048-bitarskryptering nyckellängder. Hej kombination av TLS, RSA 2 048-bitars nyckellängd, och PFS gör det mycket svårare för toointercept och komma åt data som överförs mellan Microsoft-molntjänster och kunder.

Det krypteras alltid data under överföring i [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview). Dessutom tooencrypting data tidigare toostoring toopersistent media hello data alltid skyddas under överföringen med hjälp av HTTPS. HTTPS är hello enda protokoll som stöds för hello Data Lake Store REST-gränssnitt.

## <a name="summary"></a>Sammanfattning

hello företagets åstadkommer syftet med att skydda personliga data och hello sekretessen för sådana data genom att använda HTTPS-anslutningar tooAzure lagring, använder signaturer för delad åtkomst och att aktivera säker överföring krävs på hello storage-konton. De kan även skydda personliga data med hjälp av SMB 3.0-anslutningar och implementera kryptering på klientsidan. Plats-till-plats VPN-anslutningar från hello företagsnätverket toohello virtuella Azure-nätverket och punkt-till-plats VPN-anslutningar från enskilda användare skapar en säker tunnel som personliga data på ett säkert sätt kan överföras. Microsofts standard kryptering praxis skydda ytterligare hello sekretess personliga data.

## <a name="next-steps"></a>Nästa steg

- [Metodtips för säkerhet för Azure Data och kryptering](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices)

- [Planering och design för VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

- [Vanliga frågor och svar om VPN-gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq)

- [Köpa och konfigurera ett SSL-certifikat för Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-purchase-ssl-web-site)
