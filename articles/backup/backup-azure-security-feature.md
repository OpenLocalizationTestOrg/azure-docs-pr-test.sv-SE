---
title: "aaaSecurity funktioner toohelp skydda hybrid säkerhetskopieringar som använder Azure Backup | Microsoft Docs"
description: "Lär dig hur toouse säkerhetsfunktioner i Azure Backup toomake säkerhetskopieringar säkrare"
services: backup
documentationcenter: 
author: JPallavi
manager: vijayts
editor: 
ms.assetid: 47bc8423-0a08-4191-826d-3f52de0b4cb8
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: pajosh
ms.openlocfilehash: 17a0f5e877f84af53c15062ec4a8df480383125e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="security-features-toohelp-protect-hybrid-backups-that-use-azure-backup"></a>Säkerhet funktioner toohelp skydda hybrid säkerhetskopieringar som använder Azure Backup
Frågor om säkerhetsproblem som skadlig programvara, är en utpressningstrojan som och intrångsidentifiering, ökar. Dessa säkerhetsproblem kan vara kostsamma vad gäller både pengar och data. tooguard mot sådana angrepp Azure Backup nu ger säkerhet funktioner toohelp skydda hybrid säkerhetskopieringar. Den här artikeln beskriver hur tooenable och använda dessa funktioner, med hjälp av en Azure Recovery Services-agenten och Azure Backup Server. Funktionerna är:

- **Förebyggande**. Ett extra lager av autentisering läggs till när en kritisk åtgärd som ändrar en lösenfras utförs. Den här verifieringen är tooensure som dessa åtgärder kan utföras av användare som har giltiga autentiseringsuppgifter för Azure.
- **Aviseringar**. Ett e-postmeddelande skickas toohello prenumerationsadministratör när en kritisk åtgärd som tar bort säkerhetskopierade data utförs. E-postmeddelandet garanterar hello användaren ska meddelas snabbt om sådana åtgärder.
- **Återställning**. Borttagna säkerhetskopieringsdata behålls för ytterligare 14 dagar från hello hello borttagning. Detta säkerställer hello informationen kan återställas inom en viss tidsperiod, så det finns inga data går förlorade även om en attack inträffar. Ett större antal minsta återställningspunkterna bevaras också tooguard mot skadade data.

> [!NOTE]
> Säkerhetsfunktioner bör inte aktiveras om du använder infrastruktur som en tjänst (IaaS) VM-säkerhetskopia. De här funktionerna finns ännu inte för IaaS VM säkerhetskopiering, så att de inte har någon effekt. Säkerhetsfunktioner ska aktiveras bara om du använder: <br/>
>  * **Azure Backup-agenten**. Minsta agentversion 2.0.9052. När du har aktiverat dessa funktioner, bör du uppgradera toothis agent version tooperform kritiska åtgärder. <br/>
>  * **Azure Backup-Server**. Azure Backup agent minimiversionen 2.0.9052 med Azure Backup Server uppdatering 1. <br/>
>  * **System Center Data Protection Manager**. Azure Backup agent minimiversionen 2.0.9052 med Data Protection Manager 2012 R2 UR12 eller UR2 för Data Protection Manager 2016. <br/> 


> [!NOTE]
> Dessa funktioner är endast tillgängligt för Recovery Services-valvet. Alla hello nyskapad Recovery Services-valv har dessa funktioner aktiverade som standard. Användare aktiverar dessa funktioner med hjälp av anvisningarna i följande avsnitt hello hello för befintliga Recovery Services-valv. De gäller efter hello funktioner är aktiverade, tooall hello Recovery Services-agentdatorer, Azure Backup Server-instanser och Data Protection Manager servrar har registrerats med hello-valvet. Om du aktiverar den här inställningen är en engångsåtgärd, och du inaktivera inte dessa funktioner när du har aktiverat dem.
>

## <a name="enable-security-features"></a>Aktivera säkerhetsfunktioner för
Du kan använda alla hello säkerhetsfunktioner om du skapar en Recovery Services-valvet. Om du arbetar med en befintlig valvet aktivera säkerhetsfunktioner genom att följa dessa steg:

1. Logga in på toohello Azure-portalen med dina autentiseringsuppgifter för Azure.
2. Välj **Bläddra**, och skriv **återställningstjänster**.

    ![Skärmbild av Azure portal bläddringsalternativet](./media/backup-azure-security-feature/browse-to-rs-vaults.png) <br/>

    hello visas recovery services-valv. Den här listan, Välj ett valv. hello valda valvet instrumentpanelen öppnas.
3. Hello listan med objekt som visas under hello valv under **inställningar**, klickar du på **egenskaper**.

    ![Skärmbild av Recovery Services-valvet alternativ](./media/backup-azure-security-feature/vault-list-properties.png)
4. Under **säkerhetsinställningar**, klickar du på **uppdatering**.

    ![Skärmbild av Recovery Services-valvet egenskaper](./media/backup-azure-security-feature/security-settings-update.png)

    hello uppdateringslänken öppnar hello **säkerhetsinställningar** som innehåller en översikt över hello funktioner och du kan aktivera dem.
5. Hello nedrullningsbara listan **har du konfigurerat Azure Multi-Factor Authentication?**, Välj ett värde tooconfirm om du har aktiverat [Azure Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication.md). Om det är aktiverat uppmanas tooauthenticate från en annan enhet (till exempel en mobiltelefon) vid inloggning toohello Azure-portalen.

   När du utför kritiska åtgärder i säkerhetskopieringen, har du tooenter säkerhet PIN-kod på hello Azure-portalen. Aktivera Azure Multi-Factor Authentication ger en säkerhetsnivå. Endast behöriga användare med giltiga autentiseringsuppgifter för Azure och autentiserad från en andra enhet kan komma åt hello Azure-portalen.
6. toosave säkerhetsinställningar, Välj **aktivera** och på **spara**. Du kan välja **aktivera** bara när du har markerat ett värde från hello **har du konfigurerat Azure Multi-Factor Authentication?** listan i hello föregående steg.

    ![Skärmbild av säkerhetsinställningar](./media/backup-azure-security-feature/enable-security-settings-dpm-update.png)

## <a name="recover-deleted-backup-data"></a>Återställa borttagna säkerhetskopierade data
Säkerhetskopiering behåller borttagna säkerhetskopierade data för ytterligare 14 dagar och tas inte bort omedelbart om hello **stoppa säkerhetskopiering med ta bort säkerhetskopieringsdata** åtgärden utförs. toorestore dessa data i hello 14 dagar, vidta hello följande, beroende på vad du använder:

För **Azure Recovery Services-agenten** användare:

1. Om hello datorn där säkerhetskopieringar händer är fortfarande tillgängliga använder [återställa data toohello samma dator](backup-azure-restore-windows-server.md#use-instant-restore-to-recover-data-to-the-same-machine) i Azure Recovery Services toorecover från alla hello gamla återställningspunkter.
2. Om den här datorn inte är tillgänglig använder [Återställ tooan alternativa datorn](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine) toouse tooget med en annan Azure Recovery Services-datorn dessa data.

För **Azure Backup Server** användare:

1. Om hello server där säkerhetskopieringar händer fortfarande är tillgänglig, skydda hello bort datakällorna på nytt och använda hello **återställa Data** funktion toorecover från alla hello gamla återställningspunkter.
2. Om den här servern inte är tillgänglig, använda [återställa data från en annan Azure Backup Server](backup-azure-alternate-dpm-server.md) toouse en annan Azure Backup Server-instansen tooget dessa data.

För **Data Protection Manager** användare:

1. Om hello server där säkerhetskopieringar händer fortfarande är tillgänglig, skydda hello bort datakällorna på nytt och använda hello **återställa Data** funktion toorecover från alla hello gamla återställningspunkter.
2. Om den här servern inte är tillgänglig, använda [Lägg till extern DPM](backup-azure-alternate-dpm-server.md) toouse en annan Data Protection Manager server tooget dessa data.

## <a name="prevent-attacks"></a>Förhindra attacker
Kontroller har lagts till toomake till endast giltiga användare kan utföra olika åtgärder. Dessa inkluderar att lägga till ett extra lager av autentisering och upprätthålla en minsta kvarhållningsperiod för återställning.

### <a name="authentication-tooperform-critical-operations"></a>Autentisering tooperform kritiska åtgärder
Som en del för att lägga till ett extra lager av autentisering för kritiska åtgärder, som ange tooenter säkerhet PIN-kod när du utför **stoppa skydd med radera data** och **ändra lösenfras** åtgärder .

tooreceive PIN-koden:

1. Logga in toohello Azure-portalen.
2. Bläddra för**Recovery Services-valvet** > **inställningar** > **egenskaper**.
3. Under **säkerhet PIN**, klickar du på **generera**. Då öppnas ett blad som innehåller hello PIN-kod toobe som angetts i användargränssnittet för hello Azure Recovery Services-agenten.
    PIN-koden är giltig i bara fem minuter och hämtar den genereras automatiskt efter den perioden.

### <a name="maintain-a-minimum-retention-range"></a>Underhålla en minsta kvarhållningsperiod
tooensure att det alltid är ett ogiltigt nummer för återställning av tillgängliga poäng, hello efter kontroller har lagts till:

- Dagliga bevarande, minst **sju** dagarnas kvarhållning ska göras.
- För varje vecka kvarhållning, minst **fyra** veckor av kvarhållning ska göras.
- Månatliga bevarande, minst **tre** månaders kvarhållning ska göras.
- Årlig bevarande, minst **en** års kvarhållning ska göras.

## <a name="notifications-for-critical-operations"></a>Aviseringar för kritiska åtgärder
När en kritisk åtgärd utförs, vanligtvis skickas Hej administratör för prenumeration ett e-postmeddelande med information om hello åtgärden. Du kan konfigurera ytterligare e-postmottagare för dessa meddelanden med hjälp av hello Azure-portalen.

hello säkerhetsfunktioner som nämns i denna artikel har funktioner för skydd mot riktade attacker. Viktigare, om en attack inträffar, dessa funktioner får du hello möjlighet toorecover dina data.

## <a name="troubleshooting-errors"></a>Felsökning av fel
| Åtgärd | information om fel | Lösning |
| --- | --- | --- |
| Principändring |hello säkerhetskopieringsprincip kan inte ändras. Fel: aktuella hello-åtgärden misslyckades på grund av tooan internt tjänstfel [0x29834]. Gör hello åtgärden igen senare. Kontakta Microsoft-supporten om hello problemet kvarstår. |**Orsak:**<br/>Det här felet kommer vid säkerhetsinställningar har aktiverats, tooreduce Kvarhållningsintervall under hello minsta värdena som anges ovan och du är på version som inte stöds (versioner som stöds anges i första anteckningen i den här artikeln). <br/>**Rekommenderad åtgärd:**<br/> I det här fallet bör du ställa in kvarhållningsperioden ovan hello minsta kvarhållning tidsperioden (sju dagar för varje dag, fyra veckor för varje vecka, tre veckor för varje månad eller ett år för varje år) tooproceed med principen relaterade uppdateringar. Du kan också föredragen metod skulle vara tooupdate backup-agenten, säkerhetsuppdateringar om Azure Backup-Server och/eller DPM UR tooleverage alla hello. |
| Ändra lösenfras |Säkerhet angiven PIN-kod är felaktigt. (ID: 100130) Ange hello rätt säkerhet PIN toocomplete den här åtgärden. |**Orsak:**<br/> Det här felet kommer när du anger ogiltiga eller utgångna säkerhet PIN-kod under kritiska åtgärd (t.ex. Ändra lösenfras). <br/>**Rekommenderad åtgärd:**<br/> toocomplete hello åtgärden måste du ange en giltig säkerhet PIN-kod. tooget hello PIN-kod, logga in tooAzure portal och navigera tooRecovery Services-valv > Inställningar > Egenskaper > Generera säkerhet PIN-kod. Använd lösenfrasen toochange PIN-kod. |
| Ändra lösenfras |Åtgärden misslyckades. ID: 120002 |**Orsak:**<br/>Det här felet kommer vid säkerhetsinställningar har aktiverats, toochange lösenfras och du är på version som inte stöds (giltigt versioner som anges i första anteckningen i den här artikeln).<br/>**Rekommenderad åtgärd:**<br/> toochange lösenfrasen måste du först uppdatera toominimum säkerhetskopieringsagentversion minsta 2.0.9052, Azure Backup server toominimum uppdatering 1 och/eller DPM toominimum UR12 för DPM 2012 R2 eller DPM 2016 UR2 (download länkarna nedan), ange giltig säkerhet PIN-kod. tooget hello PIN-kod, logga in tooAzure portal och navigera tooRecovery Services-valv > Inställningar > Egenskaper > Generera säkerhet PIN-kod. Använd lösenfrasen toochange PIN-kod. |

## <a name="next-steps"></a>Nästa steg
* [Kom igång med Azure Recovery Services-valvet](backup-azure-vms-first-look-arm.md) tooenable dessa funktioner.
* [Hämta hello senaste Azure Recovery Services-agenten](http://aka.ms/azurebackup_agent) toohelp skydda Windows-datorer och förhindra dina säkerhetskopierade data mot attacker.
* [Hämta hello senaste Azure Backup-Server](https://aka.ms/latest_azurebackupserver) toohelp skydda arbetsbelastningar och förhindra dina säkerhetskopierade data mot attacker.
* [Hämta UR12 för System Center 2012 R2 Data Protection Manager](https://support.microsoft.com/help/3209592/update-rollup-12-for-system-center-2012-r2-data-protection-manager) eller [hämta UR2 för System Center 2016 Data Protection Manager](https://support.microsoft.com/help/3209593/update-rollup-2-for-system-center-2016-data-protection-manager) toohelp skydda arbetsbelastningar och förhindra dina säkerhetskopierade data mot attacker.
