---
title: "Azure Active Directory Domain Services: Felsökningsguide | Microsoft Docs"
description: "Felsökningsguide för Azure AD Domain Services"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 4bc8c604-f57c-4f28-9dac-8b9164a0cf0b
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 86eb3513b7bc921c59287600b1b76eeda20c1356
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure AD Domain Services - guide för felsökning
Den här artikeln innehåller tips för felsökning för problem som kan uppstå när du konfigurerar eller administrera Azure Active Directory (AD) Domain Services.

## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>Du kan inte aktivera Azure AD Domain Services för din Azure AD-katalog
Det här avsnittet hjälper du felsöka när du försöker tooenable Azure AD Domain Services för din katalog och det misslyckas eller hämtar växlas tillbaka too'Disabled'.

Välj hello felsökningssteg som motsvarar toohello felmeddelande som du får.

| **Felmeddelande** | **Lösning** |
| --- |:--- |
| *hello namnet contoso100.com används redan i det här nätverket. Ange ett namn som inte används.* |[Domän namnkonflikt i hello virtuellt nätverk](active-directory-ds-troubleshooting.md#domain-name-conflict) |
| *Det gick inte att aktivera Domain Services i Azure AD-klient. hello-tjänsten har inte tillräcklig behörighet för toohello program kallat Azure AD Domain Services-synkronisering. Ta bort hello program kallat Azure AD Domain Services-synkronisering och försök sedan tooenable Domain Services för din Azure AD-klient.* |[Domain Services har inte tillräcklig behörighet toohello Azure AD Domain Services synkronisera program](active-directory-ds-troubleshooting.md#inadequate-permissions) |
| *Det gick inte att aktivera Domain Services i Azure AD-klient. hello Domain Services inte har hello program i Azure AD-klient krävs behörigheter tooenable Domain Services. Ta bort programmet hello med hello programmet identifierare d87dcbc6-a371-462e-88e3-28ad15ec4e64 och försök sedan tooenable Domain Services för din Azure AD-klient.* |[hello Domain Services-programmet har inte konfigurerats korrekt i din klientorganisation](active-directory-ds-troubleshooting.md#invalid-configuration) |
| *Det gick inte att aktivera Domain Services i Azure AD-klient. hello Microsoft Azure AD-program är inaktiverad i Azure AD-klienten. Aktivera hello program med hello programmet identifierare 00000002-0000-0000-c000-000000000000 och försök sedan tooenable Domain Services för din Azure AD-klient.* |[hello Microsoft Graph-program har inaktiverats i Azure AD-klient](active-directory-ds-troubleshooting.md#microsoft-graph-disabled) |

### <a name="domain-name-conflict"></a>Namnkonflikt i domänen
**Ett felmeddelande visas:**

*hello namnet contoso100.com används redan i det här nätverket. Ange ett namn som inte används.*

**Reparation:**

Kontrollera att du inte har en befintlig domän med hello samma domännamn i det virtuella nätverket. Anta exempelvis att du har en domän som heter ”contoso.com” redan finns på hello valda virtuella nätverket. Senare kan du försöka tooenable en Azure AD Domain Services-hanterad domän med hello samma domännamn (dvs, contoso.com) i det virtuella nätverket. Det uppstår ett fel vid försök tooenable Azure AD Domain Services.

Det här felet är på grund av tooname konflikter för hello domännamn i det virtuella nätverket. I den här situationen måste du använda ett annat namn tooset in din Azure AD Domain Services-hanterad domän. Du kan också avetablera hello befintlig domän och gå sedan vidare tooenable Azure AD Domain Services.

### <a name="inadequate-permissions"></a>Otillräckliga behörigheter
**Ett felmeddelande visas:**

*Det gick inte att aktivera Domain Services i Azure AD-klient. hello-tjänsten har inte tillräcklig behörighet för toohello program kallat Azure AD Domain Services-synkronisering. Ta bort hello program kallat Azure AD Domain Services-synkronisering och försök sedan tooenable Domain Services för din Azure AD-klient.*

**Reparation:**

Kontrollera toosee om det finns ett program med namnet hello Azure AD Domain Services-synkronisering i Azure AD-katalogen. Om programmet finns, ta bort den och sedan återaktivera Azure AD Domain Services.

Utföra hello följande steg toocheck hello förekomst av programmet hello och toodelete, om hello program finns:

1. Navigera toohello **klassiska Azure-portalen** ([https://manage.windowsazure.com](https://manage.windowsazure.com)).
2. Välj hello **Active Directory** i hello vänstra fönstret.
3. Välj hello Azure AD-klienten (katalogen) som du vill att tooenable Azure AD Domain Services.
4. Navigera toohello **program** fliken.
5. Välj hello **program som företaget äger** alternativ i listrutan hello.
6. Kontrollera om ett program som kallas **Azure AD Domain Services Sync**. Om programmet hello finns fortsätter toodelete den.
7. När du har tagit bort programmet hello försök tooenable Azure AD Domain Services igen.

### <a name="invalid-configuration"></a>Ogiltig konfiguration
**Ett felmeddelande visas:**

*Det gick inte att aktivera Domain Services i Azure AD-klient. hello Domain Services inte har hello program i Azure AD-klient krävs behörigheter tooenable Domain Services. Ta bort programmet hello med hello programmet identifierare d87dcbc6-a371-462e-88e3-28ad15ec4e64 och försök sedan tooenable Domain Services för din Azure AD-klient.*

**Reparation:**

Kontrollera toosee om du har ett program med namnet hello 'AzureActiveDirectoryDomainControllerServices' (med en identifierare för d87dcbc6-a371-462e-88e3-28ad15ec4e64) i Azure AD-katalogen. Om programmet finns behöver du toodelete den och sedan återaktivera Azure AD Domain Services.

Använd följande PowerShell-skriptet toofind hello programmet hello och ta bort den.

> [!NOTE]
> Det här skriptet använder **Azure AD PowerShell version 2** cmdlets. En fullständig lista över alla tillgängliga cmdlets och toodownload hello modulen läsa hello [AzureAD PowerShell referensdokumentationen](https://msdn.microsoft.com/library/azure/mt757189.aspx).
>
>

```
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted hello Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted hello Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted hello Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>Microsoft Graph inaktiverad
**Ett felmeddelande visas:**

Det gick inte att aktivera Domain Services i Azure AD-klient. hello Microsoft Azure AD-program är inaktiverad i Azure AD-klienten. Aktivera hello program med hello programmet identifierare 00000002-0000-0000-c000-000000000000 och försök sedan tooenable Domain Services för din Azure AD-klient.

**Reparation:**

Kontrollera toosee om du har inaktiverat ett program med hello identifierare 00000002-0000-0000-c000-000000000000. Det här programmet är hello Microsoft Azure AD-program och ger Graph API åtkomst tooyour Azure AD-klient. Azure AD Domain Services måste det här programmet toobe aktiverat toosynchronize din Azure AD-klient tooyour hanterade domän.

tooresolve det här felet kan aktivera programmet och försök sedan tooenable Domain Services för din Azure AD-klient.

## <a name="users-are-unable-toosign-in-toohello-azure-ad-domain-services-managed-domain"></a>Användare kan toosign i toohello Azure AD Domain Services-hanterad domän
Om en eller flera användare i Azure AD-klienten är toosign i toohello nyskapad hanterade domän, utföra hello följande felsökningssteg:

* **Logga in med UPN-format:** försök toosign med hello UPN-format (till exempel ”joeuser@contoso.com”) i stället för hello SAMAccountName format (CONTOSO\joeuser). hello SAMAccountName kan genereras automatiskt för användare vars UPN-prefixet är alltför långa eller hello är samma som en annan användare på hello-hanterad domän. hello UPN-format är garanterat toobe unika inom en Azure AD-klient.

> [!NOTE]
> Vi rekommenderar att du använder hello UPN-format toosign i toohello Azure AD Domain Services-hanterad domän.
>
>

* Kontrollera att du har [aktiverat Lösenordssynkronisering](active-directory-ds-getting-started-password-sync.md) i enlighet med hello steg som beskrivs i hello Kom igång-guiden.
* **Externa konton:** Kontrollera att användarkontot hello påverkas inte är ett externt konto i hello Azure AD-klient. Exempel på externa konton är Microsoft-konton (till exempel 'joe@live.com') eller användarkonton från en extern Azure AD-katalog. Eftersom Azure AD Domain Services inte har autentiseringsuppgifter för dessa konton kan logga användarna inte in toohello hanterad domän.
* **Synkroniserats konton:** om hello påverkas användarkonton synkroniseras från en lokal katalog, kontrollerar du att:

  * Du har distribuerat eller uppdatera toohello [senaste rekommenderade versionen av Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
  * Du har konfigurerat Azure AD Connect för[utför en fullständig synkronisering](active-directory-ds-getting-started-password-sync.md).
  * Beroende på hello storlek i din katalog, kan det ta en stund för användarkonton och autentiseringsuppgifter hashvärden toobe tillgängliga i Azure AD Domain Services. Se till att du vänta tillräckligt länge innan du försöker autentisering (beroende på hello storleken på din katalog- eller ett par för stora kataloger några timmar tooa dagar).
  * Hello problemet kvarstår efter att ha kontrollerat hello föregående steg kan du prova att starta om hello Microsoft Azure AD Sync-tjänsten. Starta en kommandotolk och kör hello följande kommandon från datorn synkronisering:

    1. net stop ”Microsoft Azure AD Sync'
    2. net start ”Microsoft Azure AD Sync'
* **Endast molnbaserad konton**: om hello påverkas användarkonto är en molnbaserad användarkonto, kontrollera hello användaren har ändrat sitt lösenord när du har aktiverat Azure AD Domain Services. Det här steget gör hello autentiseringsuppgifter autentiseringshasherna som krävs för Azure AD Domain Services toobe genereras.

## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Användare har tagits bort från Azure AD-klienten tas inte bort från din hanterade domän
Azure AD skyddar dig mot oavsiktlig borttagning av användarobjekt. När du tar bort ett användarkonto från Azure AD-klienten är hello motsvarande användarobjekt flyttade toohello Papperskorgen. När den här åtgärden är synkroniserade tooyour hanterad domän, gör hello motsvarande användarens konto toobe markerad har inaktiverats. Den här funktionen hjälper dig att återställa eller ångra borttagning hello-konto senare.

Hej användarkontot finns kvar i hello inaktiverat tillstånd i din hanterade domän, även om du vill återskapa ett användarkonto med hello samma UPN i Azure AD-katalogen. tooremove hello användarkontot från din hanterade domän, behöver du tooforce ta bort den från din Azure AD-klient.

tooremove hello användarkontot helt från din hanterade domän, ta bort hello användaren permanent från Azure AD-klienten. Använd hello Remove-MsolUser PowerShell-cmdlet med hello '-RemoveFromRecycleBin' alternativ, enligt beskrivningen i det här [MSDN-artikel](https://msdn.microsoft.com/library/azure/dn194132.aspx).

## <a name="contact-us"></a>Kontakta oss
Kontakta hello Azure Active Directory Domain Services Produktteamet för[dela feedback eller support](active-directory-ds-contact-us.md).
