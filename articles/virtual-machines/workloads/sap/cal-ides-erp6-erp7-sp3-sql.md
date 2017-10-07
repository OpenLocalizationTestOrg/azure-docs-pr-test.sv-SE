---
title: "aaaDeploy SAP IDES EHP7 SP3 för SAP ERP 6.0 i Azure | Microsoft Docs"
description: "Distribuera SAP IDES EHP7 SP3 för SAP ERP 6.0 på Azure"
services: virtual-machines-windows
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: 626c1523-1026-478f-bd8a-22c83b869231
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 09/16/2016
ms.author: hermannd
ms.openlocfilehash: 26d88c7b48a91d35602464c4f89ca7a30502c4b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-sap-ides-ehp7-sp3-for-sap-erp-60-on-azure"></a>Distribuera SAP IDES EHP7 SP3 för SAP ERP 6.0 på Azure
Den här artikeln beskriver hur toodeploy SAP IDES system körs med SQL Server och hello Windows-operativsystemet på Azure via hello SAP Molnbibliotek installation (SAP CAL) 3.0. hello skärmdumpar visar hello steg för steg. Följ toodeploy en annan lösning hello samma steg.

toostart med hello SAP CAL, gå toohello [SAP installation Molnbibliotek](https://cal.sap.com/) webbplats. SAP har också en blogg om hello nya [SAP molnet installation biblioteket 3.0](http://scn.sap.com/community/cloud-appliance-library/blog/2016/05/27/sap-cloud-appliance-library-30-came-with-a-new-user-experience). 

> [!NOTE]
Eftersom den 29 maj 2017 kan du använda hello Azure Resource Manager-distributionsmodellen dessutom toohello mindre önskade klassisk distribution modell toodeploy hello SAP CAL. Vi rekommenderar att du använder hello nya Resource Manager-modellen och ignorera hello klassiska distributionsmodellen.

Om du redan skapat ett SAP CAL-konto som använder hello klassiska modellen, *toocreate måste en annan SAP CAL-konto*. Det här kontot måste tooexclusively distribuera till Azure med hjälp av hello Resource Manager-modellen.

När du loggar in toohello SAP CAL hello första sidan normalt leder dig toohello **lösningar** sidan. hello-lösningar som erbjuds via hello SAP CAL ökar stadigt, så du kan behöva tooscroll mycket lite toofind hello lösningen som du söker. hello markerade Windows-baserade SAP IDES lösning som är tillgänglig enbart på Azure visar hello distributionsprocessen:

![SAP CAL-lösningar](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic1.jpg)

### <a name="create-an-account-in-hello-sap-cal"></a>Skapa ett konto i hello SAP CAL
1. toosign i toohello SAP CAL för hello första gången använder din SAP-S-användare eller en annan användare som har registrerats med SAP. Definiera ett SAP CAL-konto som används av hello SAP CAL toodeploy installationer på Azure. I hello konto definition behöver du:

    a. Välj hello distributionsmodell i Azure (Resource Manager eller klassisk).

    b. Ange din Azure-prenumeration. En SAP CAL-konto kan tilldelas tooone-prenumeration. Om du behöver mer än en prenumeration behöver toocreate en annan SAP CAL-konto.
    
    c. Ge hello SAP CAL behörighet toodeploy till din Azure-prenumeration.

    > [!NOTE]
    hello nästa steg visar hur toocreate en SAP-CAL-konto för Resource Manager distributioner. Om du redan har ett SAP CAL-konto som är länkade toohello klassiska distributionsmodellen du *måste* toofollow dessa steg toocreate ett nytt SAP CAL-konto. hello nya SAP CAL-konto måste toodeploy i hello Resource Manager-modellen.

2. toocreate en ny SAP-CAL-konto hello **konton** sidan visas två alternativ för Azure: 

    a. **Microsoft Azure (klassisk)** är hello klassiska distributionsmodellen och är inte längre önskade.

    b. **Microsoft Azure** är hello nya Resource Manager-distributionsmodellen.

    ![SAP CAL-konton](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic-2a.PNG)

    Välj toodeploy i hello Resource Manager-modellen **Microsoft Azure**.

    ![SAP CAL-konton](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

3. Ange hello Azure **prenumerations-ID** som finns på hello Azure-portalen. 

    ![SAP CAL prenumerations-ID](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic3c.PNG)

4. tooauthorize hello SAP CAL toodeploy till hello Azure-prenumeration som du har definierat klickar du på **auktorisera**. hello följande sida visas i hello flik i webbläsaren:

    ![Internet Explorer cloud services-inloggning](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic4c.PNG)

5. Om fler än en användare visas väljer du hello Microsoft-konto som är länkade toobe hello coadministrator av hello Azure-prenumeration du valt. hello följande sida visas i hello flik i webbläsaren:

    ![Bekräftelse för Internet Explorer cloud services](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic5a.PNG)

6. Klicka på **acceptera**. Om hello auktorisering lyckas visar hello definition för SAP CAL-konto igen. Efter en kort tid bekräftar ett meddelande att hello Auktoriseringen lyckades.

7. tooassign hello nyskapad SAP CAL tooyour användarkonto, ange din **användar-ID** i hello hello rätt textrutan och klicka på **Lägg till**. 

    ![Toouser kontokoppling](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic8a.PNG)

8. tooassociate Klicka på ditt konto med hello användare att använda toosign i toohello SAP CAL **granska**. 

9. toocreate hello associationen mellan användaren och hello nyskapad SAP CAL-konto, klickar du på **skapa**.

    ![Användarassociationen tooaccount](./media/cal-ides-erp6-ehp7-sp3-sql/s4h-pic9b.PNG)

Du har skapat ett SAP CAL-konto som kan:

- Använd hello Resource Manager-modellen.
- Distribuera SAP-system i din Azure-prenumeration.

> [!NOTE]
Innan du kan distribuera hello SAP IDES lösning baserad på Windows och SQL Server måste kanske du måste toosign för en SAP CAL-prenumeration. I annat fall hello-lösning kan visas som **låst** på översiktssidan för hello.

### <a name="deploy-a-solution"></a>Distribuera en lösning
1. När du har skapat ett SAP CAL-konto, Välj **hello SAP IDES lösning på Windows och SQL Server** lösning. Klicka på **skapa instans**, och bekräfta hello villkor för användning och villkor. 

2. På hello **grundläggande läge: skapa instans** sidan som du behöver:

    a. Ange en instans **namn**.

    b. Välj en Azure **Region**. Du kan behöva en SAP CAL prenumeration tooget som erbjuds av flera Azure-regioner.

    c.  Ange hello master **lösenord** för hello lösning, som visas:

    ![SAP CAL grundläggande läge: Skapa en instans](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic10a.png)

3. Klicka på **Skapa**. Efter en stund, beroende på hello storleken och komplexiteten för hello-lösning (hello SAP CAL ger en uppskattning), visas hello status som aktiv och redo att användas: 

    ![SAP CAL-instanser](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic12a.png)

4. toofind hello resursgruppen och alla objekt som har skapats av hello SAP CAL, gå toohello Azure-portalen. hello virtuella datorn finns från och med hello samma namn som har angetts i hello SAP CAL-instans.

    ![Resursen gruppobjekt](./media/cal-ides-erp6-ehp7-sp3-sql/ides_resource_group.PNG)

5. Gå toohello distribueras instanser på hello SAP CAL-portalen och klicka på **Anslut**. hello följande popup-fönster visas: 

    ![Ansluta toohello instans](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic14a.PNG)

6. Innan du kan använda ett av hello alternativ tooconnect toohello distribuerade system, klickar du på **komma igång**. hello dokumentationen namn hello användare för varje hello anslutningsmetoder. hello lösenorden för dessa användare ställs toohello lösenord du har definierat hello början av hello distributionsprocessen. I dokumentationen för hello andra mer funktionell användare visas med sina lösenord, som du kan använda toosign i toohello distribuerade system.

    ![Välkommen till SAP-dokumentation](./media/cal-ides-erp6-ehp7-sp3-sql/ides-pic15.jpg)

En felfri systemet SAP IDES distribueras inom några timmar i Azure.

Om du har köpt en prenumeration för SAP CAL stöd SAP fullständigt för distribution via hello SAP CAL på Azure. hello stöd kön är BC-VCM-klientåtkomstlicens.

