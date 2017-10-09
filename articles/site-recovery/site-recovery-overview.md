---
title: "aaaWhat är Azure Site Recovery? | Microsoft Docs"
description: "En översikt över hello Azure Site Recovery-tjänsten och sammanfattar distributionsscenarier."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: e9b97b00-0c92-4970-ae92-5166a4d43b68
ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/25/2017
ms.author: raynew
ms.openlocfilehash: da6755654b8036a03314ec836f014b64428d5518
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-site-recovery"></a>Vad är Site Recovery?

Välkommen toohello Azure Site Recovery-tjänsten! Den här artikeln ger en snabb överblick över hello-tjänsten.

## <a name="business-continuity-and-disaster-recovery-bcdr-with-azure-recovery-services"></a>Verksamhetskontinuitet och haveriberedskap (BCDR) med Azure Recovery Services

Som en organisation behöver du toofigure reda på hur du kommer tookeep säkra data och appar/arbetsbelastningar som körs när du planerade och oplanerade avbrott inträffar.

Azure Recovery Services bidra tooyour BCDR-strategi:

- **Site Recovery-tjänsten**: Site Recovery hjälper till att garantera kontinuitet för företag genom att hålla appar som körs på virtuella datorer och fysiska servrar tillgängliga om en plats kraschar. Site Recovery replikerar arbetsbelastningar som körs på virtuella datorer och fysiska servrar så att de fortfarande är tillgängliga på en sekundär plats om hello primära platsen inte är tillgänglig. Den återställer arbetsbelastningar toohello primära platsen när den är igång och körs igen.
- **Backup-tjänsten**: dessutom hello [Azure Backup](https://docs.microsoft.com/azure/backup/) tjänsten för att hålla dina data säkra och kan återställas genom att säkerhetskopiera tooAzure.

Site Recovery kan hantera replikering för:

- Replikera virtuella Azure-datorer mellan Azure-regioner.
- Lokala virtuella datorer och fysiska servrar som replikerar tooAzure eller tooa sekundär plats.


## <a name="what-does-site-recovery-provide"></a>Vad tillhandahåller Site Recovery?

**Funktion** | **Detaljer**
--- | ---
**Distribuera en enkel BCDR-lösning** | Använda Site Recovery kan du konfigurera och hantera replikering, redundans och återställning från en plats i hello Azure-portalen.
**Replikera virtuella Azure-datorer** | Du kan ställa in din BCDR-strategi så att virtuella Azure-datorer replikeras mellan Azure-regioner.
**Replikera lokala virtuella datorer på annan plats** | Du kan replikera lokala virtuella datorer och fysiska servrar tooAzure eller tooa sekundär lokal plats. Replikering tooAzure eliminerar hello kostnad och komplexitet för att upprätthålla ett sekundärt datacenter.
**Replikera alla arbetsbelastningar** | Du kan replikera alla typer av arbetsbelastningar som körs på virtuella Azure-datorer, lokala virtuella Hyper-V-datorer, virtuella VMware-datorer och fysiska Windows- och Linux-servrar som stöds.
**Skydda data flexibelt och säkert** | Site Recovery dirigerar replikering utan att spärra programdata. Replikerade data lagras i Azure-lagring med hello återhämtning som tillhandahåller. När det uppstår redundans, skapas virtuella Azure-datorer baserat på hello replikerade data.
**Uppfylla återställningstider och återställningspunkter** | Behåll återställningstiden (RTO) och återställningspunktmål (RPO) inom organisationen gränser. Site Recovery ger ständig replikering för virtuella Azure-datorer och virtuella datorer med VMware med replikeringsfrekvenser så låga som 30 sekunder för Hyper-V. Du kan minska återställningstiden (RTO) ytterligare genom att integrera med [Azure Traffic Manager](https://azure.microsoft.com/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/).
**Bevara appar vid redundansväxling** | Du kan konfigurera återställningspunkter med programkonsekventa skuggkopior. Programkonsekventa skuggkopior sparar hårdiskdata, alla data i minnet och alla pågående transaktioner.
**Testa utan avbrott** | Du kan enkelt köra redundanstester toosupport haveriberedskap utan att påverka pågående replikering.
**Kör flexibel redundansväxling** | Du kan köra planerade redundanser för förväntade avbrott med noll dataförlust, eller oplanerade redundanser med minimal dataförlust (beroende på replikeringsfrekvensen) för oväntade haverier. Du kan enkelt växla tillbaka tooyour primär plats när den är tillgänglig igen.
**Skapa återställningsplaner** | Du kan anpassa och ordna redundansväxling och återställning av multi-nivåapplikationer på flera virtuella datorer med återställningsplaner. Du kan gruppera datorer i planerna samt lägga till skript och manuella åtgärder. Du kan integrera återställningsplaner med runbooks i Azure Automation.
**Integrera med befintlig BCDR-teknik** | Site Recovery kan integreras med annan BCDR-teknik. Du kan till exempel använda Site Recovery tooprotect hello SQL Server-serverdel i företagets arbetsbelastningar, med inbyggt stöd för SQL Server AlwaysOn, toomanage hello redundans för Tillgänglighetsgrupper.
**Integrera med hello automation-bibliotek** | Ett omfattande Azure Automation-bibliotek med produktionsklara, programspecifika skript som kan hämtas och integrerats med Site Recovery.
**Hantera nätverksinställningar** | Site Recovery och Azure förenklar nätverkskraven för program, inklusive reservation av IP-adresser, konfiguration av belastningsutjämnare och integration av Azure Traffic Manager för effektiv nätverksväxling.


## <a name="what-can-i-replicate"></a>Vad kan jag replikera?

**Stöds** | **Detaljer**
--- | ---
**Vad kan jag replikera?** | Migrera virtuella Azure-datorer mellan Azure-regioner (förhandsversion)<br/><br/>  Lokala virtuella VMware-datorer, Hyper-V virtuella datorer, fysiska servrar (Windows och Linux) tooAzure < br /<br/> Lokala virtuella VMware-datorer, Hyper-V virtuella datorer, fysiska servrar tooa sekundär plats. Replikering tooa sekundär plats stöds endast för Hyper-V virtuella datorer, om Hyper-V-värdar som hanteras av System Center VMM.
**Vilka regioner har stöd för Site Recovery?** | [Regioner som stöds](https://azure.microsoft.com/regions/services/) |
**Vilka operativsystem behöver replikerade datorer?** | [Krav för virtuella Azure-datorer](site-recovery-support-matrix-azure-to-azure.md#support-for-replicated-machine-os-versions)s<br></br>[Krav för virtuella VMware-datorer](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)<br/><br/> För virtuella Hyper-V-datorer stöds samtliga [gästoperativsystem](https://technet.microsoft.com/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) som kan användas i Azure samt Hyper-V.<br/><br/> [Krav för fysisk server](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions)
**Vilka servrar/värdar för VMware behöver jag?** | Virtuella VMware-datorer kan finnas på [vSphere värdar/vCenter-servrar som stöds](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers)
**Vilka arbetsbelastningar kan jag replikera?** | Du kan replikera alla arbetsbelastningar som körs på en dator det finns replikeringsstöd för. Dessutom hello Site Recovery-teamet har utfört appspecifika tester för en [antal appar](site-recovery-workload.md#workload-summary).


## <a name="azure-portal-considerations"></a>Att tänka på om Azure portal

* Site Recovery kan distribueras i hello [Azure-portalen](https://portal.azure.com).
* Du kan hantera Site Recovery med hello klassiska services management-modell i hello klassiska Azure-portalen.
- hello klassiska portalen bara ska använda toomaintain befintliga Site Recovery-distributioner. Du kan inte skapa nya valv i hello klassiska portalen.

## <a name="next-steps"></a>Nästa steg
* Läs mer om [stöd för arbetsbelastning](site-recovery-workload.md)
* Kom igång med [Azure VM replikering mellan regioner](site-recovery-azure-to-azure.md), [VMware replikering tooAzure](vmware-walkthrough-overview.md), eller [Hyper-V-replikering tooAzure](hyper-v-site-walkthrough-overview.md).
