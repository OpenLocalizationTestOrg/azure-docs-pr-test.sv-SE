---
title: aaaTroubleshoot en distribuerad virtuell StorSimple-enhet | Microsoft Docs
description: "Beskriver hur toodiagnose och åtgärda fel som uppstår på en StorSimple-enhet som är för tillfället distribuerade och fungerar."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ea5d89ae-e379-423f-b68b-53785941d9d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/16/2016
ms.author: v-sharos
ms.openlocfilehash: b48433055e05e3fb27575b88dca9f6b23c649ca2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-an-operational-storsimple-device"></a>Felsöka en operativa StorSimple-enhet
## <a name="overview"></a>Översikt
Den här artikeln innehåller användbar felsökningsinformation för att lösa konfigurationsproblem som kan uppstå när din StorSimple-enhet är distribuerade och fungerar. Här beskrivs vanliga problem och möjliga orsaker rekommenderade steg toohelp du löser problem som kan uppstå när du kör Microsoft Azure StorSimple. Den här informationen gäller tooboth hello lokala fysiska StorSimple-enheten och hello virtuella StorSimple-enheten.

Hello slutet av den här artikeln som du hittar en lista över felkoder som kan uppstå under Microsoft Azure StorSimple-åtgärden, samt steg kan du vidta tooresolve hello fel. 

## <a name="setup-wizard-process-for-operational-devices"></a>Installationsguiden för enheter
Du kan använda installationsguiden för hello ([Invoke-HcsSetupWizard][1]) toocheck hello enhetskonfiguration och vidta åtgärder om det behövs.

När du kör installationsguiden för hello på en enhet som tidigare konfigurerade och fungerar är hello processflöde olika. Du kan ändra endast hello följande poster:

* IP-adress, nätmask och gateway
* Primär DNS-server
* Primär NTP-server
* Valfritt webbproxykonfigurationen

hello installationsguiden inte att utföra hello operationer relaterade toopassword insamling och registrering av enheten.

## <a name="errors-that-occur-during-subsequent-runs-of-hello-setup-wizard"></a>Fel som uppstår under efterföljande körningar av hello installationsguiden
hello i den följande tabellen beskrivs hello-fel som kan uppstå när du kör installationsguiden för hello på en operativa enhet, möjliga orsaker hello fel och rekommenderade åtgärder tooresolve dem. 

| Nej. | Felmeddelande eller villkor | Möjliga orsaker | Rekommenderad åtgärd |
|:--- |:--- |:--- |:--- |
| 1 |Fel 350032: Den här enheten har redan inaktiverats. |Det här felet visas om du kör installationsguiden för hello på en enhet som har inaktiverats. |[Kontakta Microsoft Support](storsimple-contact-microsoft-support.md) för nästa steg. En inaktiverad enhet kan inte placeras i tjänsten. En fabriksåterställning kan krävas innan hello enheten kan aktiveras igen. |
| 2 |Anropa HcsSetupWizard: ERROR_INVALID_FUNCTION (undantag från HRESULT: 0x80070001) |hello DNS-serveruppdatering misslyckas. DNS-inställningarna är globala inställningar som tillämpas på alla hello aktiverade nätverksgränssnitt. |Aktivera hello-gränssnittet och tillämpa hello DNS-inställningarna igen. Eftersom de här inställningarna är globala kan det störa hello nätverk för andra aktiverade gränssnitt. |
| 3 |hello-enhet visas toobe online i hello StorSimple Manager-tjänstportalen, men när du installationsprogrammet toocomplete hello minsta och spara konfigurationen hello hello åtgärden misslyckas. |Under installationen, har hello webbproxy inte konfigurerats, även om det har en verklig proxyserver. |Använd hello [Test-HcsmConnection cmdlet] [ 2] toolocate hello fel. [Kontakta Microsoft Support](storsimple-contact-microsoft-support.md) om du toocorrect hello problem. |
| 4 |Anropa HcsSetupWizard: Värdet hamnar inte inom intervallet för hello förväntades. |En felaktig nätmask genererar det här felet. Möjliga orsaker är: <ul><li> hello nätmask är tomt eller saknas.</li><li>hello IPv6-prefix format är felaktigt.</li><li>hello-gränssnittet är moln-aktiverat, men hello gateway är saknas eller är felaktig.</li></ul>Observera att DATA 0 är automatiskt moln-aktiverat om konfigureras via hello installationsguiden. |toodetermine hello problem, använda undernätet 0.0.0.0 eller 256.256.256.256 och titta på hello utdata. Ange rätt värden för hello nätmask och gateway Ipv6-prefix efter behov. |

## <a name="error-codes"></a>Felkoder
Fel visas i numerisk ordning.

| Felnummer | Feltext eller beskrivning | Rekommenderade användaråtgärd |
|:--- |:--- |:--- |
| 10502 |Ett fel uppstod vid åtkomst till ditt lagringskonto. |Vänta några minuter och försök sedan igen. Om hello problemet kvarstår bör du kontakta Microsoft Support för nästa steg. |
| 40017 |hello säkerhetskopieringen misslyckades eftersom en volym som angetts i hello säkerhetskopieringsprincip inte hittades på hello enhet. |Försök hello säkerhetskopieringen, om hello felet kvarstår, kontakta Microsoft Support. Nästa steg. |
| 40018 |hello säkerhetskopieringen misslyckades eftersom ingen av hello volymer som angetts i hello säkerhetskopieringsprincip hittades på hello enhet. |Försök hello säkerhetskopieringen, om hello felet kvarstår, kontakta Microsoft Support. Nästa steg. |
| 390061 |hello systemet är upptaget eller inte tillgänglig. |Vänta några minuter och försök sedan igen. Om hello problemet kvarstår bör du kontakta Microsoft Support för nästa steg. |
| 390143 |Ett fel har uppstått med felkoden 390143. (Okänt fel). |Om hello problemet kvarstår bör du kontakta Microsoft Support för nästa steg. |

## <a name="next-steps"></a>Nästa steg
Om du tooresolve hello problem [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) om du behöver hjälp. 

[1]: https://technet.microsoft.com/en-us/%5Clibrary/Dn688135(v=WPS.630).aspx
[2]: https://technet.microsoft.com/en-us/%5Clibrary/Dn715782(v=WPS.630).aspx
