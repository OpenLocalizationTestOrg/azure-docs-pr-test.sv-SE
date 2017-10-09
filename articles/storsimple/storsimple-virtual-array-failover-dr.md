---
title: aaaStorSimple virtuella matris disaster recovery och enheten redundans | Microsoft Docs
description: "Läs mer om hur toofailover din virtuella StorSimple-matris."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 3c1f9c62-af57-4634-a0d8-435522d969aa
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 5f125efd1ffb94489cdfa7cfaafae7d57cc10131
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="disaster-recovery-and-device-failover-for-your-storsimple-virtual-array-via-azure-portal"></a>Disaster recovery och enheten redundans för din virtuella StorSimple-matrisen via Azure-portalen

## <a name="overview"></a>Översikt
Den här artikeln beskriver hello katastrofåterställning för din Microsoft Azure StorSimple virtuell matris inklusive hello detaljerade steg toofail över tooanother virtuella matris. En redundansväxling kan du toomove dina data från en *källa* enhet i hello datacenter tooa *mål* enhet. hello målenhet kanske finns i hello samma eller en annan geografisk plats. hello enheten redundans är för hello hela enheten. Hello molndata för hello källenheten ändras under växling vid fel, ägarskap toothat av hello målenhet.

Den här artikeln är tillämpliga tooStorSimple virtuella matriser. toofail över en 8000-serieenhet, gå för[enheten redundans och disaster recovery av StorSimple-enheten](storsimple-device-failover-disaster-recovery.md).

## <a name="what-is-disaster-recovery-and-device-failover"></a>Vad är disaster recovery och enheten redundans?

I en (DR) katastrofåterställning, hello primära enhet slutar fungera. I det här scenariot kan du flytta hello molndata som är associerade med hello misslyckad enhet tooanother enhet. Du kan använda hello primära enhet som hello *källa* och ange en annan enhet som hello *mål*. Den här processen är refererad tooas hello *redundans*. Under växling vid fel, alla hello volymer eller hello resurser från hello källenheten ändra ägarskap och är överförda toohello målenhet. Ingen filtrering hello data är tillåten.

DR modelleras som en fullständig enhet återställning med hjälp av hello termiska karta-baserade skiktning och spårning. En termisk karta definieras genom att tilldela en termiska värdet toohello data baserat på läsa och skriva mönster. Den här termiska mappa sedan nivåer hello först lägsta termiska data segment toohello molnet samtidigt som hello hög värme (används mest) datasegment i hello lokala nivån. Under en Katastrofåterställning StorSimple använder hello termiska karta toorestore och rehydrate hello data från hello molnet. hello enheten hämtar alla hello volymer/resurser i hello senaste senaste säkerhetskopieringen (vilket anges internt) och utför en återställning från säkerhetskopian. hello virtuella matris samordnar hello hela DR-processen.

> [!IMPORTANT]
> hello källenheten raderas hello slutet av enheten redundans och därför en återställning stöds inte.
> 
> 

Katastrofåterställning är samordnade via funktionen för hello enheten växling vid fel och initieras från hello **enheter** bladet. Det här bladet tabulates alla hello StorSimple-enheter anslutna tooyour StorSimple Device Manager-tjänsten. Du kan se hello eget namn, status, etablerad och maximal kapacitet, typ och modell för varje enhet.

## <a name="prerequisites-for-device-failover"></a>Krav för växling vid fel för enheten

### <a name="prerequisites"></a>Krav

För en enhet växling, kontrollerar du att hello följande förutsättningar är uppfyllda:

* hello källenheten måste toobe i en **inaktiverad** tillstånd.
* hello målenhet måste tooshow upp som **klar tooset in** i hello Azure-portalen. Etablera en virtuell Målmatrisen av hello samma eller högre kapacitet. Använd hello lokala web UI tooconfigure och registreras hello virtuella Målmatrisen.
  
  > [!IMPORTANT]
  > Försök inte tooconfigure hello registrerade virtuella enheten via hello-tjänsten. Ingen enhetskonfiguration ska utföras via hello-tjänsten.
  > 
  > 
* hello målenhet kan inte ha samma namn som hello källenheten hello.
* hello käll- och enheten har toobe hello samma typ. Du kan bara redundansväxla virtuella matriskonfiguration som en fil tooanother filserver. hello sak samma gäller för en iSCSI-server.
* För en filserver DR rekommenderar vi att ansluta till hello mål enheten toohello samma domän som hello källa. Den här konfigurationen garanterar att hello resursbehörigheter löses automatiskt. Endast hello redundans tooa målenhet i hello samma domän.
* hello tillgängliga målenheterna för Katastrofåterställning är enheter som har hello samma eller större kapacitet jämfört med toohello källenheten. hello enheter som är anslutna tooyour tjänsten men inte uppfyller hello kriterier tillräckligt utrymme är inte tillgängliga som målenheter.

### <a name="other-considerations"></a>Andra överväganden

* För en planerad redundans 
  
  * Vi rekommenderar att du vidtar alla hello volymer eller resurser på hello källenheten offline.
  * Vi rekommenderar att du gör en säkerhetskopia av hello enhet och gå sedan vidare med hello redundans toominimize dataförlust. 
* För en oplanerad redundans använder hello enhet hello senaste säkerhetskopiering toorestore hello data.

### <a name="device-failover-prechecks"></a>Enheten redundans prechecks

Innan hello DR börjar, utför hello enheten prechecks. Dessa kontroller att säkerställa att inga fel inträffar när DR påbörjas. Hej prechecks omfattar:

* Verifiera hello storage-konto.
* Kontrollerar hello molnet anslutning tooAzure.
* Kontrollerar diskutrymme på hello målenhet.
* Kontrollerar om en iSCSI-server enheten källvolymen har
  
  * giltiga ACR-namn.
  * giltig IQN (högst 220 tecken).
  * giltig CHAP-lösenord (12-16 tecken lång).

Du kan inte fortsätta med hello DR om någon av hello föregående prechecks misslyckas. Lösa dessa problem och försök sedan DR.

När hello DR har slutförts är hello ägarskap för hello molndata på hello källenheten överförda toohello målenhet. hello källenheten är inte längre tillgänglig i portalen hello sedan. Åtkomst tooall hello volymer/resurser på hello källenheten blockeras och hello målenhet blir aktiv.

> [!IMPORTANT]
> Även om hello enheten inte är längre tillgänglig, förbrukar hello virtuell dator som du har etablerat hello värdsystemet fortfarande resurser. När hello DR har slutförts kan du ta bort den här virtuella datorn från värdsystemet.
> 
> 

## <a name="fail-over-tooa-virtual-array"></a>Växla över tooa virtuella matris

Vi rekommenderar att etablera, konfigurera och registrera en annan virtuell StorSimple-matris med din StorSimple Device Manager-tjänsten innan du kör den här proceduren.

> [!IMPORTANT]
> 
> * Du kan inte växla över från en StorSimple 8000-serien enheten tooa 1200 virtuell enhet.
> * Du kan växla över från en FIPS Federal Information Processing Standard () aktiverade virtuella enheten tooanother FIPS-aktiverad enhet eller tooa inte FIPS-enhet som distribuerats i hello Government portal.


Utföra hello följande steg toorestore hello enheten tooa mål virtuella StorSimple-enheten.

1. Etablera och konfigurera en målenhet som uppfyller hello [krav för växling vid fel för enheten](#prerequisites). Slutföra hello enhetskonfiguration via hello lokala webbgränssnittet och registrera den tooyour StorSimple enheten Manager-tjänsten. Om du skapar en filserver, gå toostep 1 av [som filserver](storsimple-virtual-array-deploy3-fs-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device). Om du skapar en iSCSI-server går toostep 1 av [konfigurerats som server för iSCSI-](storsimple-virtual-array-deploy3-iscsi-setup.md#step-1-complete-the-local-web-ui-setup-and-register-your-device).

2. Ta volymer/resurser offline på hello värden. tootake hello volymer/resurser offline finns toohello operativsystem – specifika anvisningar för hello värden. Om inte redan offline, måste tootake alla hello volymer/resurser offline på hello enheten hello följande.
   
    1. Gå för**enheter** bladet och välj din enhet.
   
    2. Gå för**Inställningar > Hantera > resurser** (eller **Inställningar > Hantera > volymer**). 
   
    3. Välj en resurs/volym, högerklicka och välj **ta offline**. 
   
    4. När du uppmanas att bekräfta att kontrollera **jag förstår hello effekten av att den här resursen tas offline.** 
   
    5. Klicka på **ta offline**.

3. I Enhetshanteraren för StorSimple-tjänsten går för**Management > enheter**. I hello **enheter** bladet Välj och klicka på din källenhet.

4. I din **enheten instrumentpanelen** bladet, klickar du på **inaktivera**.

5. I hello **inaktivera** bladet du uppmanas att bekräfta. Inaktivering av enheten är en *permanent* process som inte kan ångras. Du är också en påminnelse om tootake dina resurser/volymer offline på hello värden. Ange hello enheten namnet tooconfirm och klicka på **inaktivera**.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover1.png)
6. inaktivering av hello startar. Du får ett meddelande när hello inaktiveringen har slutförts.
   
    ![](./media/storsimple-virtual-array-failover-dr/failover2.png)
7. Hello enheter på sidan hello enhetens tillstånd kommer nu att ändras för**inaktiverad**.
    ![](./media/storsimple-virtual-array-failover-dr/failover3.png)
8. I hello **enheter** bladet, väljer och klickar på hello inaktiverade källenheten för redundans. 
9. I hello **enheten instrumentpanelen** bladet, klickar du på **växla över**. 
10. I hello **växla över enhet** bladet hello följande:
    
    1. hello källa enhetens fält fylls i automatiskt. Observera hello totala datastorleken för hello källenheten. hello Datastorleken måste vara mindre än hello tillgänglig kapacitet på hello målenhet. Granska hello information som är associerade med hello källenheten som enhetsnamn, total kapacitet och hello namnen på hello-resurser som har redundansväxlats.

    2. Hello nedrullningsbara listan över tillgängliga enheter, väljer en **målenhet**. Endast hello-enheter som har tillräckligt med kapacitet visas i listrutan för hello.

    3. Kontrollera att **jag förstår att den här åtgärden växlar över data toohello målenhet**. 

    4. Klicka på **växla över**.
    
        ![](./media/storsimple-virtual-array-failover-dr/failover4.png)
11. Initierar ett jobb för växling vid fel och du får ett meddelande. Gå för**enheter > jobb** toomonitor hello redundans.
    
     ![](./media/storsimple-virtual-array-failover-dr/failover5.png)
12. I hello **jobb** bladet du ser ett failover-jobb skapas för hello källenheten. Det här jobbet utför hello DR prechecks.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover6.png)
    
     När hello DR prechecks är lyckas, kommer hello beställningsjobbet starta återställningsjobb för varje resurs/volym som finns på källenheten.
    
    ![](./media/storsimple-virtual-array-failover-dr/failover7.png)
13. När hello redundansväxlingen är klar, går toohello **enheter** bladet.
    
    1. Välj och klicka på hello StorSimple-enhet som användes som hello målenhet för hello failover-processen.
    2. Gå för**Inställningar > Management > resurser** (eller **volymer** om iSCSI-server). I hello **resurser** bladet du kan visa alla hello resurser (volymer) från hello gamla enhet.
        ![](./media/storsimple-virtual-array-failover-dr/failover9.png)
14. Du behöver för[skapa en DNS-alias](https://support.microsoft.com/kb/168322) så att alla hello program som försöker tooconnect får omdirigerade toohello ny enhet.

## <a name="errors-during-dr"></a>Fel uppstod vid Katastrofåterställning

**Molnet anslutningen avbrott under Katastrofåterställning**

Om hello molnet anslutningen avbryts efter DR har startat och innan hello enheten återställningen är slutförd, hello DR misslyckas. Du får ett meddelande om failore. hello målenhet för Katastrofåterställning är markerad som *inte kan användas.* Du kan inte använda hello samma målenhet för framtida DRs.

**Ingen kompatibel målenheter**

Om hello tillgängliga målenheterna inte har tillräckligt med utrymme, ser du att det finns ingen kompatibel målenheterna fel toohello effekt.

**Precheck fel**

Om en av hello prechecks inte är uppfyllt, se precheck fel.

## <a name="business-continuity-disaster-recovery-bcdr"></a>Katastrofåterställning för verksamhetskontinuitet (BCDR)

En business continuity (BCDR) katastrofåterställning inträffar när hello hela Azure-datacenter slutar att fungera. Detta kan påverka din StorSimple Device Manager-tjänst och hello associerad StorSimple-enheter.

Om StorSimple-enheter som registrerats precis innan en katastrof inträffade måste dessa StorSimple-enheter toobe tas bort. Du kan återskapa och konfigurera enheterna efter hello katastrofåterställning.

## <a name="next-steps"></a>Nästa steg

Läs mer om hur för[administrera din virtuella StorSimple-matris med hello lokala webbgränssnittet](storsimple-ova-web-ui-admin.md).

