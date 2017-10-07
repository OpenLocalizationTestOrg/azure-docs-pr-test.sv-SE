---
title: "aaaHow tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory | Microsoft Docs"
description: "Konfigurera din domänanslutna Windows-enheter tooregister automatiskt med Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/16/2017
ms.author: markvi
ms.reviewer: jairoc
ms.openlocfilehash: 6a1aab753f5456ed06ba7979ab05f70f29b4ddee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a>Hur tooconfigure automatisk registrering av Windows-domänanslutna enheter med Azure Active Directory

toouse [Azure Active Directory enhetsbaserad villkorlig åtkomst](active-directory-conditional-access-azure-portal.md), dina datorer måste vara registrerad med Azure Active Directory (AD Azure). Du kan hämta en lista över registrerade enheter i organisationen med hjälp av hello [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet i hello [Azure Active Directory PowerShell-modulen](/powershell/azure/install-msonlinev1?view=azureadps-2.0). 

Den här artikeln innehåller hello steg för att konfigurera hello automatisk registrering av Windows-domänanslutna enheter med Azure AD i din organisation.


Mer information om:

- Villkorlig åtkomst finns [Azure Active Directory enhetsbaserad villkorlig åtkomst](active-directory-conditional-access-azure-portal.md). 
- Windows 10-enheter i hello arbetsplats och hello förbättrad upplevelse när registrerad med Azure AD finns [Windows 10 för hello enterprise: Använd enheter för arbetet](active-directory-azureadjoin-windows10-devices-overview.md).
- Windows 10 Enterprise E3 i CSP finns hello [Windows 10 Enterprise E3 i CSP översikt](https://docs.microsoft.com/en-us/windows/deployment/windows-10-enterprise-e3-overview).


## <a name="before-you-begin"></a>Innan du börjar

Innan du börjar konfigurera hello automatisk registrering av Windows-domänanslutna enheter i din miljö, bör du bekanta dig med hello stöds scenarier och hello begränsningar.  

tooimprove hello läsbarhet hello beskrivningar, i det här avsnittet använder hello följande villkor: 

- **Aktuella Windows-enheter** -termen refererar toodomain-anslutna enheter som kör Windows 10 eller Windows Server 2016.
- **Windows-klientversioner enheter** -termen refererar tooall **stöds** domänanslutna Windows-enheter som inte är igång Windows 10 eller Windows Server 2016.  


### <a name="windows-current-devices"></a>Aktuella Windows-enheter

- För enheter som kör hello Windows-operativsystemet, bör du använda Windows 10 årsdagar Update (version 1607) eller senare. 
- Hej registrering av den aktuella Windows-enheter **är** i ofedererad miljöer, till exempel lösenord hash-synkronisering konfigurationer som stöds.  


### <a name="windows-down-level-devices"></a>Äldre Windows-enheter

- hello följande äldre Windows-enheter stöds:
    - Windows 8.1
    - Windows 7
    - Windows Server 2012 R2
    - Windows Server 2012
    - Windows Server 2008 R2
- Hej registrering av Windows-klientversioner enheter **är** stöds i ofedererad miljöer genom sömlös enkel inloggning [Azure Active Directory sömlös enkel inloggning](https://aka.ms/hybrid/sso).
- Hej registrering av Windows-klientversioner enheter **är inte** stöds för enheter med hjälp av centrala profiler. Om du beroende av centrala profiler eller inställningar, använder du Windows 10.



## <a name="prerequisites"></a>Krav

Innan du börjar att aktivera hello automatisk registrering för domänanslutna enheter i din organisation, behöver du toomake till att du kör en uppdaterad version av Azure AD connect.

Azure AD Connect:

- Behåller hello associationen mellan hello datorkonto i din lokala Active Directory (AD) och hello enhetsobjekt i Azure AD. 
- Gör andra enheter relaterade funktioner som Windows Hello för företag.



## <a name="configuration-steps"></a>Konfigurationssteg

Det här avsnittet innehåller steg hello som krävs för alla scenarier för typisk konfiguration.  
Använd följande tabell tooget en översikt över hello steg som krävs för ditt scenario hello:  



| Steg                                      | Windows aktuella och lösenord hash-synkronisering | Windows aktuella och federation | Windows-klientversioner |
| :--                                        | :-:                                    | :-:                            | :-:                |
| Steg 1: Konfigurera tjänstanslutningspunkten | ![Markera][1]                            | ![Markera][1]                    | ![Markera][1]        |
| Steg 2: Konfigurera utfärdande av anspråk           |                                        | ![Markera][1]                    | ![Markera][1]        |
| Steg 3: Aktivera Windows 10-enheter      |                                        |                                | ![Markera][1]        |
| Steg 4: Distribution av kontrollen och distribution     | ![Markera][1]                            | ![Markera][1]                    | ![Markera][1]        |
| Steg 5: Kontrollera registrerade enheter          | ![Markera][1]                            | ![Markera][1]                    | ![Markera][1]        |



## <a name="step-1-configure-service-connection-point"></a>Steg 1: Konfigurera tjänstanslutningspunkten

hello service anslutningsobjekt punkter (SCP) används av dina enheter under hello registrering toodiscover information för Azure AD-klient. Hello SCP-objektet för hello automatisk registrering för domänanslutna enheter måste finnas i hello namngivning kontexten konfigurationspartitionen i hello datorns skog i din lokala Active Directory (AD). Det finns endast en konfigurationsnamngivningskontexten per skog. Hello tjänstanslutningspunkten måste finnas i alla skogar som innehåller domänanslutna datorer i en konfiguration för flera skogar Active Directory.

Du kan använda hello [ **Get-ADRootDSE** ](https://technet.microsoft.com/library/ee617246.aspx) cmdlet tooretrieve hello konfigurationsnamngivningskontexten i skogen.  

För en skog med hello Active Directory-domännamn *fabrikam.com*, hello konfigurationsnamngivningskontexten är:

`CN=Configuration,DC=fabrikam,DC=com`

I din skog finns hello SCP-objektet för hello automatisk registrering för domänanslutna enheter här:  

`CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Your Configuration Naming Context]`

Beroende på hur du har distribuerat Azure AD Connect, har hello SCP-objektet redan konfigurerats.
Du kan verifiera att hello objektet hello och hämta hello identifiering värden med hjälp av hello följande Windows PowerShell-skript: 

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=fabrikam,DC=com";

    $scp.Keywords;

Hej **$scp. Nyckelord** utdata visar hello Azure AD-klient information, till exempel:

    azureADName:microsoft.com
    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

Om hello tjänstanslutningspunkten inte finns, kan du skapa den genom att köra hello `Initialize-ADSyncDomainJoinedComputerSync` cmdlet på Azure AD Connect-servern. Enterprise admin-autentiseringsuppgifter är obligatoriska toorun denna cmdlet.  
hello-cmdlet:

- Skapar hello tjänstanslutningspunkt i Active Directory-skog för hello Azure AD Connect är ansluten till. 
- Kräver toospecify hello `AdConnectorAccount` parameter. Detta är hello-konto som har konfigurerats som Active Directory connector-kontot i Azure AD connect. 


hello visar följande skript ett exempel för att använda hello cmdlet. I det här skriptet `$aadAdminCred = Get-Credential` kräver tootype ett användarnamn. Du behöver tooprovide hello användarnamnet i hello användarens huvudnamn (UPN) format (`user@example.com`). 


    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;

Hej `Initialize-ADSyncDomainJoinedComputerSync` cmdlet:

- Använder hello Active Directory PowerShell-modul, som förlitar sig på Active Directory-webbtjänster som körs på en domänkontrollant. Active Directory Web Services fungerar på domänkontrollanter som kör Windows Server 2008 R2 och senare.
- Stöds endast av hello **MSOnline PowerShell Modulversion 1.1.166.0**. toodownload den här modulen används [länk](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185).   

Använd hello skriptet nedan toocreate hello tjänstanslutningspunkten för domänkontrollanter som kör Windows Server 2008 eller tidigare versioner.

Du bör använda hello följande skript toocreate hello tjänstanslutningspunkten i varje skog där det finns datorer i en konfiguration för flera skogar:
 
    $verifiedDomain = "contoso.com"    # Replace this with any of your verified domain names in Azure AD
    $tenantID = "72f988bf-86f1-41af-91ab-2d7cd011db47"    # Replace this with you tenant ID
    $configNC = "CN=Configuration,DC=corp,DC=contoso,DC=com"    # Replace this with your AD configuration naming context

    $de = New-Object System.DirectoryServices.DirectoryEntry
    $de.Path = "LDAP://CN=Services," + $configNC

    $deDRC = $de.Children.Add("CN=Device Registration Configuration", "container")
    $deDRC.CommitChanges()

    $deSCP = $deDRC.Children.Add("CN=62a0ff2e-97b9-4513-943f-0d221bd30080", "serviceConnectionPoint")
    $deSCP.Properties["keywords"].Add("azureADName:" + $verifiedDomain)
    $deSCP.Properties["keywords"].Add("azureADId:" + $tenantID)

    $deSCP.CommitChanges()


## <a name="step-2-setup-issuance-of-claims"></a>Steg 2: Konfigurera utfärdande av anspråk

I en federerad Azure AD-konfiguration enheter förlitar sig på Active Directory Federation Services (AD FS) eller 3 part lokalt federation service tooauthenticate tooAzure AD. Enheter autentiserar tooget en åtkomst-token tooregister mot hello Azure Active Directory Device Registration Service (DRS Azure).

Windows aktuella enheter autentiseras med integrerad Windows-autentisering tooan aktiv WS-Trust slutpunkt (1.3 eller 2005 versioner) hos hello lokala federationstjänsten.

> [!NOTE]
> När du använder AD FS, antingen **adfs/services/trust/13/windowstransport** eller **adfs/services/trust/2005/windowstransport** måste vara aktiverat. Om du använder hello webbproxy för autentisering kan också se till att den här slutpunkten har publicerats via hello proxy. Du kan se vilka slutpunkter aktiveras via hello AD FS-hanteringskonsol under **Service > slutpunkter**.
>
>Om du inte har AD FS som federationstjänsten lokalt, följ hello instruktionerna för din leverantör toomake att de stöder WS-Trust 1.3 eller 2005 slutpunkter och dessa publiceras via hello Metadata Exchange-fil (MEX).

hello måste följande anspråk finnas i hello-token som tas emot av Azure DRS för toocomplete för registrering av enheten. Azure DRS skapar ett enhetsobjekt i Azure AD med en del av den här informationen som sedan används av Azure AD Connect tooassociate hello nyskapad enhetsobjekt med hello datorn kontot lokalt.

* `http://schemas.microsoft.com/ws/2012/01/accounttype`
* `http://schemas.microsoft.com/identity/claims/onpremobjectguid`
* `http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`

Om du har mer än ett verifierat domännamn måste tooprovide hello följande anspråk för datorer:

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`

Om du redan utfärdar ett ImmutableID anspråk (t.ex. Alternativt inloggnings-ID) måste tooprovide ett motsvarande anspråk för datorer:

* `http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`

I följande avsnitt hello, hittar du information om:
 
- hello värden varje anspråk ska ha
- Hur en definition skulle se ut i AD FS

hello definition hjälper dig att tooverify om hello värden finns eller om du behöver toocreate dem.

> [!NOTE]
> Om du inte använder AD FS för federationsservern lokalt, följer du leverantörens instruktioner toocreate hello lämplig konfiguration tooissue dessa anspråk.

### <a name="issue-account-type-claim"></a>Problemet konto typen anspråk

**`http://schemas.microsoft.com/ws/2012/01/accounttype`**-Detta anspråk måste innehålla ett värde av **DJ**, som identifierar hello enheten som en domänansluten dator. I AD FS kan du lägga till en regel för omvandling av utfärdande som ser ut så här:

    @RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );

### <a name="issue-objectguid-of-hello-computer-account-on-premises"></a>Utfärda objectGUID av hello datorn kontot Lokal

**`http://schemas.microsoft.com/identity/claims/onpremobjectguid`**-Detta anspråk måste innehålla hello **objectGUID** värdet för hello lokalt datorkonto. I AD FS kan du lägga till en regel för omvandling av utfärdande som ser ut så här:

    @RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );
 
### <a name="issue-objectsid-of-hello-computer-account-on-premises"></a>Utfärda objectSID av hello datorn kontot Lokal

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid`**-Detta anspråk måste innehålla hello hello **objectSid** värdet för hello lokalt datorkonto. I AD FS kan du lägga till en regel för omvandling av utfärdande som ser ut så här:

    @RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);

### <a name="issue-issuerid-for-computer-when-multiple-verified-domain-names-in-azure-ad"></a>Utfärda issuerID för datorn när flera verifierat domännamn i Azure AD

**`http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid`**-Detta anspråk måste innehålla hello identifierare URI (Uniform Resource) för någon av hello verifierat domännamn som ansluter med hello lokalt federation (AD FS eller 3 part) utfärdande hello tjänsttoken. I AD FS kan du lägga till regler för utfärdandetransformering som ser ut som om hello viktiga nedan i den specifika ordning efter hello de som ovan. Observera att en regel tooexplicitly problemet hello regel för användare som är nödvändiga. En första regel som identifierar användare eller datorautentisering läggs till i hello reglerna nedan.

    @RuleName = "Issue account type with hello value User when its not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue hello IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://<verified-domain-name>/adfs/services/trust/"
    );


I hello anspråket ovan

- `$<domain>`är hello AD FS-tjänstens URL
- `<verified-domain-name>`en platshållare som du behöver tooreplace med en av dina verifierat domännamn i Azure AD



Mer information om verifierat domännamn finns [lägga till en anpassad domän namn tooAzure Active Directory](active-directory-add-domain.md).  
tooget en lista över verifierade företagets-domäner som du kan använda hello [Get-MsolDomain](/powershell/module/msonline/get-msoldomain?view=azureadps-1.0) cmdlet. 

![Get-MsolDomain](./media/active-directory-conditional-access-automatic-device-registration-setup/01.png)

### <a name="issue-immutableid-for-computer-when-one-for-users-exist-eg-alternate-login-id-is-set"></a>Utfärda ImmutableID för datorn när en användare finns (t.ex. Alternativt inloggnings-ID har angetts)

**`http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID`**-Detta anspråk måste innehålla ett giltigt värde för datorer. I AD FS kan du skapa en regel för omvandling av utfärdande som följer:

    @RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );

### <a name="helper-script-toocreate-hello-ad-fs-issuance-transform-rules"></a>Helper skriptet toocreate hello AD FS regler för utfärdandetransformering

hello hjälper följande skript dig med hello utfärdande hello skapa regler som beskrivs ovan.

    $multipleVerifiedDomainNames = $false
    $immutableIDAlreadyIssuedforUsers = $false
    $oneOfVerifiedDomainNames = 'example.com'   # Replace example.com with one of your verified domains
    
    $rule1 = '@RuleName = "Issue account type for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "DJ"
    );'

    $rule2 = '@RuleName = "Issue object GUID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'

    $rule3 = '@RuleName = "Issue objectSID for domain-joined computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(claim = c2);'

    $rule4 = ''
    if ($multipleVerifiedDomainNames -eq $true) {
    $rule4 = '@RuleName = "Issue account type with hello value User when it is not a computer"
    NOT EXISTS(
    [
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "DJ"
    ]
    )
    => add(
        Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value = "User"
    );
    
    @RuleName = "Capture UPN when AccountType is User and issue hello IssuerID"
    c1:[
        Type == "http://schemas.xmlsoap.org/claims/UPN"
    ]
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", 
        Value == "User"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = regexreplace(
        c1.Value, 
        ".+@(?<domain>.+)", 
        "http://${domain}/adfs/services/trust/"
        )
    );
    
    @RuleName = "Issue issuerID for domain-joined computers"
    c:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", 
        Value = "http://' + $oneOfVerifiedDomainNames + '/adfs/services/trust/"
    );'
    }

    $rule5 = ''
    if ($immutableIDAlreadyIssuedforUsers -eq $true) {
    $rule5 = '@RuleName = "Issue ImmutableID for computers"
    c1:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", 
        Value =~ "-515$", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ] 
    && 
    c2:[
        Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", 
        Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"
    ]
    => issue(
        store = "Active Directory", 
        types = ("http://schemas.microsoft.com/LiveID/Federation/2008/05/ImmutableID"), 
        query = ";objectguid;{0}", 
        param = c2.Value
    );'
    }

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules 

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules 

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString 

### <a name="remarks"></a>Kommentarer 

- Skriptet lägger till hello toohello befintliga regler. Körs inte hello skriptet skulle två gånger eftersom hello uppsättning regler läggas till två gånger. Se till att motsvarande inte finns några regler för dessa anspråk (under hello motsvarande) innan du kör skriptet hello igen.

- Om du har flera verifierat domännamn (som visas i hello Azure AD-portalen eller via hello Get-MsolDomains cmdlet), ange hello-värde **$multipleVerifiedDomainNames** i hello skript för**$true**. Kontrollera också att du tar bort alla befintliga issuerid anspråk som kan ha skapats av Azure AD Connect eller via andra sätt. Här är ett exempel på den här regeln:


        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"]
        => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)",  "http://${domain}/adfs/services/trust/")); 

- Om du redan har skickat en **ImmutableID** anspråk för användarkonton genom att ange hello värdet för **$immutableIDAlreadyIssuedforUsers** i hello skript för**$true**.

## <a name="step-3-enable-windows-down-level-devices"></a>Steg 3: Aktivera äldre Windows-enheter

Om vissa enheter domänanslutna Windows äldre enheter, måste du:

- Ange en princip i Azure AD tooenable användare tooregister enheter.
 
- Konfigurera din lokala federation service tooissue anspråk toosupport **Windows IWA (Integrated Authentication)** för registrering av enheten.
 
- Lägg till hello Azure AD enheten autentisering slutpunkt toohello lokalt intranät zoner tooavoid certifikat uppmanar vid autentisering hello enhet.

### <a name="set-policy-in-azure-ad-tooenable-users-tooregister-devices"></a>Ange princip för i Azure AD tooenable användare tooregister enheter

tooregister Windows äldre enheter behöver du toomake till att hello inställningen tooallow användare tooregister enheter i Azure AD har angetts. Du hittar den här inställningen under i hello Azure-portalen:

`Azure Active Directory > Users and groups > Device settings`
    
hello följande princip måste anges för**alla**: **användarna kan registrera sina enheter med Azure AD**

![Registrera enheter](./media/active-directory-conditional-access-automatic-device-registration-setup/23.png)


### <a name="configure-on-premises-federation-service"></a>Konfigurera lokala federationstjänsten 

Federationstjänsten lokalt måste ha stöd för utfärdande hello **authenticationmehod** och **wiaormultiauthn** anspråk när du tar emot en autentisering begära toohello Azure AD förlitande part innehar en resouce_params parameter med ett kodat värde som visas nedan:

    eyJQcm9wZXJ0aWVzIjpbeyJLZXkiOiJhY3IiLCJWYWx1ZSI6IndpYW9ybXVsdGlhdXRobiJ9XX0

    which decoded is {"Properties":[{"Key":"acr","Value":"wiaormultiauthn"}]}

När en sådan begäran kommer hello lokalt federationstjänsten måste autentisera hello-användare som använder integrerad Windows-autentisering och vid lyckades, utfärda hello följande två anspråk:

    http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/windows
    http://schemas.microsoft.com/claims/wiaormultiauthn

I AD FS, du måste lägga till en regel för omvandling av utfärdande som överför via hello autentiseringsmetod.  

**tooadd regeln:**

1. I hello AD FS-hanteringskonsolen gå för`AD FS > Trust Relationships > Relying Party Trusts`.
2. Högerklicka på hello Identitetsplattformen för Microsoft Office 365 förlitande part förtroende-objekt och välj sedan **redigera Anspråksregler**.
3. På hello **regler för Utfärdandetransformering** väljer **Lägg till regel**.
4. I hello **anspråksregel** mall-listan, Välj **skicka anspråk med en anpassad regel**.
5. Välj **nästa**.
6. I hello **Regelnamn för anspråk** skriver **Auth metoden Anspråksregel**.
7. I hello **anspråksregel** rutan, typen hello följande regel:

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"] => issue(claim = c);`

8. Skriv hello PowerShell-kommandot nedan på federationsservern när du ersätter  **\<RPObjectName\>**  med hello förlitande part objektnamn för din Azure AD förlitande part förtroende-objekt. Det här objektet vanligen namnet **Identitetsplattformen för Microsoft Office 365**.
   
    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

### <a name="add-hello-azure-ad-device-authentication-end-point-toohello-local-intranet-zones"></a>Lägg till hello Azure AD device autentisering slutpunkt toohello lokalt intranät zoner

tooavoid certifikat uppmanar när användare registrera enheter autentiseras tooAzure AD du skicka en princip tooyour domänanslutna enheter tooadd hello följande URL toohello lokalt intranät i Internet Explorer:

`https://device.login.microsoftonline.com`

## <a name="step-4-control-deployment-and-rollout"></a>Steg 4: Distribution av kontrollen och distribution

När du har slutfört steg hello krävs är domänanslutna enheter klar tooautomatically registrera med Azure AD. Alla domänanslutna enheter som kör Windows 10 årsdagar Update och Windows Server 2016 registrera med Azure AD på enheten startas om automatiskt eller användarinloggning. Att registrera nya enheter med Azure AD när hello enheten startas om efter hello domän join-åtgärden har slutförts.

Enheter som fanns tidigare arbetsplatsanslutna tooAzure AD (till exempel för Intune) övergång för ”*domänanslutna, AAD registrerade*”; men tar det lite tid för den här processen toocomplete på alla enheter på grund av toohello normal flödet av domän- och användaraktivitet.

### <a name="remarks"></a>Kommentarer

- Du kan använda en Grupprincip objektet toocontrol hello distribution av automatisk registrering av Windows 10 och Windows Server 2016 domänanslutna datorer.

- Windows 10 November 2015 Update automatiskt registrerar med Azure AD **endast** om hello grupprincipobjektet för distribution har angetts.

- toorollout hello automatisk registrering av äldre Windows-datorer, kan du distribuera en [Windows Installer-paketet](#windows-installer-packages-for-non-windows-10-computers) toocomputers som du väljer.

- Om du överför hello gruppolicy objekt tooWindows 8.1 domänanslutna enheter görs registrering; men det rekommenderas att du använder hello [Windows Installer-paketet](#windows-installer-packages-for-non-windows-10-computers) tooregister alla dina Windows äldre enheter. 

### <a name="create-a-group-policy-object"></a>Skapa ett grupprincipobjekt 

toocontrol hello distributionen av automatisk registrering av aktuella Windows-datorer, bör du distribuera hello **registrera domänanslutna datorer som enheter** gruppolicy objekt toohello enheter du vill tooregister. Du kan till exempel distribuera princip hello tooan organisationsenhet eller tooa säkerhetsgrupp.

**tooset hello princip:**

1. Öppna **Serverhanteraren**, och sedan gå för`Tools > Group Policy Management`.
2. Gå toohello domännod som motsvarar toohello domänen där du vill tooactivate automatisk registrering av aktuella Windows-datorer.
3. Högerklicka på **grupprincipobjekt**, och välj sedan **ny**.
4. Ange ett namn för grupprincipobjekt. Till exempel *automatisk registrering tooAzure AD*. Välj **OK**.
5. Högerklicka på din nya grupprincipobjekt och välj sedan **redigera**.
6. Gå för**Datorkonfiguration** > **principer** > **Administrationsmallar** > **Windows Komponenter** > **Enhetsregistrering**. Högerklicka på **registrera domänanslutna datorer som enheter**, och välj sedan **redigera**.
   
   > [!NOTE]
   > Den här mallen har ändrats från tidigare versioner av hello hanteringskonsolen för Grupprincip. Om du använder en tidigare version av hello-konsolen, gå för`Computer Configuration > Policies > Administrative Templates > Windows Components > Workplace Join > Automatically workplace join client computers`. 

7. Välj **aktiverad**, och välj sedan **tillämpa**.
8. Välj **OK**.
9. Länken hello gruppolicy objekt tooa önskad plats. T.ex, kan du länka den tooa organisationsenheten. Dessutom kan du länka det tooa säkerhetsgrupp för datorer som registreras automatiskt med Azure AD. tooset principen för alla domänanslutna Windows 10 och Windows Server 2016 datorer i din organisation, länk hello gruppolicy objekt toohello domän.

### <a name="windows-installer-packages-for-non-windows-10-computers"></a>Windows Installer-paket för Windows 10-datorer

tooregister domänanslutna Windows-klientversioner datorer i en federerad miljö kan du hämta och installera de här Windows Installer-paketet (.msi) från Download Center på hello [Microsoft till arbetsplatsen för Windows 10 - datorer](https://www.microsoft.com/en-us/download/details.aspx?id=53554) sidan.

Du kan distribuera hello paketet med hjälp av ett program distributionssystem som System Center Configuration Manager. hello paketet stöder hello standard tyst installation alternativ med hello *tyst* parameter. System Center Configuration Manager Current Branch erbjuder ytterligare fördelar jämfört med tidigare versioner som hello möjlighet tootrack slutförts registreringar. Mer information finns i [System Center Configuration Manager](https://www.microsoft.com/cloud-platform/system-center-configuration-manager).

hello installationsprogrammet skapar en schemalagd aktivitet på hello-system som körs i hello användarens kontext. hello aktiviteten utlöses när hello användare loggar in tooWindows. hello uppgiften registrerar tyst hello enhet med Azure AD med hello autentiseringsuppgifter efter autentiseras med integrerad Windows-autentisering. toosee hello schemalagd aktivitet, i hello enhet, gå för**Microsoft** > **Arbetsplatsanslutning**, och sedan gå toohello Schemaläggaren bibliotek.

## <a name="step-5-verify-registered-devices"></a>Steg 5: Kontrollera registrerade enheter

Du kan kontrollera lyckade registrerade enheter i organisationen med hjälp av hello [Get-MsolDevice](https://docs.microsoft.com/powershell/msonline/v1/get-msoldevice) cmdlet i hello [Azure Active Directory PowerShell-modulen](/powershell/azure/install-msonlinev1?view=azureadps-2.0).

hello utdata från den här cmdleten visar enheter som har registrerats i Azure AD. tooget alla enheter använda hello **-alla** parameter och filtrera dem med hjälp av hello **deviceTrustType** egenskapen. Domänanslutna enheter har värdet **domänanslutna**.

## <a name="next-steps"></a>Nästa steg

* [Automatisk enhetsregistrering vanliga frågor och svar](active-directory-device-registration-faq.md)
* [Felsökning av automatisk registrering av domän domänanslutna datorer tooAzure AD – Windows 10 och Windows Server 2016](active-directory-device-registration-troubleshoot-windows.md)
* [Felsökning av automatisk registrering av domän domänanslutna datorer tooAzure AD – Windows 10](active-directory-device-registration-troubleshoot-windows-legacy.md)
* [Villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-azure-portal.md)



<!--Image references-->
[1]: ./media/active-directory-conditional-access-automatic-device-registration-setup/12.png
