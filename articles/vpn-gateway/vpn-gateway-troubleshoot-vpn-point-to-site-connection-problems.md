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
# <a name="troubleshooting-azure-point-to-site-connection-problems"></a>Felsökning: Anslutningsproblem med Azure punkt-till-plats

Den här artikeln innehåller vanliga anslutningsproblem som kan uppstå i punkt-till-plats. Här beskrivs också möjliga orsaker och lösningar på problemen.

## <a name="vpn-client-error-a-certificate-could-not-be-found"></a>VPN-klientfel: Det gick inte att hitta ett certifikat

### <a name="symptom"></a>Symtom

När du försöker tooconnect tooan virtuella Azure-nätverket med hjälp av hello VPN-klienten får du hello följande felmeddelande:

**Det gick inte att hitta ett certifikat som kan användas med Extensible Authentication Protocol. (Fel 798)**

### <a name="cause"></a>Orsak

Det här problemet uppstår om hello Klientcertifikatet saknas från **certifikat - aktuell User\Personal\Certificates**.

### <a name="solution"></a>Lösning

Kontrollera att hello Klientcertifikatet har installerats i hello följande lagringsplats för hello certifikat (Certmgr.msc):
 
**Certifikat - aktuell User\Personal\Certificates**

Mer information om hur tooinstall hello klientcertifikat finns [generera och exportera certifikat för plats-till-plats-anslutningar](vpn-gateway-certificates-point-to-site.md).

> [!NOTE]
> När du importerar hello klientcertifikatet inte väljer hello **aktivera starkt skydd av den privata nyckeln** alternativet.

## <a name="vpn-client-error-hello-message-received-was-unexpected-or-badly-formatted"></a>VPN-klientfel: hello-meddelande togs emot var oväntad eller felaktigt formaterat

### <a name="symptom"></a>Symtom

När du försöker tooconnect tooan virtuella Azure-nätverket med hjälp av hello VPN-klienten får du hello följande felmeddelande:

**hello-meddelande togs emot var oväntat eller felaktigt formaterat. (Fel 0x80090326)**

### <a name="cause"></a>Orsak

Det här problemet uppstår om hello rot certifikatets offentliga nyckel inte har överförts till hello Azure VPN-gateway. Det kan också inträffa om hello nyckeln är skadad eller upphört att gälla.

### <a name="solution"></a>Lösning

tooresolve problemet hello statusen för hello root certificate i hello Azure portal toosee om den har återkallats. Om det inte har återkallats försök toodelete hello-rotcertifikat och reupload. Mer information finns i [skapa certifikat](vpn-gateway-howto-point-to-site-classic-azure-portal.md#generatecerts).

## <a name="vpn-client-error-a-certificate-chain-processed-but-terminated"></a>VPN-klientfel: en certifikatkedja bearbetas men avslutades 

### <a name="symptom"></a>Symtom 

När du försöker tooconnect tooan virtuella Azure-nätverket med hjälp av hello VPN-klienten får du hello följande felmeddelande:

**En certifikatkedja bearbetas men avslutades med ett rotcertifikat som inte är betrodd av hello förtroende.**

### <a name="solution"></a>Lösning

1. Kontrollera att hello följande certifikat i hello rätt plats:

    | Certifikat | Plats |
    | ------------- | ------------- |
    | AzureClient.pfx  | Aktuella User\Personal\Certificates |
    | Azuregateway -*GUID*. cloudapp.net  | Aktuella User\Trusted rotcertifikatutfärdare|
    | AzureGateway -*GUID*. cloudapp.net AzureRoot.cer    | Lokal dator\Betrodda certifikatutfärdare|

2. Om hello certifikat finns redan i hello plats, försök toodelete hello certifikat och installera om dem. Hej  **azuregateway -*GUID*. cloudapp.net** certifikatet finns i hello VPN-klienten med konfigurationspaket som du hämtade från hello Azure-portalen. Du kan använda archivers tooextract hello-filer från hello-paketet.

## <a name="file-download-error-target-uri-is-not-specified"></a>Fel vid hämtning av filen: mål-URI har inte angetts

### <a name="symptom"></a>Symtom

Hello följande felmeddelande visas:

**Fel vid hämtning av filen. Mål-URI har inte angetts.**

### <a name="cause"></a>Orsak 

Det här problemet uppstår på grund av en felaktig gateway-typen. 

### <a name="solution"></a>Lösning

hello VPN gateway-typ måste vara **VPN**, och hello VPN-typ måste vara **RouteBased**.

## <a name="vpn-client-error-azure-vpn-custom-script-failed"></a>VPN-klientfel: Azure VPN-anpassade skript misslyckades 

### <a name="symptom"></a>Symtom

När du försöker tooconnect tooan virtuella Azure-nätverket med hjälp av hello VPN-klienten får du hello följande felmeddelande:

**Anpassat skript (tooupdate din routning tabell) misslyckades. (Fel 8007026f)**

### <a name="cause"></a>Orsak

Det här problemet kan inträffa om du försöker tooopen hello plats-till-punkt VPN-anslutning med hjälp av en genväg.

### <a name="solution"></a>Lösning 

Öppna hello VPN-paketet direkt i stället för att öppna den från hello genväg.

## <a name="cannot-install-hello-vpn-client"></a>Det går inte att installera hello VPN-klienten

### <a name="cause"></a>Orsak 

Ett ytterligare certifikat är obligatoriska tootrust hello VPN-gateway för det virtuella nätverket. hello certifikatet ingår i hello VPN-klienten med konfigurationspaket som genereras från hello Azure-portalen.

### <a name="solution"></a>Lösning

Extrahera hello VPN-klientpaketet konfiguration och hitta hello .cer-fil. tooinstall hello certifikat, gör du följande:

1. Öppna mmc.exe.
2. Lägg till hello **certifikat** snapin-modulen.
3. Välj hello **datorn** konto för hello lokal dator.
4. Högerklicka på hello **betrodda rotcertifikatutfärdare** nod. Klicka på **All aktivitet** > **Import**, och bläddra toohello .cer-fil som du har extraherat från hello VPN-klientpaketet konfiguration.
5. Starta om datorn hello. 
6. Försök tooinstall hello VPN-klienten.

## <a name="azure-portal-error-failed-toosave-hello-vpn-gateway-and-hello-data-is-invalid"></a>Azure portal fel: kunde inte toosave hello VPN-gateway och hello data är ogiltiga

### <a name="symptom"></a>Symtom

När du försöker toosave hello ändringar för hello VPN-gateway i hello Azure-portalen får du hello följande felmeddelande:

**Misslyckade toosave virtuell nätverksgateway &lt;* gatewaynamnet*&gt;. Data för certifikatet &lt; *certifikat ID* &gt; är invalid.* *

### <a name="cause"></a>Orsak 

Det här problemet kan inträffa om hello rot certifikatets offentliga nyckel som du överfört innehåller ett ogiltigt tecken, till exempel ett blanksteg.

### <a name="solution"></a>Lösning

Kontrollera att hello data i hello certifikatet inte innehåller ogiltiga tecken, till exempel radbrytningar (vagnreturer). hello hela värdet bör vara en lång rad. hello efter texten är ett exempel på hello certifikat:

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

## <a name="azure-portal-error-failed-toosave-hello-vpn-gateway-and-hello-resource-name-is-invalid"></a>Azure portal fel: kunde inte toosave hello VPN-gateway och hello resursnamnet är ogiltigt

### <a name="symptom"></a>Symtom

När du försöker toosave hello ändringar för hello VPN-gateway i hello Azure-portalen får du hello följande felmeddelande: 

**Misslyckade toosave virtuell nätverksgateway &lt;* gatewaynamnet*&gt;. Resursnamnet &lt; *certifikatnamn försök tooupload* &gt; är ogiltig **.

### <a name="cause"></a>Orsak

Det här problemet uppstår eftersom hello namnet på hello certifikat innehåller ett ogiltigt tecken, till exempel ett blanksteg. 

## <a name="azure-portal-error-vpn-package-file-download-error-503"></a>Azure portal fel: VPN-paketet filen fel vid hämtning av 503

### <a name="symptom"></a>Symtom

När konfigurationen för toodownload hello VPN-klientpaketet felmeddelandet hello följande felmeddelande:

**Det gick inte toodownload hello-filen. Information om felet: fel 503. hello-servern är upptagen.**
 
### <a name="solution"></a>Lösning

Det här felet kan bero på tillfälliga nätverksproblem. Försök toodownload hello VPN-paketet igen efter några minuter.

## <a name="azure-vpn-gateway-upgrade-all-p2s-clients-are-unable-tooconnect"></a>Uppgraderingen av Azure VPN-Gateway: alla P2S-klienter är tooconnect

### <a name="cause"></a>Orsak

Om hello certifikat är mer än 50 procent via dess livslängd hello certifikatet förnyas.

### <a name="solution"></a>Lösning

tooresolve problemet, skapa och distribuera nya certifikat toohello VPN-klienter. 

## <a name="too-many-vpn-clients-connected-at-once"></a>För många VPN-klienter anslutna samtidigt

För varje VPN-gateway är hello maximalt antal tillåtna anslutningar 128. Du kan se hello Totalt antal anslutna klienter i hello Azure-portalen.

## <a name="point-to-site-vpn-incorrectly-adds-a-route-for-100008-toohello-route-table"></a>Punkt-till-plats VPN felaktigt lägger till en väg för 10.0.0.0/8 toohello routningstabellen

### <a name="symptom"></a>Symtom

När du ringer hello VPN-anslutning på hello punkt-till-plats klienten hello VPN-klienten ska lägga till en väg mot hello virtuella Azure-nätverket. hello IP helper-tjänsten bör du lägga till en väg för undernätet hello hello VPN-klienter. 

hello VPN-klienten intervallet tillhör tooa mindre undernätet 10.0.0.0/8, till exempel 10.0.12.0/24. I stället för en väg för 10.0.12.0/24 läggs en väg för 10.0.0.0/8 som har högre prioritet. 

Felaktig vägen bryts anslutningen till andra lokala nätverk som tillhör tooanother undernät inom intervallet för hello 10.0.0.0/8, till exempel 10.50.0.0/24, som inte har en specifik väg som har definierats. 

### <a name="cause"></a>Orsak

Det här beteendet är avsiktligt för Windows-klienter. När hello klienten använder hello PPP IPCP-protokollet, hämtar hello IP-adress för hello tunnelgränssnitt från hello server (hello VPN-gateway i det här fallet). Men på grund av en begränsning i hello protokollet hello klienten har inte hello nätmask. Eftersom det inte finns några andra sätt tooget, hello klienten försöker programmet tooguess hello nätmask som baseras på klassen hello hello tunnel gränssnittet IP-adress. 

Därför läggs en väg baserat på hello följande avbildningar: 

Om adressen hör tooclass A gäller/8 som för-->

Om adressen hör gäller tooclass B--> /16

Om adressen hör gäller tooclass C--> /24

## <a name="vpn-client-cannot-access-network-file-shares"></a>VPN-klienten kan inte komma åt filresurser över nätverket

### <a name="symptom"></a>Symtom

hello VPN-klient har anslutit toohello virtuella Azure-nätverket. Dock hello-klienten kan inte komma åt nätverksresurser.

### <a name="cause"></a>Orsak

hello SMB-protokollet används för åtkomst till resursen för filen. När hello anslutningen initieras hello VPN-klienten lägger till hello session autentiseringsuppgifter och hello fel inträffar. Efter hello anslutningen har upprättats tvingas hello klienten toouse hello cache autentiseringsuppgifter för Kerberos-autentisering. Den här processen initierar frågor toohello Key Distribution Center (domänkontrollanten) tooget en token. Eftersom hello klienten ansluter från hello Internet, kanske inte kan tooreach hello-domänkontrollant. Därför inte kan hello-klienten växla över från Kerberos tooNTLM. 

hello endast tid uppmanas att hello-klienten för en autentiseringsuppgift är när den har ett giltigt certifikat (med SAN = UPN) utfärdat av hello domän toowhich som den är ansluten. hello-klienten måste också vara fysiskt ansluten toohello domännätverk. I det här fallet hello klienten försöker toouse hello certifikat och når ut toohello domänkontrollant. Hello Key Distribution Center returnerar ett ”KDC_ERR_C_PRINCIPAL_UNKNOWN”-fel. hello-klienten är framtvingad toofail över tooNTLM. 

### <a name="solution"></a>Lösning

toowork runt hello problemet, inaktivera hello cachelagring av autentiseringsuppgifter för domänen från hello följande registerundernyckel: 

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Lsa\DisableDomainCreds - Set hello value too1 


## <a name="cannot-find-hello-point-to-site-vpn-connection-in-windows-after-reinstalling-hello-vpn-client"></a>Det går inte att hitta hello punkt-till-plats VPN-anslutning i Windows efter ominstallationen hello VPN-klienten

### <a name="symptom"></a>Symtom

Du tar bort hello punkt-till-plats VPN-anslutning och sedan installera om hello VPN-klienten. I den här situationen har hello VPN-anslutningen inte konfigurerats korrekt. Du ser inte hello VPN-anslutningen i hello **nätverksanslutningar** inställningar i Windows.

### <a name="solution"></a>Lösning

tooresolve hello problem, ta bort hello gamla VPN-klientkonfigurationsfiler från **C:\Users\TheUserName\AppData\Roaming\Microsoft\Network\Connections**, och kör sedan hello VPN klientinstallationsprogrammet igen.
