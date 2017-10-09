---
title: "aaaSet hello källa och mål för Hyper-V-replikering tooa sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur tooset upp hello källa och mål när replikera virtuella Hyper-V-datorer toosecondary VMM webbplats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: fa7809f1-7633-425f-b25d-d10d004e8d0b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 451cb4413ca5c09777a7faf512b1c8ea43e695f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-set-up-hello-replication-source-and-target"></a>Steg 6: Konfigurera hello replikeringskälla och mål


När du har skapat en återställningstjänster valvet för Hyper-V-replikering tooa sekundär VMM-plats med [Azure Site Recovery](site-recovery-overview.md), Använd den här artikeln tooset hello källa och mål replikering platser. 

När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).




## <a name="set-up-hello-source-environment"></a>Ställ in hello källmiljön

Installera hello Azure Site Recovery-providern på VMM-servrar och identifiera och registrera servrarna i hello-valvet.

1. Klicka på **steg 1: förbereda infrastrukturen** > **källa**.

    ![Konfigurera källan](./media/vmm-to-vmm-walkthrough-source-target/goals-source.png)
2. I **Förbered källa**, klickar du på **+ VMM** tooadd en VMM-server.

    ![Konfigurera källan](./media/vmm-to-vmm-walkthrough-source-target/set-source1.png)
3. I **Lägg till Server**, kontrollera att **System Center VMM-servern** visas i **servertyp** och den hello VMM-servern uppfyller hello [krav](#prerequisites).
4. Hämta installationsfilen för hello Azure Site Recovery-providern.
5. Hämta hello registreringsnyckel. Du behöver den när du kör installationsprogrammet. hello nyckeln är giltig i fem dagar efter genereringen.

    ![Konfigurera källan](./media/vmm-to-vmm-walkthrough-source-target/set-source3.png)
6. Installera hello Azure Site Recovery Provider på hello VMM-servern. Du behöver inte installera tooexplicitly något på Hyper-V-värdservrar.


## <a name="install-hello-azure-site-recovery-provider"></a>Installera hello Azure Site Recovery Provider

1. Kör installationsfilen för hello-providern på varje VMM-servern. Om VMM distribueras i ett kluster, följande hello hello första gången du installerar:
    -  Installera hello-providern på en aktiv nod och slutför hello installation tooregister hello VMM-servern i valvet hello.
    - Installera sedan hello providern på hello andra noder. Klusternoder bör köra hello samma version av hello providern.
2. Installationsprogrammet körs några kontroller av förutsättningar och begär behörighet toostop hello VMM-tjänsten. hello VMM-tjänsten kommer att startas om automatiskt när installationen är klar. Om du installerar på en VMM-klustret är begärd toostop hello klustrad roll.
3. I **Microsoft Update**, kan du välja toospecify att provideruppdateringarna installeras i enlighet med Microsoft Update-princip.
4. I **Installation**acceptera eller ändra hello standardplatsen för installation och klicka på **installera**.

    ![Installationsplats](./media/vmm-to-vmm-walkthrough-source-target/provider-location.png)
5. När installationen är klar klickar du på **registrera** tooregister hello server i hello-valvet.

    ![Installationsplats](./media/vmm-to-vmm-walkthrough-source-target/provider-register.png)
6. I **valvnamnet**, verifierar hello valvet som servern ska registreras i vilka hello hello-namn. Klicka på *Nästa*.

    ![Serverregistrering](./media/vmm-to-vmm-walkthrough-source-target/vaultcred.png)
7. I **Internetanslutning**, ange hur hello-providern som körs på hello VMM-servern ansluter tooAzure.

    ![Internetinställningar](./media/vmm-to-vmm-walkthrough-source-target/proxydetails.png)

   - Du kan ange hello providern ska ansluta direkt toohello internet, eller via en proxyserver.
   - Ange proxy-inställningar om det behövs.
   - Om du använder en proxyserver VMM RunAs-konto (DRAProxyAccount) skapas automatiskt med hjälp av hello angivna autentiseringsuppgifterna för proxyn. Konfigurera hello proxyserver så att det här kontot kan autentiseras. hello inställningarna för RunAs-konto kan ändras i hello VMM-konsolen > **inställningar** > **säkerhet** > **kör som-konton**. Starta om hello VMM-tjänsten tooupdate ändras.
8. I **registreringsnyckel**väljer hello nyckel som du hämtade från Azure Site Recovery och kopierat toohello VMM-servern.
9. hello krypteringsinställning används bara när du replikerar virtuella Hyper-V-datorer i VMM-moln tooAzure. Om du replikerar tooa sekundär plats används den inte.
10. I **servernamn**, ange ett eget namn tooidentify hello VMM-servern i hello-valvet. Ange hello VMM klusternamnet roll i en klusterkonfiguration.
11. I **synkronisera molnmetadata**väljer du om du vill använda toosynchronize metadata för alla moln på hello VMM-servern med hello-valvet. Den här åtgärden behöver bara toohappen en gång på varje server. Om du inte vill toosynchronize alla moln kan du lämna den här inställningen avmarkerad och synkronisera varje moln individuellt i hello molnegenskaperna hello VMM-konsolen.
12. Klicka på **nästa** toocomplete hello processen. Efter registreringen hämtas metadata från hello VMM-servern av Azure Site Recovery. hello servern visas på hello **VMM-servrar** fliken på hello **servrar** sida i hello-valvet.

    ![Server](./media/vmm-to-vmm-walkthrough-source-target/provider13.png)
13. När hello-servern är tillgänglig i hello Site Recovery-konsolen i **källa** > **Förbered källa** Välj hello VMM-servern och välj hello moln i vilka hello Hyper-V-värd finns. Klicka sedan på **OK**.

Du kan också installera hello-provider från hello kommandorad:

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-hello-target-environment"></a>Ställ in hello målmiljön

Välj VMM-målservern hello och molnet:

1. Klicka på **Förbered infrastruktur** > **mål**, och välj hello VMM-målservern du vill toouse.
2. Moln på hello-servern som är synkroniserade med Site Recovery visas. Välj hello målmoln.

   ![mål](./media/vmm-to-vmm-walkthrough-source-target/target-vmm.png)



## <a name="next-steps"></a>Nästa steg

Gå för[steg 7: Konfigurera nätverksmappning](vmm-to-vmm-walkthrough-network-mapping.md).
