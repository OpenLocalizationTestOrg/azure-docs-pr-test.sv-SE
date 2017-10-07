---
title: "aaaPreparing din miljö tooback in virtuella Azure-datorer | Microsoft Docs"
description: "Kontrollera att din miljö har förberetts för att säkerhetskopiera virtuella datorer i Azure"
services: backup
documentationcenter: 
author: markgalioto
manager: carmonn
editor: 
keywords: "säkerhetskopiering. Säkerhetskopiera;"
ms.assetid: 238ab93b-8acc-4262-81b7-ce930f76a662
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 4/25/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: 3b914c507dd6ad703ea799115ae84ac229e27787
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="prepare-your-environment-tooback-up-azure-virtual-machines"></a>Förbereda din miljö tooback av Azure virtuella datorer
> [!div class="op_single_selector"]
> * [Resource Manager-modellen](backup-azure-arm-vms-prepare.md)
> * [Klassiska modellen](backup-azure-vms-prepare.md)
>
>

Innan du kan säkerhetskopiera en Azure virtuell dator (VM) finns det tre villkor som måste finnas.

* Du behöver toocreate ett säkerhetskopieringsvalv eller identifiera ett befintligt säkerhetskopieringsvalvet *i hello samma region som din virtuella dator*.
* Upprätta nätverksanslutningen mellan hello Azure offentliga Internet-adresser och hello Azure storage-slutpunkter.
* Installera hello VM-agenten på hello VM.

Om du känner till dessa villkor redan finns i din miljö och sedan fortsätta toohello [säkerhetskopiera virtuella datorer artikeln](backup-azure-vms.md). Annars kan leder läsas på den här artikeln dig igenom hello steg tooprepare din miljö tooback upp en Azure VM.

##<a name="supported-operating-system-for-backup"></a>Operativsystem som stöds för säkerhetskopiering
 * **Linux**: Azure Backup stöder [en lista över distributioner som godkänts av Azure](../virtual-machines/linux/endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) med undantag för Core OS Linux. _Andra Bring-Your-äger-Linux-distributioner kan också fungera så länge hello VM agent är tillgänglig på den virtuella datorn hello och stöd för Python finns. Vi inte stödja dessa distributioner för säkerhetskopiering._
 * **Windows Server**: versioner som är äldre än Windows Server 2008 R2 stöds inte.


## <a name="limitations-when-backing-up-and-restoring-a-vm"></a>Begränsningar när du säkerhetskopierar och återställer en virtuell dator
> [!NOTE]
> Azure har två distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md). hello följande lista innehåller hello begränsningar när du distribuerar i hello klassiska modellen.
>
>

* Säkerhetskopiera virtuella datorer med fler än 16 datadiskar stöds inte.
* Säkerhetskopiering av virtuella datorer med en reserverad IP-adress och ingen definierad slutpunkt stöds inte.
* Säkerhetskopierade data innehåller nätverk monterade enheter som ansluts tooVM.
* Att ersätta en befintlig virtuell dator under återställningen stöds inte. Först tar bort hello befintlig virtuell dator och alla associerade diskar och sedan återställa hello data från en säkerhetskopia.
* Region mellan säkerhetskopiering och återställning stöds inte.
* Säkerhetskopiering av virtuella datorer med hjälp av hello Azure Backup-tjänsten stöds i alla offentliga områden i Azure (se hello [checklista](https://azure.microsoft.com/regions/#services) av regioner som stöds). Om inte hello region som du letar efter stöds idag visas det inte i hello listrutan under skapande av valvet.
* Säkerhetskopiering av virtuella datorer med hjälp av hello Azure Backup-tjänsten stöds bara för väljer operativsystemversioner:
* Återställa en domänkontrollant stöds (DC) virtuell dator som är en del av en multi-DC-konfiguration bara via PowerShell. Läs mer om [återställa en multi-DC-domänkontrollant](backup-azure-restore-vms.md#restoring-domain-controller-vms).
* Återställning av virtuella datorer som har följande särskilda nätverkskonfigurationer hello stöds bara via PowerShell. Virtuella datorer som du skapar med hjälp av hello Återställ arbetsflöde i hello Användargränssnittet inte dessa nätverkskonfigurationer när hello återställningen är klar. Det finns fler toolearn [återställa virtuella datorer med särskilda nätverkskonfigurationer](backup-azure-restore-vms.md#restoring-vms-with-special-network-configurations).
  * Virtuella datorer under konfigurationen av belastningsutjämnaren (interna och externa)
  * Virtuella datorer med flera reserverade IP-adresser
  * Virtuella datorer med flera nätverkskort

## <a name="create-a-backup-vault-for-a-vm"></a>Skapa ett säkerhetskopieringsvalv för en virtuell dator
Ett säkerhetskopieringsvalv är en entitet som lagrar alla hello säkerhetskopieringar och återställningspunkter som har skapats med tiden. Hej säkerhetskopieringsvalvet innehåller också hello säkerhetskopieringsprinciper som kommer att tillämpas toohello virtuella datorer säkerhetskopieras.

> [!IMPORTANT]
> Från mars 2017 kan använda du inte längre hello klassiska portal toocreate säkerhetskopieringsvalv. Befintliga säkerhetskopieringsvalv stöds fortfarande och det är möjligt för[använda Azure PowerShell toocreate säkerhetskopieringsvalv](./backup-client-automation-classic.md#create-a-backup-vault). Microsoft rekommenderar emellertid att du skapar Recovery Services-valv för alla distributioner eftersom framtida förbättringar gäller tooRecovery Services valv, endast.


Den här bilden visar hello relationer mellan hello olika Azure Backup-entiteter: ![Azure Backup-enheter och relationer](./media/backup-azure-vms-prepare/vault-policy-vm.png)



## <a name="network-connectivity"></a>Nätverksanslutning
Ordning toomanage hello VM ögonblicksbilder hello reservanknytning måste anslutningen toohello Azure offentliga IP-adresser. Utan hello rätt anslutning till Internet, hello virtuella datorns HTTP-begäranden timeout och hello säkerhetskopieringen misslyckas. Om distributionen har åtkomstbegränsningar (via en nätverkssäkerhetsgrupp (NSG), till exempel), väljer du något av följande alternativ för att tillhandahålla en tydlig sökväg för säkerhetskopiering trafik:

* [Godkända hello Azure-datacenter IP-adressintervall](http://www.microsoft.com/en-us/download/details.aspx?id=41653) -finns hello artikeln anvisningar om hur toowhitelist hello IP-adresser.
* Distribuera en HTTP-proxyserver för dirigera trafiken.

När du bestämmer vilka alternativet toouse är hello avvägningarna mellan hanterbarhet, granulär kontroll och kostnader.

| Alternativ | Fördelar | Nackdelar |
| --- | --- | --- |
| Whitelist IP-adressintervall |Inga ytterligare kostnader.<br><br>Använd för att öppna åtkomst i en NSG hello <i>Set AzureNetworkSecurityRule</i> cmdlet. |Komplexa toomanage som hello påverkas IP-adressintervall ändring över tid.<br><br>Tillhandahåller åtkomst toohello hela Azure och inte bara lagring. |
| HTTP-proxy |Granulär kontroll i hello proxy över hello lagringen webbadresser tillåts. toosetup granulär kontroll i hello proxy https://\*.blob.core.windows.net/\* URL mönstret måste toobe godkända. toowhitelist endast hello storage-konto som används av hello VM, https://\<storageAccount\>.blob.core.windows.net/\* URL mönstret måste toobe godkända. <br>Enskild plats Internet access tooVMs.<br>Som inte omfattas av tooAzure IP-adress ändras. |Ytterligare kostnader för att köra en virtuell dator med hello proxy-programvara. |

### <a name="whitelist-hello-azure-datacenter-ip-ranges"></a>Godkända hello Azure-datacenter IP-adressintervall
toowhitelist hello Azure-datacenter IP-adressintervall finns hello [Azure-webbplatsen](http://www.microsoft.com/en-us/download/details.aspx?id=41653) detaljer om hello IP-adressintervall och instruktioner.

### <a name="using-an-http-proxy-for-vm-backups"></a>Med hjälp av en HTTP-proxy för VM-säkerhetskopieringar
När du säkerhetskopierar en virtuell dator skickar hello reservanknytning på hello VM hello ögonblicksbild management kommandon tooAzure lagring med hjälp av en HTTPS-API. Dirigera hello reservanknytning trafik via hello HTTP-proxy eftersom det är hello endast komponenten har konfigurerats för åtkomst toohello offentliga Internet.

> [!NOTE]
> Det finns ingen rekommendation för hello proxy-programvara som ska användas. Se till att du väljer en proxy som har utgående varaktighet och som är kompatibel med hello konfigurationssteg nedan. Kontrollera att programvara från tredje part inte kan ändra hello proxy-inställningar
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

*Se till att ersätta hello namnen i hello exempel med hello information lämpliga tooyour distribution.*

## <a name="vm-agent"></a>VM-agent
Innan du kan säkerhetskopiera hello Azure-dator, bör du kontrollera den hello Azure VM-agenten har installerats rätt på hello virtuell dator. Eftersom hello VM-agenten är en valfri komponent på hello gång hello virtuella datorn skapas, kontrollera hello kryssrutan för hello VM-agenten är markerad innan hello virtuella datorn har etablerats.

### <a name="manual-installation-and-update"></a>Manuell installation och uppdatera
hello VM-agenten finns redan i virtuella datorer som skapas från hello Azure-galleriet. Virtuella datorer som har migrerats från lokala Datacenter skulle inte ha hello VM-agenten är installerad. För sådana virtuella datorer kan måste hello VM-agenten toobe installerat explicit. Läs mer om [hello VM agent installeras på en befintlig virtuell dator](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx).

| **Åtgärd** | **Windows** | **Linux** |
| --- | --- | --- |
| Installera hello VM-agent |<li>Hämta och installera hello [agent MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Du måste administratören privilegier toocomplete hello installation. <li>[Uppdatera hello VM egenskapen](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate som hello agenten är installerad. |<li> Installera hello senaste [Linux-agenten](https://github.com/Azure/WALinuxAgent) från GitHub. Du måste administratören privilegier toocomplete hello installation. <li> [Uppdatera hello VM egenskapen](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx) tooindicate som hello agenten är installerad. |
| Uppdatera hello VM-agenten |Uppdaterar hello VM-agenten är så enkelt som att installera om hello [binärfilerna för VM-agenten](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). <br><br>Se till att inga Säkerhetskopieringsåtgärden körs medan hello VM-agenten uppdateras. |Följ instruktionerna för hello på [uppdatering hello Linux VM-agenten ](../virtual-machines/linux/update-agent.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). <br><br>Se till att inga Säkerhetskopieringsåtgärden körs medan hello VM-agenten uppdateras. |
| Verifiera hello VM agentinstallation |<li>Navigera toohello *C:\WindowsAzure\Packages* mapp i hello Azure VM. <li>Du ska hitta hello WaAppAgent.exe filen finns.<li> Högerklicka på filen hello finns för**egenskaper**, och välj sedan hello **information** fliken hello produktversionen fältet måste innehålla 2.6.1198.718 eller högre. |Saknas |

Lär dig mer om hello [Virtuella datoragenten](https://go.microsoft.com/fwLink/?LinkID=390493&clcid=0x409) och [hur tooinstall den](https://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/).

### <a name="backup-extension"></a>Säkerhetskopieringstillägg
tooback hello virtuella datorn hello Azure Backup-tjänsten installeras en tillägget toohello VM-agent. hello Azure Backup service sömlöst uppgraderar och korrigeringsfiler hello reservanknytning utan ytterligare åtgärder.

Hej reservanknytning installeras om hello VM körs. En aktiv virtuell dator innehåller också hello största möjlighet att få en programkonsekvent återställningspunkt. Däremot installeras hello Azure Backup-tjänsten fortsätter tooback in hello VM – även om den är avstängd och hello-tillägget inte kunde (aka Offline VM). I det här fallet hello återställningspunkt blir *krasch konsekvent* som beskrivs ovan.

## <a name="questions"></a>Frågor?
Om du har frågor eller om det inte finns någon funktion som du vill att toosee ingår, [skicka feedback](http://aka.ms/azurebackup_feedback).

## <a name="next-steps"></a>Nästa steg
Nu när du har förberett din miljö för att säkerhetskopiera den virtuella datorn, är nästa logiska steg toocreate en säkerhetskopia. hello planera artikeln innehåller mer detaljerad information om hur du säkerhetskopierar virtuella datorer.

* [Säkerhetskopiera virtuella datorer](backup-azure-vms.md)
* [Planera infrastrukturen för säkerhetskopiering VM](backup-azure-vms-introduction.md)
* [Hantera säkerhetskopiering för virtuella datorer](backup-azure-manage-vms.md)
