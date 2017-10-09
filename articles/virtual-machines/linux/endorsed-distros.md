---
title: aaaEndorsed distributioner av Linux | Microsoft Docs
description: "Läs mer om Linux på Azure-godkända distributioner, inklusive riktlinjer för Ubuntu, CentOS, Oracle och SUSE."
services: virtual-machines-linux
documentationcenter: 
author: szarkos
manager: timlt
editor: tysonn
tags: azure-service-management,azure-resource-manager
ms.assetid: 2777a526-c260-4cb9-a31a-bdfe1a55fffc
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/02/2017
ms.author: szark
ms.openlocfilehash: f006972d4611434c62b72a1d8df60caf753e15f2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="linux-on-distributions-endorsed-by-azure"></a>Linux på distributioner godkända av Azure
Partners ger Linux bilder i hello Azure Marketplace. Vi arbetar med olika Linux communities tooadd ännu mer varianter toohello godkända distributionslista. I hello tiden för distributioner som inte är tillgängliga från hello Marketplace, du kan alltid ta egna Linux med hjälp av hello riktlinjer på [skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem](classic/create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2fclassic%2ftoc.json).

## <a name="supported-distributions-and-versions"></a>Distributioner som stöds och versioner
hello följande tabell visas hello Linux-distributioner och versioner som stöds på Azure. Se för[stöd för Linux bilder i Microsoft Azure](https://support.microsoft.com/en-us/kb/2941892) mer detaljerad information.

hello Linux Integration Services (LIS) drivrutiner för Hyper-V och Azure är kernel-moduler att Microsoft direkt bidrar toohello överordnade Linux kernel.  Vissa drivrutiner LIS är inbyggda i hello distribution kernel som standard. Äldre distributioner som är baserade på Red Hat Enterprise (RHEL) och CentOS är tillgängliga som en separat fil på [Linux Integration Services Version 4.1 för Hyper-V](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409). Se [Linux kernel krav](create-upload-generic.md#linux-kernel-requirements) för mer information om hello LIS drivrutiner.

hello Azure Linux-agenten är redan installerad på hello Azure Marketplace-bilder och är vanligtvis tillgänglig från hello distribution paketdatabasen. Källkoden kan hittas på [GitHub](https://github.com/azure/walinuxagent).

| Distribution | Version | Drivrutiner | Agent |
| --- | --- | --- | --- |
| CentOS |CentOS 6.3 + 7.0 + |CentOS 6.3: [LIS hämta](http://go.microsoft.com/fwlink/?LinkID=403033&clcid=0x409)<p>CentOS 6.4 +: I kernel |Paket: I [lagringsplatsen](http://olcentgbl.trafficmanager.net/openlogic/6/openlogic/x86_64/RPMS/) under ”WALinuxAgent” <br/>Källkod: [GitHub](https://github.com/Azure/WALinuxAgent) |
| [CoreOS](https://coreos.com/docs/running-coreos/cloud-providers/azure/) |494.4.0+ |I kernel |Källkod: [GitHub](https://github.com/coreos/coreos-overlay/tree/master/app-emulation/wa-linux-agent) |
| Debian |Debian 7,9 +, 8.2 + |I kernel |Paket: I lagringsplatsen under ”waagent” <br/>Källkod: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Oracle Linux |6.4+, 7.0+ |I kernel |Paket: I lagringsplatsen under ”WALinuxAgent” <br/>Källkod: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998) |
| Red Hat Enterprise Linux |RHEL 6.7 +, 7.1 + |I kernel |Paket: I lagringsplatsen under ”WALinuxAgent” <br/>Källkod: [GitHub](https://github.com/Azure/WALinuxAgent) |
| SUSE Linux Enterprise |SLES/SLES för SAP<br>11 SP4<br>12 SP1 +|I kernel |Paket:<p> för 11 i [molnet: verktyg](https://build.opensuse.org/project/show/Cloud:Tools) lagringsplatsen<br>för 12 ingår i modulen ”offentliga moln” under ”python-azure-agent”<br/>Källkod: [GitHub](http://go.microsoft.com/fwlink/p/?LinkID=250998) |
| openSUSE |openSUSE Leap 42.1 + |I kernel |Paket: I [molnet: verktyg](https://build.opensuse.org/project/show/Cloud:Tools) lagringsplatsen under ”python-azure-agent” <br/>Källkod: [GitHub](https://github.com/Azure/WALinuxAgent) |
| Ubuntu |Ubuntu 12.04, 14.04, 16.04, 16,10 |I kernel |Paket: I lagringsplatsen under ”walinuxagent” <br/>Källkod: [GitHub](https://github.com/Azure/WALinuxAgent) |

## <a name="partners"></a>Partner

### <a name="coreos"></a>CoreOS
[https://coreos.com/docs/running-coreos/cloud-providers/Azure/](https://coreos.com/docs/running-coreos/cloud-providers/azure/)

Från hello virtuell CoreOS webbplats:

*Virtuell CoreOS är utformad för säkerhet, konsekvens och tillförlitlighet. I stället för att installera paket via yum eller lgh använder virtuell CoreOS Linux behållare toomanage dina tjänster på en högre abstraktionsnivå. En enskild tjänst kod och alla beroenden paketeras i en behållare som kan köras på en eller flera virtuell CoreOS-datorer.*

### <a name="credativ"></a>credativ
[http://www.credativ.co.uk/credativ-Blog/debian-images-Microsoft-Azure](http://www.credativ.co.uk/credativ-blog/debian-images-microsoft-azure)

Credativ är en oberoende rådgivning och tjänster företaget som är specialiserat på hello utveckling och implementering av professional lösningar med hjälp av kostnadsfri programvara. Som inledande öppen källkod specialister har Credativ internationella recognition med många IT-avdelningar som använder sina stöd. Tillsammans med Microsoft förbereds Credativ för närvarande för motsvarande Debian avbildningar för Debian 8 (Jessie) och Debian innan 7 (Wheezy). Båda bilderna är särskilt utformad toorun på Azure och kan enkelt hanteras via hello-plattformen. Credativ stöder också hello på lång sikt och uppdatering av hello Debian avbildningar för Azure via dess supportcenter för öppen källa.

### <a name="oracle"></a>Oracle
[http://www.Oracle.com/technetwork/topics/cloud/FAQ-1963009.HTML](http://www.oracle.com/technetwork/topics/cloud/faq-1963009.html)

Oracles strategi är toooffer en bred portfölj av lösningar för offentliga och privata moln. hello strategi ger kunder valmöjligheterna och flexibiliteten i hur de distribuerar en Oracle-programvara i Oracle-moln och andra moln. Oracles partnerskap med Microsoft kan kunder toodeploy Oracle-programvara i Microsoft offentliga och privata moln med hello förtroende för certifiering och support från Oracle.  Oracles åtaganden och investering i Oracle offentliga och privata molnlösningar är oförändrade.

### <a name="red-hat"></a>Red Hat
[http://www.Redhat.com/en/partners/Strategic-Alliance/Microsoft](http://www.redhat.com/en/partners/strategic-alliance/microsoft)

Hej världens ledande leverantör av lösningar med öppen källkod, Red Hat hjälper mer än 90% av Fortune 500-företagen lösa utmaningar för företag, justera sina IT och företag strategier och förbereda för hello framtida teknik. Red Hat gör detta genom att tillhandahålla säkra lösningar via en öppen affärsmodell och prenumerationer prisvärt, förutsägbar modellen.

### <a name="suse"></a>SUSE
[http://www.SUSE.com/SUSE-Linux-Enterprise-Server-on-Azure](http://www.suse.com/suse-linux-enterprise-server-on-azure)

SUSE Linux Enterprise Server på Azure är en beprövad plattform som ger högre tillförlitlighet och säkerhet för molntjänster. SUSES flexibel Linux-plattformen integreras sömlöst med Azure cloud services toodeliver ett enkelt hanterbar molnmiljö. Mer än 9,200 certifierade program från mer än 1 800 oberoende programvaruleverantörer för SUSE Linux Enterprise Server säkerställer SUSE att arbetsbelastningar som körs stöds i hello Datacenter säkert sätt kan distribueras på Azure.

### <a name="canonical"></a>Canonical
[http://www.Ubuntu.com/cloud/Azure](http://www.ubuntu.com/cloud/azure)

Kanoniskt tekniker och öppna community styrning enheten Ubuntus lyckade i klienten, servern och molntjänster som innehåller personliga molntjänster för konsumenterna. Kanoniskt visionen för en enhetlig ledigt plattform i Ubuntu, från phone toocloud innehåller en familj av sammanhängande gränssnitt för hello telefon, surfplatta, TV och skrivbordet. Denna vision gör Ubuntu hello första val för olika institutioner från offentligt moln providers toohello beslutsfattare konsumenten Electronics och en favorit bland enskilda tekniker.

Med utvecklare och engineering Datacenter runt hello world är Canonical unikt positionerade toopartner med maskinvara beslutsfattare och innehållsleverantörer programvara utvecklare toobring Ubuntu lösningar toomarket för datorer, servrar och handdatorer.
