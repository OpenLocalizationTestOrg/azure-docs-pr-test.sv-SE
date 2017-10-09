---
title: "aaaMonitor och felsöka skydd för virtuella datorer och fysiska servrar | Microsoft Docs"
description: "Azure Site Recovery samordnar hello replikering, redundans och återställning av virtuella datorer på lokala servrar tooAzure eller ett sekundärt datacenter. Använd den här artikeln toomonitor och felsökning av skydd för Hyper-V eller Virtual Machine Manager-plats."
services: site-recovery
documentationcenter: 
author: ruturaj
manager: mkjain
editor: 
ms.assetid: 0fc8e368-0c0e-4bb1-9d50-cffd5ad0853f
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: rajanaki
ms.openlocfilehash: d790375db5f30e98f009c8d8272558188c51934c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-troubleshoot-protection-for-virtual-machines-and-physical-servers"></a>Övervaka och felsöka skydd för virtuella datorer och fysiska servrar
Den här övervakning och felsökning av guiden kan du lära dig hur tootrack replikeringshälsa och felsöker tekniker för Azure Site Recovery.

## <a name="understand-hello-components"></a>Förstå komponenterna hello
### <a name="vmware-virtual-machine-or-physical-server-site-deployment-for-replication-between-on-premises-and-azure"></a>Virtuell VMware-dator eller fysisk server webbplatsdistribution för replikering mellan lokala och Azure
tooset in databasåterställning mellan en lokal virtuell VMware-dator eller fysisk server och Azure måste tooset hello konfigurationsservern, huvudmålservern och processen server-komponenter på den virtuella datorn eller servern. När du aktiverar skyddet för hello källservern installerar Azure Site Recovery hello Mobile Apps-funktionen i Microsoft Azure App Service. När ett lokalt avbrott och efter hello källa server växlar över tooAzure måste kunder tooset in en processerver i Azure och en huvudmålserver på källservern för lokala toorebuild hello lokalt.

![VMware/fysisk plats distributionen för replikering mellan lokala och Azure](media/site-recovery-monitoring-and-troubleshooting/image18.png)

### <a name="virtual-machine-manager-site-deployment-for-replication-between-on-premises-sites"></a>Virtual Machine Manager webbplatsdistribution för replikering mellan lokala platser
tooset in databasåterställning mellan två lokala platser, du behöver toodownload hello Azure Site Recovery-providern och installera den på hello Virtual Machine Manager-servern. hello måste Internet anslutning tooensure att alla hello-åtgärder som utlöses från hello Azure-portalen hämta översatta tooon lokala åtgärder.

![Virtual Machine Manager webbplatsdistribution för replikering mellan lokala platser](media/site-recovery-monitoring-and-troubleshooting/image1.png)

### <a name="virtual-machine-manager-site-deployment-for-replication-between-on-premises-locations-and-azure"></a>Virtual Machine Manager webbplatsdistribution för replikering mellan lokala platser och Azure
När du ställer in databasåterställning mellan lokala platser och Azure måste toodownload hello Azure Site Recovery-providern och installera den på hello Virtual Machine Manager-servern. Du måste också tooinstall hello Azure Recovery Services-agenten, som måste toobe installeras på varje Hyper-V-värd. [Lär dig mer](site-recovery-hyper-v-azure-architecture.md) för mer information.

![Virtual Machine Manager webbplatsdistribution för replikering mellan lokala platser och Azure](media/site-recovery-monitoring-and-troubleshooting/image2.png)

### <a name="hyper-v-site-deployment-for-replication-between-on-premises-locations-and-azure"></a>Distribution av Hyper-V-platsen för replikering mellan lokala platser och Azure
Den här processen är liknande tooVirtual Machine Manager-distribution. hello endast skillnaden är att hello Azure Site Recovery-providern och Azure Recovery Services-agenten installeras på hello Hyper-V-värden. [Läs mer](site-recovery-hyper-v-azure-architecture.md). .

## <a name="monitor-configuration-protection-and-recovery-operations"></a>Övervaka åtgärder för konfiguration, skydd och återställning
Varje åtgärd i Azure Site Recovery granskas och följs under hello **jobb** fliken. För att konfigurationen, skydd och Återställningsfel, gå toohello **jobb** fliken och Sök efter fel.

![hello misslyckades filter i fliken för hello-jobb](media/site-recovery-monitoring-and-troubleshooting/image3.png)

Om du hittar fel under hello **jobb** , klicka på hello jobb och på **FELINFORMATION** för jobbet.

![hello FELINFORMATION knappen](media/site-recovery-monitoring-and-troubleshooting/image4.png)

hello felinformation hjälper dig att identifiera en möjlig orsak och rekommendation för hello problemet.

![Dialogruta som visar information om fel för ett specifikt jobb](media/site-recovery-monitoring-and-troubleshooting/image5.png)

I föregående exempel hello verkar en annan åtgärd pågår toobe orsakar hello skydd configuration toofail. Lös hello problem utifrån hello rekommendation och klicka sedan på **RESART** tooinitiate hello igen.

![hello omstartningsknappen hello fliken för jobb](media/site-recovery-monitoring-and-troubleshooting/image6.png)

Hej **starta om** alternativet är inte tillgängligt för alla åtgärder. Om en åtgärd inte har hello **starta om** alternativ, gå tillbaka toohello objekt och gör om hello igen. Du kan avbryta jobb som pågår med hjälp av hello **Avbryt** knappen.

![hello Avbryt-knapp](media/site-recovery-monitoring-and-troubleshooting/image7.png)

## <a name="monitor-replication-health-for-virtual-machines"></a>Övervaka hälsotillstånd för replikering för virtuella datorer
Du kan använda hello Azure portal tooremotely övervakaren Azure Site Recovery-providrar för varje hello skyddade enheter. Klicka på **skyddade objekt**, och klicka sedan på **VMM-MOLN** eller **SKYDDSGRUPPER**. Hej **VMM-MOLN** fliken är endast tillgängligt för distributioner som är baserade på Virtual Machine Manager. För andra scenarier hello skyddade enheter som är under hello **SKYDDSGRUPPER** fliken.

![hello alternativ för VMM-moln och SKYDDSGRUPPER](media/site-recovery-monitoring-and-troubleshooting/image8.png)

Klicka på en skyddad entitet under hello respektive moln eller protection grupp toosee visas alla tillgängliga åtgärder i hello längst ned i fönstret.

![Tillgängliga alternativ för en skyddad entitet](media/site-recovery-monitoring-and-troubleshooting/image9.png)

I hello föregående skärmbild visas hälsotillståndet för den virtuella datorn hello är **kritisk**. Du kan klicka på hello **FELINFORMATION** hello nedre toosee hello fel-knappen. Baserat på hello **möjliga orsaker** och **rekommendation**, hello problemet.

![hello felinformation knappen](media/site-recovery-monitoring-and-troubleshooting/image10.png)

![Fel och rekommendationerna i dialogrutan för hello felinformation](media/site-recovery-monitoring-and-troubleshooting/image11.png)

> [!NOTE]
> Om alla aktiva åtgärder pågår eller inte, gå toohello **jobb** vyn som nämnts tidigare tooview hello fel för ett visst jobb.
>
>

## <a name="troubleshoot-on-premises-hyper-v-issues"></a>Felsökning av problem med lokala Hyper-V
Anslut toohello lokala Hyper-V manager-konsolen väljer hello virtuella datorn och se hello replikeringshälsa.

![Alternativet tooview replikeringshälsa i hello Hyper-V manager-konsolen](media/site-recovery-monitoring-and-troubleshooting/image12.png)

I det här fallet **Replikeringshälsa** är **kritisk**. Högerklicka på hello virtuella datorn och klicka sedan på **replikering** > **visa Replikeringshälsa** toosee hello information.

![Hälsotillstånd för replikering för en specifik virtuell dator](media/site-recovery-monitoring-and-troubleshooting/image13.png)

Om replikeringen har pausats för hello virtuella datorn, högerklicka på hello virtuella datorn och sedan på **replikering** > **återuppta replikering**.

![Alternativet tooresume replikering i hello Hyper-V manager-konsolen](media/site-recovery-monitoring-and-troubleshooting/image19.png)

Om en virtuell dator migreras en ny Hyper-V-värd som ligger inom hello kluster eller en fristående dator och hello Hyper-V-värden har konfigurerats i Azure Site Recovery påverkas replikering för hello virtuell dator inte. Se till att nya Hyper-V-värden för hello uppfyller alla krav på hello och konfigureras med hjälp av Azure Site Recovery.

### <a name="event-log"></a>Händelseloggen
| Händelsekällan | Information |
| --- |:--- |
| **Program och tjänsten loggar/Microsoft/VirtualMachineManager/Server/Admin** (Virtual Machine Manager-server) |Ger användbar loggning tootroubleshoot många olika problem i Virtual Machine Manager. |
| **Program och -loggarna/MicrosoftAzureRecoveryServices/replikeringen** (Hyper-V-värd) |Ger användbar loggning tootroubleshoot många problem vid Microsoft Azure Recovery Services-agenten. <br/> ![Platsen för replikering händelsekällan för Hyper-V-värd](media/site-recovery-monitoring-and-troubleshooting/eventviewer03.png) |
| **Program och tjänsten loggar/Microsoft/Azure Site Recovery/providern/Operational** (Hyper-V-värd) |Ger användbar loggning tootroubleshoot många problem vid Microsoft Azure Site Recovery-tjänsten. <br/> ![Platsen för operativa händelsekällan för Hyper-V-värd](media/site-recovery-monitoring-and-troubleshooting/eventviewer02.png) |
| **Program och tjänsten loggar/Microsoft/Windows/Hyper-V-VMMS/Admin** (Hyper-V-värd) |Ger användbar loggning tootroubleshoot många problem med hantering av Hyper-V virtuell dator. <br/> ![Platsen för Virtual Machine Manager händelsekällan för Hyper-V-värd](media/site-recovery-monitoring-and-troubleshooting/eventviewer01.png) |

### <a name="hyper-v-replication-logging-options"></a>Alternativ för loggning av Hyper-V-replikering
Alla händelser som rör tooHyper-V-replikering är inloggad hello Hyper-V-VMMS\\Admin logg finns under program- och tjänstloggar\\Microsoft\\Windows. Dessutom kan du aktivera en analytiska logg för hello Hyper-V Virtual Machine Management-tjänsten. tooenable detta loggar, först göra hello analys och felsökning loggar kan visas i hello Loggboken. Öppna Loggboken och klicka sedan på **visa** > **visa analytiska loggar och felsökningsloggar**.

![hello visa analytiska loggar och felsökningsloggar alternativet](media/site-recovery-monitoring-and-troubleshooting/image14.png)

En analytiska loggen visas under **Hyper-V-VMMS**.

![hello analytiska loggar i hello Loggboken träd](media/site-recovery-monitoring-and-troubleshooting/image15.png)

I hello **åtgärder** rutan klickar du på **aktivera loggning**. När den är aktiverad, visas den i **Prestandaövervakaren** som en **spårningssessionen för händelser** finns under **datainsamlaruppsättningar.**

![Spårningssessioner för händelser i hello Prestandaövervakaren träd](media/site-recovery-monitoring-and-troubleshooting/image16.png)

tooview Hej Insamlad information, först stoppa hello spårningssessionen genom att inaktivera hello-loggen. Spara hello logg och öppna den igen i Loggboken eller använda andra verktyg tooconvert den efter behov.

## <a name="reach-out-for-microsoft-support"></a>Nå ut Support för Microsoft
### <a name="log-collection"></a>Logginsamling
Skydd av Virtual Machine Manager-plats finns för[Azure Site Recovery Logginsamling Support Diagnostics plattformen (SDP) verktyget](http://social.technet.microsoft.com/wiki/contents/articles/28198.asr-data-collection-and-analysis-using-the-vmm-support-diagnostics-platform-sdp-tool.aspx) toocollect hello krävs loggar.

Skydd för Hyper-V-platsen, kan du hämta den [verktyget](https://dcupload.microsoft.com/tools/win7files/DIAG_ASRHyperV_global.DiagCab) och köra på hello Hyper-V-värd toocollect hello loggar.

VMware/fysisk server scenarier finns för[Azure Site Recovery Logginsamling för VMware och fysiska plats protection](http://social.technet.microsoft.com/wiki/contents/articles/30677.azure-site-recovery-log-collection-for-vmware-and-physical-site-protection.aspx) toocollect hello krävs loggar.

hello verktyget samlar in hello loggar lokalt i en slumpmässigt undermapp under % LocalAppData%\ElevatedDiagnostics.

![Exempel steg visas från skydd för Hyper-V-platsen.](media/site-recovery-monitoring-and-troubleshooting/animate01.gif)

### <a name="open-a-support-ticket"></a>Öppna ett supportärende
tooraise ett supportärende för Azure Site Recovery nå ut tooAzure Support med hjälp av URL-Adressen på <http://aka.ms/getazuresupport>.

## <a name="kb-articles"></a>KB-artiklar
* [Hur toopreserve hello enhetsbeteckningen för skyddade virtuella datorer som har redundansväxlats eller migreras tooAzure](http://support.microsoft.com/kb/3031135)
* [Hur toomanage lokalt tooAzure skydd av nätverksbandbredd](https://support.microsoft.com/kb/3056159)
* [Azure Site Recovery: ”hello klusterresurs kunde inte hittas” fel när du försöker tooenable skydd för en virtuell dator](http://support.microsoft.com/kb/3010979)
* [Förstå och felsöka Hyper-V-replikering guide](http://social.technet.microsoft.com/wiki/contents/articles/21948.hyper-v-replica-troubleshooting-guide.aspx)

## <a name="common-azure-site-recovery-errors-and-their-resolutions"></a>Vanliga fel i Azure Site Recovery och sina lösningar
Nedan följer vanliga fel och sina lösningar. Varje fel dokumenteras i en separat wiki-sida.

### <a name="general"></a>Allmänt
* <span style="color:green;">NYA</span> [jobb misslyckas med felet ”en åtgärd pågår”. Fel 505, 514, 532.](http://social.technet.microsoft.com/wiki/contents/articles/32190.azure-site-recovery-jobs-failing-with-error-an-operation-is-in-progress-error-505-514-532.aspx)
* <span style="color:green;">NYA</span> [jobb misslyckas med felet ”servern är inte anslutna toohello Internet”. Fel 25018.](http://social.technet.microsoft.com/wiki/contents/articles/32192.azure-site-recovery-jobs-failing-with-error-server-isn-t-connected-to-the-internet-error-25018.aspx)

### <a name="setup"></a>Konfiguration
* [hello Virtual Machine Manager-servern kan inte registreras på grund av tooan internt fel. Mer information om hello fel finns toohello jobbvyn i hello Site Recovery-portalen. Kör installationsprogrammet igen tooregister hello-server.](http://social.technet.microsoft.com/wiki/contents/articles/25570.the-vmm-server-cannot-be-registered-due-to-an-internal-error-please-refer-to-the-jobs-view-in-the-site-recovery-portal-for-more-details-on-the-error-run-setup-again-to-register-the-server.aspx)
* [En anslutning kan inte vara etablerade toohello Hyper-V Recovery Manager-valvet. Kontrollera hello proxyinställningarna eller försök igen senare.](http://social.technet.microsoft.com/wiki/contents/articles/25571.a-connection-cant-be-established-to-the-hyper-v-recovery-manager-vault-verify-the-proxy-settings-or-try-again-later.aspx)

### <a name="configuration"></a>Konfiguration
* [Det går inte toocreate Skyddsgrupp: ett fel uppstod vid inläsningen hello listan över servrar.](http://blogs.technet.com/b/somaning/archive/2015/08/12/unable-to-create-the-protection-group-in-azure-site-recovery-portal.aspx)
* [Hyper-V-värdkluster innehåller minst ett statiskt nätverkskort eller så inga anslutna nätverkskort konfigurerade toouse DHCP.](http://social.technet.microsoft.com/wiki/contents/articles/25498.hyper-v-host-cluster-contains-at-least-one-static-network-adapter-or-no-connected-adapters-are-configured-to-use-dhcp.aspx)
* [Virtual Machine Manager har inte behörighet toocomplete en åtgärd.](http://social.technet.microsoft.com/wiki/contents/articles/31110.vmm-does-not-have-permissions-to-complete-an-action.aspx)
* [Det går inte att markera hello lagringskonto inom hello prenumeration vid konfiguration av skydd.](http://social.technet.microsoft.com/wiki/contents/articles/32027.can-t-select-the-storage-account-within-the-subscription-while-configuring-protection.aspx)

### <a name="protection"></a>Skydd
* <span style="color:green;">NYA</span> [Aktivera skydd misslyckas med felet ”Det gick inte att konfigurera skydd för virtuella hello”. Fel 60007, 40003.](http://social.technet.microsoft.com/wiki/contents/articles/32194.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-configured-for-the-virtual-machine-error-60007-40003.aspx)
* <span style="color:green;">NYA</span> [Aktivera skydd misslyckas med felet ”Det gick inte att aktivera skydd för hello virtuell dator”. Fel 70094.](http://social.technet.microsoft.com/wiki/contents/articles/32195.azure-site-recovery-enable-protection-failing-with-error-protection-couldn-t-be-enabled-for-the-virtual-machine-error-70094.aspx)
* <span style="color:green;">NYA</span> [Direktmigrering fel 23848 - hello virtuella datorn försätts toobe flyttas med typen Live. Det kan innebära att hello status för hello virtuell dator.](http://social.technet.microsoft.com/wiki/contents/articles/32021.live-migration-error-23848-the-virtual-machine-is-going-to-be-moved-using-type-live-this-could-break-the-recovery-protection-status-of-the-virtual-machine.aspx)
* [Aktivera skyddet misslyckades eftersom agenten inte installerad på värddatorn.](http://social.technet.microsoft.com/wiki/contents/articles/31105.enable-protection-failed-since-agent-not-installed-on-host-machine.aspx)
* [En lämplig värd för hello replikerade virtuella datorn kan inte hittas - på grund av toolow beräkningsresurser.](http://social.technet.microsoft.com/wiki/contents/articles/25501.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-low-compute-resources.aspx)
* [En lämplig värd för hello replikerade virtuella datorn kan inte hittas - på grund av toono logiska nätverk är kopplat.](http://social.technet.microsoft.com/wiki/contents/articles/25502.a-suitable-host-for-the-replica-virtual-machine-can-t-be-found-due-to-no-logical-network-attached.aspx)
* [Det går inte att ansluta toohello värddatorn för replik - det gick inte att ansluta.](http://social.technet.microsoft.com/wiki/contents/articles/31106.cannot-connect-to-the-replica-host-machine-connection-could-not-be-established.aspx)

### <a name="recovery"></a>Återställning
* Virtual Machine Manager kan inte slutföra värdåtgärden hello:
  * [Redundans toohello valt återställningspunkt för den virtuella datorn: Allmänt meddelande om nekad åtkomst.](http://social.technet.microsoft.com/wiki/contents/articles/25504.fail-over-to-the-selected-recovery-point-for-virtual-machine-general-access-denied-error.aspx)
  * [Hyper-V misslyckade toofail över toohello valt återställningspunkt för den virtuella datorn: åtgärden avbröts.  Försök senare återställningspunkt. (0x80004004).](http://social.technet.microsoft.com/wiki/contents/articles/25503.hyper-v-failed-to-fail-over-to-the-selected-recovery-point-for-virtual-machine-operation-aborted-try-a-more-recent-recovery-point-0x80004004.aspx)
  * En anslutning med hello-servern gick inte att upprätta (0x00002EFD).
    * [Det gick inte att tooenable omvänd replikering för virtuella Hyper-V.](http://social.technet.microsoft.com/wiki/contents/articles/25505.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-reverse-replication-for-virtual-machine.aspx)
    * [Det gick inte att tooenable replikering för den virtuella datorn virtuella Hyper-V.](http://social.technet.microsoft.com/wiki/contents/articles/25506.a-connection-with-the-server-could-not-be-established-0x00002efd-hyper-v-failed-to-enable-replication-for-virtual-machine-virtual-machine.aspx)
  * [Det gick inte att genomföra redundans för den virtuella datorn.](http://social.technet.microsoft.com/wiki/contents/articles/25508.could-not-commit-failover-for-virtual-machine.aspx)
* [Hej återställningsplanen innehåller virtuella datorer som inte är redo för planerad redundans.](http://social.technet.microsoft.com/wiki/contents/articles/25509.the-recovery-plan-contains-virtual-machines-which-are-not-ready-for-planned-failover.aspx)
* [hello virtuella datorn är inte redo för planerad redundans.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
* [Virtuella datorn körs inte och är inte avstängda.](http://social.technet.microsoft.com/wiki/contents/articles/25510.virtual-machine-is-not-running-and-is-not-powered-off.aspx)
* [Det uppstod ett out-of-band-åtgärd på en virtuell dator och genomföra redundans misslyckades.](http://social.technet.microsoft.com/wiki/contents/articles/25507.the-virtual-machine-isn-t-ready-for-planned-failover.aspx)
* Testa redundans
  * [Redundansväxlingen kunde inte initieras eftersom redundanstestning pågår.](http://social.technet.microsoft.com/wiki/contents/articles/31111.failover-could-not-be-initiated-since-test-failover-is-in-progress.aspx)
* <span style="color:green;">NYA</span> växling vid fel på grund av timeout 'PreFailoverWorkflow uppgift WaitForScriptExecutionTaskTimeout' på grund av toohello konfigurationsinställningarna på hello Nätverkssäkerhetsgrupp kopplad till hello virtuell dator eller hello undernät toowhich hello datorn tillhör. Se för['PreFailoverWorkflow aktivitet WaitForScriptExecutionTaskTimeout'](https://aka.ms/troubleshoot-nsg-issue-azure-site-recovery) mer information.

### <a name="configuration-server-process-server-master-target"></a>Konfigurationsservern, processervern, huvudmålservern
* [hello ESXi-värd på vilka hello PS/CS finns som en virtuell dator misslyckas med en lila skärm död.](http://social.technet.microsoft.com/wiki/contents/articles/31107.vmware-esxi-host-experiences-a-purple-screen-of-death.aspx)

### <a name="remote-desktop-troubleshooting-after-failover"></a>Fjärrskrivbord felsökning efter växling vid fel
* Många kunder har upptäckt ett problem tooconnect toohello redundansväxlade virtuella datorn i Azure. [Använd hello felsöka dokumentet tooRDP i hello virtuella](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).

#### <a name="adding-a-public-ip-on-a-resource-manager-virtual-machine"></a>Lägga till en offentlig IP-adress på en resource manager virtuell dator
Om hello **Anslut** knapp i hello-portalen är nedtonad och du inte är ansluten tooAzure via en Express Route eller plats-till-plats VPN-anslutning, du behöver toocreate och tilldela den virtuella datorn en offentlig IP-adress innan du kan använda fjärråtkomst Fjärrskrivbord/delat Shell. Sedan kan du lägga en offentlig IP-adress för hello gränssnitt för hello virtuell dator.  

![Lägger till en offentlig IP-adress på hello nätverksgränssnittet för redundansväxling virtuell dator](media/site-recovery-monitoring-and-troubleshooting/createpublicip.gif)
