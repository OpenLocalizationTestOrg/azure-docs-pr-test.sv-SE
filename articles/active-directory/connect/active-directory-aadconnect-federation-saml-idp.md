---
title: "Azure AD Connect: Använda en SAML-identitetsprovider 2.0 för enkel inloggning på | Microsoft Docs"
description: "Det här avsnittet beskriver hur du använder en kompatibel Idp SAML 2.0 för enkel inloggning på."
services: active-directory
author: billmath
manager: femila
ms.custom: it-pro
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: f9653dc44fb284a9b3c1988f623c33f27ae148cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
#  <a name="use-a-saml-20-identity-provider-idp-for-single-sign-on"></a>Använda en SAML 2.0-identitetsprovider (IdP) för enkel inloggning på

Det här avsnittet innehåller information om hur du använder en SAML 2.0 kompatibel SP-Lite profil baserat identitetsleverantör som hello önskad säkerhet säkerhetstokentjänst (STS) / identitetsleverantör. Detta är användbart om du redan har ett katalog för användarnamn och lösenord som lagras lokalt som kan nås med SAML 2.0. Den här befintliga användarkatalog kan användas för inloggning tooOffice 365 och andra Azure AD-skyddade resurser. hello SAML 2.0 SP-Lite profil baseras på hello ofta använda Security Assertion Markup Language (SAML) federerad identitet standard tooprovide en inloggning och attributet exchange framework.

>[!NOTE]
>Idps som har testats för användning med Azure AD finns i en lista med 3 part hello [kompatibilitetslista för Azure AD-federation](active-directory-aadconnect-federation-compatibility.md)

Microsoft stöder den här inloggning som hello integrering av en Microsoft-molntjänst som Office 365, med en korrekt konfigurerad SAML 2.0 baserade profilen IdP. SAML 2.0 identitetsleverantörer är produkter från tredje part och därmed Microsoft tillhandahåller inte stöd för hello-distribution, konfiguration, felsökning Metodtips om dem. En gång korrekt konfigurerad hello integrering med hello SAML 2.0 identitetsleverantör kan testas för korrekt konfiguration med hjälp av hello Microsoft Analyzer anslutningsverktyget som beskrivs i detalj nedan. Mer information om SAML 2.0 SP-Lite profilen baserat identitetsprovider be hello organisation som tillhandahöll den.

>[!IMPORTANT]
>Endast en mindre uppsättning klienter är tillgängliga i det här scenariot inloggning med SAML 2.0 identitetsleverantörer, bland annat:

>- Webbaserade klienter, till exempel Outlook Web Access och SharePoint Online
- E-post-RTF-klienter som använder grundläggande autentisering och en Exchange åtkomstmetod som stöds till exempel IMAP-, POP-, Active Sync-, MAPI, etc. (hello utökad klientprotokoll slutpunkten är obligatorisk toobe distribuerats), inklusive:
    - Microsoft Outlook 2010/Outlook 2013/Outlook 2016, Apple iPhone (olika versioner av iOS)
    - Olika Google Android-enheter
    - Windows Phone 7, Windows Phone 7,8 och Windows Phone 8.0
    - E-postklient för Windows 8 och Windows 8.1 e-postklient
    - Windows 10-e-postklient

Alla andra klienter är inte tillgängliga i det här scenariot inloggning med SAML 2.0 identitetsprovider. Hello Lync 2010 klientversionen är till exempel inte kan toologin i hello-tjänsten med SAML 2.0 identitetsprovider konfigurerats för enkel inloggning.

## <a name="azure-ad-saml-20-protocol-requirements"></a>Krav för Azure AD SAML 2.0-protokollet
Det här avsnittet innehåller detaljerade krav på hello-protokollet och meddelande formatering som identitetsprovider SAML 2.0 måste implementera toofederate med Azure AD tooenable inloggning tooone eller flera Microsoft-molntjänster (till exempel Office 365). hello SAML 2.0 förlitande part (SP-STS) för en Microsoft-molntjänst som används i det här scenariot är Azure AD.

Vi rekommenderar att du säkerställer att din SAML 2.0 identitet providern utgående meddelanden vara liknande toohello som exempel spår som möjligt. Använd attributvärden från hello gav även Azure AD-metadata om möjligt. När du är nöjd med utgående meddelanden, kan du testa med hello Microsoft anslutningen Analyzer som beskrivs nedan.

hello Azure AD-metadata kan hämtas från denna URL: [https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml](http://https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml).
För kunder i Kina med hello Kina-specifik instans av Office 365, hello följande federation slutpunkten ska användas: [https://nexus.partner.microsoftonline-p.cn/federationmetadata/saml20/federationmetadata.xml](https://nexus.partner.microsoftonline-p.cn/federationmetadata/saml20/federationmetadata.xml).

## <a name="saml-protocol-requirements"></a>Krav för SAML-protokoll
Det här avsnittet beskriver hur hello förfrågan och svar meddelandepar är tillsammans i ordning toohelp du tooformat meddelanden på rätt sätt.

Azure AD kan vara konfigurerade toowork med identitetsleverantörer som använder SAML 2.0 SP Lite hello-profilen med vissa krav enligt nedan. Du kan arbeta tooachieve samverkan med Azure AD med exempel SAML förfrågan och svar hälsningsmeddelande tillsammans med automatisk och manuell testning.

## <a name="signature-block-requirements"></a>Krav på signatur block
Meddelandenod hello signaturen innehåller information om hello digital signatur för hello-meddelande själva inom hello SAML-svar. Hej signaturblocket har hello följande krav:

1. hello assertion noden måste signeras.
2.  hello RSA-sha1-algoritmen måste användas som hello DigestMethod. Andra algoritmer för digital signatur godkänns inte.
   `<ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1"/>`
3.  Du kan också registrera hello XML-dokument. 
4.  hello transformera algoritmen måste matcha hello värden i följande exempel hello:`<ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
       <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>`
9.  Hej SignatureMethod algoritmen måste matcha hello följande exempel:`<ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1"/>`

## <a name="supported-bindings"></a>Stöds bindningar
Bindningar är hello transport relaterade kommunikation parametrar som krävs. hello följande krav gäller toohello bindningar

1. HTTPS är hello krävs transport.
2.  Azure AD kräver HTTP POST för token som skickas automatiskt vid inloggning
3.  Azure AD använder HTTP POST för hello begäran toohello identitet autentiseringsprovider och OMDIRIGERING för hello utloggning meddelandet toohello identitetsleverantör.

## <a name="required-attributes"></a>Obligatoriska attribut
Den här tabellen visar kraven för specifika attribut i hello SAML 2.0-meddelande.
 
|Attribut|Beskrivning|
| ----- | ----- |
|NameID|hello-värdet för den här kontrollen måste hello samma som ImmutableID hello Azure AD-användare. Det kan vara upp too64 alfanumeriska tecken. Säker tecken icke HTML måste kodas, till exempel tecknet ”+” visas som ”.2b”.|
|IDPEmail|hello UPN (User Principal Name) visas i hello SAML svaret som ett element med hello namn IDPEmail är hello användares UserPrincipalName (UPN) i Azure AD/Office 365. hello UPN är i formatet för e-postadress. UPN-värdet i Windows Office 365 (Azure Active Directory).|
|Utfärdaren|Detta är obligatorisk toobe en URI för hello identitetsleverantör. Du bör inte återanvända hello utfärdaren från exemplet hälsningsmeddelande. Om du har flera domäner på toppnivå i din Azure AD-klienter hello utfärdaren måste matcha angetts hello URI-inställningar som konfigureras per domän.|

>[!IMPORTANT]
>Azure AD för närvarande stöder följande NameID Format URI för SAML 2.0:urn:oasis:names:tc:SAML:2.0:nameid hello-format: beständig.

## <a name="sample-saml-request-and-response-messages"></a>Exempel SAML förfrågan och svar-meddelanden
Paret förfrågan och svar meddelandet visas för hello inloggning meddelandet exchange.
Det här är ett meddelande om begäran exempel som skickas från Azure AD tooa exempel SAML 2.0 identitetsleverantör. hello exempel SAML 2.0-identitetsleverantör är Active Directory Federation Services (AD FS) konfigurerats toouse SAML-P-protokollet. Interoperabiliteten är också klar med andra identitetsleverantörer SAML 2.0.

    `<samlp:AuthnRequest xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol" xmlns:saml="urn:oasis:names:tc:SAML:2.0:assertion" ID="_7171b0b2-19f2-4ba2-8f94-24b5e56b7f1e" IssueInstant="2014-01-30T16:18:35Z" Version="2.0" AssertionConsumerServiceIndex="0" >
    <saml:Issuer>urn:federation:MicrosoftOnline</saml:Issuer>
    <samlp:NameIDPolicy Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent"/>
    </samlp:AuthnRequest>`

Det här är ett exempel svarsmeddelande som skickas från hello exempel SAML 2.0 kompatibla identitet providern tooAzure AD / Office 365.

    `<samlp:Response ID="_592c022f-e85e-4d23-b55b-9141c95cd2a5" Version="2.0" IssueInstant="2014-01-31T15:36:31.357Z" Destination="https://login.microsoftonline.com/login.srf" Consent="urn:oasis:names:tc:SAML:2.0:consent:unspecified" InResponseTo="_049917a6-1183-42fd-a190-1d2cbaf9b144" xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">http://WS2012R2-0.contoso.com/adfs/services/trust</Issuer>
    <samlp:Status>
    <samlp:StatusCode Value="urn:oasis:names:tc:SAML:2.0:status:Success" />
    </samlp:Status>
    <Assertion ID="_7e3c1bcd-f180-4f78-83e1-7680920793aa" IssueInstant="2014-01-31T15:36:31.279Z" Version="2.0" xmlns="urn:oasis:names:tc:SAML:2.0:assertion">
    <Issuer>http://WS2012R2-0.contoso.com/adfs/services/trust</Issuer>
    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      <ds:SignedInfo>
        <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
        <ds:SignatureMethod Algorithm="http://www.w3.org/2000/09/xmldsig#rsa-sha1" />
        <ds:Reference URI="#_7e3c1bcd-f180-4f78-83e1-7680920793aa">
          <ds:Transforms>
            <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature" />
            <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#" />
          </ds:Transforms>
          <ds:DigestMethod Algorithm="http://www.w3.org/2000/09/xmldsig#sha1" />
          <ds:DigestValue>CBn/5YqbheaJP425c0pHva9PhNY=</ds:DigestValue>
        </ds:Reference>
      </ds:SignedInfo>
      <ds:SignatureValue>TciWMyHW2ZODrh/2xrvp5ggmcHBFEd9vrp6DYXp+hZWJzmXMmzwmwS8KNRJKy8H7XqBsdELA1Msqi8I3TmWdnoIRfM/ZAyUppo8suMu6Zw+boE32hoQRnX9EWN/f0vH6zA/YKTzrjca6JQ8gAV1ErwvRWDpyMcwdYCiWALv9ScbkAcebOE1s1JctZ5RBXggdZWrYi72X+I4i6WgyZcIGai/rZ4v2otoWAEHS0y1yh1qT7NDPpl/McDaTGkNU6C+8VfjD78DrUXEcAfKvPgKlKrOMZnD1lCGsViimGY+LSuIdY45MLmyaa5UT4KWph6dA==</ds:SignatureValue>
      <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
        <ds:X509Data>
          <ds:X509Certificate>MIIC7jCCAdagAwIBAgIQRrjsbFPaXIlOG3GTv50fkjANBgkqhkiG9w0BAQsFADAzMTEwLwYDVQQDEyhBREZTIFNpZ25pbmcgLSBXUzIwMTJSMi0wLnN3aW5mb3JtZXIuY29tMB4XDTE0MDEyMDE1MTY0MFoXDTE1MDEyMDE1MTY0MFowMzExMC8GA1UEAxMoQURGUyBTaWduaW5nIC0gV1MyMDEyUjItMC5zd2luZm9ybWVyLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKe+rLVmXy1QwCwZwqgbbp1/+3ZWxd9T/jV0hpLIIWr+LCOHqq8n8beJvlivgLmDJo8f+EITnAxWcsJUvVai/35AhHCUq9tc9sqMp5PWtabAEMb2AU72/QlX/72D2/NbGQq1BWYbqUpgpCZ2nSgvlWDHlCiUo//UGsvfox01kjTFlmqQInsJVfRxF5AcCAwEAATANBgkqhkiG9w0BAQsFAAOCAQEAi8c6C4zaTEc7aQiUgvnGQgCbMZbhUXXLGRpjvFLKaQzkwa9eq7WLJibcSNyGXBa/SfT5wJgsm3TPKgSehGAOTirhcqHheZyvBObAScY7GOT+u9pVYp6raFrc7ez3c+CGHeV/tNvy1hJNs12FYH4X+ZCNFIT9tprieR25NCdi5SWUbPZL0tVzJsHc1y92b2M2FxqRDohxQgJvyJOpcg2mSBzZZIkvDg7gfPSUXHVS1MQs0RHSbwq/XdQocUUhl9/e/YWCbNNxlM84BxFsBUok1dH/gzBySx+Fc8zYi7cOq9yaBT3RLT6cGmFGVYZJW4FyhPZOCLVNsLlnPQcX3dDg9A==</ds:X509Certificate>
        </ds:X509Data>
      </KeyInfo>
    </ds:Signature>
    <Subject>
      <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">ABCDEG1234567890</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData InResponseTo="_049917a6-1183-42fd-a190-1d2cbaf9b144" NotOnOrAfter="2014-01-31T15:41:31.357Z" Recipient="https://login.microsoftonline.com/login.srf" />
      </SubjectConfirmation>
    </Subject>
    <Conditions NotBefore="2014-01-31T15:36:31.263Z" NotOnOrAfter="2014-01-31T16:36:31.263Z">
      <AudienceRestriction>
        <Audience>urn:federation:MicrosoftOnline</Audience>
      </AudienceRestriction>
    </Conditions>
    <AttributeStatement>
      <Attribute Name="IDPEmail">
        <AttributeValue>administrator@contoso.com</AttributeValue>
      </Attribute>
    </AttributeStatement>
    <AuthnStatement AuthnInstant="2014-01-31T15:36:30.200Z" SessionIndex="_7e3c1bcd-f180-4f78-83e1-7680920793aa">
      <AuthnContext>
        <AuthnContextClassRef>urn:oasis:names:tc:SAML:2.0:ac:classes:PasswordProtectedTransport</AuthnContextClassRef>
      </AuthnContext>
    </AuthnStatement>
    </Assertion>
    </samlp:Response>`

## <a name="configure-your-saml-20-compliant-identity-provider"></a>Konfigurera din SAML 2.0 kompatibla identitetsleverantören.
Det här avsnittet innehåller riktlinjer för hur tooconfigure SAML 2.0 identitet providern toofederate med Azure AD tooenable åtkomst för enkel inloggning tooone eller flera Microsoft cloud services (till exempel Office 365) med hello SAML 2.0-protokollet. hello är SAML 2.0 förlitande part för en Microsoft-molntjänst som används i det här scenariot Azure AD.

## <a name="add-azure-ad-metadata"></a>Lägga till Azure AD-metadata
Identitetsprovider SAML 2.0 måste tooadhere tooinformation om hello Azure AD förlitande part. Azure AD publicerar metadata på https://nexus.microsoftonline-p.com/federationmetadata/saml20/federationmetadata.xml.

Vi rekommenderar att du alltid importera hello senaste Azure AD-metadata när du konfigurerar din SAML 2.0-identitetsleverantör. Observera att Azure AD inte läsa metadata från hello identitetsleverantören.

## <a name="add-azure-ad-as-a-relying-party"></a>Lägg till Azure AD som en förlitande part
Du har tooenable kommunikationen mellan SAML 2.0 identitetsleverantör och Azure AD. Den här konfigurationen är beroende av specifika identitetsprovider och toodocumentation bör finns. Du skulle vanligtvis ange hello förlitande part ID toohello samma som hello ID för entiteterna från hello Azure AD-metadata.

>[!NOTE]
>Kontrollera hello klockan på serverns SAML 2.0 identity-providern är synkroniserade tooan korrekt tidskälla. En felaktig systemtid kan orsaka federerade inloggningar toofail.

## <a name="install-windows-powershell-for-sign-on-with-saml-20-identity-provider"></a>Installera Windows PowerShell för inloggning med SAML 2.0 identitetsleverantören.
När du har konfigurerat identitetsprovider SAML 2.0 för användning med Azure AD-inloggning, är hello nästa steg toodownload och installera hello Azure Active Directory-modulen för Windows PowerShell. När du har installerat använder du dessa cmdlets tooconfigure Azure AD-domäner som externa domäner.

hello Azure Active Directory-modulen för Windows PowerShell är ett hämtningsbart program för att hantera organisationens data i Azure AD. Den här modulen installerar en uppsättning cmdlets tooWindows PowerShell; du kör dessa cmdletar tooset in åtkomst för enkel inloggning tooAzure AD och i sin tur tooall av hello molntjänster du prenumererar på. Anvisningar om hur toodownload och installera hello cmdlets finns [http://technet.microsoft.com/library/jj151815.aspx](http://technet.microsoft.com/library/jj151815.aspx)

## <a name="set-up-a-trust-between-your-saml-identity-provider-and-azure-ad"></a>Skapa ett förtroende mellan SAML-identitetsprovider och Azure AD
Innan du konfigurerar federation på en Azure AD-domän, måste den ha en anpassad domän som har konfigurerats. Du kan federera hello standarddomän som tillhandahålls av Microsoft. hello standarddomän från Microsoft som slutar med ”onmicrosoft.com”.
Du kan köra en uppsättning cmdlets i hello Windows PowerShell-kommandoradsgränssnittet tooadd eller konvertera domäner för enkel inloggning.

Varje Azure Active Directory-domän som du vill toofederate med hjälp av din identitetsleverantör SAML 2.0 måste antingen läggas till som en domän för enkel inloggning eller konverterade toobe en enda domän inloggning från en standarddomän. Att lägga till eller konvertera en domän anger du ett förtroende mellan din identitetsleverantör SAML 2.0 och Azure AD.

hello följande procedur beskriver hur du konverterar en befintlig standarddomän tooa federerade domänen med SAML 2.0 SP-Lite. Observera att din domän kan uppstå ett avbrott som påverkar användare in too2 timmar efter att du har utfört det här steget.

## <a name="configuring-a-domain-in-your-azure-ad-directory-for-federation"></a>Konfigurera en domän i din Azure AD-katalog för federation


1. Ansluta tooyour Azure AD-katalog som en Innehavaradministratör: ansluta MsolService.
2.  Konfigurera dina önskade Office 365 toouse domänfederation med SAML 2.0:`$dom = "contoso.com" $BrandName - "Sample SAML 2.0 IDP" $LogOnUrl = "https://WS2012R2-0.contoso.com/passiveLogon" $LogOffUrl = "https://WS2012R2-0.contoso.com/passiveLogOff" $ecpUrl = "https://WS2012R2-0.contoso.com/PAOS" $MyURI = "urn:uri:MySamlp2IDP" $MySigningCert = @" MIIC7jCCAdagAwIBAgIQRrjsbFPaXIlOG3GTv50fkjANBgkqhkiG9w0BAQsFADAzMTEwLwYDVQQDEyh BREZTIFNpZ25pbmcgLSBXUzIwMTJSMi0wLnN3aW5mb3JtZXIuY29tMB4XDTE0MDEyMDE1MTY0MFoXDT E1MDEyMDE1MTY0MFowMzExMC8GA1UEAxMoQURGUyBTaWduaW5nIC0gV1MyMDEyUjItMC5zd2luZm9yb WVyLmNvbTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKe+rLVmXy1QwCwZwqgbbp1/kupQ VcjKuKLitVDbssFyqbDTjP7WRjlVMWAHBI3kgNT7oE362Gf2WMJFf1b0HcrsgLin7daRXpq4Qi6OA57 sW1YFMj3sqyuTP0eZV3S4+ZbDVob6amsZIdIwxaLP9Zfywg2bLsGnVldB0+XKedZwDbCLCVg+3ZWxd9 T/jV0hpLIIWr+LCOHqq8n8beJvlivgLmDJo8f+EITnAxWcsJUvVai/35AhHCUq9tc9sqMp5PWtabAEM b2AU72/QlX/72D2/NbGQq1BWYbqUpgpCZ2nSgvlWDHlCiUo//UGsvfox01kjTFlmqQInsJVfRxF5AcC AwEAATANBgkqhkiG9w0BAQsFAAOCAQEAi8c6C4zaTEc7aQiUgvnGQgCbMZbhUXXLGRpjvFLKaQzkwa9 eq7WLJibcSNyGXBa/SfT5wJgsm3TPKgSehGAOTirhcqHheZyvBObAScY7GOT+u9pVYp6raFrc7ez3c+ CGHeV/tNvy1hJNs12FYH4X+ZCNFIT9tprieR25NCdi5SWUbPZL0tVzJsHc1y92b2M2FxqRDohxQgJvy JOpcg2mSBzZZIkvDg7gfPSUXHVS1MQs0RHSbwq/XdQocUUhl9/e/YWCbNNxlM84BxFsBUok1dH/gzBy Sx+Fc8zYi7cOq9yaBT3RLT6cGmFGVYZJW4FyhPZOCLVNsLlnPQcX3dDg9A==" "@ $uri = "http://WS2012R2-0.contoso.com/adfs/services/trust" $Protocol = "SAMLP" Set-MsolDomainAuthentication -DomainName $dom -FederationBrandName $dom -Authentication Federated -PassiveLogOnUri $MyURI -ActiveLogOnUri $ecpUrl -SigningCertificate $MySigningCert -IssuerUri $uri -LogOffUri $url -PreferredAuthenticationProtocol $Protocol` 

3.  Du kan hämta hello signering certifikat base64-kodad sträng från din IDP-metadatafil. Ett exempel på den här platsen har angetts men kan avvika något baserat på din implementering.

    `<IDPSSODescriptor protocolSupportEnumeration="urn:oasis:names:tc:SAML:2.0:protocol"> <KeyDescriptor use="signing"> <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#"> <X509Data> <X509Certificate>MIIC5jCCAc6gAwIBAgIQLnaxUPzay6ZJsC8HVv/QfTANBgkqhkiG9w0BAQsFADAvMS0wKwYDVQQDEyRBREZTIFNpZ25pbmcgLSBmcy50ZWNobGFiY2VudHJhbC5vcmcwHhcNMTMxMTA0MTgxMzMyWhcNMTQxMTA0MTgxMzMyWjAvMS0wKwYDVQQDEyRBREZTIFNpZ25pbmcgLSBmcy50ZWNobGFiY2VudHJhbC5vcmcwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCwMdVLTr5YTSRp+ccbSpuuFeXMfABD9mVCi2wtkRwC30TIyPdORz642MkurdxdPCWjwgJ0HW6TvXwcO9afH3OC5V//wEGDoNcI8PV4enCzTYFe/h//w51uqyv48Fbb3lEXs+aVl8155OAj2sO9IX64OJWKey82GQWK3g7LfhWWpp17j5bKpSd9DBH5pvrV+Q1ESU3mx71TEOvikHGCZYitEPywNeVMLRKrevdWI3FAhFjcCSO6nWDiMqCqiTDYOURXIcHVYTSof1YotkJ4tG6mP5Kpjzd4VQvnR7Pjb47nhIYG6iZ3mR1F85Ns9+hBWukQWNN2hcD/uGdPXhpdMVpBAgMBAAEwDQYJKoZIhvcNAQELBQADggEBAK7h7jF7wPzhZ1dPl4e+XMAr8I7TNbhgEU3+oxKyW/IioQbvZVw1mYVCbGq9Rsw4KE06eSMybqHln3w5EeBbLS0MEkApqHY+p68iRpguqa+W7UHKXXQVgPMCpqxMFKonX6VlSQOR64FgpBme2uG+LJ8reTgypEKspQIN0WvtPWmiq4zAwBp08hAacgv868c0MM4WbOYU0rzMIR6Q+ceGVRImlCwZ5b7XKp4mJZ9hlaRjeuyVrDuzBkzROSurX1OXoci08yJvhbtiBJLf3uPOJHrhjKRwIt2TnzS9ElgFZlJiDIA26Athe73n43CT0af2IG6yC7e6sK4L3NEXJrwwUZk=</X509Certificate> </X509Data> </KeyInfo> </KeyDescriptor>` 

Läs mer om ”Set-MsolDomainAuthentication”: [http://technet.microsoft.com/library/dn194112.aspx](http://technet.microsoft.com/library/dn194112.aspx).

>[!NOTE]
>Du måste köra användning ”$ecpUrl =” https://WS2012R2-0.contoso.com/PAOS ”” om du ställer in ett ECP-tillägg för din identitetsleverantör. Exchange Online-klienter, exklusive Outlook Web Application (OWA) förlitar sig på en POST baserat aktiv slutpunkt. Om din SAML 2.0 STS implementerar en aktiv slutpunkt liknande Tooshibboleth's ECP implementering av en aktiv slutpunkt kan det vara möjligt för dessa omfattande klienter toointeract med hello Exchange Online-tjänsten.

När federationstjänsten har konfigurerats kan du byta tillbaka för ”ofedererad” (eller ”hanterad”), men den här ändringen tar upp tootwo timmar toocomplete och det kräver att tilldela nya slumpmässigt lösenord för molnet baserat inloggning tooeach användare. Växla tillbaka för ”hanterad” kan vara obligatoriskt i vissa scenarier tooreset ett fel i inställningarna. Läs mer på domänen konvertering: [http://msdn.microsoft.com/library/windowsazure/dn194122.aspx](http://msdn.microsoft.com/library/windowsazure/dn194122.aspx).

## <a name="provision-user-principals-tooazure-ad--office-365"></a>Etablera användare säkerhetsobjekt tooAzure AD / Office 365
Innan du kan autentisera dina användare tooOffice 365 måste du etablera Azure AD med användare som anspråk för säkerhetsobjekt som motsvarar toohello assertion i hello SAML 2.0. Om dessa användare säkerhetsobjekt inte kända i förväg tooAzure AD kan sedan de inte användas för federerad inloggning. Azure AD Connect eller Windows PowerShell kan vara används tooprovision användaren säkerhetsobjekt.

Azure AD Connect kan vara används tooprovision säkerhetsobjekt tooyour domäner i din Azure AD-katalog från hello lokala Active Directory. Mer information finns i [integrera dina lokala kataloger med Azure Active Directory](active-directory-aadconnect.md).

Windows PowerShell kan också använda tooautomate att lägga till nya användare tooAzure AD och toosynchronize ändras från hello lokala katalog. toouse hello Windows PowerShell-cmdlets måste du hämta hello [Azure Active Directory-moduler](https://docs.microsoft.com/powershell/azure/install-adv2?view=azureadps-2.0).

Den här proceduren visar hur tooadd en enskild användare tooAzure AD.


1. Ansluta tooyour Azure AD-katalog som en Innehavaradministratör: ansluta MsolService.
2.  Skapa en ny UPN:` New-MsolUser
        -UserPrincipalName elwoodf1@contoso.com
        -ImmutableId ABCDEFG1234567890
        -DisplayName "Elwood Folk"
        -FirstName Elwood 
        -LastName Folk 
        -AlternateEmailAddresses "Elwood.Folk@contoso.com" 
        -LicenseAssignment "samlp2test:ENTERPRISEPACK" 
        -UsageLocation "US" ` 

Mer information om ”New MsolUser” utcheckningen, [http://technet.microsoft.com/library/dn194096.aspx](http://technet.microsoft.com/library/dn194096.aspx)

>[!NOTE]
>Hej ”UserPrinciplName” värdet måste matcha hello-värde som ska skickas för ”IDPEmail” i SAML 2.0 anspråk och hello ”ImmutableID” värdet måste matcha hello-värdet som skickas i ”NameID”-kontrollen.

## <a name="verify-single-sign-on-with-your-saml-20-idp"></a>Kontrollera enkel inloggning med din SAML 2.0 IDP
Granska hello information och utför hello steg i följande artiklar tooset in enkel inloggning med SAML 2.0 SP-Lite baserat identitetsprovider hello som Hej administratör innan du verifiera och hantera enkel inloggning (kallas även identitetsfederation):


1.  Du har granskat hello krav för Azure AD SAML 2.0-protokollet
2.  Du har konfigurerat din SAML 2.0 identitetsleverantören.
3.  Installera Windows PowerShell för enkel inloggning med SAML 2.0 identitetsleverantören.
4.  Skapa ett förtroende mellan SAML 2.0 identitetsleverantör och Azure AD
5.  Etableras en känd test användarens huvudnamn tooAzure Active Directory (Office 365) via Windows PowerShell eller Azure AD Connect.
6.  Konfigurera directory synkronisering med hjälp av [Azure AD Connect](active-directory-aadconnect.md).

När du ställer in enkel inloggning med din SAML 2.0 SP-Lite baserat identitet Provider, bör du kontrollera att den fungerar korrekt.

>[!NOTE]
>Om du har konverterat en domän i stället för att lägga till en kan det ta upp too24 timmar tooset in enkel inloggning.
Innan du verifiera enkel inloggning ska du slutföra konfigurationen av Active Directory-synkronisering, synkronisera dina kataloger och aktivera synkroniserade användare.

### <a name="use-hello-tool-tooverify-that-single-sign-on-has-been-set-up-correctly"></a>Använd hello verktyget tooverify som enkel inloggning har ställts in korrekt
tooverify att enkel inloggning har ställts in korrekt kan du utföra hello följa proceduren tooconfirm du är kan toosign i toohello Molntjänsten med företagets autentiseringsuppgifter.

Microsoft tillhandahåller ett verktyg som du kan använda tootest SAML 2.0 baserat identitetsleverantör. Innan du kör hello testa verktyget måste du ha konfigurerat en Azure AD-klient toofederate med din identitetsprovider.

>[!NOTE]
>hello anslutningen Analyzer kräver Internet Explorer 10 eller senare.



1. Hämta hello anslutningen Analyzer från [https://testconnectivity.microsoft.com/?tabid=Client](https://testconnectivity.microsoft.com/?tabid=Client).
2.  Klicka på Installera nu toobegin hämtar och installerar hello-verktyget.
3.  Välj ”det går inte att konfigurera federation med Office 365, Azure eller andra tjänster som använder Azure Active Directory”.
4.  När hello verktyget laddas ned och kör, ser hello anslutningen Diagnostics-fönstret. hello verktyget vägleds du genom att testa federation-anslutning.
5.  hello anslutningen Analyzer ska öppna din SAML 2.0 IDP för du toologon, ange hello autentiseringsuppgifter för hello UPN som du testar: ![SAML](media/active-directory-aadconnect-federation-saml-idp/saml1.png)
6.  Testa inloggningen fönstret du bör ange ett kontonamn och lösenord för hello Azure AD-klient som är konfigurerade toobe federerade med identitetsprovider SAML 2.0 på hello Federation. hello verktyget försöker toosign i med de autentiseringsuppgifterna och detaljerade resultat för tester som utfördes under hello inloggning försök kommer att framställas som utdata.
![SAML](media/active-directory-aadconnect-federation-saml-idp/saml2.png)
7. I fönstret visas en misslyckad resultatet tester. Klicka på granska detaljerade resultat att visa information om hello resultat för varje test som har utförts. Du kan också spara hello resultat toodisk i ordning tooshare dem.
 
>[!NOTE]
>hello anslutningen analyzer också testar Active Federation med hello WS *-baserade och ECP/PAOS protokoll. Om du inte använder dessa kan du ignorera hello följande fel: testa hello Active inloggning-flöde med hjälp av din identitetsleverantör Active federation slutpunkt.

### <a name="manually-verify-that-single-sign-on-has-been-set-up-correctly"></a>Verifiera manuellt att enkel inloggning har ställts in korrekt
Manuell kontroll ger ytterligare steg som du kan vidta tooensure din SAML 2.0-identitet providern fungerar korrekt i många scenarier.
tooverify som enkel inloggning har ställts in korrekt, fullständig hello följande steg:


1. Logga in tooyour molntjänst med samma inloggningsnamn som du använder för företagets inloggningsuppgifter hello på en domänansluten dator.
2.  Klicka i hello lösenordsrutan. Om enkel inloggning har konfigurerats, hello lösenordsrutan blir skuggad och hello följande meddelande visas ”: du är nu nödvändiga toosign i på <your company>”.
3.  Klicka på hello logga in på <your company> länk. Om du är kan toosign i har sedan enkel inloggning ställts in.

## <a name="next-steps"></a>Nästa steg


- [Active Directory Federation Services-hantering och anpassning med Azure AD Connect](active-directory-aadconnect-federation-management.md)
- [Kompatibilitetslista för Azure AD-federation](active-directory-aadconnect-federation-compatibility.md)
- [Azure AD Connect Anpassad Installation](active-directory-aadconnect-get-started-custom.md)
