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
# <a name="split-merge-security-configuration"></a>Dela dokument säkerhetskonfiguration
toouse hello dela/kopplingstjänsten, måste du ska konfigurera säkerhet. hello service är en del av hello elastisk skalbarhet funktion i Microsoft Azure SQL Database. Mer information finns i [elastisk skala dela och sammanfoga Service kursen](sql-database-elastic-scale-configure-deploy-split-and-merge.md).

## <a name="configuring-certificates"></a>Konfigurera certifikat
Certifikat är konfigurerade på två sätt. 

1. [tooConfigure hello SSL-certifikat](#to-configure-the-ssl-certificate)
2. [tooConfigure klientcertifikat](#to-configure-client-certificates) 

## <a name="tooobtain-certificates"></a>tooobtain certifikat
Certifikat kan hämtas från offentlig certifikatutfärdare (CA) eller från hello [Windows certifikattjänst](http://msdn.microsoft.com/library/windows/desktop/aa376539.aspx). Är detta hello önskad metoder tooobtain certifikat.

Om dessa alternativ inte är tillgängliga kan du generera **självsignerade certifikat**.

## <a name="tools-toogenerate-certificates"></a>Verktyg toogenerate certifikat
* [MakeCert.exe](http://msdn.microsoft.com/library/bfsktky3.aspx)
* [pvk2pfx.exe](http://msdn.microsoft.com/library/windows/hardware/ff550672.aspx)

### <a name="toorun-hello-tools"></a>toorun hello-verktyg
* Från en utvecklare kommandotolk för Visual Studios finns [Kommandotolken Visual Studio](http://msdn.microsoft.com/library/ms229859.aspx) 
  
    Om installerat, gå till:
  
        %ProgramFiles(x86)%\Windows Kits\x.y\bin\x86 
* Hämta hello WDK från [Windows 8.1: Hämta paket och verktyg](http://msdn.microsoft.com/windows/hardware/gg454513#drivers)

## <a name="tooconfigure-hello-ssl-certificate"></a>tooconfigure hello SSL-certifikat
Ett SSL-certifikat är obligatoriska tooencrypt hello kommunikation och autentisera hello-servern. Välj hello som är mest användbart hello tre scenarier nedan och kör alla steg:

### <a name="create-a-new-self-signed-certificate"></a>Skapa ett nytt självsignerat certifikat
1. [Skapa ett självsignerat certifikat](#create-a-self-signed-certificate)
2. [Skapa PFX-fil för ett självsignerat SSL-certifikat](#create-pfx-file-for-self-signed-ssl-certificate)
3. [Överför SSL-certifikat tooCloud Service](#upload-ssl-certificate-to-cloud-service)
4. [Uppdatera SSL-certifikat i Tjänstkonfigurationsfilen](#update-ssl-certificate-in-service-configuration-file)
5. [Importera SSL-certifikatutfärdare](#import-ssl-certification-authority)

### <a name="toouse-an-existing-certificate-from-hello-certificate-store"></a>toouse ett befintligt certifikat från hello certifikat lagra
1. [Exportera SSL-certifikatet från certifikatarkivet](#export-ssl-certificate-from-certificate-store)
2. [Överför SSL-certifikat tooCloud Service](#upload-ssl-certificate-to-cloud-service)
3. [Uppdatera SSL-certifikat i Tjänstkonfigurationsfilen](#update-ssl-certificate-in-service-configuration-file)

### <a name="toouse-an-existing-certificate-in-a-pfx-file"></a>toouse ett befintligt certifikat i en PFX-fil
1. [Överför SSL-certifikat tooCloud Service](#upload-ssl-certificate-to-cloud-service)
2. [Uppdatera SSL-certifikat i Tjänstkonfigurationsfilen](#update-ssl-certificate-in-service-configuration-file)

## <a name="tooconfigure-client-certificates"></a>tooconfigure klientcertifikat
Klientcertifikat krävs i ordning tooauthenticate begäranden toohello service. Välj hello som är mest användbart hello tre scenarier nedan och kör alla steg:

### <a name="turn-off-client-certificates"></a>Stäng av klientcertifikat
1. [Inaktivera certifikatbaserad klientautentisering](#turn-off-client-certificate-based-authentication)

### <a name="issue-new-self-signed-client-certificates"></a>Utfärda nya självsignerade certifikat
1. [Skapa en självsignerat certifikatutfärdare](#create-a-self-signed-certification-authority)
2. [Ladda upp CA-certifikat tooCloud Service](#upload-ca-certificate-to-cloud-service)
3. [Uppdatera CA-certifikat i Tjänstkonfigurationsfilen](#update-ca-certificate-in-service-configuration-file)
4. [Utfärda klientcertifikat](#issue-client-certificates)
5. [Skapa PFX-filer för klientcertifikat](#create-pfx-files-for-client-certificates)
6. [Importera klientcertifikatet](#Import-Client-Certificate)
7. [Kopiera Certifikattumavtryck för klienten](#copy-client-certificate-thumbprints)
8. [Konfigurera tillåtna klienter i hello-Tjänstkonfigurationsfil](#configure-allowed-clients-in-the-service-configuration-file)

### <a name="use-existing-client-certificates"></a>Använd befintlig klientcertifikat
1. [Hitta Certifikatutfärdarens offentliga nyckel](#find-ca-public-key)
2. [Ladda upp CA-certifikat tooCloud Service](#Upload-CA-certificate-to-cloud-service)
3. [Uppdatera CA-certifikat i Tjänstkonfigurationsfilen](#Update-CA-Certificate-in-Service-Configuration-File)
4. [Kopiera Certifikattumavtryck för klienten](#Copy-Client-Certificate-Thumbprints)
5. [Konfigurera tillåtna klienter i hello-Tjänstkonfigurationsfil](#configure-allowed-clients-in-the-service-configuration-file)
6. [Konfigurera klienten Återkallningskontrollen för lövcertifikatet](#Configure-Client-Certificate-Revocation-Check)

## <a name="allowed-ip-addresses"></a>Tillåtna IP-adresser
Åtkomst toohello slutpunkter kan vara begränsad toospecific intervall med IP-adresser.

## <a name="tooconfigure-encryption-for-hello-store"></a>tooconfigure kryptering för hello store
Ett certifikat är obligatoriska tooencrypt hello autentiseringsuppgifter som lagras i hello metadata store. Välj hello som är mest användbart hello tre scenarier nedan och kör alla steg:

### <a name="use-a-new-self-signed-certificate"></a>Använda ett nytt självsignerat certifikat
1. [Skapa ett självsignerat certifikat](#create-a-self-signed-certificate)
2. [Skapa PFX-fil för ett självsignerat certifikat för kryptering](#create-pfx-file-for-self-signed-ssl-certificate)
3. [Överför krypteringscertifikat tooCloud Service](#upload-encryption-certificate-to-cloud-service)
4. [Uppdatera certifikat för kryptering i konfigurationsfilen för tjänsten](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-from-hello-certificate-store"></a>Använda ett befintligt certifikat från certifikatarkivet hello
1. [Exportera krypteringscertifikat från certifikatarkivet](#export-encryption-certificate-from-certificate-store)
2. [Överför krypteringscertifikat tooCloud Service](#upload-encryption-certificate-to-cloud-service)
3. [Uppdatera certifikat för kryptering i konfigurationsfilen för tjänsten](#update-encryption-certificate-in-service-configuration-file)

### <a name="use-an-existing-certificate-in-a-pfx-file"></a>Använd ett befintligt certifikat i en PFX-fil
1. [Överför krypteringscertifikat tooCloud Service](#upload-encryption-certificate-to-cloud-service)
2. [Uppdatera certifikat för kryptering i konfigurationsfilen för tjänsten](#update-encryption-certificate-in-service-configuration-file)

## <a name="hello-default-configuration"></a>hello standardkonfigurationen
hello standardkonfigurationen nekar alla åtkomst toohello HTTP-slutpunkten. Detta är hello rekommenderad inställning, eftersom hello begäranden toothese slutpunkter kan utföra känslig information, till exempel autentiseringsuppgifter på databasen.
hello standardkonfigurationen kan alla åtkomst toohello HTTPS-slutpunkt. Den här inställningen kan begränsas ytterligare.

### <a name="changing-hello-configuration"></a>Ändra hello konfiguration
hello grupp av regler för åtkomstkontroll som gäller tooand slutpunkt har konfigurerats i hello  **<EndpointAcls>**  avsnitt i hello **tjänstkonfigurationsfilen**.

    <EndpointAcls>
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpIn" accessControl="DenyAll" />
      <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="AllowAll" />
    </EndpointAcls>

hello regler i en grupp för kontroll av åtkomst som konfigureras i en <AccessControl name=""> i hello service konfigurationsfil. 

hello format beskrivs i dokumentationen för nätverket ACL-listor.
Till exempel tooallow endast IP-adresser i hello intervallet 100.100.0.0 too100.100.255.255 tooaccess hello HTTPS-slutpunkt, hello regler skulle se ut så här:

    <AccessControl name="Retricted">
      <Rule action="permit" description="Some" order="1" remoteSubnet="100.100.0.0/16"/>
      <Rule action="deny" description="None" order="2" remoteSubnet="0.0.0.0/0" />
    </AccessControl>
    <EndpointAcls>
    <EndpointAcl role="SplitMergeWeb" endPoint="HttpsIn" accessControl="Restricted" />

## <a name="denial-of-service-prevention"></a>För tjänsten förebyggande
Det finns två olika metoder stöds toodetect och förhindra Denial of Service-attacker:

* Begränsa antalet samtidiga förfrågningar per fjärrvärden (inaktiverat som standard)
* Begränsa antalet åtkomst per fjärrvärden (på som standard)

Dessa är baserade på hello funktioner ytterligare dokumenteras i den dynamiska IP-säkerhet i IIS. När du ändrar den här konfigurationen varning för hello följande faktorer:

* hello beteendet för proxyservrar och Network Address Translation enheter över hello fjärrvärden information
* Varje begäran tooany resurs i hello webbroll anses (t.ex. läser in skript, bilder, osv)

## <a name="restricting-number-of-concurrent-accesses"></a>Begränsa antalet samtidiga användningar
hello-inställningar som konfigurerar det här beteendet är:

    <Setting name="DynamicIpRestrictionDenyByConcurrentRequests" value="false" />
    <Setting name="DynamicIpRestrictionMaxConcurrentRequests" value="20" />

Ändra DynamicIpRestrictionDenyByConcurrentRequests tootrue tooenable skyddet.

## <a name="restricting-rate-of-access"></a>Begränsa antalet åtkomst
hello-inställningar som konfigurerar det här beteendet är:

    <Setting name="DynamicIpRestrictionDenyByRequestRate" value="true" />
    <Setting name="DynamicIpRestrictionMaxRequests" value="100" />
    <Setting name="DynamicIpRestrictionRequestIntervalInMilliseconds" value="2000" />

## <a name="configuring-hello-response-tooa-denied-request"></a>Konfigurera hello svar tooa nekade begäran
hello konfigurerar följande inställning hello svar tooa nekade begäran:

    <Setting name="DynamicIpRestrictionDenyAction" value="AbortRequest" />
Läs toohello dokumentationen för den dynamiska IP-säkerhet i IIS för andra värden som stöds.

## <a name="operations-for-configuring-service-certificates"></a>Åtgärder för att konfigurera tjänstcertifikat
Det här avsnittet är endast för referens. Följ konfigurationsstegen för hello i:

* Konfigurera hello SSL-certifikat
* Konfigurera klientcertifikat

## <a name="create-a-self-signed-certificate"></a>Skapa ett självsignerat certifikat
Kör:

    makecert ^
      -n "CN=myservice.cloudapp.net" ^
      -e MM/DD/YYYY ^
      -r -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.1" ^
      -a sha1 -len 2048 ^
      -sv MySSL.pvk MySSL.cer

toocustomize:

* -n med hello tjänst-URL. Jokertecken (”CN = * .cloudapp .net”) och alternativa namn (”CN=myservice1.cloudapp.net, CN=myservice2.cloudapp.net”) stöds.
* e - med hello certifikatets förfallodatum skapa ett starkt lösenord och ange den när du tillfrågas.

## <a name="create-pfx-file-for-self-signed-ssl-certificate"></a>Skapa PFX-filen för självsignerade SSL-certifikatet
Kör:

        pvk2pfx -pvk MySSL.pvk -spc MySSL.cer

Ange lösenordet och sedan exportera certifikat med dessa alternativ:

* Ja, exportera hello privat nyckel
* Exportera alla utökade egenskaper

## <a name="export-ssl-certificate-from-certificate-store"></a>Exportera SSL-certifikatet från certifikatarkivet
* Hitta certifikat
* Klicka på Åtgärder -> alla uppgifter -> Exportera...
* Exportera certifikatet till en. PFX-filen med dessa alternativ:
  * Ja, exportera hello privat nyckel
  * Inkludera om möjligt alla certifikat i certifieringssökvägen hello * exportera alla utökade egenskaper

## <a name="upload-ssl-certificate-toocloud-service"></a>Överför SSL-certifikat toocloud-tjänsten
Överför certifikatet med hello befintliga eller genereras. PFX-filen med hello SSL-nyckelpar:

* Ange hello lösenord som skyddar hello information om privat nyckel

## <a name="update-ssl-certificate-in-service-configuration-file"></a>Uppdatera SSL-certifikat i tjänstkonfigurationsfilen
Hello tumavtrycksvärde på hello följande inställning i hello tjänstkonfigurationsfil med hello hello certifikatets tumavtryck har överförts toohello Molntjänsten:

    <Certificate name="SSL" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="import-ssl-certification-authority"></a>Importera SSL-certifikatutfärdare
Följ dessa steg på alla konto/datorn som kommunicerar med hello-tjänsten:

* Dubbelklicka på hello. CER-filen i Utforskaren i Windows
* Klicka på Installera certifikat i dialogrutan för hello certifikat...
* Importera certifikatet till hello betrodda rotcertifikatutfärdare

## <a name="turn-off-client-certificate-based-authentication"></a>Inaktivera certifikatbaserad klientautentisering
Endast certifikatbaserad klientautentisering stöds och inaktiverar den tillåter för allmän åtkomst toohello slutpunkter, såvida inte andra mekanismer finns på plats (t.ex. Microsoft Azure Virtual Network).

Ändra dessa inställningar toofalse i hello service configuration tooturn hello funktionen:

    <Setting name="SetupWebAppForClientCertificates" value="false" />
    <Setting name="SetupWebserverForClientCertificates" value="false" />

Kopiera sedan hello samma tumavtryck som hello SSL-certifikatet i inställningen för hello CA-certifikat:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="create-a-self-signed-certification-authority"></a>Skapa en självsignerat certifikatutfärdare
Kör hello följande steg toocreate ett självsignerat certifikat tooact som en certifikatutfärdare:

    makecert ^
    -n "CN=MyCA" ^
    -e MM/DD/YYYY ^
     -r -cy authority -h 1 ^
     -a sha1 -len 2048 ^
      -sr localmachine -ss my ^
      MyCA.cer

toocustomize den

* -e med hello utgångsdatum för certifikatutfärdare

## <a name="find-ca-public-key"></a>Hitta Certifikatutfärdarens offentliga nyckel
Alla certifikat måste har utfärdats av en certifikatutfärdare som betrodd av hello-tjänsten. Hitta hello offentliga nyckel toohello certifikatutfärdare som utfärdade hello klientcertifikat som ska toobe användes för autentisering i ordning tooupload toohello Molntjänsten.

Om hello-filen med hello offentliga nyckel inte är tillgänglig, kan du exportera det från certifikatarkivet hello:

* Hitta certifikat
  * Sök efter ett klientcertifikat utfärdat av hello samma certifikatutfärdare
* Dubbelklicka på hello certifikatet.
* Välj hello certifieringssökvägen fliken i dialogrutan för hello-certifikat.
* Dubbelklicka på hello CA post i hello sökväg.
* Anteckna av hello certifikategenskaper.
* Stäng hello **certifikat** dialogrutan.
* Hitta certifikat
  * Sök efter hello Certifikatutfärdare som nämns ovan.
* Klicka på Åtgärder -> alla uppgifter -> Exportera...
* Exportera certifikatet till en. CER med dessa alternativ:
  * **Nej, exportera inte privat nyckel för hello**
  * Inkludera om möjligt alla certifikat i certifieringssökvägen hello.
  * Exportera alla utökade egenskaper.

## <a name="upload-ca-certificate-toocloud-service"></a>Ladda upp CA-certifikat toocloud-tjänsten
Överför certifikatet med hello befintliga eller genereras. CER-filen med hello CA offentlig nyckel.

## <a name="update-ca-certificate-in-service-configuration-file"></a>Uppdatera CA-certifikat i tjänstkonfigurationsfilen
Hello tumavtrycksvärde på hello följande inställning i hello tjänstkonfigurationsfil med hello hello certifikatets tumavtryck har överförts toohello Molntjänsten:

    <Certificate name="CA" thumbprint="" thumbprintAlgorithm="sha1" />

Uppdatera hello värdet för hello följande inställning med hello samma tumavtryck:

    <Setting name="AdditionalTrustedRootCertificationAuthorities" value="" />

## <a name="issue-client-certificates"></a>Utfärda klientcertifikat
Varje enskild behöriga tooaccess hello-tjänst bör ha ett klientcertifikat utfärdat för his/hers exklusiv använder och bör välja his/hers äger starkt lösenord tooprotect dess privata nyckel. 

hello följande steg måste utföras i hello samma dator där hello självsignerade certifikat har genererats och lagras:

    makecert ^
      -n "CN=My ID" ^
      -e MM/DD/YYYY ^
      -cy end -sky exchange -eku "1.3.6.1.5.5.7.3.2" ^
      -a sha1 -len 2048 ^
      -in "MyCA" -ir localmachine -is my ^
      -sv MyID.pvk MyID.cer

Anpassa:

* -n med ett ID för toohello-klient som autentiseras med det här certifikatet
* e - med hello certifikatets förfallodatum
* MyID.pvk och MyID.cer med unika filnamn för det här klientcertifikatet

Det här kommandot ombeds du att ange ett lösenord toobe skapas och sedan används en gång. Använd ett starkt lösenord.

## <a name="create-pfx-files-for-client-certificates"></a>Skapa PFX-filer för klienten certifikat
För varje genererade klientcertifikat, kör du:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Anpassa:

    MyID.pvk and MyID.cer with hello filename for hello client certificate

Ange lösenordet och sedan exportera certifikat med dessa alternativ:

* Ja, exportera hello privat nyckel
* Exportera alla utökade egenskaper
* hello enskilda toowhom detta certifikat utfärdas ska välja hello export lösenord

## <a name="import-client-certificate"></a>Importera klientcertifikatet
Varje enskild person som ett certifikat har utfärdats ska importera hello nyckelpar i hello datorer han eller hon använda toocommunicate med hello-tjänsten:

* Dubbelklicka på hello. PFX-filen i Utforskaren i Windows
* Importera certifikatet till hello personligt arkiv med minst det här alternativet:
  * Inkludera alla utökade egenskaper markerat

## <a name="copy-client-certificate-thumbprints"></a>Kopiera certifikattumavtryck för klienten
Varje enskild person som ett certifikat har utfärdats måste följa stegen i ordning tooobtain hello tumavtryck his/hers certifikat som kommer att läggas till toohello tjänstkonfigurationsfilen:

* Kör certmgr.exe
* Välj fliken för hello personlig
* Dubbelklicka på hello klientcertifikat toobe används för autentisering
* I hello certifikat dialogruta som öppnas väljer du fliken för hello-information
* Se till att visa visas alla
* Välj hello fält med namnet tumavtrycket i hello lista
* Kopiera hello värdet för hello tumavtrycket ** ta bort icke-synliga Unicode-tecken framför hello första siffran ** ta bort alla blanksteg

## <a name="configure-allowed-clients-in-hello-service-configuration-file"></a>Konfigurera tillåtna klienter i konfigurationsfilen för hello-tjänsten
Uppdatera hello värdet för hello följande inställning i hello tjänstkonfigurationsfil med en kommaavgränsad lista över hello tumavtryck av hello klientcertifikat tillåten toohello-tjänsten för dataåtkomst:

    <Setting name="AllowedClientCertificateThumbprints" value="" />

## <a name="configure-client-certificate-revocation-check"></a>Konfigurera klienten Återkallningskontrollen för lövcertifikatet
hello standardinställningen kontrollerar inte med hello certifikatutfärdare för certifikatåterkallningsstatus för klienten. tooturn på hello kontrollerar om hello certifikatutfärdare som utfärdade hello klientcertifikat har stöd för dessa kontroller, ändra hello följande inställning med något av hello värden som definierats i hello X509RevocationMode uppräkningen:

    <Setting name="ClientCertificateRevocationCheck" value="NoCheck" />

## <a name="create-pfx-file-for-self-signed-encryption-certificates"></a>Skapa PFX-filen för självsignerade krypteringscertifikat
För ett certifikat för kryptering, kör du:

    pvk2pfx -pvk MyID.pvk -spc MyID.cer

Anpassa:

    MyID.pvk and MyID.cer with hello filename for hello encryption certificate

Ange lösenordet och sedan exportera certifikat med dessa alternativ:

* Ja, exportera hello privat nyckel
* Exportera alla utökade egenskaper
* Du behöver hello lösenord när du överför hello certifikat toohello Molntjänsten.

## <a name="export-encryption-certificate-from-certificate-store"></a>Exportera krypteringscertifikat från certifikatarkivet
* Hitta certifikat
* Klicka på Åtgärder -> alla uppgifter -> Exportera...
* Exportera certifikatet till en. PFX-filen med dessa alternativ: 
  * Ja, exportera hello privat nyckel
  * Inkludera om möjligt alla certifikat i certifieringssökvägen hello 
* Exportera alla utökade egenskaper

## <a name="upload-encryption-certificate-toocloud-service"></a>Överför krypteringstjänsten certifikat toocloud
Överför certifikatet med hello befintliga eller genereras. PFX-filen med hello krypteringsnyckelpar:

* Ange hello lösenord som skyddar hello information om privat nyckel

## <a name="update-encryption-certificate-in-service-configuration-file"></a>Uppdatera certifikat för kryptering i konfigurationsfilen för tjänsten
Hello tumavtrycksvärde på hello följande inställningar i hello tjänstkonfigurationsfil med hello hello certifikatets tumavtryck har överförts toohello Molntjänsten:

    <Certificate name="DataEncryptionPrimary" thumbprint="" thumbprintAlgorithm="sha1" />

## <a name="common-certificate-operations"></a>Vanliga certifikatåtgärder
* Konfigurera hello SSL-certifikat
* Konfigurera klientcertifikat

## <a name="find-certificate"></a>Hitta certifikat
Följ de här stegen:

1. Kör mmc.exe.
2. Arkiv -> Lägg till/ta bort snapin-modulen...
3. Välj **certifikat**.
4. Klicka på **Lägg till**.
5. Välj lagringsplats för hello certifikat.
6. Klicka på **Slutför**.
7. Klicka på **OK**.
8. Expandera **certifikat**.
9. Expandera hello certificate store nod.
10. Expandera hello certifikat underordnad nod.
11. Välj ett certifikat i hello-listan.

## <a name="export-certificate"></a>Exportera certifikat
I hello **guiden Exportera certifikat**:

1. Klicka på **Nästa**.
2. Välj **Ja**, sedan **Export hello privata nyckeln**.
3. Klicka på **Nästa**.
4. Välj filformat för hello önskade utdata.
5. Kontrollera hello önskat alternativ.
6. Kontrollera **lösenord**.
7. Ange ett starkt lösenord och bekräfta.
8. Klicka på **Nästa**.
9. Skriv eller bläddra fram ett filnamn där toostore hello certifikat (använder en. Filnamnstillägget PFX).
10. Klicka på **Nästa**.
11. Klicka på **Slutför**.
12. Klicka på **OK**.

## <a name="import-certificate"></a>Importera certifikat
I hello guiden Importera certifikat:

1. Välj hello lagringsplatsen.
   
   * Välj **aktuell användare** om endast processer som körs under aktuell användare kommer åt hello-tjänsten
   * Välj **lokal dator** om andra processer i den här datorn ansluter till hello-tjänsten
2. Klicka på **Nästa**.
3. Om du importerar från en fil, bekräfta hello filsökväg.
4. Om du importerar en. PFX-filen:
   1. Ange hello lösenord som skyddar hello privat nyckel
   2. Välj alternativ för import
5. Välj ”plats”-certifikat i hello följande store
6. Klicka på **Browse** (Bläddra).
7. Välj önskad hello-arkivet.
8. Klicka på **Slutför**.
   
   * Om hello betrodda rotcertifikatutfärdare har valts klickar du på **Ja**.
9. Klicka på **OK** på alla fönster för dialogrutan.

## <a name="upload-certificate"></a>Överför certifikat
I hello [Azure-portalen](https://portal.azure.com/)

1. Välj **molntjänster**.
2. Välj hello-Molntjänsten.
3. På hello översta menyn **certifikat**.
4. Klicka på hello nedre **överför**.
5. Välj hello certifikatfil.
6. Om det är en. PFX-fil, ange hello lösenord för privat nyckel för hello.
7. Kopiera hello certifikatets tumavtryck från hello ny post i listan hello när klar.

## <a name="other-security-considerations"></a>Andra säkerhetsaspekter
hello SSL-inställningarna som beskrivs i det här dokumentet kryptera kommunikationen mellan hello service och dess klienter när hello HTTPS-slutpunkt används. Detta är viktigt eftersom autentiseringsuppgifter för åtkomst till databasen och eventuellt andra känslig information finns i hello-meddelande. Observera dock att tjänsten hello kvarstår interna status, inklusive inloggningsuppgifter, i dess interna tabeller i hello Microsoft Azure SQL-databas som du har angett för lagring av metadata i din Microsoft Azure-prenumeration. Databasen har definierats som en del av hello följande inställning i din tjänstekonfigurationsfil (. CSCFG-fil): 

    <Setting name="ElasticScaleMetadata" value="Server=…" />

Autentiseringsuppgifterna som lagras i den här databasen krypteras. Dock som bästa praxis bör du kontrollera att både webb- och arbetsroller roller för dina tjänstdistributioner hålls in toodate och säker som de har båda åtkomst toohello metadata-databasen och hello certifikatet som används för kryptering och dekryptering av lagrade autentiseringsuppgifter. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

