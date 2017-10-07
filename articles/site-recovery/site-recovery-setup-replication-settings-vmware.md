---
title: "aaaSet in replikeringsinställningar för Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur toodeploy Site Recovery tooorchestrate replikering, redundans och återställning av virtuella Hyper-V-datorer i VMM-moln tooAzure."
services: site-recovery
documentationcenter: 
author: sujayt
manager: rochakm
editor: rayne-wiselman
ms.assetid: 8e7d868e-00f3-4e8b-9a9e-f23365abf6ac
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2017
ms.author: sutalasi
ms.openlocfilehash: 618e92e42411732a2a1bb75c5e5ea8a433cd7d58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-replication-policy-for-vmware-tooazure"></a>Hantera replikeringsprincip för VMware tooAzure


## <a name="create-a-replication-policy"></a>Skapa replikeringsprincip

1. Välj **Hantera** > **Site Recovery-infrastruktur**.
2. Välj **Replikeringsprinciper** under **For VMware and Physical machines** (För VMware-datorer och fysiska datorer).
3. Välj **+Replikeringsprincip**.

    ![Skapa replikeringsprincip](./media/site-recovery-setup-replication-settings-vmware/createpolicy.png)

4. Ange hello principnamn.

5. I **Återställningspunktmål tröskelvärdet**, ange hello Återställningspunktmål gränsen. Aviseringar genereras när den kontinuerliga replikeringen överskrider den här gränsen.
6. I **kvarhållningstid för återställningspunkten**, ange (i timmar) hello varaktighet hello kvarhållningsperiod för varje återställningspunkt. Skyddade datorer kan återställas tooany punkt inom en kvarhållningsperiod.

    > [!NOTE]
    > In too24 timmars kvarhållning stöds för datorer replikerade toopremium lagring. In too72 timmars kvarhållning stöds för datorer replikerade toostandard lagring.

    > [!NOTE]
    > En replikeringsprincip för redundans skapas automatiskt.

7. I **Appkompatibel ögonblicksbildsfrekvens** anger du hur ofta (i minuter) återställningspunkter som innehåller programkonsekventa ögonblicksbilder ska skapas.

8. Klicka på **OK**. hello princip ska skapas på 30 too60 sekunder.

![Replikeringsprincipsgeneration](./media/site-recovery-setup-replication-settings-vmware/Creating-Policy.png)

## <a name="associate-a-configuration-server-with-a-replication-policy"></a>Associera en konfigurationsserver med en replikeringsprincip
1. Välj hello replikering princip toowhich du vill tooassociate hello konfigurationsservern.
2. Klicka på **Associera**.
![Associera konfigurationsserver](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-1.PNG)

3. Välj hello konfigurationsservern hello listan över servrar.
4. Klicka på **OK**. hello konfigurationsservern ska kopplas en tootwo minuter.

![Associering av konfigurationsserver](./media/site-recovery-setup-replication-settings-vmware/Associate-CS-2.png)

## <a name="edit-a-replication-policy"></a>Redigera en replikeringsprincip
1. Välj hello replikeringsprincip som du vill tooedit replikeringsinställningarna.
![Redigera replikeringsprincip](./media/site-recovery-setup-replication-settings-vmware/Select-Policy.png)

2. Klicka på **Redigera inställningar**.
![Redigera inställningar för replikeringsprinciper](./media/site-recovery-setup-replication-settings-vmware/Edit-Policy.png)

3. Ändra inställningarna för hello baserat på dina behov.
4. Klicka på **Spara**. hello princip ska sparas i toofive minut, beroende på hur många virtuella datorer använder den principen för lösenordsreplikering.

![Spara replikeringsprincip](./media/site-recovery-setup-replication-settings-vmware/Save-Policy.png)

## <a name="dissociate-a-configuration-server-from-a-replication-policy"></a>Koppla bort en konfigurationsserver från replikeringsprincipen
1. Välj hello replikering princip toowhich du vill tooassociate hello konfigurationsservern.
2. Klicka på **Koppla bort**.
3. Välj hello konfigurationsservern hello listan över servrar.
4. Klicka på **OK**. hello konfigurationsservern ska tas bort i en tootwo minuter.

    > [!NOTE]
    > Du kan koppla bort en konfigurationsserver om det finns minst ett replikerade objekt med hjälp av hello princip. Kontrollera att det inte finns några replikerade objekt med hjälp av hello princip innan du koppla bort hello konfigurationsservern.

## <a name="delete-a-replication-policy"></a>Skapa en replikeringsprincip

1. Välj hello replikeringsprincip som du vill toodelete.
2. Klicka på **Ta bort**. hello principen ska tas bort efter 30 too60 sekunder.

    > [!NOTE]
    > Du kan inte ta bort en replikeringsprincip om den har minst en konfiguration server hör tooit. Kontrollera att det inte finns några replikerade objekt med hjälp av hello princip och ta bort alla hello associerade configuration-servrar innan du tar bort hello princip.
