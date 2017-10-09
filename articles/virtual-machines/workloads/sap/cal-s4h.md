---
title: "aaaDeploy SAP S/4HANA eller BW/4HANA på en virtuell dator i Azure | Microsoft Docs"
description: "Distribuera SAP S/4HANA eller BW/4HANA på en virtuell dator i Azure"
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 44bbd2b6-a376-4b5c-b824-e76917117fa9
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/15/2016
ms.author: hermannd
ms.openlocfilehash: 7e57f7daa7667b7c7dbcb86f6892a54e4295b74c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-sap-s4hana-or-bw4hana-on-azure"></a>Distribuera SAP S/4HANA eller BW/4HANA på Azure
Den här artikeln beskriver hur toodeploy S/4HANA på Azure med hjälp av hello SAP Molnbibliotek installation (SAP CAL) 3.0. toodeploy andra SAP HANA-baserade lösningar, till exempel BW/4HANA följer hello samma steg.

> [!NOTE]
Mer information om hello SAP CAL gå toohello [SAP installation Molnbibliotek](https://cal.sap.com/) webbplats. SAP har också en blogg om hello [SAP molnet installation biblioteket 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience).

> [!NOTE]
Eftersom den 29 maj 2017 kan du använda hello Azure Resource Manager-distributionsmodellen dessutom toohello mindre önskade klassisk distribution modell toodeploy hello SAP CAL. Vi rekommenderar att du använder hello nya Resource Manager-modellen och ignorera hello klassiska distributionsmodellen.

## <a name="step-by-step-process-toodeploy-hello-solution"></a>Steg för steg toodeploy hello lösning

hello följande sekvens av skärmdumpar visar hur toodeploy S/4HANA på Azure med hjälp av hello SAP CAL. hello processen fungerar hello samma sätt som andra lösningar, till exempel BW/4HANA.

Hej **lösningar** sidan visar några av hello SAP HANA för CAL-baserade lösningar som är tillgängliga på Azure. **SAP S/4HANA 1610 FPS01, Fully-Activated installation** hello mellersta rad:

![SAP CAL-lösningar](./media/cal-s4h/s4h-pic-1c.png)

### <a name="create-an-account-in-hello-sap-cal"></a>Skapa ett konto i hello SAP CAL
1. toosign i toohello SAP CAL för hello första gången använder din SAP-S-användare eller en annan användare som har registrerats med SAP. Definiera ett SAP CAL-konto som används av hello SAP CAL toodeploy installationer på Azure. I hello konto definition behöver du:

    a. Välj hello distributionsmodell i Azure (Resource Manager eller klassisk).

    b. Ange din Azure-prenumeration. En SAP CAL-konto kan tilldelas tooone-prenumeration. Om du behöver mer än en prenumeration behöver toocreate en annan SAP CAL-konto.

    c. Ge hello SAP CAL behörighet toodeploy till din Azure-prenumeration.

    > [!NOTE]
    hello nästa steg visar hur toocreate en SAP-CAL-konto för Resource Manager distributioner. Om du redan har ett SAP CAL-konto som är länkade toohello klassiska distributionsmodellen du *måste* toofollow dessa steg toocreate ett nytt SAP CAL-konto. hello nya SAP CAL-konto måste toodeploy i hello Resource Manager-modellen.

2. Skapa ett nytt SAP CAL-konto. Hej **konton** sidan visas tre alternativ för Azure: 

    a. **Microsoft Azure (klassisk)** är hello klassiska distributionsmodellen och är inte längre önskade.

    b. **Microsoft Azure** är hello nya Resource Manager-distributionsmodellen.

    c. **Windows Azure som drivs av 21Vianet** är ett alternativ i Kina som använder hello klassiska distributionsmodellen.

    Välj toodeploy i hello Resource Manager-modellen **Microsoft Azure**.

    ![Information om SAP CAL-konto](./media/cal-s4h/s4h-pic-2a.png)

3. Ange hello Azure **prenumerations-ID** som finns på hello Azure-portalen.

   ![SAP CAL-konton](./media/cal-s4h/s4h-pic3c.png)

4. tooauthorize hello SAP CAL toodeploy till hello Azure-prenumeration som du har definierat klickar du på **auktorisera**. hello följande sida visas i hello flik i webbläsaren:

   ![Internet Explorer cloud services-inloggning](./media/cal-s4h/s4h-pic4c.png)

5. Om fler än en användare visas väljer du hello Microsoft-konto som är länkade toobe hello coadministrator av hello Azure-prenumeration du valt. hello följande sida visas i hello flik i webbläsaren:

   ![Bekräftelse för Internet Explorer cloud services](./media/cal-s4h/s4h-pic5a.png)

6. Klicka på **acceptera**. Om hello auktorisering lyckas visar hello definition för SAP CAL-konto igen. Efter en kort tid bekräftar ett meddelande att hello Auktoriseringen lyckades.

7. tooassign hello nyskapad SAP CAL tooyour användarkonto, ange din **användar-ID** i hello hello rätt textrutan och klicka på **Lägg till**.

   ![Toouser kontokoppling](./media/cal-s4h/s4h-pic8a.png)

8. tooassociate Klicka på ditt konto med hello användare att använda toosign i toohello SAP CAL **granska**. 
 
9. toocreate hello associationen mellan användaren och hello nyskapad SAP CAL-konto, klickar du på **skapa**.

   ![Användarassociationen tooSAP CAL-konto](./media/cal-s4h/s4h-pic9b.png)

Du har skapat ett SAP CAL-konto som kan:

- Använd hello Resource Manager-modellen.
- Distribuera SAP-system i din Azure-prenumeration.

Du kan nu starta toodeploy S/4HANA till din prenumeration för användare i Azure.

> [!NOTE]
Innan du fortsätter kan du avgöra om du har Azure core kvoter för virtuella datorer i Azure H-serien. Hello tillfället hello SAP CAL använder H-serien virtuella datorer i Azure toodeploy vissa hello SAP HANA-baserade lösningar. Din Azure-prenumeration kanske inte H-serien core kvoter för H-serien. I så fall kanske du måste toocontact Azure-supporten tooget en kvot på minst 16 H-serien kärnor.

> [!NOTE]
När du distribuerar en lösning på Azure i hello SAP CAL kanske du upptäcker att du kan välja endast en Azure-region. toodeploy i Azure-regioner än hello en föreslås av hello SAP CAL måste du toopurchase CAL prenumeration från SAP. Du måste kanske också tooopen ett meddelande med SAP toohave toodeliver din CAL-kontot är aktiverat i Azure-regioner än hello som ursprungligen förslag.

### <a name="deploy-a-solution"></a>Distribuera en lösning

Nu ska vi distribuera en lösning från hello **lösningar** sidan hello SAP CAL. hello SAP CAL har två sekvenser toodeploy:

- En grundläggande sekvens som använder en sida toodefine hello system toobe distribueras
- En avancerad sekvens som ger dig vissa alternativ på VM-storlekar 

Vi visar hello grundläggande sökvägen toodeployment här.

1. På hello **kontoinformation** sidan som du behöver:

    a. Välj ett SAP CAL-konto. (Använda ett konto som är associerade toodeploy med hello Resource Manager-distributionsmodellen.)

    b. Ange en instans **namn**.

    c. Välj en Azure **Region**. hello SAP CAL föreslår en region. Om du behöver en annan Azure-region och du inte har en SAP CAL-prenumeration kan behöver du tooorder en CAL-prenumeration med SAP.

    d. Ange en **lösenord** för hello lösning av åtta eller nio tecken. hello lösenordet används för hello administratörer av hello olika komponenter.

   ![SAP CAL grundläggande läge: Skapa en instans](./media/cal-s4h/s4h-pic10a.png)

2. Klicka på **skapa**, och i hello meddelanderutan som visas, klickar du på **OK**.

   ![SAP CAL VM-storlekar som stöds](./media/cal-s4h/s4h-pic10b.png)

3. I hello **privat nyckel** dialogrutan klickar du på **Store** toostore hello privata nyckeln i hello SAP CAL. toouse lösenordsskydd för hello privata nyckel, klickar du på **hämta**. 

   ![SAP CAL privat nyckel](./media/cal-s4h/s4h-pic10c.png)

4. Läs hello SAP CAL **varning** meddelande och på **OK**.

   ![SAP CAL-varning](./media/cal-s4h/s4h-pic10d.png)

    Nu hello distribution sker. Efter en stund, beroende på hello storleken och komplexiteten för hello-lösning (hello SAP CAL ger en uppskattning), visas hello status som aktiv och redo för användning.

5. toofind hello virtuella datorer som har samlats in med Hej andra associerade resurser i en resursgrupp, gå toohello Azure-portalen: 

   ![SAP CAL-objekt som distribuerats i nya hello-portalen](./media/cal-s4h/sapcaldeplyment_portalview.png)

6. På hello SAP CAL-portalen hello status visas som **Active**. tooconnect toohello lösning, klickar du på **Anslut**. Olika alternativ tooconnect toohello olika komponenter har distribuerats i den här lösningen.

   ![SAP CAL-instanser](./media/cal-s4h/active_solution.png)

7. Innan du kan använda ett av hello alternativ tooconnect toohello distribuerade system, klickar du på **komma igång**. 

   ![Ansluta toohello instans](./media/cal-s4h/connect_to_solution.png)

    hello dokumentationen namn hello användare för varje hello anslutningsmetoder. hello lösenorden för dessa användare ställs toohello lösenord du har definierat hello början av hello distributionsprocessen. I dokumentationen för hello andra mer funktionell användare visas med sina lösenord, som du kan använda toosign i toohello distribuerade system. 

    Till exempel om du använder hello SAP grafiskt användargränssnitt som är förinstallerat på hello Windows Fjärrskrivbord datorn hello S/4 system kan se ut så här:

   ![SM50 i hello förinstallerat SAP GUI](./media/cal-s4h/gui_sm50.png)

    Eller om du använder hello DBACockpit hello instans kan se ut så här:

   ![SM50 i hello DBACockpit SAP GUI](./media/cal-s4h/dbacockpit.png)

En felfri SAP S/4 installation distribueras inom några timmar i Azure.

Om du har köpt en prenumeration för SAP CAL stöd SAP fullständigt för distribution via hello SAP CAL på Azure. hello stöd kön är BC-VCM-klientåtkomstlicens.







