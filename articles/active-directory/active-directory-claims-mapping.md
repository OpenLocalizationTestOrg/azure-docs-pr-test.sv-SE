---
title: "Anspråk mappning i Azure Active Directory (förhandsversion) | Microsoft Docs"
description: "Den här sidan beskrivs Azure Active Directory anspråk mappning."
services: active-directory
author: billmath
manager: femila
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: billmath
ms.openlocfilehash: ff07b9954d5c2ce71ab0ffd0db49fde15f323586
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="claims-mapping-in-azure-active-directory-public-preview"></a>Anspråk mappning i Azure Active Directory (förhandsversion)

>[!NOTE]
>Den här funktionen ersätter och ersätter hello [anspråk anpassning](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-claims-customization) erbjuds via hello portal idag. Om du har ändrat anspråk med hjälp av hello portal dessutom toohello diagram/PowerShell-metoden beskrivs i det här dokumentet hello samma program, token som utfärdats för programmet kommer att ignorera hello konfigurationen hello-portalen.
Konfigurationer som görs via hello metoderna som beskrivs i det här dokumentet visas inte i hello-portalen.

Den här funktionen används av klienten administratörer toocustomize hello anspråk som hänvisas till i token för ett visst program i klientorganisationen. Du kan använda anspråk mappning av principer för att:

- Välj vilka anspråk som ingår i token.
- Skapa anspråkstyper som inte redan finns.
- Välj eller ändra hello datakälla som hänvisas till i specifika anspråk.

>[!NOTE]
>Den här funktionen är för närvarande i förhandsversion. Förbereda toorevert eller ta bort alla ändringar. hello-funktionen är tillgänglig i alla Azure Active Directory (Azure AD)-prenumeration under förhandsversion. När hello funktionen blir allmänt tillgänglig, kan vissa aspekter av hello funktionen kräver en Azure Active Directory premium-prenumeration.

## <a name="claims-mapping-policy-type"></a>Anspråk mappning Principtyp
I Azure AD, en **princip** -objektet representerar en uppsättning regler som tillämpas på enskilda program, eller på alla program i en organisation. Varje typ av princip har en unik struktur med en uppsättning egenskaper som sedan tillämpas tooobjects toowhich som de har tilldelats.

Ett anspråk mappning princip är en typ av **princip** objekt som ändrar hello anspråk som hänvisas till i token som utfärdats för specifika program.

## <a name="claim-sets"></a>Anspråksuppsättningar
Det finns vissa uppsättningar av anspråk som definierar hur och när de används i token.

### <a name="core-claim-set"></a>Kärnor anspråksuppsättningen
Anspråk i hello core Anspråkstypen uppsättningen finns i varje token, oavsett princip. De här anspråken anses också vara begränsad och kan inte ändras.

### <a name="basic-claim-set"></a>Grundläggande anspråksuppsättningen
hello innehåller grundläggande anspråk hello anspråk som släpps som standard för token (i tillägget toohello kärnor anspråksuppsättningen). Dessa anspråk kan utelämnas eller ändras med hjälp av hello anspråk Mappa principer.

### <a name="restricted-claim-set"></a>Begränsad anspråksuppsättningen
Begränsat anspråk kan inte ändras med hjälp av Grupprincip. hello-datakällan kan inte ändras och ingen transformation som används vid generering av dessa anspråk.

#### <a name="table-1-json-web-token-jwt-restricted-claim-set"></a>Tabell 1: JSON-Webbtoken (JWT) begränsad anspråksuppsättning
|Anspråkstyp (namn)|
| ----- |
|_claim_names|
|_claim_sources|
|access_token|
|account_type|
|ACR|
|aktören|
|actortoken|
|AIO|
|altsecid|
|amr|
|app_chain|
|app_displayname|
|app_res|
|appctx|
|appctxsender|
|AppID|
|appidacr|
|kontrollen|
|at_hash|
|eller|
|auth_data|
|auth_time|
|authorization_code|
|azp|
|azpacr|
|c_hash|
|ca_enf|
|Kopia|
|cert_token_use|
|client_id|
|cloud_graph_host_name|
|cloud_instance_name|
|cnf|
|Koden|
|kontroller|
|credential_keys|
|CSR|
|csr_type|
|DeviceID|
|dns_names|
|domain_dns_name|
|domain_netbios_name|
|e_exp|
|E-post|
|slutpunkt|
|enfpolids|
|EXP|
|expires_on|
|grant_type|
|Diagrammet|
|group_sids|
|grupper|
|hasgroups|
|hash_alg|
|home_oid|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/AuthenticationInstant|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/AuthenticationMethod|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/Expiration|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/EXPIRED|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/emailaddress|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/Name|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/NameIdentifier|
|IAT|
|identityprovider|
|IDP|
|in_corp|
|Instans|
|ADR|
|isbrowserhostedapp|
|ISS|
|jwk|
|key_id|
|key_type|
|mam_compliance_url|
|mam_enrollment_url|
|mam_terms_of_use_url|
|mdm_compliance_url|
|mdm_enrollment_url|
|mdm_terms_of_use_url|
|nameid|
|NBF|
|netbios_name|
|temporärt ID|
|OID|
|on_prem_id|
|onprem_sam_account_name|
|onprem_sid|
|openid2_id|
|lösenord|
|platf|
|polids|
|pop_jwk|
|preferred_username|
|previous_refresh_token|
|primary_sid|
|PUID|
|pwd_exp|
|pwd_url|
|redirect_uri|
|refresh_token|
|refreshtoken|
|request_nonce|
|Resursen|
|Rollen|
|roles|
|Omfång|
|SCP|
|SID|
|Signatur|
|signin_state|
|src1|
|src2|
|Sub|
|tbid|
|tenant_display_name|
|tenant_region_scope|
|thumbnail_photo|
|tid|
|tokenAutologonEnabled|
|trustedfordelegation|
|unique_name|
|UPN|
|user_setting_sync_url|
|användarnamn|
|uti|
|ver|
|verified_primary_email|
|verified_secondary_email|
|wids|
|win_ver|

#### <a name="table-2-security-assertion-markup-language-saml-restricted-claim-set"></a>Tabell 2: Security Assertion Markup Language (SAML) begränsad anspråksuppsättning
|Anspråkstyp (URI)|
| ----- |
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/Expiration|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/EXPIRED|
|http://schemas.microsoft.com/Identity/Claims/accesstoken|
|http://schemas.microsoft.com/Identity/Claims/openid2_id|
|http://schemas.microsoft.com/Identity/Claims/identityprovider|
|http://schemas.microsoft.com/Identity/Claims/objectidentifier|
|http://schemas.microsoft.com/Identity/Claims/PUID|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/NameIdentifier [MR1] |
|http://schemas.microsoft.com/Identity/Claims/tenantid|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/AuthenticationInstant|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/AuthenticationMethod|
|http://schemas.microsoft.com/AccessControlService/2010/07/Claims/identityprovider|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/groups|
|http://schemas.microsoft.com/Claims/groups.Link|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/role|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/wids|
|http://schemas.microsoft.com/2014/09/devicecontext/Claims/iscompliant|
|http://schemas.microsoft.com/2014/02/devicecontext/Claims/isknown|
|http://schemas.microsoft.com/2012/01/devicecontext/Claims/ismanaged|
|http://schemas.microsoft.com/2014/03/psso|
|http://schemas.microsoft.com/Claims/authnmethodsreferences|
|http://schemas.xmlsoap.org/ws/2009/09/Identity/Claims/Actor|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/samlissuername|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/confirmationkey|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsaccountname|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/primarygroupsid|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/primarysid|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/authorizationdecision|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/Authentication|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/SID|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/denyonlyprimarygroupsid|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/denyonlyprimarysid|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/denyonlysid|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/denyonlywindowsdevicegroup|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsdeviceclaim|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsdevicegroup|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsfqbnversion|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowssubauthority|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/windowsuserclaim|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/x500distinguishedname|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/UPN|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/GroupSID|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/SPN|
|http://schemas.microsoft.com/ws/2008/06/Identity/Claims/ispersistent|
|http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/privatepersonalidentifier|
|http://schemas.microsoft.com/Identity/Claims/scope|

## <a name="claims-mapping-policy-properties"></a>Anspråk mappning Principegenskaper
Använda hello egenskaperna för ett anspråksprovider-mappning princip toocontrol vilka anspråk som släpps och där hello data kommer från. Om ingen princip har angetts hello system utfärdar token som innehåller hello kärnor anspråksuppsättningen, grundläggande hello anspråk och valfria anspråk som hello programmet har valt tooreceive.

### <a name="include-basic-claim-set"></a>Innehåller grundläggande anspråksuppsättningen

**Sträng:** IncludeBasicClaimSet

**Datatyp:** booleskt värde (True eller False)

**Sammanfattning:** den här egenskapen anger om hello grundläggande anspråksuppsättningen ingår i token som påverkas av den här principen. 

- Om set tooTrue, alla anspråk i hello grundläggande regeluppsättningen orsakat i token som påverkas av hello princip. 
- Om set tooFalse, anspråk i hello grundläggande regeluppsättningen inte hello token, om de inte enskilt läggs i hello anspråk schema-egenskapen för hello samma princip.

>[!NOTE] 
>Anspråk i hello core Anspråkstypen uppsättningen finns i varje token, oavsett vad den här egenskapen. 

### <a name="claims-schema"></a>Anspråk schema

**Sträng:** ClaimsSchema

**Datatyp:** JSON-blob med en eller flera anspråk schemat poster

**Sammanfattning:** egenskapen definierar vilka anspråk finns i hello-token som påverkas av hello princip, förutom grundläggande toohello anspråksuppsättningen och hello kärnor anspråksuppsättningen.
Viss information som krävs för varje anspråk schemat post definieras i den här egenskapen. Måste du ange var hello data kommer från (**värdet** eller **käll-ID par**), och som hello anspråksdata genereras som (**anspråk typen**).

### <a name="claim-schema-entry-elements"></a>Anspråk schemaelement post

**Värde:** hello värdet elementet definierar ett statiskt värde som hello data toobe orsakat i hello anspråket.

**Käll-ID par:** hello källa och ID-element definiera där hello data i hello begäran kommer från. 

hello källelementet måste anges tooone av hello följande: 


- ”användare”: hello data i hello anspråk är en egenskap på hello användarobjektet. 
- ”program”: hello data i hello anspråk är en egenskap på hello programmet (klient) tjänstens huvudnamn. 
- ”resurser”: hello data i hello anspråk är en egenskap på hello resurs tjänstens huvudnamn.
- ”målgruppen”: hello hello anspråk som är en egenskap på hello tjänstens huvudnamn som hello målgruppen för hello token (antingen hello klient- eller tjänstens huvudnamn).
- ”företag”: hello data i hello anspråk är en egenskap på hello resurs innehavarens företagets objekt.
- ”omvandling”: hello data i hello anspråk är från anspråkstransformering (se avsnittet hello ”anspråkstransformering” senare i den här artikeln). 

Om hello källa omvandling hello **TransformationID** element måste inkluderas i den här anspråksdefinitionen.

hello-ID-element identifierar vilken egenskap på hello källan innehåller hello värdet för hello anspråk. hello visas följande tabell hello värdena för ID för varje värde i datakällan.

#### <a name="table-3-valid-id-values-per-source"></a>Tabell 3: Giltig ID-värden per källa
|Källa|ID|Beskrivning|
|-----|-----|-----|
|Användare|Efternamn|Efternamn|
|Användare|givenName|Förnamn|
|Användare|visningsnamn|Visningsnamn|
|Användare|objekt-ID|Objekt-ID|
|Användare|E-post|E-postadress|
|Användare|userPrincipalName|Användarens huvudnamn|
|Användare|Avdelning|Avdelning|
|Användare|onpremisessamaccountname|På lokala Sam-kontonamn|
|Användare|NetBIOS-namn|NetBios-namn|
|Användare|DNS-domännamn|DNS-domännamn|
|Användare|onpremisesecurityidentifier|lokala säkerhetsidentifierare|
|Användare|Företagsnamn|Organisationens namn|
|Användare|streetAddress|Gatuadress|
|Användare|Postnummer|Postnummer|
|Användare|preferredlanguange|Önskat språk|
|Användare|onpremisesuserprincipalname|lokal UPN|
|Användare|mailNickname|Smeknamn för e-post|
|Användare|extensionattribute1|Attributet för anknytning 1|
|Användare|extensionattribute2|Attributet för anknytning 2|
|Användare|extensionattribute3|Attributet för anknytning 3|
|Användare|extensionattribute4|Attributet för anknytning 4|
|Användare|extensionattribute5|Attributet för anknytning 5|
|Användare|extensionattribute6|Attributet för anknytning 6|
|Användare|extensionattribute7|Attributet för anknytning 7|
|Användare|extensionattribute8|Attributet för anknytning 8|
|Användare|extensionattribute9|Attributet för anknytning 9|
|Användare|extensionattribute10|Attributet för anknytning 10|
|Användare|extensionattribute11|Attributet för anknytning 11|
|Användare|extensionattribute12|Attributet för anknytning 12|
|Användare|extensionattribute13|Attributet för anknytning 13|
|Användare|extensionattribute14|Attributet för anknytning 14|
|Användare|extensionattribute15|Attributet för anknytning 15|
|Användare|othermail|Andra e-post|
|Användare|Land|Land/region|
|Användare|city|Ort|
|Användare|state|Status|
|Användare|befattning|Befattning|
|Användare|EmployeeID|Anställnings-ID|
|Användare|facsimiletelephonenumber|Fax telefonnummer|
|program, resurs, målgrupp|visningsnamn|Visningsnamn|
|program, resurs, målgrupp|objekt|Objekt-ID|
|program, resurs, målgrupp|tags|Tjänstens huvudnamn tagg|
|Företag|tenantcountry|Klientens land|

**TransformationID:** hello TransformationID element måste anges om hello källelementet anges för ”omvandling”.

- Det här elementet måste matcha hello-ID-element för hello omvandling post i hello **ClaimsTransformation** egenskap som definierar hur hello data för denna begäran ska skapas.

**Anspråkstyp:** hello **JwtClaimType** och **SamlClaimType** element definiera vilka anspråk anspråk schemat transaktionen refererar till.

- Hej JwtClaimType måste innehålla hello av hello anspråk toobe som hänvisas till i JWTs.
- Hej SamlClaimType måste innehålla hello URI för hello anspråk toobe orsakat i SAML-token.

>[!NOTE]
>Namn och URI: er av anspråk i hello begränsad anspråk set inte kan användas för hello anspråk typen element. Mer information finns i avsnittet för hello ”undantag och begränsningar” senare i den här artikeln.

### <a name="claims-transformation"></a>Omvandling av anspråk

**Sträng:** ClaimsTransformation

**Datatyp:** JSON-blob med en eller flera poster för omvandling 

**Sammanfattning:** använder den här egenskapen tooapply vanliga transformationer toosource data, toogenerate hello utdata för anspråk som anges i hello anspråk schemat.

**ID:** Använd hello ID elementet tooreference omvandling posten i hello TransformationID anspråk schemat post. Värdet måste vara unikt för varje omvandling transaktion i den här principen.

**TransformationMethod:** hello TransformationMethod element som identifierar operationer som ska utföras toogenerate hello data för hello anspråk.

Utifrån hello-metod som valts är förväntas en uppsättning av in- och utdataenheter. Dessa definieras med hjälp av hello **InputClaims**, **indataparametrar** och **OutputClaims** element.

#### <a name="table-4-transformation-methods-and-expected-inputs-and-outputs"></a>Tabell 4: Omvandling metoder och förväntade in- och utdataenheter
|TransformationMethod|Förväntade indata|Utdata som förväntas|Beskrivning|
|-----|-----|-----|-----|
|Slå ihop|sträng1, sträng2, avgränsare|outputClaim|Kopplingar ange strängar med avgränsare mellan. Till exempel: sträng1 ”:foo@bar.com”, sträng2: ”sandbox”, avgränsare ”:”. resulterar i outputClaim ”:foo@bar.com.sandbox”|
|ExtractMailPrefix|E-post|outputClaim|Extraherar hello lokala en del av en e-postadress. Till exempel: e-post ”:foo@bar.com” resulterar i outputClaim: ”foo”. Om Nej @ finns logga returneras hello orignal Indatasträngen som är.|

**InputClaims:** använder en InputClaims elementet toopass hello data från en schemat post tooa anspråksomvandling. Den har två attribut: **ClaimTypeReferenceId** och **TransformationClaimType**.

- **ClaimTypeReferenceId** kopplas till ID-elementet i hello anspråk schemat post toofind hello lämplig inkommande anspråk. 
- **TransformationClaimType** är används toogive ett unikt namn toothis indata. Det här namnet måste matcha en av hello förväntades indata för hello transformation-metod.

**Indataparametrar:** använder en indataparametrar elementet toopass omvandling av tooa ett konstant värde. Den har två attribut: **värdet** och **ID**.

- **Värdet** skickas hello faktiska konstantvärde toobe.
- **ID** är används toogive ett unikt namn toothis indata. Det här namnet måste matcha en av hello förväntades indata för hello transformation-metod.

**OutputClaims:** använder en OutputClaims elementet toohold hello data som genereras av en omvandling och koppla den tooa anspråk schemat post. Den har två attribut: **ClaimTypeReferenceId** och **TransformationClaimType**.

- **ClaimTypeReferenceId** kopplas till hello-ID för hello anspråk schemat post toofind hello lämplig utgående anspråk.
- **TransformationClaimType** är används toogive ett unikt namn toothis utdata. Det här namnet måste matcha en för hello förväntades utflöden för hello transformation-metod.

### <a name="exceptions-and-restrictions"></a>Undantag och begränsningar

**SAML-NameID och UPN:** hello attribut från vilken du datakälla hello NameID och UPN-värden och hello anspråk transformationer som tillåts är begränsade.

#### <a name="table-5-attributes-allowed-as-a-data-source-for-saml-nameid"></a>Tabell 5: Attribut användas som en datakälla för SAML-NameID
|Källa|ID|Beskrivning|
|-----|-----|-----|
|Användare|E-post|E-postadress|
|Användare|userPrincipalName|Användarens huvudnamn|
|Användare|onpremisessamaccountname|På lokala Sam-kontonamn|
|Användare|EmployeeID|Anställnings-ID|
|Användare|extensionattribute1|Attributet för anknytning 1|
|Användare|extensionattribute2|Attributet för anknytning 2|
|Användare|extensionattribute3|Attributet för anknytning 3|
|Användare|extensionattribute4|Attributet för anknytning 4|
|Användare|extensionattribute5|Attributet för anknytning 5|
|Användare|extensionattribute6|Attributet för anknytning 6|
|Användare|extensionattribute7|Attributet för anknytning 7|
|Användare|extensionattribute8|Attributet för anknytning 8|
|Användare|extensionattribute9|Attributet för anknytning 9|
|Användare|extensionattribute10|Attributet för anknytning 10|
|Användare|extensionattribute11|Attributet för anknytning 11|
|Användare|extensionattribute12|Attributet för anknytning 12|
|Användare|extensionattribute13|Attributet för anknytning 13|
|Användare|extensionattribute14|Attributet för anknytning 14|
|Användare|extensionattribute15|Attributet för anknytning 15|

#### <a name="table-6-transformation-methods-allowed-for-saml-nameid"></a>Tabell 6: Omvandling metoder som tillåts för SAML-NameID
|TransformationMethod|Begränsningar|
| ----- | ----- |
|ExtractMailPrefix|Ingen|
|Slå ihop|hello-suffix som ansluten måste vara en verifierad domän av hello resurs innehavaren.|

### <a name="custom-signing-key"></a>Anpassad signeringsnyckel
En anpassad signeringsnyckel måste tilldelas toohello tjänstens huvudnamn objekt för ett anspråksprovider-mappning princip tootake effekt. Alla token som utfärdas som har påverkats av hello princip signeras med den här nyckeln. Program måste vara konfigurerade tooaccept token som signerats med den här nyckeln. Detta säkerställer att bekräftelse att token har ändrats av hello skapat hello anspråk mappning princip. Detta skyddar program från anspråk som mappning av principer som skapas med skadliga aktörer.

### <a name="cross-tenant-scenarios"></a>Scenarier för flera innehavare
Anspråk Mappa principer gäller inte tooguest användare. Om en gästanvändare försöker tooaccess ett program med ett anspråk mappning tilldelats tooits service principal, hello standardtoken utfärdas (hello policyn har ingen effekt).

## <a name="claims-mapping-policy-assignment"></a>Anspråk mappning tilldelning av principer
Anspråk mappning principer kan endast tilldelas tooservice UPN-objekt.

### <a name="example-claims-mapping-policies"></a>Exempel anspråk mappning principer

Många scenarier är möjliga i Azure AD när du kan anpassa anspråk som hänvisas till i token för specifika tjänstens huvudnamn. I det här avsnittet går vi igenom några vanliga scenarier som kan hjälpa dig att sätta hur toouse hello anspråk mappning principtypen.

#### <a name="prerequisites"></a>Krav
I följande exempel hello, du skapa, uppdatera, länkar och ta bort principer för tjänstens huvudnamn. Om du är ny tooAzure AD, rekommenderar vi att du lär dig mer om hur tooget en Azure AD-klient innan du fortsätter med de här exemplen. 

tooget igång, hello följande steg:


1. Hämta senaste hello [Azure AD PowerShell-modulen offentliga förhandsversionen](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.127).
2.  Kör hello Anslut kommandot toosign i tooyour Azure AD-administratörskonto. Kör det här kommandot varje gång startar du en ny session.
    
     ``` powershell
    Connect-AzureAD -Confirm
    
    ```
3.  toosee alla principer som har skapats i din organisation, kör hello efter kommandot. Vi rekommenderar att du kör det här kommandot efter de flesta åtgärder i hello följande scenarier, toocheck som principerna skapas som förväntat.
   
    ``` powershell
        Get-AzureADPolicy
    
    ```
#### <a name="example-create-and-assign-a-policy-tooomit-hello-basic-claims-from-tokens-issued-tooa-service-principal"></a>Exempel: Skapa och tilldela en princip tooomit hello grundläggande anspråk från token som utfärdats tooa tjänstens huvudnamn.
I det här exemplet skapar du en princip som tar bort hello grundläggande anspråksuppsättningen från token som utfärdats toolinked tjänstens huvudnamn.


1. Skapa ett anspråk mappning av principen. Den här principen, länkade toospecific tjänstens huvudnamn tar bort hello grundläggande anspråksuppsättningen från token.
    1. toocreate hello princip köra det här kommandot: 
    
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"false"}}') -DisplayName "OmitBasicClaims” -Type "ClaimsMappingPolicy"
    ```
    2. toosee din nya principen och tooget hello princip ObjectId kör hello följande kommando:
    
     ``` powershell
    Get-AzureADPolicy
    ```
2.  Tilldela hello princip tooyour tjänstens huvudnamn. Du måste också tooget hello ObjectId för tjänstens huvudnamn. 
    1.  toosee din organisations tjänstens huvudnamn, du kan ställa frågor till Microsoft Graph. Eller logga in tooyour Azure AD-kontot i Azure AD Graph Explorer.
    2.  När du har hello ObjectId för din service principal, kör hello som följande kommando:  
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
    ```
#### <a name="example-create-and-assign-a-policy-tooinclude-hello-employeeid-and-tenantcountry-as-claims-in-tokens-issued-tooa-service-principal"></a>Exempel: Skapa och tilldela en princip tooinclude hello EmployeeID och TenantCountry som anspråk i token som utfärdas tooa tjänstens huvudnamn.
I det här exemplet skapar du en princip som lägger till hello EmployeeID och TenantCountry tootokens utfärdade toolinked tjänstens huvudnamn. hello EmployeeID genereras som hello namn Anspråkstyp i både SAML-token och JWTs. Hej TenantCountry genereras som hello land Anspråkstyp i både SAML-token och JWTs. I det här exemplet fortsätta vi tooinclude hello enkla anspråk i hello-token.

1. Skapa ett anspråk mappning av principen. Den här principen, länkade toospecific tjänstens huvudnamn lägger till hello EmployeeID och TenantCountry anspråk tootokens.
    1. toocreate hello princip köra det här kommandot:  
     
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema": [{"Source":"user","ID":"employeeid","SamlClaimType":"http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name","JwtClaimType":"name"},{"Source":"company","ID":" tenantcountry ","SamlClaimType":" http://schemas.xmlsoap.org/ws/2005/05/identity/claims/country ","JwtClaimType":"country"}]}}') -DisplayName "ExtraClaimsExample” -Type "ClaimsMappingPolicy"
    ```
    
    2. toosee din nya principen och tooget hello princip ObjectId kör hello följande kommando:
     
     ``` powershell  
    Get-AzureADPolicy
    ```
2.  Tilldela hello princip tooyour tjänstens huvudnamn. Du måste också tooget hello ObjectId för tjänstens huvudnamn. 
    1.  toosee din organisations tjänstens huvudnamn, du kan ställa frågor till Microsoft Graph. Eller logga in tooyour Azure AD-kontot i Azure AD Graph Explorer.
    2.  När du har hello ObjectId för din service principal, kör hello som följande kommando:  
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
    ```
#### <a name="example-create-and-assign-a-policy-that-uses-a-claims-transformation-in-tokens-issued-tooa-service-principal"></a>Exempel: Skapa och tilldela en princip som använder en anspråkstransformering i token som utfärdats tooa tjänstens huvudnamn.
I det här exemplet skapar du en princip som genererar ett anpassat anspråk ”JoinedData” tooJWTs utfärdade toolinked tjänstens huvudnamn. Denna begäran innehåller ett värde som skapats genom att anslutas hello data som lagras i hello extensionattribute1 attribut för hello användarobjekt med ”.sandbox”. I det här exemplet exkludera vi hello enkla anspråk i hello-token.


1. Skapa ett anspråk mappning av principen. Den här principen, länkade toospecific tjänstens huvudnamn lägger till hello EmployeeID och TenantCountry anspråk tootokens.
    1. toocreate hello princip köra det här kommandot: 
     
     ``` powershell
    New-AzureADPolicy -Definition @('{"ClaimsMappingPolicy":{"Version":1,"IncludeBasicClaimSet":"true", "ClaimsSchema":[{"Source":"user","ID":"extensionattribute1"},{"Source":"transformation","ID":"DataJoin","TransformationId":"JoinTheData","JwtClaimType":"JoinedData"}],"ClaimsTransformation":[{"ID":"JoinTheData","TransformationMethod":"Join","InputClaims":[{"ClaimTypeReferenceId":"extensionattribute1","TransformationClaimType":"string1"}], "InputParameters": [{"Id":"string2","Value":"sandbox"},{"Id":"separator","Value":"."}],"OutputClaims":[{"ClaimTypeReferenceId":"DataJoin","TransformationClaimType":"outputClaim"}]}]}}') -DisplayName "TransformClaimsExample” -Type "ClaimsMappingPolicy"
    ```
    
    2. toosee din nya principen och tooget hello princip ObjectId kör hello följande kommando: 
     
     ``` powershell
    Get-AzureADPolicy
    ```
2.  Tilldela hello princip tooyour tjänstens huvudnamn. Du måste också tooget hello ObjectId för tjänstens huvudnamn. 
    1.  toosee din organisations tjänstens huvudnamn, du kan ställa frågor till Microsoft Graph. Eller logga in tooyour Azure AD-kontot i Azure AD Graph Explorer.
    2.  När du har hello ObjectId för din service principal, kör hello som följande kommando: 
     
     ``` powershell
    Add-AzureADServicePrincipalPolicy -Id <ObjectId of hello ServicePrincipal> -RefObjectId <ObjectId of hello Policy>
    ```
