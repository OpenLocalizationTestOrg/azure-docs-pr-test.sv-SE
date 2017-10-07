---
title: "aaaSet hello källa och mål för Hyper-V-replikering tooAzure (med System Center VMM) med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello steg tooset inställningar för källa och mål för replikering av Hyper-V virtuella datorer i VMM-moln tooAzure lagring med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5edb6d87-25a5-40fe-b6f1-ddf7b55a6b31
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/25/2017
ms.author: raynew
ms.openlocfilehash: 3f8c5386cb64527c775aef636980bac098ee9905
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-hyper-v-with-vmm-replication-tooazure"></a>Steg 8: Ställ in hello källa och mål för tooAzure för replikering av Hyper-V (med VMM)

Efter [skapar ett valv](vmm-to-azure-walkthrough-create-vault.md) och ange vad du vill tooreplicate, använder den här artikeln tooconfigure källa och mål inställningar vid replikering av lokala Hyper-V virtuella datorer i System Center Virtual Machine Manager (VMM) moln tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.

Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).


## <a name="set-up-hello-source-environment"></a>Ställ in hello källmiljön

S1. Klicka på **Förbereda infrastrukturen** > **Källa**.

    ![Set up source](./media/vmm-to-azure-walkthrough-source-target/set-source1.png)

2. I **Förbered källa**, klickar du på **+ VMM** tooadd en VMM-server.

    ![Konfigurera källan](./media/vmm-to-azure-walkthrough-source-target/set-source2.png)

3. I **Lägg till Server**, kontrollera att **System Center VMM-servern** visas i **servertyp** och den hello VMM-servern uppfyller hello [krav och URL: en krav för](#prerequisites).
4. Hämta installationsfilen för hello Azure Site Recovery-providern.
5. Hämta hello registreringsnyckel. Du behöver den när du kör installationsprogrammet. hello nyckeln är giltig i fem dagar efter genereringen.

    ![Konfigurera källan](./media/vmm-to-azure-walkthrough-source-target/set-source3.png)

## <a name="install-hello-provider-on-hello-vmm-server"></a>Installera hello providern på hello VMM-servern

1. Kör installationsfilen för hello-providern på hello VMM-servern.
2. I **Microsoft Update** kan du välja uppdateringar så att provideruppdateringarna installeras i enlighet med din Microsoft Update-princip.
3. I **Installation**, acceptera eller ändra hello standardinstallationsplatsen för providern och klickar på **installera**.

    ![Installationsplats](./media/vmm-to-azure-walkthrough-source-target/provider2.png)
4. När installationen är klar klickar du på **registrera** tooregister hello VMM-servern i hello-valvet.
5. I hello **Valvinställningar** klickar du på **Bläddra** tooselect hello valvnyckelfilen. Ange hello Azure Site Recovery-prenumerationen och valvnamnet hello.

    ![Serverregistrering](./media/vmm-to-azure-walkthrough-source-target/provider10.png)
6. I **Internetanslutning**, ange hur providern som körs på hello VMM-servern ansluter tooSite Recovery via hello hello internet.

   * Om du vill hello providern tooconnect direkt markerar **ansluter direkt tooAzure Site Recovery utan en proxy**.
   * Om den befintliga proxyservern kräver autentisering eller om du vill toouse en anpassad proxyserver, Välj **ansluta tooAzure Platsåterställningen genom att använda en proxyserver**.
   * Om du använder en anpassad proxyserver, ange hello-adress, port och autentiseringsuppgifter.
   * Om du använder en proxyserver du bör redan ha tillåtit hello webbadresser som beskrivs i [krav](#on-premises-prerequisites).
   * Om du använder en anpassad proxyserver VMM RunAs-konto (DRAProxyAccount) skapas automatiskt med hello angivna autentiseringsuppgifterna för proxyn. Konfigurera hello proxyserver så att det här kontot kan autentiseras. inställningarna för hello VMM RunAs-konto kan ändras i hello VMM-konsolen. I **inställningar**, expandera **säkerhet** > **kör som-konton**, och sedan ändra hello lösenordet för DRAProxyAccount. Du behöver toorestart hello VMM-tjänsten så att den här inställningen träder i kraft.

     ![Internet](./media/vmm-to-azure-walkthrough-source-target/provider13.png)
7. Acceptera eller ändra hello platsen för ett SSL-certifikat som genereras automatiskt för datakryptering. Det här certifikatet används om du aktiverar datakryptering för ett moln som skyddas av Azure hello Azure Site Recovery-portalen. Skydda det här certifikatet. När du kör en växling vid fel tooAzure måste den toodecrypt, om datakryptering är aktiverat.
8. I **servernamn**, ange ett eget namn tooidentify hello VMM-servern i hello-valvet. Ange hello VMM klusternamnet roll i en klusterkonfiguration.
9. Aktivera **synkronisera molnmetadata**om du vill toosynchronize metadata för alla moln på hello VMM-servern med hello-valvet. Den här åtgärden behöver bara toohappen en gång på varje server. Om du inte vill toosynchronize alla moln kan du lämna den här inställningen avmarkerad och synkronisera varje moln individuellt i hello molnegenskaperna hello VMM-konsolen. Klicka på **registrera** toocomplete hello processen.

    ![Serverregistrering](./media/vmm-to-azure-walkthrough-source-target/provider16.png)
10. Registreringen startar. När registreringen är klar hello server visas i **Site Recovery-infrastruktur** > **VMM-servrar**.


## <a name="install-hello-azure-recovery-services-agent-on-hyper-v-hosts"></a>Installera hello Azure Recovery Services-agenten på Hyper-V-värdar

1. När du har konfigurerat hello providern behöver du toodownload hello installationsfilen för hello Azure Recovery Services-agenten. Kör installationsprogrammet på varje Hyper-V-server i hello VMM-moln.

    ![Hyper-V-platser](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent1.png)
2. I **Kravkontroll**, klicka på **Nästa**. Alla nödvändiga komponenter som saknas installeras automatiskt.

    ![Krav för Recovery Services-agenten](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent2.png)
3. I **installationsinställningar**Godkänn eller ändra hello installationsplatsen och hello cacheplatsen. Du kan konfigurera hello cachen på en enhet som har minst 5 GB tillgängligt utrymme, men vi rekommenderar en cacheenhet med 600 GB eller mer ledigt utrymme. Klicka på **Installera**.
4. När installationen är klar klickar du på **Stäng** toofinish.

    ![Registrera MARS-agenten](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent3.png)

### <a name="command-line-installation"></a>Kommandoradsinstallation
Du kan installera hello Microsoft Azure Recovery Services-agenten från kommandoraden med hjälp av hello följande kommando:

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-toosite-recovery-from-hyper-v-hosts"></a>Konfigurera internet proxy åtkomst tooSite återställning från Hyper-V-värdar

hello Recovery Services-agenten körs på Hyper-V-värdar måste tooAzure för internet-åtkomst för VM-replikering. Om du försöker komma åt hello internet via en proxyserver, ställa in den på följande sätt:

1. Öppna hello Microsoft Azure Backup MMC snapin-modulen på hello Hyper-V-värd. Som standard finns en genväg till Microsoft Azure Backup på hello skrivbordet eller i C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.
2. I snapin-modulen hello, klickar du på **ändra egenskaper för**.
3. På hello **proxykonfiguration** anger du information om proxyservern.

    ![Registrera MARS-agenten](./media/vmm-to-azure-walkthrough-source-target/mars-proxy.png)
4. Kontrollera att hello-agenten kan nå hello webbadresser som beskrivs i hello [krav](#on-premises-prerequisites).

## <a name="set-up-hello-target-environment"></a>Ställ in hello målmiljön
Ange hello Azure storage-konto toobe används för replikering och ansluter hello Azure-nätverk toowhich virtuella Azure-datorer efter redundans.

1. Klicka på **Förbered infrastruktur** > **mål**hello prenumerationen och välj hello resursgruppen där du vill att toocreate hello misslyckades för virtuella datorer. Välj hello distributionsmodell som du vill toouse i Azure (klassiska eller resource management) för hello misslyckades för virtuella datorer.

    ![Lagring](./media/vmm-to-azure-walkthrough-source-target/enablerep3.png)

2. Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk.

    ![Lagring](./media/vmm-to-azure-walkthrough-source-target/compatible-storage.png)

3. Om du inte har skapat ett lagringskonto och du vill toocreate en med Resource Manager klickar du på **+ lagringskonto** toodo det internt.  På hello **skapa lagringskonto** anger ett kontonamn, typ, prenumeration och plats. hello-konto måste finnas i hello samma plats som hello Recovery Services-valvet.

   ![Lagring](./media/vmm-to-azure-walkthrough-source-target/gs-createstorage.png)


   * Om du vill toocreate ett lagringskonto med hjälp av hello klassiska modellen gör det hello Azure-portalen. [Läs mer](../storage/common/storage-create-storage-account.md)
   * Om du använder ett premium-lagringskonto för replikerade data, skapa en ytterligare standardlagringskonto toostore replikeringsloggar som samlar in löpande ändringar tooon lokala data.
5. Om du inte har skapat ett Azure-nätverk och du vill toocreate en med Resource Manager klickar du på **+ nätverk** toodo det internt. På hello **skapa virtuellt nätverk** anger ett nätverksnamn, adressintervall, information om undernät, prenumeration och plats. hello nätverket bör finnas i hello samma plats som hello Recovery Services-valvet.

   ![Nätverk](./media/vmm-to-azure-walkthrough-source-target/gs-createnetwork.png)

   Om du vill toocreate ett nätverk med hello klassiska modellen gör det hello Azure-portalen. [Läs mer](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).





## <a name="next-steps"></a>Nästa steg

Gå för[steg 9: Konfigurera nätverksmappning](vmm-to-azure-walkthrough-network-mapping.md)
