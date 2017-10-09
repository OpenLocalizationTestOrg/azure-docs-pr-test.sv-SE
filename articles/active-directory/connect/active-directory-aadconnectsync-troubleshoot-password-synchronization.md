---
title: "aaaTroubleshoot Lösenordssynkronisering med Azure AD Connect-synkronisering | Microsoft Docs"
description: "Den här artikeln innehåller information om hur tootroubleshoot lösenord synkroniseringsproblem."
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: 390eafec792cb39251627c14cb754f8bb30035b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-password-synchronization-with-azure-ad-connect-sync"></a>Felsöka Lösenordssynkronisering med Azure AD Connect-synkronisering
Det här avsnittet innehåller instruktioner för hur tootroubleshoot problem med synkronisering av lösenord. Om lösenord inte synkroniseras som förväntat, kan det vara för en delmängd av användare eller för alla användare. För Azure Active Directory (AD Azure) ansluta distribution med version 1.1.524.0 eller senare, det finns nu en diagnostiska cmdlet som du kan använda tootroubleshoot lösenord synkroniseringsproblem:

* Om du har ett problem om inga lösenord synkroniseras, se toohello [utan lösenord synkroniseras: felsöka med cmdleten hello diagnostiska](#no-passwords-are-synchronized-troubleshoot-by-using-the-diagnostic-cmdlet) avsnitt.

* Om du har problem med enskilda objekt, läsa toohello [ett objekt synkroniserar inte lösenord: felsöka med cmdleten hello diagnostiska](#one-object-is-not-synchronizing-passwords-troubleshoot-by-using-the-diagnostic-cmdlet) avsnitt.

För äldre versioner av Azure AD Connect-distribution:

* Om du har ett problem om inga lösenord synkroniseras, se toohello [utan lösenord synkroniseras: manuell felsökningssteg](#no-passwords-are-synchronized-manual-troubleshooting-steps) avsnitt.

* Om du har problem med enskilda objekt, läsa toohello [ett objekt synkroniserar inte lösenord: manuell felsökningssteg](#one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps) avsnitt.

## <a name="no-passwords-are-synchronized-troubleshoot-by-using-hello-diagnostic-cmdlet"></a>Inga lösenord ska synkroniseras: felsöka med cmdleten hello diagnostik
Du kan använda hello `Invoke-ADSyncDiagnostics` cmdlet toofigure reda på varför utan lösenord synkroniseras.

> [!NOTE]
> Hej `Invoke-ADSyncDiagnostics` cmdlet är endast tillgänglig för Azure AD Connect-version 1.1.524.0 eller senare.

### <a name="run-hello-diagnostics-cmdlet"></a>Kör hello diagnostik cmdlet
tootroubleshoot problem där ingen lösenord ska synkroniseras:

1. Öppna en ny Windows PowerShell-session på Azure AD Connect-servern med hello **kör som administratör** alternativet.

2. Kör `Set-ExecutionPolicy RemoteSigned` eller `Set-ExecutionPolicy Unrestricted`.

3. Kör `Import-Module ADSyncDiagnostics`.

4. Kör `Invoke-ADSyncDiagnostics -PasswordSync`.

### <a name="understand-hello-results-of-hello-cmdlet"></a>Förstå hello resultatet för hello-cmdlet
diagnostiska hello-cmdlet utför hello efter kontroller.

* Verifierar att hello funktionen för synkronisering av lösenord har aktiverats för din Azure AD-klient.

* Verifierar att hello Azure AD Connect servern inte är i mellanlagringsläge.

* För varje befintlig lokal Active Directory-koppling (vilket motsvarar tooan befintliga Active Directory-skog):

   * Verifierar att hello funktionen för synkronisering av lösenord är aktiverat.
   
   * Söker efter lösenord synkronisering pulsslag händelser i hello Windows händelse loggar.

   * För varje Active Directory-domän under hello lokala Active Directory-koppling:

      * Verifierar att hello domän kan nås från hello Azure AD Connect-servern.

      * Verifierar att hello Active Directory Domain Services (AD DS)-konton som används av hello lokala Active Directory-koppling har hello rätt användarnamn, lösenord och behörigheter som krävs för synkronisering av lösenord.

hello följande diagram visar hello resultatet för hello-cmdlet för en domän, lokala Active Directory-topologi:

![Diagnostikdata för Lösenordssynkronisering](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalgeneral.png)

hello resten av det här avsnittet beskriver specifika resultat som returneras av hello-cmdlet och motsvarande problem.

#### <a name="password-synchronization-feature-isnt-enabled"></a>Funktionen för synkronisering av lösenord är inte aktiverat
Om du inte har aktiverat Lösenordssynkronisering med hjälp av hello Azure AD Connect-guiden, returnerades hello följande fel:

![Lösenordssynkronisering är inte aktiverat](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobaldisabled.png)

#### <a name="azure-ad-connect-server-is-in-staging-mode"></a>Azure AD Connect-servern är i mellanlagringsläge
Om hello Azure AD Connect-servern är i mellanlagringsläge, Lösenordssynkronisering är tillfälligt inaktiverad och hello följande felmeddelande:

![Azure AD Connect-servern är i mellanlagringsläge](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalstaging.png)

#### <a name="no-password-synchronization-heartbeat-events"></a>Inga lösenord synkronisering heartbeat-händelser
Varje lokala Active Directory-koppling har sin egen synkroniseringskanalen för lösenord. När hello lösenord synkronisering kanal upprättas och det inte finns några toobe för ändringar av lösenord synkroniseras, genereras heartbeat-händelser (händelse-ID 654) var 30: e minut under hello Windows händelselogg. För varje Active Directory connector lokalt hello cmdleten söker igenom för motsvarande heartbeat-händelser i hello senaste tre timmar. Om inga heartbeat-händelsen hittades, felmeddelande hello följande:

![Inga lösenord synkronisering hjärta slå händelse](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalnoheartbeat.png)

#### <a name="ad-ds-account-does-not-have-correct-permissions"></a>AD DS-kontot har inte rätt behörighet
Om hello AD DS-konto som används av hello lokal Active Directory connector toosynchronize lösenordshashvärden inte har åtkomstbehörighet hello returneras hello följande fel:

![Felaktig autentiseringsuppgifter](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectpermission.png)

#### <a name="incorrect-ad-ds-account-username-or-password"></a>Felaktigt användarnamn för AD DS-konto eller lösenord
Om hello AD DS-konto används av hello lokala Active Directory connector toosynchronize lösenordshashvärden har ett felaktigt användarnamn eller lösenord, hello följande felmeddelande:

![Felaktig autentiseringsuppgifter](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phsglobalaccountincorrectcredential.png)

## <a name="one-object-is-not-synchronizing-passwords-troubleshoot-by-using-hello-diagnostic-cmdlet"></a>Ett objekt synkroniserar inte lösenord: felsöka med cmdleten hello diagnostik
Du kan använda hello `Invoke-ADSyncDiagnostics` cmdlet toodetermine varför ett objekt inte synkroniserar lösenord.

> [!NOTE]
> Hej `Invoke-ADSyncDiagnostics` cmdlet är endast tillgänglig för Azure AD Connect-version 1.1.524.0 eller senare.

### <a name="run-hello-diagnostics-cmdlet"></a>Kör hello diagnostik cmdlet
tootroubleshoot problem där ingen lösenord ska synkroniseras:

1. Öppna en ny Windows PowerShell-session på Azure AD Connect-servern med hello **kör som administratör** alternativet.

2. Kör `Set-ExecutionPolicy RemoteSigned` eller `Set-ExecutionPolicy Unrestricted`.

3. Kör `Import-Module ADSyncDiagnostics`.

4. Kör följande cmdlet hello:
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName <Name-of-AD-Connector> -DistinguishedName <DistinguishedName-of-AD-object>
   ```
   Exempel:
   ```
   Invoke-ADSyncDiagnostics -PasswordSync -ADConnectorName "contoso.com" -DistinguishedName "CN=TestUserCN=Users,DC=contoso,DC=com"
   ```

### <a name="understand-hello-results-of-hello-cmdlet"></a>Förstå hello resultatet för hello-cmdlet
diagnostiska hello-cmdlet utför hello efter kontroller.

* Undersöker hello tillståndet för hello Active Directory-objekt i hello Active Directory connector diskutrymme, metaversum och Azure AD-anslutningsplatsen.

* Verifierar att det finns Synkroniseringsregler med Lösenordssynkronisering aktiverad och tillämpas toohello Active Directory-objekt.

* Försöker tooretrieve och visa hello resultaten av hello senaste försök toosynchronize hello lösenord för hello-objektet.

hello följande diagram visar hello resultatet för hello-cmdlet när du felsöker Lösenordssynkronisering för ett enskilt objekt:

![Diagnostikdata för Lösenordssynkronisering - objekt](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectgeneral.png)

hello resten av det här avsnittet beskriver specifika resultat som returneras av hello-cmdlet och motsvarande problem.

#### <a name="hello-active-directory-object-isnt-exported-tooazure-ad"></a>hello Active Directory-objekt är inte exporterade tooAzure AD
Lösenordssynkronisering för den här lokala Active Directory-konto misslyckas eftersom det finns inget motsvarande objekt i hello Azure AD-klient. hello följande fel returnerades:

![Azure AD-objekt som saknas](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnotexported.png)

#### <a name="user-has-a-temporary-password"></a>Användaren har ett tillfälligt lösenord
För närvarande stöder inte Azure AD Connect synkroniserar tillfälliga lösenord med Azure AD. Ett lösenord anses toobe tillfällig om hello **ändra lösenord vid nästa inloggning** alternativet är inställt på hello lokala Active Directory-användare. hello följande fel returnerades:

![Tillfälligt lösenord exporteras inte](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjecttemporarypassword.png)

#### <a name="results-of-last-attempt-toosynchronize-password-arent-available"></a>Resultaten av senaste försök toosynchronize lösenord är inte tillgängliga
Som standard lagrar Azure AD Connect hello resultaten av lösenord synkroniseringsförsök under sju dagar. Om det finns inga resultat för hello valda Active Directory-objekt, returneras hello följande varning:

![Diagnostikdata för objekt - ingen historik för synkronisering av lösenord](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/phssingleobjectnohistory.png)


## <a name="no-passwords-are-synchronized-manual-troubleshooting-steps"></a>Inga lösenord ska synkroniseras: manuell felsökningssteg
Följ dessa steg toodetermine varför utan lösenord synkroniseras:

1. Är hello Connect-servern i [mellanlagringsläge](active-directory-aadconnectsync-operations.md#staging-mode)? En server i mellanlagringsläge synkroniseras inte eventuella lösenord.

2. Kör skript hello hello [hämta hello status för synkronisering lösenordsinställningar](#get-the-status-of-password-sync-settings) avsnitt. Den ger dig en översikt över hello synkroniseringskonfiguration för lösenord.  

    ![PowerShell-skriptets utdata från inställningar för synkronisering av lösenord](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/psverifyconfig.png)  

3. Om hello-funktionen inte har aktiverats i Azure AD eller om hello synkroniseringsstatus för kanal inte har aktiverats, kör du installationsguiden för hello Connect. Välj **anpassa synkroniseringsalternativ**, och avmarkera Lösenordssynkronisering. Den här ändringen inaktiverar tillfälligt hello-funktionen. Sedan kör hello guiden igen och aktivera synkronisering av lösenord på nytt. Kör skript hello igen tooverify som hello konfigurationen är korrekt.

4. Titta i hello efter fel i händelseloggen. Leta efter hello följande händelser, vilket visar att det finns ett problem:
    * Meddelandekälla: ID för ”katalogsynkronisering”: 0, 611, 652, 655 om du ser dessa händelser du har anslutningsproblem med. hello händelseloggmeddelande innehåller information om skog där du har problem. Mer information finns i [anslutningsproblem](#connectivity problem).

5. Om du ser inget pulsslag från eller om inget annat arbetat köra [utlösa en fullständig synkronisering av alla lösenord](#trigger-a-full-sync-of-all-passwords). Kör skriptet för hello bara en gång.

6. Se hello [felsökning av ett objekt som inte synkroniserar lösenord](#one-object-is-not-synchronizing-passwords) avsnitt.

### <a name="connectivity-problems"></a>Problem med nätverksanslutningen

Har du anslutningen till Azure AD?

Hello-konto har nödvändiga behörigheter tooread hello lösenord hash-värden i alla domäner? Om du har installerat Connect med standardinställningar bör hello redan vara korrekt. 

Om du har använt anpassad installation kan du ange hello behörigheter manuellt genom att göra hello följande:
    
1. toofind hello konto som används av hello Active Directory-koppling start **Synchronization Service Manager**. 
 
2. Gå för**kopplingar**, och sök sedan efter hello lokala Active Directory-skog som du felsöker. 
 
3. Välj hello-koppling och klicka sedan på **egenskaper**. 
 
4. Gå för**ansluta tooActive Directory-skog**.  
    
    ![Konto som används av Active Directory-koppling](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/connectoraccount.png)  
    Observera hello användarnamn och hello domän där hello konto finns.
    
5. Starta **Active Directory-användare och datorer**, och sedan kontrollera att hello-kontot som du hittade tidigare har hello Följ behörigheter hello roten i alla domäner i skogen:
    * Replikera katalogändringar
    * Replikera katalogen ändras alla

6. Är hello domänkontrollanter som kan nås av Azure AD Connect? Om hello Connect-servern inte kan ansluta tooall domänkontrollanter, konfigurera **bara använda prioriterade domänkontrollant**.  
    
    ![Domänkontrollanten som används av Active Directory-koppling](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/preferreddc.png)  
    
7. Gå tillbaka för**Synchronization Service Manager** och **konfigurera katalogpartitionen**. 
 
8. Välj din domän i **Välj katalogpartitioner**väljer hello **endast använder prioriterade domänkontrollanter** kryssrutan och klicka sedan på **konfigurera**. 

9. Ange hello domänkontrollanter som Connect ska använda för synkronisering av lösenord i hello-listan. hello samma lista används för att importera och exportera samt. Utföra de här stegen för alla domäner.

10. Om hello skript visar att det finns inget pulsslag, köra hello skript i [utlösa en fullständig synkronisering av alla lösenord](#trigger-a-full-sync-of-all-passwords).

## <a name="one-object-is-not-synchronizing-passwords-manual-troubleshooting-steps"></a>Ett objekt synkroniserar inte lösenord: manuell felsökningssteg
Du kan enkelt felsöka problem för synkronisering av lösenord genom att granska hello statusen på ett objekt.

1. I **Active Directory-användare och datorer**, söka efter hello användare och kontrollera sedan att hello **användaren måste byta lösenord vid nästa inloggning** kryssrutan är avmarkerad.  

    ![Active Directory-produktiva lösenord](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/adprodpassword.png)  

    Om hello är markerad, be hello användaren toosign i och ändra hello lösenord. Temporära lösenord är inte synkroniserade med Azure AD.

2. Om hello lösenord verkar vara korrekta i Active Directory, följer du hello användare i hello Synkroniseringsmotorn. Du kan se om det finns ett felmeddelande på hello objekt av följande hello användare från lokala Active Directory tooAzure AD.

    a. Starta hello [Synchronization Service Manager](active-directory-aadconnectsync-service-manager-ui.md).

    b. Klicka på **kopplingar**.

    c. Välj hello **Active Directory-kopplingen** där hello användaren finns.

    d. Välj **söka Anslutarplats**.

    e. I hello **omfång** väljer **DN eller fästpunkt**, och ange sedan hello fullständig DN hello-användare som du felsöker.

    ![Sök efter användare i anslutningsplatsen med DN](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/searchcs.png)  

    f. Leta upp hello användare du söker efter och klicka sedan på **egenskaper** toosee alla hello attribut. Om hello användaren inte är i hello sökresultat, kontrollera din [filtreringsregler](active-directory-aadconnectsync-configure-filtering.md) och se till att du kör [tillämpa och verifiera ändringar](active-directory-aadconnectsync-configure-filtering.md#apply-and-verify-changes) för hello användaren tooappear i Connect.

    g. toosee hello lösenord synkronisera information om hello-objekt för hello gångna veckan klickar du på **loggen**.  

    ![Information om objekt logg](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/csobjectlog.png)  

    Om hello grupprincipobjektet loggar är tom, har Azure AD Connect tooread hello lösenords-hash från Active Directory. Fortsätta felsökningen med [anslutningsfel](#connectivity-errors). Om du ser något annat värde än **lyckade**, se toohello tabellen i [loggen för synkronisering av lösenord](#password-sync-log).

    h. Välj hello **härkomst** fliken och se till att minst en synkroniseringsregel hello **PasswordSync** kolumnen är **SANT**. I standardkonfigurationen hello hello hello synkroniseringsregel heter **i från AD - användare AccountEnabled**.  

    ![Övriga information om en användare](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync.png)  

    Jag. Klicka på **metaversum objektegenskaper** toodisplay en lista över användarattribut.  

    ![Metaversum-information](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvpasswordsync.png)  

    Kontrollera att det finns inga **cloudFiltered** attributet. Se till att hello domänattribut (domainFQDN och domainNetBios) har hello förväntade värden.

    j. Klicka på hello **kopplingar** fliken. Kontrollera att du ser kopplingar tooboth lokala Active Directory och Azure AD.

    ![Metaversum-information](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/mvconnectors.png)  

    k. Välj hello raden som representerar Azure AD, klickar du på **egenskaper**, och klicka sedan på hello **härkomst** fliken hello utrymme kopplingsobjektet måste ha en utgående regel hello **PasswordSync** kolumnuppsättningen för**SANT**. I standardkonfigurationen hello hello hello synkroniseringsregel heter **ut tooAAD - användare ansluta**.  

    ![Dialogrutan för koppling objektegenskaper för Anslutarplats](./media/active-directory-aadconnectsync-troubleshoot-password-synchronization/cspasswordsync2.png)  

### <a name="password-sync-log"></a>Logg för synkronisering av lösenord
hello i statuskolumnen kan ha hello följande värden:

| Status | Beskrivning |
| --- | --- |
| Lyckades |Lösenordet har synkroniserats. |
| FilteredByTarget |Ange lösenordet för**användaren måste byta lösenord vid nästa inloggning**. Lösenordet har inte synkroniserats. |
| NoTargetConnection |Inga objekt i hello metaversum eller hello Azure AD-anslutningsplatsen. |
| SourceConnectorNotPresent |Inga objekt hittades i hello lokala Active Directory-anslutningsplatsen. |
| TargetNotExportedToDirectory |hello-objekt på hello Azure AD-anslutningsplatsen har inte ännu exporterats. |
| MigratedCheckDetailsForMoreInfo |Loggposten skapades innan version 1.0.9125.0 och visas i det tidigare tillståndet. |
| Fel |Tjänsten returnerade ett okänt fel. |
| Okänd |Ett fel uppstod vid försök tooprocess en batch med lösenordshashvärden.  |
| MissingAttribute |Specifika attribut (till exempel Kerberos-hash) krävs av Azure AD Domain Services är inte tillgängliga. |
| RetryRequestedByTarget |Specifika attribut (till exempel Kerberos-hash) krävs av Azure AD Domain Services var inte tillgängliga tidigare. Det görs ett försök tooresynchronize hello användarens lösenords-hash. |

## <a name="scripts-toohelp-troubleshooting"></a>Felsökning av skript toohelp

### <a name="get-hello-status-of-password-sync-settings"></a>Hämta hello statusen för inställningar för synkronisering av lösenord
```
Import-Module ADSync
$connectors = Get-ADSyncConnector
$aadConnectors = $connectors | Where-Object {$_.SubType -eq "Windows Azure Active Directory (Microsoft)"}
$adConnectors = $connectors | Where-Object {$_.ConnectorTypeName -eq "AD"}
if ($aadConnectors -ne $null -and $adConnectors -ne $null)
{
    if ($aadConnectors.Count -eq 1)
    {
        $features = Get-ADSyncAADCompanyFeature -ConnectorName $aadConnectors[0].Name
        Write-Host
        Write-Host "Password sync feature enabled in your Azure AD directory: "  $features.PasswordHashSync
        foreach ($adConnector in $adConnectors)
        {
            Write-Host
            Write-Host "Password sync channel status BEGIN ------------------------------------------------------- "
            Write-Host
            Get-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector.Name
            Write-Host
            $pingEvents =
                Get-EventLog -LogName "Application" -Source "Directory Synchronization" -InstanceId 654  -After (Get-Date).AddHours(-3) |
                    Where-Object { $_.Message.ToUpperInvariant().Contains($adConnector.Identifier.ToString("D").ToUpperInvariant()) } |
                    Sort-Object { $_.Time } -Descending
            if ($pingEvents -ne $null)
            {
                Write-Host "Latest heart beat event (within last 3 hours). Time " $pingEvents[0].TimeWritten
            }
            else
            {
                Write-Warning "No ping event found within last 3 hours."
            }
            Write-Host
            Write-Host "Password sync channel status END ------------------------------------------------------- "
            Write-Host
        }
    }
    else
    {
        Write-Warning "More than one Azure AD Connectors found. Please update hello script toouse hello appropriate Connector."
    }
}
Write-Host
if ($aadConnectors -eq $null)
{
    Write-Warning "No Azure AD Connector was found."
}
if ($adConnectors -eq $null)
{
    Write-Warning "No AD DS Connector was found."
}
Write-Host
```

#### <a name="trigger-a-full-sync-of-all-passwords"></a>Starta en fullständig synkronisering av alla lösenord
> [!NOTE]
> Kör skriptet en gång. Om du behöver toorun den mer än en gång, är något annat hello problem. tootroubleshoot hello problem, kontakta Microsoft support.

Du kan utlösa en fullständig synkronisering av alla lösenord med hjälp av hello följande skript:

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"
$aadConnector = "<CASE SENSITIVE AAD CONNECTOR NAME>"
Import-Module adsync
$c = Get-ADSyncConnector -Name $adConnector
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1
$c.GlobalParameters.Remove($p.Name)
$c.GlobalParameters.Add($p)
$c = Add-ADSyncConnector -Connector $c
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $false
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $aadConnector -Enable $true
```

## <a name="next-steps"></a>Nästa steg
* [Implementera Lösenordssynkronisering med Azure AD Connect-synkronisering](active-directory-aadconnectsync-implement-password-synchronization.md)
* [Azure AD Connect Sync: Anpassa synkroniseringsalternativ](active-directory-aadconnectsync-whatis.md)
* [Integrera dina lokala identiteter med Azure Active Directory](active-directory-aadconnect.md)
