---
title: "aaaConfigure CHAP för StorSimple 8000-serieenhet | Microsoft Docs"
description: "Beskriver hur tooconfigure hello CHAP Challenge Handshake Authentication Protocol () på en StorSimple-enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 467044d7-7885-4382-90bd-3148dbbd341f
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: 272ef2c184f56ad262e55410357494c72e45cf83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-chap-for-your-storsimple-device"></a>Konfigurera CHAP för din StorSimple-enhet
Den här självstudiekursen beskrivs hur tooconfigure CHAP för din StorSimple-enhet. hello proceduren som beskrivs i den här artikeln gäller tooStorSimple 8000-serien samt 1200 StorSimple-enheter.

CHAP står för Challenge Handshake Authentication Protocol. Det är ett autentiseringsschema som används av servrar toovalidate hello identiteten för fjärrklienter. hello verifiering baseras på en delad lösenord eller hemlighet. CHAP kan vara enkelriktade (enkelriktad) eller ömsesidig (dubbelriktad). Enkelriktade CHAP är när hello mål autentiserar en initierare. Ömsesidig eller omvänd CHAP hello däremot måste hello mål autentisera hello initieraren och sedan hello initieraren autentisera hello mål. Initieraren autentisering kan genomföras utan target autentisering. Mål-autentisering kan dock implementeras endast om initieraren authentication implementeras också. 

Som bästa praxis rekommenderar vi att du använder CHAP tooenhance iSCSI-säkerhet.

> [!NOTE]
> Tänk på att IPSEC inte stöds för närvarande på StorSimple-enheter.
> 
> 

hello CHAP-inställningar på hello StorSimple-enhet kan konfigureras i hello följande sätt:

* Enkelriktade eller envägs-autentisering
* Dubbelriktad eller ömsesidig eller omvänd autentisering

I dessa fall måste hello portal hello enhet och hello serverprogramvara iSCSI-initieraren toobe konfigurerats. hello beskrivs detaljerade anvisningar för den här konfigurationen i hello följa kursen.

## <a name="unidirectional-or-one-way-authentication"></a>Enkelriktade eller envägs-autentisering
I enkelriktad autentisering autentiserar hello målet hello initieraren. Den här autentiseringen måste du konfigurera inställningar för hello CHAP-initieraren på hello StorSimple-enhet och hello iSCSI-initieraren programvara på hello värden. Hej detaljerade anvisningar för din StorSimple-enhet och Windows-värd beskrivs nedan.

#### <a name="tooconfigure-your-device-for-one-way-authentication"></a>tooconfigure din enhet för enkelriktad autentisering
1. I hello klassiska Azure-portalen på hello **enheter** klickar du på hello **konfigurera** fliken.
   
    ![CHAP-initieraren](./media/storsimple-configure-chap/IC740943.png)
2. Rulla ned på den här sidan och i hello **CHAP-initieraren** avsnitt:
   
   1. Ange ett användarnamn för CHAP-initieraren.
   2. Ange ett lösenord för CHAP-initieraren.
      
    > [!IMPORTANT]
    > hello CHAP-användarnamn måste innehålla färre än 233 tecken. hello CHAP-lösenord måste vara mellan 12 och 16 tecken. Ett längre användarnamn eller lösenord resulterar i ett autentiseringsfel på hello Windows-värd.
   
   3. Bekräfta hello lösenord.
3. Klicka på **Spara**. Att kommer visas ett bekräftelsemeddelande. Klicka på **OK** toosave hello ändringar.

#### <a name="tooconfigure-one-way-authentication-on-hello-windows-host-server"></a>tooconfigure enkelriktad autentisering på Windows hello värdservern
1. Starta hello iSCSI-initieraren på värdservern för hello Windows.
2. I hello **iSCSI-Initieraregenskaper** fönstret utföra hello följande steg:
   
   1. Klicka på hello **identifiering** fliken.
      
       ![iSCSI-initieraregenskaper](./media/storsimple-configure-chap/IC740944.png)
   2. Klicka på **identifiera Portal**.
3. I hello **identifiera målportal** dialogrutan:
   
   1. Ange hello IP-adressen för din enhet.
   2. Klicka på **avancerade**.
      
       ![Identifiera målportal](./media/storsimple-configure-chap/IC740945.png)
4. I hello **avancerade inställningar** dialogrutan:
   
   1. Välj hello **aktivera CHAP inloggning** kryssrutan.
   2. I hello **namn** fält, ange hello användarnamnet som angetts för hello CHAP-initieraren i hello klassiska portalen.
   3. I hello **mål hemlighet** fält, ange hello lösenordet du angav för hello CHAP-initieraren i hello klassiska portalen.
   4. Klicka på **OK**.
      
       ![Avancerade inställningar som är allmänt](./media/storsimple-configure-chap/IC740946.png)
5. På hello **mål** för hello **iSCSI-Initieraregenskaper** fönstret hello enhetens status borde visas som **ansluten**. Om du använder en enhet med StorSimple 1200 sedan monteras varje volym som en iSCSI-mål som visas nedan. Steg 3 – 4 måste därför toobe upprepas för varje volym.
   
    ![Volymerna monterade som separata mål](./media/storsimple-configure-chap/chap4.png)
   
   > [!IMPORTANT]
   > Om du ändrar hello iSCSI-namn används hello nytt namn för den nya iSCSI-sessioner. Nya inställningar används inte för befintliga sessioner förrän du logga ut och logga in igen.
   > 
   > 

Mer information om hur du konfigurerar CHAP på värdservern med Windows hello gå för[ytterligare överväganden](#additional-considerations).

## <a name="bidirectional-or-mutual-authentication"></a>Dubbelriktad eller ömsesidig autentisering
I dubbelriktad autentisering hello mål autentiserar hello initieraren och sedan hello initieraren hello mål. Detta kräver hello tooconfigure hello CHAP-initieraren användarinställningar, samt hello omvänd CHAP-inställningar på enheten hello och iSCSI-initieraren programvara på hello värden. hello följande procedurer beskrivs hello steg tooconfigure ömsesidig autentisering på hello enheten och på Windows hello-värd.

#### <a name="tooconfigure-your-device-for-mutual-authentication"></a>tooconfigure enheten för ömsesidig autentisering
1. I hello klassiska Azure-portalen på hello **enheter** klickar du på hello **konfigurera** fliken.
   
    ![CHAP-målet](./media/storsimple-configure-chap/IC740948.png)
2. Rulla ned på den här sidan och i hello **CHAP Target** avsnitt:
   
   1. Ange en **omvänd CHAP-användarnamn** för din enhet.
   2. Ange en **omvänd CHAP-lösenord** för din enhet.
   3. Bekräfta hello lösenord.
3. I hello **CHAP-initieraren** avsnitt:
   
   1. Ange en **användarnamn** för din enhet.
   2. Ange en **lösenord** för din enhet.
   3. Bekräfta hello lösenord.
4. Klicka på **Spara**. Att kommer visas ett bekräftelsemeddelande. Klicka på **OK** toosave hello ändringar.

#### <a name="tooconfigure-bidirectional-authentication-on-hello-windows-host-server"></a>tooconfigure dubbelriktad autentisering på Windows hello värdservern
1. Starta hello iSCSI-initieraren på värdservern för hello Windows.
2. I hello **iSCSI-Initieraregenskaper** -fönstret klickar du på hello **Configuration** fliken.
3. Klicka på **CHAP**.
4. I hello **iSCSI Initiator ömsesidig CHAP Secret** dialogrutan:
   
   1. Typen hello **omvänd CHAP-lösenord** som du konfigurerade i hello klassiska Azure-portalen.
   2. Klicka på **OK**.
      
       ![iSCSI-initieraren ömsesidig CHAP-hemlighet](./media/storsimple-configure-chap/IC740949.png)
5. Klicka på hello **mål** fliken.
6. Klicka på hello **Anslut** knappen. 
7. I hello **ansluta tooTarget** dialogrutan klickar du på **Avancerat**.
8. I hello **avancerade egenskaper** dialogrutan:
   
   1. Välj hello **aktivera CHAP inloggning** kryssrutan.
   2. I hello **namn** fält, ange hello användarnamnet som angetts för hello CHAP-initieraren i hello klassiska portalen.
   3. I hello **mål hemlighet** fält, ange hello lösenordet du angav för hello CHAP-initieraren i hello klassiska portalen.
   4. Välj hello **utför ömsesidig autentisering** kryssrutan.
      
       ![Avancerade inställningar ömsesidig autentisering](./media/storsimple-configure-chap/IC740950.png)
   5. Klicka på **OK** toocomplete hello CHAP-konfiguration

Mer information om hur du konfigurerar CHAP på värdservern med Windows hello gå för[ytterligare överväganden](#additional-considerations).

## <a name="additional-considerations"></a>Annat som är bra att tänka på
Hej **Snabbanslut** funktionen har inte stöd för anslutningar som har aktiverat CHAP. När CHAP är aktiverat, kontrollera att du använder hello **Anslut** knapp som finns på hello **mål** fliken tooconnect tooa mål.

![Ansluta tootarget](./media/storsimple-configure-chap/IC740947.png)

I hello **ansluta tooTarget** dialogruta som presenteras, Välj hello **lägga till den här anslutningen toohello lista över favoritmål** kryssrutan. Detta säkerställer att varje gång hello datorn startas om ett försök görs toorestore hello anslutning toohello iSCSI-favorit mål.

## <a name="errors-during-configuration"></a>Fel under konfigurationen
Om CHAP-konfigurationen är felaktig så kan du förmodligen toosee en **autentiseringsfel** felmeddelande.

## <a name="verification-of-chap-configuration"></a>Verifiering av CHAP-konfiguration
Du kan verifiera att CHAP används genom att slutföra hello följande steg.

#### <a name="tooverify-your-chap-configuration"></a>tooverify CHAP-konfiguration
1. Klicka på **favorit mål**.
2. Välj hello målobjekt som du har aktiverat autentisering.
3. Klicka på **information**.
   
    ![iSCSI-initieraren egenskaper favorit mål](./media/storsimple-configure-chap/IC740951.png)
4. I hello **information om favorit mål** dialogrutan hello kommentar i hello **autentisering** fältet. Om hello konfigurationen lyckades, ska det stå **CHAP**.
   
    ![Information om favorit mål](./media/storsimple-configure-chap/IC740952.png)

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [StorSimple-säkerhet](storsimple-security.md).
* Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

