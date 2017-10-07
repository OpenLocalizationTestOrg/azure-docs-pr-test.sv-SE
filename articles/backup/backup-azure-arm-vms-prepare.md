---
title: "Azure-säkerhetskopiering: Förbereda tooback av virtuella datorer | Microsoft Docs"
description: "Kontrollera att din miljö har förberetts för att säkerhetskopiera virtuella datorer i Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "säkerhetskopiering. Säkerhetskopiera;"
ms.assetid: e87e8db2-b4d9-40e1-a481-1aa560c03395
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/10/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: 5c3a41b5d3bd56e62ca5f207442867913aa99816
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-environment-tooback-up-resource-manager-deployed-virtual-machines"></a>Förbereda din miljö tooback av Resource Manager distribuerade virtuella datorer
> [!div class="op_single_selector"]
> * [Resource Manager-modellen](backup-azure-arm-vms-prepare.md)
> * [Klassiska modellen](backup-azure-vms-prepare.md)
>
>

Den här artikeln innehåller hello steg för att förbereda din miljö tooback av en Resource Manager-distribuerad virtuell dator (VM). hello använder stegen som visas i hello procedurer hello Azure-portalen.  

hello Azure Backup-tjänsten har två typer av valv (säkerhetskopiera valv och recovery services-valv) för att skydda dina virtuella datorer. Ett säkerhetskopieringsvalv skyddar virtuella datorer som distribueras med hjälp av hello klassiska distributionsmodellen. Recovery services-ventilen skyddar **både distribuerade klassiska eller Resource Manager distribuerade virtuella datorer**. Du måste använda en Recovery Services-valvet tooprotect en Resource Manager-distribuerad virtuell dator.

> [!NOTE]
> Azure har två distributionsmodeller som används för att skapa och arbeta med resurser: [Resource Manager och den klassiska distributionsmodellen](../azure-resource-manager/resource-manager-deployment-model.md). Se [förbereda din miljö tooback av Azure virtuella datorer](backup-azure-vms-prepare.md) mer information om hur du arbetar med klassisk distribution modellen virtuella datorer.
>
>

Innan du kan skydda eller säkerhetskopiera en Resource Manager-distribuerad virtuell dator (VM), se till att dessa krav finns:

* Skapa ett recovery services-valv (eller identifiera befintliga recovery services-ventilen) *i hello samma plats som den virtuella datorn*.
* Välj ett scenario, definiera hello säkerhetskopieringsprincip och definiera objekt tooprotect.
* Kontrollera hello installationen av VM-agenten på den virtuella datorn.
* Kontrollera nätverksanslutningen
* För Linux virtuella datorer, om du vill ha toocustomize säkerhetskopiering miljön för programmet konsekvent säkerhetskopiering Följ hello [steg tooconfigure ögonblicksbild före och efter ögonblicksbild skript](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent)

Om du känner till dessa villkor redan finns i din miljö och sedan fortsätta toohello [säkerhetskopiera virtuella datorer artikeln](backup-azure-vms.md). Om du behöver tooset upp eller kontrollera någon av dessa förutsättningar leder i den här artikeln dig genom hello steg tooprepare som förutsättning.

##<a name="supported-operating-system-for-backup"></a>Operativsystem som stöds för säkerhetskopiering
 * **Linux**: Azure Backup stöder [en lista över distributioner som godkänts av Azure](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) med undantag för Core OS Linux. _Andra Bring-Your-äger-Linux-distributioner kan också fungera så länge hello VM agent är tillgänglig på den virtuella datorn hello och stöd för Python finns. Vi inte stödja dessa distributioner för säkerhetskopiering._
 * **Windows Server**: versioner som är äldre än Windows Server 2008 R2 stöds inte.

## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Begränsningar när du säkerhetskopierar och återställer en virtuell dator
Innan du förbereder din miljö, vill vi påpeka hello begränsningar.

* Säkerhetskopiera virtuella datorer med fler än 16 datadiskar stöds inte.
* Säkerhetskopiering av virtuella datorer med data storlekar för diskar som är större än 1 023 GB stöds inte.
* Säkerhetskopiering av virtuella datorer med en reserverad IP-adress och ingen definierad slutpunkt stöds inte.
* Säkerhetskopiering av virtuella datorer som har krypterats med bara BEK stöds inte. Säkerhetskopiering av virtuella Linux-datorer krypteras med LUKS kryptering stöds inte.
* Säkerhetskopiering av virtuella datorer på skala ut filservern konfigurationen rekommenderas inte.
* Säkerhetskopierade data innehåller nätverk monterade enheter som ansluts tooVM.
* Att ersätta en befintlig virtuell dator under återställningen stöds inte. Om du försöker toorestore hello VM när hello VM finns misslyckas hello återställningen.
* Region mellan säkerhetskopiering och återställning stöds inte.
* Du kan säkerhetskopiera virtuella datorer i alla offentliga områden i Azure (se hello [checklista](https://azure.microsoft.com/regions/#services) av regioner som stöds). Om inte hello region som du letar efter stöds idag visas det inte i hello listrutan under skapande av valvet.
* Återställa en domänkontrollant stöds (DC) virtuell dator som är en del av en multi-DC-konfiguration bara via PowerShell. Läs mer om [återställa en multi-DC-domänkontrollant](backup-azure-restore-vms.md#restoring-domain-controller-vms).
* Återställning av virtuella datorer som har följande särskilda nätverkskonfigurationer hello stöds bara via PowerShell. Virtuella datorer som skapades med hello återställning arbetsflödet i hello Användargränssnittet inte dessa nätverkskonfigurationer när hello återställningen är klar. Det finns fler toolearn [återställa virtuella datorer med särskilda nätverkskonfigurationer](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations).
  * Virtuella datorer under konfigurationen av belastningsutjämnaren (interna och externa)
  * Virtuella datorer med flera reserverade IP-adresser
  * Virtuella datorer med flera nätverkskort

## <a name="create-a-recovery-services-vault-for-a-vm"></a>Skapa ett Recovery Services-valv för en virtuell dator
Recovery services-ventilen är en entitet som lagrar hello säkerhetskopieringar och återställningspunkter som har skapats med tiden. hello recovery services-ventilen innehåller också hello principer för säkerhetskopiering som är associerade med hello skyddade virtuella datorer.

toocreate en recovery services-valv:

1. Logga in toohello [Azure-portalen](https://portal.azure.com/).
2. Hej hubbmenyn, klicka på **Bläddra** och Skriv i hello lista över resurser, **återställningstjänster**. När du börjar skriva in hello listan filtreras baserat på dina indata. Klicka på **Recovery Services-valv**.

    ![Klicka på bläddringsknappen hello och ange Recovery Services. När du ser hello återställningstjänster valvet alternativet genom att klicka på tooopen hello valvet återställningstjänster bladet.](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

    hello lista över Recovery Services-valv visas.
3. På hello **Recovery Services-valv** -menyn klickar du på **Lägg till**.

    ![Skapa Recovery Services-valv (steg 2)](./media/backup-azure-arm-vms-prepare/rs-vault-menu.png)

    hello återställningstjänster valvet blad öppnas, där du uppmanas tooprovide en **namn**, **prenumeration**, **resursgruppen**, och **plats**.

    ![Skapa ett Recovery Services-valv (steg 5)](./media/backup-azure-arm-vms-prepare/rs-vault-attributes.png)
4. För **namn**, ange ett eget namn tooidentify hello valv. hello namn måste toobe unika för hello Azure-prenumeration. Skriv ett namn som innehåller mellan 2 och 50 tecken. Det måste börja med en bokstav och får endast innehålla bokstäver, siffror och bindestreck.
5. Klicka på **prenumeration** toosee hello tillgängliga listan över prenumerationer. Om du inte är säker på vilken prenumeration toouse använder standard-hello (eller förslag) prenumerationen. Du kan bara välja mellan flera alternativ om ditt organisationskonto är associerat med flera Azure-prenumerationer.
6. Klicka på **resursgruppen** toosee hello tillgängliga listan över resursgrupper, eller klicka på **ny** toocreate en ny resursgrupp. För komplett information om resursgrupper, se [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
7. Klicka på **plats** tooselect hello geografiskt område för hello-valvet. hello valvet **måste** vara i hello samma region som hello virtuella datorer som du vill tooprotect.

   > [!IMPORTANT]
   > Om du är osäker på hello plats där den virtuella datorn finns Stäng utanför hello valvet skapa dialogrutan och gå toohello listan över virtuella datorer i hello-portalen. Om du har virtuella datorer i flera områden, behöver toocreate Recovery Services-valvet i varje region. Skapa hello valvet hello första plats innan du fortsätter toohello nästa plats. Det finns inga behov toospecify lagringskonton toostore hello säkerhetskopieringsdata--hello Recovery Services-valvet och hello Azure Backup service hantera det automatiskt.
   >
   >

8. Klicka på **Skapa**. Det kan ta en stund innan hello Recovery Services-valvet toobe skapas. Övervaka hello statusmeddelanden i övre högra delen hello hello-portalen. När din valvet har skapats visas den i hello lista över Recovery Services-valv. Om du inte ser ditt valv, klickar du på **uppdatera** till

    ![Lista över säkerhetskopieringsvalv](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

    Nu när du har skapat ditt valv, lär du dig hur tooset hello storage-replikering.

## <a name="set-storage-replication"></a>Konfigurera lagringsreplikering
hello lagringsalternativ för replikering kan du toochoose mellan geo-redundant lagring och lokalt redundant lagring. Valvet använder geo-redundant lagring som standard. Lämna hello alternativet set toogeo-redundant lagring om det här är den primära säkerhetskopieringen. Välj lokalt redundant lagring om du vill använda ett billigare alternativ som inte är lika beständigt.

replikeringsinställningen för tooedit hello lagring:

1. På hello **Recovery Services-valv** bladet välj ditt valv.
    När du klickar på ditt valv hello inställningsbladet (*som har hello namnet på valvet hello överst hello*) och hello valvet detaljer blad öppnas.

    ![Välj ditt valv hello listan över säkerhetskopieringsvalv](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)

2. På hello **inställningar** blad, Använd hello lodräta skjutreglaget tooscroll ned toohello **hantera** avsnitt. Klicka på **säkerhetskopiering infrastruktur** tooopen dess bladet. I hello **allmänna** Klicka på avsnittet **konfigurering av säkerhetskopiering** tooopen dess bladet. På hello **konfigurering av säkerhetskopiering** bladet välj hello replikering lagringsalternativ för ditt valv. Valvet använder geo-redundant lagring som standard. Om du ändrar hello lagringstyp replikering, klickar du på **spara**.

    ![Lista över säkerhetskopieringsvalv](./media/backup-azure-arm-vms-prepare/full-blade.png)

     Om du använder Azure som en primär slutpunkt för lagring av säkerhetskopior fortsätter du att använda geo-redundant lagring. Om du använder Azure som en icke-primär säkerhetskopieringslagring slutpunkt Välj lokalt redundant lagring. Läs mer om [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) och [lokalt redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) lagringsalternativ i hello [Azure Storage-replikering: översikt](../storage/common/storage-redundancy.md).
    När du väljer hello lagringsalternativ för ditt valv, är du redo tooassociate hello VM hello-valvet. toobegin hello association, ska du identifiera och registrera hello Azure virtuella datorer.

## <a name="select-a-backup-goal-set-policy-and-define-items-tooprotect"></a>Välj ett mål för säkerhetskopiering, ange principen och definiera objekt tooprotect
Innan du registrerar en virtuell dator med ett valv, kör du hello identifiering processen tooensure att alla nya virtuella datorer som har lagts till toohello prenumeration identifieras. hello begär Azure hello listan över virtuella datorer i hello-prenumeration, tillsammans med ytterligare information som hello molntjänstnamnet och hello region. I hello Azure-portalen refererar scenariot toowhat ska tooput i hello recovery services-valvet. Principen är hello schemat för hur ofta och när återställningspunkter tas. Principen innehåller också hello Kvarhållningsintervall för hello Återställningspunkter.

1. Om du redan har ett Recovery Services-valv öppna fortsätta toostep 2. Om du inte har ett Recovery Services-valv öppna och öppna sedan hello [Azure-portalen](https://portal.azure.com/) och klickar på hello hubbmenyn **fler tjänster**.

   * Skriv i hello lista över resurser, **återställningstjänster**.
   * När du börjar skriva in hello listan filtreras baserat på dina indata. När du ser **Recovery Services-valv** klickar du på det.

     ![Skapa Recovery Services-valv (steg 1)](./media/backup-azure-arm-vms-prepare/browse-to-rs-vaults-updated.png) <br/>

     hello visas Recovery Services-valv. Om det finns några valv i din prenumeration kan är den här listan tom.

    ![Vy över hello Recovery Services-valv lista](./media/backup-azure-arm-vms-prepare/rs-list-of-vaults.png)

   * Hello listan över Recovery Services-valv och välj en valvet tooopen dess instrumentpanel.

     hello inställningsbladet och hello valvet instrumentpanel för hello valt valvet, öppnas.

     ![Öppna bladet för valvet](./media/backup-azure-arm-vms-prepare/new-vault-settings-blade.png)
2. På menyn för hello valvet instrumentpanelen **säkerhetskopiering** tooopen hello säkerhetskopiering bladet.

    ![Öppna bladet Säkerhetskopiering](./media/backup-azure-arm-vms-prepare/backup-button.png)

    hello säkerhetskopierings- och mål för säkerhetskopian blad öppna.

    ![Öppna bladet Scenario](./media/backup-azure-arm-vms-prepare/select-backup-goal-1.png)

3. Ange på hello säkerhetskopiering målet bladet **var körs din arbetsbelastning** tooAzure och **vad vill du vill toobackup** tooVirtual datorn och klicka sedan på **OK**.

    Detta registrerar hello VM-tillägget med hello-valvet. hello säkerhetskopiering målet blad stängs och hello **säkerhetskopiera princip** blad öppnas.

    ![Öppna bladet Scenario](./media/backup-azure-arm-vms-prepare/select-backup-goal-2.png)
4. Välj hello säkerhetskopieringsprincip som du vill tooapply toohello valvet på bladet av hello säkerhetskopiering.

    ![Välja säkerhetskopieringspolicy](./media/backup-azure-arm-vms-prepare/setting-rs-backup-policy-new.png)

    hello information om hello standardprincipen visas under hello nedrullningsbara menyn. Om du vill toocreate en ny princip väljer **Skapa nytt** hello nedrullningsbara menyn. Mer information om hur du definierar en säkerhetskopieringspolicy finns i [Definiera en säkerhetskopieringspolicy](backup-azure-vms-first-look-arm.md#defining-a-backup-policy).
    Klicka på **OK** tooassociate hello säkerhetskopieringsprincip med hello-valvet.

    Hej säkerhetskopiering princip blad stängs och hello **Välj virtuella datorer** blad öppnas.
5. I hello **Välj virtuella datorer** bladet välj hello virtuella datorer tooassociate med hello angetts princip och klicka på **OK**.

    ![Välja arbetsbelastning](./media/backup-azure-arm-vms-prepare/select-vms-to-backup.png)

    hello valda virtuella datorn har verifierats. Om du inte ser hello virtuella datorer som du förväntade toosee kontrollerar du att de finns i samma Azure-plats som hello Recovery Services-valvet och inte är redan skyddade i ett annat valv hello. hello platsen för hello Recovery Services-valvet visas på instrumentpanelen för hello-valvet.

6. Nu när du har definierat alla inställningar för hello valvet i hello Backup-bladet klickar du på **Aktivera säkerhetskopiering**. Detta distribuerar hello princip toohello valvet och hello virtuella datorer. Detta skapar inte hello första återställningspunkten för hello virtuella datorn.

    ![Aktivera säkerhetskopiering](./media/backup-azure-arm-vms-prepare/vm-validated-click-enable.png)

När du har aktiverat har hello säkerhetskopiering, körs din princip för säkerhetskopiering enligt schemat. Om du vill att nu toogenerate en säkerhetskopiering på begäran tooback av hello virtuella datorer, se [Triggering hello säkerhetskopieringsjobb](./backup-azure-arm-vms.md#triggering-the-backup-job).

Om du har problem med att registrera hello virtuella datorn finns i hello efter information om hur du installerar hello VM-agenten och på nätverksanslutningen. Måste du förmodligen inte hello efter information om du skyddar virtuella datorer som skapats i Azure. Men om du har migrerat virtuella datorer till Azure sedan vara att du har korrekt installerat hello VM-agenten och att den virtuella datorn kan kommunicera med hello virtuellt nätverk.

## <a name="install-hello-vm-agent-on-hello-virtual-machine"></a>Installera hello VM-agenten på hello virtuella datorn
hello Azure VM-agenten måste installeras på hello Azure virtuell dator för hello säkerhetskopiering tillägget toowork. Hello VM-agenten är redan installerat på hello virtuella datorn om den virtuella datorn skapades från hello Azure-galleriet. Den här informationen anges för hello situationer där du har *inte* med hjälp av en virtuell dator skapas från hello Azure-galleriet – till exempel du migrerat en virtuell dator från ett lokalt datacenter. I sådana fall måste hello VM-agenten toobe installerad på ordning tooprotect hello virtuella datorn. Lär dig mer om hello [VM-agenten](../virtual-machines/windows/classic/agents-and-extensions.md#azure-vm-agents-for-windows-and-linux).

Om du har problem med säkerhetskopiera hello Azure VM, kontrollera att hello Azure VM-agenten har installerats rätt på hello virtuell dator (se hello tabellen nedan). hello följande tabell innehåller ytterligare information om hello VM-agenten för Windows och Linux virtuella datorer.

| **Åtgärd** | **Windows** | **Linux** |
| --- | --- | --- |
| Installera hello VM-Agent |Hämta och installera hello [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Du måste administratören privilegier toocomplete hello installation. |<li> Installera hello senaste [Linux-agenten](../virtual-machines/linux/agent-user-guide.md). Du måste administratören privilegier toocomplete hello installation. Vi rekommenderar att du installerar agenten från databasen för din distribution. Vi **rekommenderar inte** Linux VM-agent som installeras direkt från github.  |
| Uppdatera hello VM-Agent |Uppdaterar hello VM-agenten är så enkelt som att installera om hello [binärfilerna för VM-agenten](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br>Se till att inga Säkerhetskopieringsåtgärden körs medan hello VM-agenten uppdateras. |Följ instruktionerna för hello på [uppdatering hello Linux VM-agenten](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Vi rekommenderar att du uppdaterar agenten från databasen för din distribution. Vi **rekommenderar inte** uppdatering Linux VM-agenten direkt från github.<br>Se till att inga Säkerhetskopieringsåtgärden körs vid hello VM-agenten uppdateras. |
| Verifiera hello VM agentinstallation |<li>Navigera toohello *C:\WindowsAzure\Packages* mapp i hello Azure VM. <li>Du ska hitta hello WaAppAgent.exe filen finns.<li> Högerklicka på filen hello finns för**egenskaper**, och välj sedan hello **information** fliken hello produktversionen fältet måste innehålla 2.6.1198.718 eller högre. |Saknas |

### <a name="backup-extension"></a>Säkerhetskopieringstillägg
Hello VM-agenten är installerad på hello virtuell dator, hello Azure Backup-tjänsten installeras en gång hello reservanknytning toohello VM-agenten. hello Azure Backup service sömlöst uppgraderar och korrigeringsfiler hello reservanknytning.

Hej reservanknytning installeras som hello Backup-tjänsten hello VM körs eller inte. En aktiv virtuell dator innehåller hello största möjlighet att få en programkonsekvent återställningspunkt. Hello Azure Backup-tjänsten fortsätter dock tooback in hello VM även om den är avstängd och hello-tillägget kunde inte installeras. Detta kallas för en virtuell offlinedator. I det här fallet hello återställningspunkt blir *krasch konsekvent*.

## <a name="network-connectivity"></a>Nätverksanslutning
Ordning toomanage hello VM ögonblicksbilder hello reservanknytning måste anslutningen toohello Azure offentliga IP-adresser. Utan hello rätt anslutning till Internet, hello virtuella datorns HTTP-begäranden timeout och hello säkerhetskopieringen misslyckas. Om distributionen har åtkomstbegränsningar (via en nätverkssäkerhetsgrupp (NSG), till exempel), väljer du något av följande alternativ för att tillhandahålla en tydlig sökväg för säkerhetskopiering trafik:

* [Godkända hello Azure-datacenter IP-adressintervall](http://www.microsoft.com/en-us/download/details.aspx?id=41653) -finns hello artikeln anvisningar om hur toowhitelist hello IP-adresser.
* Distribuera en HTTP-proxyserver för dirigera trafiken.

När du bestämmer vilka alternativet toouse är hello avvägningarna mellan hanterbarhet, granulär kontroll och kostnader.

| Alternativ | Fördelar | Nackdelar |
| --- | --- | --- |
| Whitelist IP-adressintervall |Inga ytterligare kostnader.<br><br>Använd för att öppna åtkomst i en NSG hello <i>Set AzureNetworkSecurityRule</i> cmdlet. |Komplexa toomanage som hello påverkas IP-adressintervall ändring över tid.<br><br>Tillhandahåller åtkomst toohello hela Azure och inte bara lagring. |
| HTTP-proxy |Granulär kontroll i hello proxy över hello lagringen webbadresser tillåts.<br>Enskild plats Internet access tooVMs.<br>Som inte omfattas av tooAzure IP-adress ändras. |Ytterligare kostnader för att köra en virtuell dator med hello proxy-programvara. |

### <a name="whitelist-hello-azure-datacenter-ip-ranges"></a>Godkända hello Azure-datacenter IP-adressintervall
toowhitelist hello Azure-datacenter IP-adressintervall finns hello [Azure-webbplatsen](http://www.microsoft.com/en-us/download/details.aspx?id=41653) detaljer om hello IP-adressintervall och instruktioner.

### <a name="using-an-http-proxy-for-vm-backups"></a>Med hjälp av en HTTP-proxy för VM-säkerhetskopieringar
När du säkerhetskopierar en virtuell dator skickar hello reservanknytning på hello VM hello ögonblicksbild management kommandon tooAzure lagring med hjälp av en HTTPS-API. Dirigera hello reservanknytning trafik via hello HTTP-proxy eftersom det är hello endast komponenten har konfigurerats för åtkomst toohello offentliga Internet.

> [!NOTE]
> Det finns ingen rekommendation för hello proxy-programvara som ska användas. Se till att du väljer en proxy som är kompatibel med hello konfigurationssteg nedan.
>
>

hello exempel bilden nedan visar hello tre konfigurationssteg nödvändiga toouse en HTTP-proxy:

* Appen VM vägar alla HTTP-trafik som är bunden till hello offentliga Internet via Proxy VM.
* Proxy VM tillåter inkommande trafik från virtuella datorer i hello virtuellt nätverk.
* Hej Nätverkssäkerhetsgrupp (NSG) med namnet NSF låsning måste en säkerhet regeln möjliggör utgående Internet-trafik från VM-Proxy.

![NSG: N med HTTP-proxy deployment diagram](./media/backup-azure-vms-prepare/nsg-with-http-proxy.png)

toouse en HTTP-proxy toocommunicating toohello offentliga Internet, gör du följande:

#### <a name="step-1-configure-outgoing-network-connections"></a>Steg 1. Konfigurera utgående nätverksanslutningar
###### <a name="for-windows-machines"></a>För Windows-datorer
Detta konfigurerar proxyserverkonfiguration för det lokala systemkontot.

1. Hämta [PsExec](https://technet.microsoft.com/sysinternals/bb897553)
2. Kör följande kommando från en upphöjd kommandotolk

     ```
     psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"
     ```
     Fönster i internet explorer öppnas.
3. Gå tooTools -> Internet-alternativ -> anslutningar LAN-inställningar ->.
4. Kontrollera proxy-inställningar för System-kontot. Ange Proxy-IP och port.
5. Stäng Internet Explorer.

Detta ställer in en datoromfattande proxy-konfiguration och kommer att användas för all utgående HTTP/HTTPS-trafik.

Om du har konfigurerat en proxyserver för ett aktuella användarkonto (inte ett lokalt systemkonto) använda hello följande skript tooapply dem tooSYSTEMACCOUNT:

```
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name DefaultConnectionSettings -Value $obj.DefaultConnectionSettings
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings\Connections" -Name SavedLegacySettings -Value $obj.SavedLegacySettings
   $obj = Get-ItemProperty -Path Registry::”HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Internet Settings"
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name ProxyEnable -Value $obj.ProxyEnable
   Set-ItemProperty -Path Registry::”HKEY_USERS\S-1-5-18\Software\Microsoft\Windows\CurrentVersion\Internet Settings" -Name Proxyserver -Value $obj.Proxyserver
```

> [!NOTE]
> Om du upptäcker ”(407) krävs proxyautentisering” i serverloggen proxy, kontrollera autentiseringen har konfigurerats korrekt.
>
>

###### <a name="for-linux-machines"></a>För Linux-datorer
Lägg till följande rad toohello hello ```/etc/environment``` fil:

```
http_proxy=http://<proxy IP>:<proxy port>
```

Lägg till följande rader toohello hello ```/etc/waagent.conf``` fil:

```
HttpProxy.Host=<proxy IP>
HttpProxy.Port=<proxy port>
```

#### <a name="step-2-allow-incoming-connections-on-hello-proxy-server"></a>Steg 2. Tillåt inkommande anslutningar på hello proxyserver:
1. Öppna Windows-brandväggen på hello proxyserver. hello enklaste sättet tooaccess hello brandväggen är toosearch för Windows-brandväggen med avancerad säkerhet.

    ![Öppna hello brandväggen](./media/backup-azure-vms-prepare/firewall-01.png)
2. I dialogrutan för Windows-brandväggen hello Högerklicka **regler för inkommande trafik** och på **ny regel...** .

    ![Skapa en ny regel](./media/backup-azure-vms-prepare/firewall-02.png)
3. I hello **ny inkommande regel för**, Välj hello **anpassad** alternativ för hello **regeltyp** och på **nästa**.
4. På hello sidan tooselect hello **programmet**, Välj **alla program** och på **nästa**.
5. På hello **protokoll och portar** anger hello följande information och klicka på **nästa**:

    ![Skapa en ny regel](./media/backup-azure-vms-prepare/firewall-03.png)

   * för *protokolltyp* Välj *TCP*
   * för *lokal port* Välj *specifika portar*, ange hello i hello fältet nedan ```<Proxy Port>``` som har konfigurerats.
   * för *Fjärrport* Välj *alla portar*

     För hello resten av hello guiden, klickar du på alla hello sätt toohello slutar och ge regeln ett namn.

#### <a name="step-3-add-an-exception-rule-toohello-nsg"></a>Steg 3. Lägg till ett undantag regeln toohello NSG:
Ange hello följande kommando i en Azure PowerShell-kommandotolk:

hello följande kommando lägger till ett undantag toohello NSG. Det här undantaget tillåter TCP-trafik från alla portar på 10.0.0.5 tooany Internet-adress på port 80 (HTTP) eller 443 (HTTPS). Om du behöver en specifik port i hello offentliga Internet vara säker på att tooadd som port toohello ```-DestinationPortRange``` samt.

```
Get-AzureNetworkSecurityGroup -Name "NSG-lockdown" |
Set-AzureNetworkSecurityRule -Name "allow-proxy " -Action Allow -Protocol TCP -Type Outbound -Priority 200 -SourceAddressPrefix "10.0.0.5/32" -SourcePortRange "*" -DestinationAddressPrefix Internet -DestinationPortRange "80-443"
```


*De här stegen kan du använda specifika namn och värden för det här exemplet. Använd hello namn och värden för din distribution när du anger, eller kopiera och klistra in information i koden.*

Nu när du vet att du har nätverksanslutning, är du redo tooback upp den virtuella datorn. Se [säkerhetskopiera Resource Manager distribuerade virtuella datorer](backup-azure-arm-vms.md).

## <a name="questions"></a>Frågor?
Om du har frågor eller om det inte finns någon funktion som du vill att toosee ingår, [skicka feedback](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Nästa steg
Nu när du har förberett din miljö för att säkerhetskopiera den virtuella datorn, är nästa logiska steg toocreate en säkerhetskopia. hello planera artikeln innehåller mer detaljerad information om hur du säkerhetskopierar virtuella datorer.

* [Säkerhetskopiera virtuella datorer](backup-azure-vms.md)
* [Planera infrastrukturen för säkerhetskopiering VM](backup-azure-vms-introduction.md)
* [Hantera säkerhetskopiering för virtuella datorer](backup-azure-manage-vms.md)
