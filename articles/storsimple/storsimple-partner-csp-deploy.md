---
title: "aaaMicrosoft Azure StorSimple och molnet providern programmet översikt över lösningar med | Microsoft Docs"
description: "En översikt över hur hello StorSimple- och CSP för StorSimple-partner."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/08/2017
ms.author: alkohli
ms.openlocfilehash: b5d999f2fbb9a27e7404ff454957b29dbef56af6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array-for-cloud-solution-provider-program"></a>Distribuera virtuella StorSimple-matrisen för Cloud Solution Provider Program

## <a name="overview"></a>Översikt

StorSimple virtuell matris kan distribueras av hello Cloud Solution Providers (CSP) partners för sina kunder. En CSP-partner kan skapa en StorSimple Device Manager-tjänst. Den här tjänsten kan sedan använda toodeploy och hantera virtuella StorSimple-matris och hello associerade resurser, volymer och säkerhetskopieringar.

Den här artikeln beskrivs hur en CSP-partner kan lägga till en kund eller en ny prenumeration tooan befintliga kund och skapa sedan en service toodeploy en virtuell StorSimple-matris i CSP.

## <a name="prerequisites"></a>Krav

Innan du börjar bör du se till att:

- Du är registrerad under hello CSP-programmet.
- Du har en giltig [Partnercenter](http://partnercenter.microsoft.com/) inloggningsuppgifter. Aktivera toosign toohello Partner portal tooadd nya kunder hello autentiseringsuppgifter, söka efter kunder eller navigera tooa kundkontot hello Partner instrumentpanel. hello CSP kan fungera som en StorSimple-administratör åt hello kunden i hello Azure-portalen.
                             
## <a name="add-a-customer"></a>Lägg till en kund

Om du lägger till en kund, skapas automatiskt en prenumeration. tooadd en kund (och automatiskt skapa en prenumeration) utför hello följa stegen i hello Partner-portalen.

1. Gå toohello [Partnercenter](http://partnercenter.microsoft.com/) och logga in med dina inloggningsuppgifter för CSP. Klicka på **instrumentpanelen**.

     ![Instrumentpanelen Partner Center](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. I hello vänstra fönsterrutan klickar du på **kunder**. I hello högra rutan, klickar du på **Lägg till kunder**. Ange information om hello hello kunden. Klicka på **nästa: prenumerationer** toocreate en kundprenumeration.

    ![Lägg till kund](./media/storsimple-partner-csp-deploy/image2.png)

3.  Välj **Microsoft Azure** erbjuder. Rulla toohello längst ned på sidan hello och klicka på **granska**.

    ![Granska information om prenumerationen](./media/storsimple-partner-csp-deploy/image3.png)
                              
4. Granska hello information och klickar på **skicka**.

    ![Skicka prenumeration](./media/storsimple-partner-csp-deploy/image4.png)

5. Spara hello bekräftelse information för framtida bruk.

    ![Spara-bekräftelse](./media/storsimple-partner-csp-deploy/image5.png)

6. Sök eller navigera toohello kund som du just lagt till. Klicka på hello **företagsnamn** toodrill ned hello detaljer.

    ![Sök efter hello kunden](./media/storsimple-partner-csp-deploy/image6.png)  

7. Välj i hello vänstra fönsterrutan **Service management**. I hello högra rutan under **administrera tjänster**, klickar du på **Microsoft Azure Management Portal** toosign i som Azure-administratör för din kund.

    ![Logga in tooAzure portal](./media/storsimple-partner-csp-deploy/image9.png)

8. toocreate en StorSimple-Enhetshanteraren, klicka på **+ ny** och Sök eller navigera för**StorSimple virtuell enhet serien**. Mer information finns för[distribuera en tjänst för StorSimple Enhetshanteraren](storsimple-virtual-array-manage-service.md).

    ![Skapa StorSimple enheten Manager-tjänsten](./media/storsimple-partner-csp-deploy/image8.png)


## <a name="add-a-subscription"></a>Lägga till en prenumeration

I vissa fall kan du kanske har en befintlig kund, och du behöver tooadd en prenumeration. tooadd en prenumeration tooan befintliga kund, utföra hello följa stegen i hello Partner-portalen.

1. Gå toohello [Partnercenter](http://partnercenter.microsoft.com/) och logga in med dina inloggningsuppgifter för CSP. Klicka på **instrumentpanelen**.

     ![Instrumentpanelen Partner Center](./media/storsimple-partner-csp-deploy/image1.png)
                              
2. I hello vänstra fönsterrutan klickar du på **kunder**. Sök eller navigera toohello kund som du vill tooadd en prenumeration på. Klicka på hello ![Expandera kryssikonen](./media/storsimple-partner-csp-deploy/expand_pane_icon.png) ikonen tooexpand hello rad för hello företagsnamn för kunden. I hello information klickar du på **lägga till prenumerationer**.

    ![Kunder](./media/storsimple-partner-csp-deploy/image10.png)

3. Kontrollera **Microsoft Azure** för hello **uppifrån erbjudanden** i hello prenumeration och klicka på **skicka**. Detta skapar en ny prenumeration.

    ![Lägg till ny prenumeration](./media/storsimple-partner-csp-deploy/image11.png)

6. När en ny prenumeration skapas, klickar du på **<--kunder** i hello vänstra fönsterrutan tooreturn toohello **kunder** sidan. Sök efter hello kund som du just har skapat en prenumeration. Klicka på hello **företagsnamn** toodrill ned hello detaljer.

    ![Sök efter hello kunden](./media/storsimple-partner-csp-deploy/image6.png)  

7. Välj i hello vänstra fönsterrutan **Service management**. I hello högra rutan under **administrera tjänster**, klickar du på **Microsoft Azure Management Portal** toosign i som Azure-administratör för din kund.

    ![Logga in tooAzure portal](./media/storsimple-partner-csp-deploy/image9.png)

8. toocreate en StorSimple-Enhetshanteraren, klicka på **+ ny** och Sök eller navigera för**StorSimple virtuell enhet serien**. Mer information finns för[distribuera en tjänst för StorSimple Enhetshanteraren](storsimple-virtual-array-manage-service.md).

    ![Skapa StorSimple enheten Manager-tjänsten](./media/storsimple-partner-csp-deploy/image8.png)

## <a name="next-steps"></a>Nästa steg

- Om du har fler frågor angående hello StorSimple i CSP gå för[StorSimple i CSP: vanliga frågor och](storsimple-partner-csp-faq.md).
- Om du är klar toodeploy din StorSimple, gå för[distribuera din StorSimple i CSP](storsimple-partner-csp-deploy.md).
