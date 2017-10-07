---
title: "aaaOverview för Linux virtuella datorer i Azure | Microsoft Docs"
description: "Beskriver Azure Compute, lagring och nätverk tjänster med Linux virtuella datorer."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: rickstercdn
manager: timlt
editor: 
ms.assetid: 7965a80f-ea24-4cc2-bc43-60b574101902
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/14/2016
ms.author: rclaus
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 958e219d53026d787a7a41f2fd13c29c739a9238
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-and-linux"></a>Azure och Linux
Microsoft Azure är en växande samling integrerade offentliga molntjänster, inklusive analytics, virtuella datorer, databaser, mobil-, nätverk, lagring, och web&mdash;perfekt för värd för dina lösningar.  Microsoft Azure tillhandahåller en skalbar plattform som gör att du tooonly betala för det du använder, när du vill att den - utan tooinvest i lokal maskinvara.  Azure är klar när du är tooscale dina lösningar in och ut toowhatever skala du behöver tooservice hello behoven hos dina klienter.

Om du är bekant med hello olika funktioner i Amazon's AWS, du kan undersöka hello Azure vs AWS [definitionen mappning dokumentet](https://azure.microsoft.com/campaigns/azure-vs-aws/mapping/).

## <a name="regions"></a>Regioner
Microsoft Azure-resurser är fördelade på flera geografiska regioner hello världen.  ”Region” representerar flera datacenter i ett enda geografiskt område.  Vi har 34 regioner som är allmänt tillgänglig hello världen med en ytterligare 4 regioner meddelats. Eftersom vi fortsätter tooexpand våra global täckning - upprätthålla en uppdaterad lista av befintliga och nya meddelats regioner.

* [Azure-regioner](https://azure.microsoft.com/regions/)

## <a name="availability"></a>Tillgänglighet
Vi har meddelat en branschen inledande instans virtuella serviceavtal för 99,9% som du distribuerar hello VM med premium-lagring för alla diskar.  För din distribution tooqualify för våra standard 99,95% VM servicenivåavtal, måste du ändå toodeploy två eller flera virtuella datorer som körs din arbetsbelastning i en tillgänglighetsuppsättning. Detta garanterar dina virtuella datorer är fördelade på flera feldomäner i våra datacenter som distribuerats på värdar med olika underhållsfönster. hello fullständig [Azure-serviceavtalet](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_0/) förklarar hello garanteras tillgänglighet för Azure som helhet.

## <a name="managed-disks"></a>Managed Disks

Hanterade diskar Azure Storage skapande av konton och hantering i bakgrunden hello du och garanterar att du inte har tooworry om hello skalbarhetsbegränsningar av hello storage-konto. Du bara ange hello diskstorleken och hello prestandanivån (Standard eller Premium) och Azure skapar och hanterar hello disk åt dig. Även om du lägger till diskar eller skala upp eller ned hello VM, kan du inte har tooworry om hello lagring som används. Om du skapar nya virtuella datorer, [använda hello Azure CLI 2.0](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller hello Azure portal toocreate virtuella datorer med hanterade Operativsystemet och datadiskarna. Om du har virtuella datorer med ohanterad diskar, kan du [Konvertera din toobe för virtuella datorer som säkerhetskopieras med hanterade diskar](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Du kan också hantera egna, anpassade avbildningar i ett lagringskonto per Azure-region och använda dem toocreate hundratals för virtuella datorer i hello samma prenumeration. Mer information om hanterade diskar finns hello [översikt för hanterade diskar](../windows/managed-disks-overview.md).

## <a name="azure-virtual-machines--instances"></a>Virtuella Azure-datorer & instanser
Microsoft Azure har stöd för flera populära Linux-distributioner tillhandahålls och underhålls av ett antal partners.  Du hittar distributioner, till exempel Red Hat Enterprise, CentOS, Debian, Ubuntu, virtuell CoreOS, RancherOS, FreeBSD och mycket mer i hello Azure Marketplace. Vi arbetar aktivt tillsammans med olika Linux communities tooadd ännu mer varianter toohello [Azure-godkända Linux Distros](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) lista.

Om din önskade Linux distro väljer inte är för närvarande finns i hello-galleriet, du kan ”ta egna Linux” VM av [skapa och ladda upp en Linux VHD i Azure](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Virtuella Azure-datorer kan du toodeploy en mängd olika databehandling lösningar i en flexibel metod. Du kan distribuera valfri arbetsbelastning och alla språk på nästan alla operativsystem - Windows, Linux, eller en anpassad skapat en från någon av våra växande listan över partners. Fortfarande inte kan se vad du letar efter?  Oroa dig inte – du kan också hämta dina egna avbildningar från lokala.

## <a name="vm-sizes"></a>VM-storlekar
När du distribuerar en virtuell dator i Azure ska tooselect VM-storlek i en av våra serie storlekar som är lämplig tooyour arbetsbelastning. hello storlek påverkar också hello processorkraft, minne, och lagringskapaciteten för hello virtuell dator. Du debiteras baserat på hello mängden tid hello VM körs och använda dess allokerade resurser. En fullständig lista över [storlekar på virtuella datorer](sizes.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Här följer några riktlinjer för att välja en VM-storlek från en av våra (A, D, DS, G och GS).
* A-serien är vår värdet prissatta enklare virtuella datorer för enstaka arbetsbelastningar och scenarier för utveckling och testning. De kan är allmänt tillgänglig i alla regioner och ansluta och använda alla standard resurser tillgängliga toovirtual datorer.
* A-series-storlekar (A8 - A11) är särskilda beräkning beräkningsintensiva konfigurationer lämpligt för högpresterande kluster datorprogram.
* D-serien är utformad toorun program som kräver högre datorkraft och tillfällig diskprestanda. Virtuella datorer i D-serien ger snabbare processorer och minne till core förhållandet SSD (SSD) för hello diskutrymme.
* Dv2-serien är hello senaste versionen av vår D-serien, har en kraftfullare processor. Hej Dv2-serien CPU är ungefär 35% snabbare än hello D-serien CPU. Den är baserad på hello senaste generationens 2,4 GHz Intel Xeon® E5-2673 v3 (Haskell) processor, och med hello Intel Turbo förstärkningen Technology 2.0, gå upp too3.2 GHz. Hej Dv2-serien har hello samma minne och konfigurationer som hello D-serien.
* G-serien virtuella datorer och erbjuder hello mest minne och kör på värdar som har Intel Xeon E5 V3 family processorer.

Obs: DS-serien och GS-serien virtuella datorer ha åtkomst tooPremium lagring - vår SSD säkerhetskopieras hög prestanda och låg latens lagring för i/o-intensiv arbetsbelastning. Premium Storage är tillgängligt i vissa regioner. Mer information finns i:

* [Premium-lagring: Högpresterande lagring för arbetsbelastningar på virtuella Azure-datorn](../../storage/common/storage-premium-storage.md)

## <a name="automation"></a>Automation
tooachieve en korrekt DevOps kultur, alla infrastrukturen måste vara kod.  När alla hello infrastruktur liv i koden den enkelt kan återskapas (Phoenix servrar).  Azure fungerar med alla hello-större automation tooling som Ansible, Chef, SaltStack och Puppet.  Azure har också ett eget verktygsuppsättning för automatisering:

* [Azure-mallar](create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Azure VMAccess](using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Stöd för inför Azure [moln init](http://cloud-init.io/) i de flesta Linux Distros som stöder detta.  För närvarande distribueras Canonical's Ubuntu virtuella datorer med molnet init aktiverad som standard.  Röd tak RHEL, CentOS och Fedora stöder molnet init, men hello Azure avbildningar som underhålls av RedHat inte har installerat molnet initiering.  toouse moln initiering på en RedHat-OS, måste du skapa en anpassad avbildning med molnet init installerad.

* [Med hjälp av molnet initiering på virtuella Azure Linux-datorer](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="quotas"></a>Kvoter
Varje Azure-prenumeration har standard kvotgränser som kan påverka hello distributionen av ett stort antal virtuella datorer för projektet. hello aktuella gränsen på grundval av per prenumeration är 20 virtuella datorer per region.  Kvotgränser kan aktiveras snabbt och genom att arkivera ett supportärende begär en gräns ökar enkelt.  Mer information om kvotgränserna:

* [Tjänstbegränsningarna för Azure-prenumeration](../../azure-subscription-service-limits.md)

## <a name="partners"></a>Partner
Microsoft nära samarbete med våra samarbetspartners tooensure hello tillgängliga avbildningarna för, uppdateras och optimerats för en Azure körning.  Kontrollera marketplace sidorna nedan för mer information om våra partner.

* Linux på Azure - [-godkända distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* SUSE - [Azure Marketplace - SUSE Linux Enterprise Server](https://azuremarketplace.microsoft.com/en-us/marketplace/apps?search=%27SUSE%27)
* Redhat - [Azure Marketplace - RedHat Enterprise Linux 7.2](https://azure.microsoft.com/marketplace/partners/redhat/redhatenterpriselinux72/)
* Kanoniskt - [Azure Marketplace - Ubuntu Server 16.04 LTS](https://azure.microsoft.com/marketplace/partners/canonical/ubuntuserver1604lts/)
* Debian - [Azure Marketplace - Debian 8 ”Jessie”](https://azure.microsoft.com/marketplace/partners/credativ/debian8/)
* FreeBSD - [Azure Marketplace - FreeBSD 10.3](https://azure.microsoft.com/marketplace/partners/microsoft/freebsd103/)
* Virtuell CoreOS - [Azure Marketplace - virtuell CoreOS (stabil)](https://azure.microsoft.com/marketplace/partners/coreos/coreosstable/)
* RancherOS - [Azure Marketplace - RancherOS](https://azure.microsoft.com/marketplace/partners/rancher/rancheros/)
* Bitnami - [Bitnami bibliotek för Azure](https://azure.bitnami.com/)
* Mesosphere - [Azure Marketplace - Mesosphere DC/OS på Azure](https://azure.microsoft.com/marketplace/partners/mesosphere/dcosdcos/)
* Docker - [Azure Marketplace - Azure Container Service med Docker Swarm](https://azure.microsoft.com/marketplace/partners/microsoft/acsswarms/)
* Jenkins - [Azure Marketplace - CloudBees Jenkins plattform](https://azure.microsoft.com/marketplace/partners/cloudbees/jenkins-platformjenkins-platform/)

## <a name="getting-started-with-linux-on-azure"></a>Komma igång med Linux på Azure
toobegin med Azure som du behöver ett Azure-konto, hello Azure CLI är installerad och ett par SSH offentliga och privata nycklar.

### <a name="sign-up-for-an-account"></a>Registrera dig för ett konto
hello första steget i med hjälp av hello Azure-molnet är toosign för ett Azure-konto.  Gå toohello [Azure kontoinloggning](https://azure.microsoft.com/pricing/free-trial/) sidan tooget igång.

### <a name="install-hello-cli"></a>Installera hello CLI
Med ditt nya Azure-konto kan du sätta igång direkt med hello Azure-portalen är en webbaserad admin-panel.  toomanage hello Azure-molnet via hello kommandoradsverktyg, installera hello `azure-cli`.  Installera hello [Azure CLI 2.0](/cli/azure/install-azure-cli) på arbetsstationen Mac eller Linux.

### <a name="create-an-ssh-key-pair"></a>Skapa ett SSH-nyckelpar
Nu har du en Azure-konto och hello Azure webbportalen hello Azure CLI.  hello nästa steg är toocreate en SSH-nyckelpar som används tooSSH i Linux utan lösenord.  [Skapa SSH-nycklar för Linux och Mac](mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooenable inloggningar och bättre säkerhet.

### <a name="create-a-vm-using-hello-cli"></a>Skapa en virtuell dator med hjälp av hello CLI
Skapa ett Linux VM är använda hello CLI ett snabbt sätt toodeploy en virtuell dator utan lämnar hello terminal du arbetar med.  Allt du kan ange på hello web-portalen är tillgänglig via en kommandorad flagga eller växel.  

* [Skapa en Linux VM som använder hello CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="create-a-vm-in-hello-portal"></a>Skapa en virtuell dator i hello-portalen
Skapa en Linux VM i hello Azure web-portalen är ett sätt tooeasily punkt och klicka dig igenom hello olika alternativ tooget tooa distribution.  Istället för att använda kommandoradsverktyget flaggor och växlar som kan tooview en bra Webblayout av olika alternativ och inställningar.  Allt tillgängligt via kommandoradsgränssnittet hello finns också i hello-portalen.

* [Skapa en Linux VM som använder hello Portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

### <a name="login-using-ssh-without-a-password"></a>Logga in med hjälp av SSH utan lösenord
hello VM körs nu på Azure och du är redo toolog i.  Det är osäker och tidskrävande att använda lösenord toolog i via SSH.  Använda SSH-nycklar är hello säkraste sättet och hello snabbaste sättet toologin.  När du skapar Linux VM via hello-portalen eller hello CLI, har du två alternativ för autentisering.  Om du väljer ett lösenord för SSH konfigurerar hello VM tooallow inloggningar via lösenord i Azure.  Om du väljer toouse en offentlig SSH-nyckel, Azure konfigurerar hello VM tooonly Tillåt inloggningar via SSH-nycklar och inaktiverar Lösenordsinloggning. toosecure din Linux VM genom att endast tillåta SSH-nyckel inloggningar använda hello SSH offentlig nyckel alternativet under hello skapa en virtuell dator i hello portal eller CLI.

## <a name="related-azure-components"></a>Relaterade Azure-komponenter
## <a name="storage"></a>Lagring
* [Introduktion tooMicrosoft Azure Storage](../../storage/common/storage-introduction.md)
* [Lägg till en disk tooa Linux VM med hello azure-cli](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Hur tooattach data disk tooa Linux VM i hello Azure-portalen](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="networking"></a>Nätverk
* [Översikt över virtuella nätverk](../../virtual-network/virtual-networks-overview.md)
* [IP-adresser i Azure](../../virtual-network/virtual-network-ip-addresses-overview-arm.md)
* [Öppna portar tooa Linux VM i Azure](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Skapa ett fullständigt kvalificerat domännamn i hello Azure-portalen](portal-create-fqdn.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="containers"></a>Behållare
* [Virtuella datorer och behållare i Azure](containers.md)
* [Azure Container Service-introduktion](../../container-service/container-service-intro.md)
* [Distribuera ett Azure Container Service-kluster](../../container-service/dcos-swarm/container-service-deployment.md)

## <a name="next-steps"></a>Nästa steg
Nu har du en översikt över Linux på Azure.  hello nästa steg är toodive i och skapa ett fåtal virtuella datorer!

* [Utforska våra växande listan över exempel på skript för vanliga uppgifter via AzureCLI](cli-samples.md)
