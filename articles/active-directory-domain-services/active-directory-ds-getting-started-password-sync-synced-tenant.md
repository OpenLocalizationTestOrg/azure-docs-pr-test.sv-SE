---
title: "Azure AD Domain Services: Aktivera lösenordssynkronisering | Microsoft Docs"
description: "Komma igång med Azure Active Directory Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 8731f2b2-661c-4f3d-adba-2c9e06344537
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/30/2017
ms.author: maheshu
ms.openlocfilehash: a7a6ee0f83d3d9bdaf236717efb39155a26934e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-password-synchronization-tooazure-active-directory-domain-services"></a>Aktivera lösenord synkronisering tooAzure Active Directory Domain Services
I föregående uppgifter aktiverade du Azure Active Directory Domain Services för din Azure Active Directory-klient (Azure AD). hello nästa uppgift är tooenable synkroniseringen av hashvärdena som krävs för NT LAN Manager (NTLM) och Kerberos-autentisering tooAzure AD DS. När du har konfigurerat autentiseringsuppgifter synkronisering kan användarna logga in toohello hanterade domänen med sina företagsuppgifter.

hello stegen skiljer sig för endast molnbaserad användaren konton jämfört med användarkonton som synkroniseras från din lokala katalog med Azure AD Connect. Om Azure AD-klienten har en kombination av molnet endast användare och användare från din lokala AD och du behöver tooperform båda dessa steg.

<br>

> [!div class="op_single_selector"]
> * **Endast molnbaserad användarkonton**: [synkronisera lösenord för endast molnbaserad användarkonton tooyour hanterad domän](active-directory-ds-getting-started-password-sync.md)
> * **Lokala användarkonton**: [synkronisera lösenord för användarkonton synkroniseras från din lokala AD tooyour hanterad domän](active-directory-ds-getting-started-password-sync-synced-tenant.md)
>
>

<br>

## <a name="task-5-enable-password-synchronization-tooyour-managed-domain-for-user-accounts-synced-with-your-on-premises-ad"></a>Uppgift 5: aktivera lösenord synkronisering tooyour hanterad domän för användarkonton som har synkroniserats med din lokala AD
En synkroniserad Azure AD-klient anges toosynchronize med din organisations lokala katalog med Azure AD Connect. Som standard synkroniseras inte NTLM och Kerberos-autentiseringsuppgifter hashvärden tooAzure AD Azure AD Connect. toouse Azure AD Domain Services måste tooconfigure Azure AD Connect toosynchronize autentiseringshasherna som krävs för NTLM och Kerberos-autentisering. hello följande aktivera synkroniseringen av hashvärdena för hello krävs från din lokala katalog tooyour Azure AD-klient.

> [!NOTE]
> Om din organisation har användarkonton som synkroniseras från din lokala katalog, du måste aktivera synkroniseringen av NTLM och Kerberos-hashvärden i ordning toouse hello hanterade domän. Synkroniserade användarkonto är ett konto som har skapats i din lokala katalog och är synkroniserade med Azure AD Connect tooyour Azure AD-klient.
>
>

### <a name="install-or-update-azure-ad-connect"></a>Installera eller uppdatera Azure AD Connect
Installera hello senaste rekommenderade versionen av Azure AD Connect på en domän anslutna datorn. Om du har en befintlig instans av Azure AD Connect-installationen måste tooupdate den toouse hello senaste versionen av Azure AD Connect. tooavoid kända problem/buggar som kanske redan har åtgärdats, se till att du alltid använda hello senaste versionen av Azure AD Connect.

**[Ladda ned Azure AD Connect](http://www.microsoft.com/download/details.aspx?id=47594)**

Rekommenderad version: **1.1.553.0** – publicerades 27 juni 2017.

> [!WARNING]
> Du måste installera hello senaste rekommenderade versionen av Azure AD Connect tooenable hello äldre lösenord autentiseringsuppgifter (krävs för NTLM och Kerberos-autentisering) toosynchronize tooyour Azure AD-klient. Den här funktionen är inte tillgänglig i tidigare versioner av Azure AD Connect eller med hello äldre DirSync-verktyget.
>
>

Installationsinstruktioner för Azure AD Connect är tillgängliga i hello följande artikel – [komma igång med Azure AD Connect](../active-directory/active-directory-aadconnect.md)

### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-tooazure-ad"></a>Aktivera synkroniseringen av NTLM och Kerberos-autentiseringsuppgifter hashvärden tooAzure AD
Köra hello följande PowerShell-skript på varje AD-skog, tooforce fullständig Lösenordssynkronisering och aktivera alla lokala användares autentiseringsuppgifter hashvärden toosync tooyour Azure AD-klient. Det här skriptet kan hello autentiseringshasherna som krävs för NTLM/Kerberos-autentisering toobe synkroniserade tooyour Azure AD-klient.

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

Beroende på hello storleken på din katalog (antal användare, grupper osv), hashar tooAzure AD tar tid för synkronisering av autentiseringsuppgifter. hello lösenorden kan användas på hello Azure AD Domain Services hanterade domänen strax efter att hashvärdena hello har synkroniserat tooAzure AD.

<br>

## <a name="related-content"></a>Relaterat innehåll
* [Aktivera lösenord synkronisering tooAAD Domain Services för en molnbaserad Azure AD-katalog](active-directory-ds-getting-started-password-sync.md)
* [Administrera en Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-administer-domain.md)
* [Ansluta till en Windows virtuella tooan Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-join-windows-vm.md)
* [Ansluta till en Red Hat Enterprise Linux virtuella tooan Azure AD Domain Services-hanterad domän](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
