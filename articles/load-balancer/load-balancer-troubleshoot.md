---
title: "aaaTroubleshoot Azure belastningsutjämnare | Microsoft Docs"
description: "Felsöka kända problem med Azure belastningsutjämnare"
services: load-balancer
documentationcenter: na
author: RamanDhillon
manager: timlt
editor: 
ms.assetid: 
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/10/2017
ms.author: kumud
ms.openlocfilehash: 56b73fcbf0bbf18cedfd113a280cfafa2a3dc9f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-azure-load-balancer"></a>Felsöka Azure belastningsutjämnare

Den här sidan innehåller felsökningsinformation för frågor till Azure belastningsutjämnare. När hello belastningsutjämnaren anslutning inte är tillgänglig är hello vanligaste symptomen följande: 
- Virtuella datorer bakom belastningsutjämnaren inte svarar toohealth avsökningar hello 
- Virtuella datorer bakom hello belastningsutjämnaren svarar inte toohello trafik på port för hello konfigurerad

## <a name="symptom-vms-behind-hello-load-balancer-are-not-responding-toohealth-probes"></a>Symptom: Virtuella datorer bakom belastningsutjämnaren inte svarar toohealth avsökningar hello
Hello backend-servrar tooparticipate i hello belastningen belastningsutjämnaren uppsättning klarar de hello avsökningen kontroll. Läs mer om hälsoavsökningar [förstå belastningen belastningsutjämnaren avsökningar](load-balancer-custom-probe-overview.md). 

hello belastningsutjämnaren serverdelspool virtuella datorer kanske inte svarar toohello avsökningar förfaller tooany av hello följande orsaker: 
- Belastningsutjämnarens serverdelspool VM är felfri 
- Läsa in serverdelspool VM inte lyssnar på hello avsökningsport 
- Brandvägg eller en nätverkssäkerhetsgrupp blockerar hello port på hello belastningsutjämnaren serverdelspool virtuella datorer 
- Andra felaktig konfiguration av belastningsutjämnare

### <a name="cause-1-load-balancer-backend-pool-vm-is-unhealthy"></a>Orsak 1: Belastningsutjämnarens serverdelspool VM är felfri 

**Validering och lösning**

tooresolve problemet, logga in toohello deltar virtuella datorer och kontrollera om hello tillstånd för virtuell dator är felfri, och kan svara för**PsPing** eller **TCPing** från en annan virtuell dator i hello pool. Om hello VM är skadad eller är toorespond toohello avsökningen, måste du åtgärda problemet hello och få hello VM tillbaka tooa felfritt tillstånd innan den kan delta i belastningsutjämning.

### <a name="cause-2-load-balancer-backend-pool-vm-is-not-listening-on-hello-probe-port"></a>Orsak 2: Belastningsutjämnarens serverdelspool VM inte lyssnar på hello avsökningsport
Om hello VM är felfri, men svarar inte toohello avsökning, kan en möjlig orsak vara att hello avsökningen porten är inte öppen på hello deltar VM eller hello VM inte lyssnar på porten.

**Validering och lösning**

1. Logga in toohello serverdelens VM. 
2. Öppna en kommandotolk och kör följande kommando toovalidate det finns ett program som lyssnar på hello avsökningsport hello:   
            netstat - ett
3. Om inte hello port status visas som **lyssna**, konfigurera hello rätt port. 
4. Du kan också välja en annan port som anges som **lyssna**, och därefter ladda konfigurationen av belastningsutjämnaren.              

###<a name="cause-3-firewall-or-a-network-security-group-is-blocking-hello-port-on-hello-load-balancer-backend-pool-vms"></a>Orsak 3: Brandvägg eller en nätverkssäkerhetsgrupp blockerar hello port på hello belastningsutjämnarens serverdelspool virtuella datorer  
Om hello-brandväggen på hello VM blockerar hello avsökningsport eller en eller flera nätverkssäkerhetsgrupper konfigureras i hello undernät eller på hello VM, inte tillåter hello avsökningsport tooreach hello, är toorespond toohello hälsoavsökningen hello VM.          

**Validering och lösning**

* Om hello brandvägg är aktiverad, kontrollera om den är konfigurerad tooallow hello avsökningsport. Om inte, konfigurera hello brandväggen tooallow trafik på hello avsökningsport och testa igen. 
* Kontrollera om hello inkommande eller utgående trafik på port för avsökning av hello har störningar hello listan över nätverkssäkerhetsgrupper och. 
* Kontrollera också om en **neka alla** nätverkssäkerhetsgrupper-regel för hello nätverkskort på hello VM eller hello undernät som har högre prioritet än hello Standardregeln som tillåter LB avsökningar & trafik (nätverkssäkerhetsgrupper måste tillåta Load Balancer IP 168.63.129.16). 
* Om någon av de här reglerna blockerar hello avsökningen trafik, ta bort och konfigurera om hello regler tooallow hello avsökningen trafik.  
* Testa om hello VM nu har börjat svarar toohello hälsoavsökning. 

### <a name="cause-4-other-misconfigurations-in-load-balancer"></a>Orsak 4: Andra felaktig konfiguration av belastningsutjämnare
Om alla hello verka föregående orsaker toobe valideras och matcha korrekt och hello serverdelens VM fortfarande inte svara toohello hälsoavsökningen och sedan manuellt för anslutningstestet och samla in vissa spårningar toounderstand hello-anslutning.

**Validering och lösning**

* Använd **Psping** från en hello hello andra virtuella datorer inom VNet tootest hello avsökningen port svar (exempel:.\psping.exe -t 10.0.0.4:3389) och registrerar resultaten. 
* Använd **TCPing** från en hello hello andra virtuella datorer inom VNet tootest hello avsökningen port svar (exempel:.\tcping.exe 10.0.0.4 3389) och registrerar resultaten. 
* Om inget svar tas emot i dessa tester ping sedan
    - Kör en samtidig Netsh trace på hello mål serverdelspool VM och en annan test VM från hello samma virtuella nätverk. Nu kör ett test för PsPing under en viss tid, samla in vissa nätverksspår och sedan stoppa hello testet. 
    - Analysera hello nätverksbild och se om det finns både inkommande och utgående paket relaterade toohello ping-fråga. 
        - Om inga inkommande paket observeras på hello serverdelspool VM finns eventuellt en nätverkssäkerhetsgrupper eller UDR felkonfigurerad blockerande hello trafik. 
        - Om inga utgående paket observeras på hello serverdelspool VM måste hello VM toobe sökning efter orelaterade problem (för eample program blockerande hello avsökningsport). 
    - Kontrollera om avsökningen hälsningspaket som framtvingad tooanother mål (eventuellt via UDR inställningar) innan det nådde hello belastningsutjämnaren. Detta kan orsaka hello trafik toonever reach hello serverdelens VM. 
* Ändra hello avsökningen typ (till exempel HTTP tooTCP) och konfigurera hello motsvarande port i nätverkssäkerhetsgrupper ACL: er och brandväggen toovalidate om hello problemet med hello konfiguration av avsökningen svar. Mer information om hälsa avsökningen konfiguration finns [Endpoint belastningsutjämning avsökningen hälsokonfiguration](https://blogs.msdn.microsoft.com/mast/2016/01/26/endpoint-load-balancing-heath-probe-configuration-details/).

## <a name="symptom-vms-behind-load-balancer-are-not-responding-tootraffic-on-hello-configured-data-port"></a>Symptom: Virtuella datorer bakom belastningsutjämnaren svarar inte tootraffic på hello konfigurerade dataport

Om en serverdelspool VM visas som felfri och svarar toohello hälsoavsökningar, men ändå inte deltar i hello belastningsutjämning eller svarar inte toohello trafik, kanske på grund av tooany av hello följande orsaker: 
* Läsa in Belastningsutjämnarens serverdelspool VM inte lyssnar på porten för hello-data 
* Nätverkssäkerhetsgruppen blockerar hello port på hello belastningsutjämnaren serverdelspool VM  
* Åtkomst till hello hello belastningsutjämnaren från samma virtuella dator och NIC 
* Åtkomst till hello Internet VIP för belastningsutjämnare från hello deltar belastningsutjämnaren serverdelspool VM 

### <a name="cause-1-load-balancer-backend-pool-vm-is-not-listening-on-hello-data-port"></a>Orsak 1: Belastningsutjämnarens serverdelspool VM inte lyssnar på porten för hello-data 
Om en virtuell dator inte svarar toohello datatrafik kan det bero på att antingen hälsning målporten är inte öppen hello deltar VM eller hello VM inte lyssnar på porten. 

**Validering och lösning**

1. Logga in toohello serverdelens VM. 
2. Öppna en kommandotolk och kör hello efter kommandot toovalidate det finns ett program som lyssnar på porten för hello-data:  
            netstat - ett 
3. Om inte hello port visas med tillståndet ”lyssnar” konfigurera hello rätt lyssningsport 
4. Om hello port har markerats som lyssna, kontrollerar du hello målprogrammet på porten för möjliga problem. 

### <a name="cause-2-network-security-group-is-blocking-hello-port-on-hello-load-balancer-backend-pool-vm"></a>Orsak 2: Nätverkssäkerhetsgruppen blockerar hello port på hello belastningsutjämnaren serverdelspool VM  

Om en eller flera nätverk säkerhetsgrupperna som konfigureras på hello undernät eller hello VM, blockerar hello käll-IP och port, och sedan hello VM är toorespond.

* Lista över hello nätverkssäkerhetsgrupper som konfigurerats på hello serverdelens VM. Mer information finns i:
    -  [Hantera nätverkssäkerhetsgrupper med hello Portal](../virtual-network/virtual-network-manage-nsg-arm-portal.md)
    -  [Hantera nätverkssäkerhetsgrupper med hjälp av PowerShell](../virtual-network/virtual-network-manage-nsg-arm-ps.md)
* Kontrollera om hello listan över nätverkssäkerhetsgrupper och:
    - hello inkommande eller utgående trafik på hello-dataporten har störningar. 
    - en **neka alla** grupp nätverkssäkerhetsregeln på hello NIC av hello VM eller hello undernät som har högre prioritet som hello Standardregeln som tillåter avsökningar belastningsutjämnaren och trafik (nätverkssäkerhetsgrupper måste tillåta Load Balancer IP 168.63.129.16 som är avsökningsport) 
* Om någon av reglerna för hello blockerar hello trafik, ta bort och konfigurera om dessa regler tooallow hello trafik.  
* Testa om hello VM nu har börjat toorespond toohello hälsoavsökningar.

### <a name="cause-3-accessing-hello-load-balancer-from-hello-same-vm-and-network-interface"></a>Orsak 3: Åtkomst till hello belastningsutjämnaren från hello samma VM och ett nätverksgränssnitt 

Om ditt program finns i hello serverdelens VM av en belastningsutjämnare försöker tooaccess ett annat program som finns i hello samma backend VM över hello samma Network Interface, det är ett scenario som stöds inte och kommer att misslyckas. 

**Lösning** du kan lösa det här problemet via en hello följande metoder:
* Konfigurera separata serverdelspool virtuella datorer per program. 
* Konfigurera programmet hello i virtuella datorer med dubbla nätverkskort så att varje program med sin egen gränssnitt och IP-adress. 

### <a name="cause-4-accessing-hello-internal-load-balancer-vip-from-hello-participating-load-balancer-backend-pool-vm"></a>Orsak 4: Komma åt hello interna VIP för belastningsutjämnare från hello deltar belastningsutjämnaren serverdelspool VM

Om en ILB VIP har konfigurerats i ett virtuellt nätverk och en hello deltagare backend VMs försöker hello tooaccess interna VIP för belastningsutjämnare, som resulterar i fel. Det här är ett scenario som inte stöds.
**Lösning** utvärdera Programgateway eller andra proxyservrar (till exempel nginx eller haproxy) toosupport som kind scenario. Mer information om Application Gateway finns [översikt för Programgateway](../application-gateway/application-gateway-introduction.md)

## <a name="additional-network-captures"></a>Ytterligare nätverksinsamlingar
Samla in följande information för en snabbare lösning hello om du väljer tooopen ett supportärende. Välj en enda backend VM tooperform hello följande test:
- Använd Psping från en hello backend virtuella datorer i hello VNet tootest hello avsökningen port svar (exempel: psping 10.0.0.4:3389) och registrerar resultaten. 
- Använda TCPing från en hello backend virtuella datorer i hello VNet tootest hello avsökningen port svar (exempel: psping 10.0.0.4:3389) och registrerar resultaten.
- Om inget svar tas emot i dessa tester ping spårningar samtidiga Netsh hello serverdelens VM och hello VNet test VM när du kör PsPing och sedan stoppa hello Netsh-spårning. 
  
## <a name="next-steps"></a>Nästa steg

Om hello föregående stegen inte löser problemet hello kan öppna en [stöder biljett](https://azure.microsoft.com/support/options/).

