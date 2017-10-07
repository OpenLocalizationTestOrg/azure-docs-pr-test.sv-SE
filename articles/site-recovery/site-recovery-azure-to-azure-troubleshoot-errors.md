---
title: "aaaAzure Site Recovery felsökning för Azure-Azure-replikeringsproblem och fel | Microsoft Docs"
description: "Felsökning av fel och problem vid replikering av virtuella Azure-datorer för katastrofåterställning"
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/10/2017
ms.author: sujayt
ms.openlocfilehash: bca957dd0f40e6b16e68913caf522f3431c55bd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-to-azure-vm-replication-issues"></a>Felsökning av problem med Virtuella Azure-Azure-replikering

Den här artikeln beskriver hello vanliga problem i Azure Site Recovery när replikering och återställa virtuella Azure-datorer från en region tooanother region och förklarar hur tootroubleshoot dem. Mer information om konfigurationer som stöds finns i hello [stöd matrix för att replikera virtuella datorer i Azure](site-recovery-support-matrix-azure-to-azure.md).

## <a name="azure-resource-quota-issues-error-code-150097"></a>Problem med Azure-resurs kvoten (felkod 150097)
Din prenumeration ska vara aktiverade toocreate virtuella Azure-datorer i hello målregionen som du planerar toouse som disaster recovery region. Dessutom prenumerationen ska ha tillräcklig kvot aktiverad toocreate virtuella datorer i en viss storlek. Som standard hello Site Recovery plockningar samma storlek för hello mål VM som hello Virtuella källdatorn. Om hello matchande storlek inte är tillgänglig, hämtas hello närmaste möjliga storlek automatiskt. Om det finns ingen matchande storlek som stöder VM-konfiguration för datakällan, visas detta felmeddelande:

**Felkod** | **Möjliga orsaker** | **Rekommendation**
--- | --- | ---
150097<br></br>**Meddelandet**: Det gick inte att aktivera replikering för hello virtuella VmName. | -Prenumerationen ID inte kanske aktiverat toocreate virtuella datorer i hello region målplats.</br></br>-Ditt prenumerations-ID kanske inte är aktiverat eller har inte tillräcklig kvot toocreate specifika VM-storlekar hello målplatsen region.</br></br>-En passande VM Målstorlek som matchar hello källa VM NIC antalet (2) inte kan hittas för hello prenumerations-ID i hello region målplats.| Kontakta [Azure faktureringssupporten](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) tooenable skapa en virtuell dator för hello krävs VM-storlekar hello målplatsen för din prenumeration. När den har aktiverats misslyckades hello försök igen.

### <a name="fix-hello-problem"></a>Åtgärda problemet hello
Du kan kontakta [Azure faktureringssupporten](https://docs.microsoft.com/azure/azure-supportability/resource-manager-core-quotas-request) tooenable din prenumeration toocreate virtuella datorer av nödvändiga storlekar hello målplatsen.

Om hello målplats har en begränsning på kapacitet, inaktivera replikering och aktivera det för tooa annan plats där din prenumeration har tillräcklig kvot toocreate virtuella datorer i hello storlekar.

## <a name="trusted-root-certificates-error-code-151066"></a>Betrodda rotcertifikat (felkod 151066)

Om alla hello senaste betrodda rotcertifikat inte finns i hello VM kan ”aktivera replikering” jobbet misslyckas. Anrop från hello VM misslyckas utan hello certifikat, hello autentisering och auktorisering av Site Recovery-tjänsten. Hej felmeddelande för hello misslyckades ”Aktivera replikering” Site Recovery-jobb:

**Felkod** | **Möjlig orsak** | **Rekommendationer**
--- | --- | ---
151066<br></br>**Meddelandet**: Site Recovery-konfigurationen misslyckades. | hello krävs betrodda rotcertifikat som används för auktorisering och autentisering inte finns i hello-datorn. | -Se till att hello betrodda rotcertifikat som finns på datorn hello för en virtuell dator som kör operativsystemet för Windows hello. Mer information finns i [konfigurera betrodda rotcertifikat och otillåtna certifikat](https://technet.microsoft.com/library/dn265983.aspx).<br></br>– För en virtuell dator som kör hello Linux-operativsystem, följer du hello vägledning för betrodda rotcertifikat som publicerats av hello Linux operativsystem version distributören.

### <a name="fix-hello-problem"></a>Åtgärda problemet hello
**Windows**

Installera alla hello senaste Windows-uppdateringar på hello VM så att alla hello betrodda rotcertifikat som finns på hello-datorn. Om du är i en frånkopplad miljö, följ hello Windows uppdatering standardprocessen i din organisation tooget hello certifikat. Om hello krävs certifikat inte finns i hello VM, hello anrop toohello Site Recovery-tjänsten inte av säkerhetsskäl.

Följ hello vanliga Windows update management eller process för hantering av certifikat i din organisation tooget alla hello senaste rotcertifikat och hello uppdateras certifikatåterkallning listan på hello virtuella datorer.

tooverify som hello problemet är löst, gå toologin.microsoftonline.com från en webbläsare i den virtuella datorn.

**Linux**

Följ hello riktlinjer som tillhandahålls av din Linux distributören tooget hello senaste betrodda rotcertifikat och hello senaste listan över återkallade certifikat på hello VM.

Eftersom SuSE Linux använder symlinks toomaintain en lista över certifikat, så här:

1.  Logga in som rotanvändare.

2.  Kör följande kommando:

      ``# cd /etc/ssl/certs``

3.  toosee om hello Symantec rotcertifikatutfärdarens certifikat finns eller inte, kör du kommandot:

      ``# ls VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

4.  Om hello filen inte hittas kan du köra följande kommandon:

      ``# wget https://www.symantec.com/content/dam/symantec/docs/other-resources/verisign-class-3-public-primary-certification-authority-g5-en.pem -O VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem``

      ``# c_rehash``

5.  toocreate en symlink med b204d74a.0 -> VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem genom att köra det här kommandot:

      ``# ln -s  VeriSign_Class_3_Public_Primary_Certification_Authority_G5.pem b204d74a.0``

6.  Kontrollera toosee om det här kommandot har hello följande utdata. Om inte du har toocreate en symlink:

      ``# ls -l | grep Baltimore
      -rw-r--r-- 1 root root   1303 Apr  7  2016 Baltimore_CyberTrust_Root.pem
      lrwxrwxrwx 1 root root     29 May 30 04:47 3ad48a91.0 -> Baltimore_CyberTrust_Root.pem
      lrwxrwxrwx 1 root root     29 May 30 05:01 653b494a.0 -> Baltimore_CyberTrust_Root.pem``

7. Använd det här kommandot toocreate en symlink om symlink 653b494a.0 saknas:

      ``# ln -s Baltimore_CyberTrust_Root.pem 653b494a.0``


## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Utgående anslutning för Site Recovery-URL: er eller IP-adressintervall (felkod 151037 eller 151072)

Utgående anslutning toospecific URL: er eller IP-adressintervall krävs för Site Recovery replikering toowork från hello VM. Om den virtuella datorn finns bakom en brandvägg eller använder Nätverksanslutningar security group (NSG) regler toocontrol utgående, något av dessa felmeddelanden kan visas:

**Felkoder** | **Möjliga orsaker** | **Rekommendationer**
--- | --- | ---
151037<br></br>**Meddelandet**: Det gick inte tooregister virtuella Azure-datorn med Site Recovery. | -Du använder NSG toocontrol utgående åtkomst på hello VM och hello krävs IP adressintervall inte är godkända för utgående åtkomst.</br></br>-Du använder verktyg från tredje part brandvägg och hello krävs IP-intervall /-URL: er inte är vitlistad.</br>| – Om du använder brandväggen proxy toocontrol utgående nätverksanslutningen på hello VM, se till att hello nödvändiga URL: er eller datacenter IP-adressintervall är godkända. Mer information finns i [brandväggen proxy vägledning](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>– Om du använder NSG-regler toocontrol utgående nätverksanslutningen på hello VM, kan du kontrollera att hello nödvändiga datacenter IP-adressintervall är godkända. Mer information finns i [nätverk grupp säkerhetsvägledning](https://aka.ms/a2a-nsg-guidance).
151072<br></br>**Meddelandet**: Site Recovery-konfigurationen misslyckades. | Anslutningen får inte vara etablerade tooSite slutpunkter för återställning. | – Om du använder brandväggen proxy toocontrol utgående nätverksanslutningen på hello VM, se till att hello nödvändiga URL: er eller datacenter IP-adressintervall är godkända. Mer information finns i [brandväggen proxy vägledning](https://aka.ms/a2a-firewall-proxy-guidance).</br></br>– Om du använder NSG-regler toocontrol utgående nätverksanslutningen på hello VM, kan du kontrollera att hello nödvändiga datacenter IP-adressintervall är godkända. Mer information finns i [nätverk grupp säkerhetsvägledning](https://aka.ms/a2a-nsg-guidance).

### <a name="fix-hello-problem"></a>Åtgärda problemet hello
toowhitelist [hello krävs URL: er](site-recovery-azure-to-azure-networking-guidance.md#outbound-connectivity-for-azure-site-recovery-urls) eller hello [krävs för IP-adressintervall](site-recovery-azure-to-azure-networking-guidance.md#outbound-connectivity-for-azure-site-recovery-ip-ranges), gör hello i hello [nätverk riktlinjerna](site-recovery-azure-to-azure-networking-guidance.md).

## <a name="disk-not-found-in-hello-machine-error-code-150039"></a>Hittades inte i hello datorn (felkod 150039)

En ny disk kopplade toohello VM måste initieras.

**Felkod** | **Möjliga orsaker** | **Rekommendationer**
--- | --- | ---
150039<br></br>**Meddelandet**: Azure-datadisk (DiskName) (DiskURI) med logiska enhetsnummer (LUN) (LUNValue) har inte motsvarande mappade tooa disk som har rapporterats från inom hello VM som har hello samma LUN-värde. | -En ny datadisk var anslutna toohello VM, men det fanns inte initierats.</br></br>-hello datadisk inuti hello VM inte korrekt rapporterar hello LUN-värdet på vilka hello disken var anslutna toohello VM.| Kontrollera att hello datadiskar har initierats och försök sedan utföra hello igen.</br></br>För Windows: [Anslut och initiera en ny disk](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-portal#option-1-attach-and-initialize-a-new-disk).</br></br>För Linux: [initierar en ny datadisk i Linux](https://docs.microsoft.com/azure/virtual-machines/linux/classic/attach-disk#initialize-a-new-data-disk-in-linux).

### <a name="fix-hello-problem"></a>Åtgärda problemet hello
Se till att hello datadiskar har initierats och försök sedan utföra hello igen:

- För Windows: [Anslut och initiera en ny disk](https://docs.microsoft.com/azure/virtual-machines/windows/attach-disk-portal#option-1-attach-and-initialize-a-new-disk).
- För Linux: [initierar en ny datadisk i Linux](https://docs.microsoft.com/azure/virtual-machines/linux/classic/attach-disk#initialize-a-new-data-disk-in-linux).

Kontakta supporten om hello problemet kvarstår.


## <a name="unable-toosee-hello-azure-vm-for-selection-in-enable-replication"></a>Det går inte toosee hello Azure VM väljas i ”Aktivera replikering”

Du kanske inte se dina Azure-VM för val i [Aktivera replikering: steg 2](./site-recovery-azure-to-azure.md#step-2-select-virtual-machines). Det här problemet kan bero på att toostale Site Recovery-konfiguration på hello Azure VM. hello inaktuell konfiguration kan lämnas på en Azure VM i hello följande fall:

- Du aktiverat replikering för hello virtuella Azure-datorn med hjälp av Site Recovery och sedan bort hello Site Recovery-valvet utan att uttryckligen inaktivera replikering på hello VM.
- Du har aktiverat replikering för hello virtuella Azure-datorn med hjälp av Site Recovery och bort hello resursgruppen som innehåller hello Site Recovery-valvet utan att uttryckligen inaktivera replikering på hello VM.

### <a name="fix-hello-problem"></a>Åtgärda problemet hello

Du kan använda [ta bort inaktuella ASR konfigurationsskript](https://gallery.technet.microsoft.com/Azure-Recovery-ASR-script-3a93f412) och ta bort hello inaktuella Site Recovery-konfigurationen på hello Azure VM. Du bör se hello VM i [Aktivera replikering: steg 2](./site-recovery-azure-to-azure.md#step-2-select-virtual-machines) när du tar bort inaktuella hello-konfigurationen.


## <a name="next-steps"></a>Nästa steg
[Replikera virtuella Azure-datorer](site-recovery-replicate-azure-to-azure.md)
