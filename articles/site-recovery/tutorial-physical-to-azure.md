---
title: "Ställ in katastrofåterställning till Azure för fysiska lokala servrar med Azure Site Recovery | Microsoft Docs"
description: "Lär dig hur du ställer in katastrofåterställning till Azure för lokala Windows- och Linux-servrar, med Azure Site Recovery-tjänsten."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 805f6946-c6da-491f-980e-bf724bebdf0b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/13/2017
ms.author: raynew
ms.openlocfilehash: ceb4b13e326b24360799c1a7a25fe48f213fabd7
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/02/2017
---
# <a name="set-up-disaster-recovery-to-azure-for-on-premises-physical-servers"></a>Ställ in haveriberedskap för lokala fysiska servrar till Azure

Den [Azure Site Recovery](site-recovery-overview.md) tjänsten bidrar till din strategi för katastrofåterställning genom att hantera och samordna replikering, redundans och återställning efter fel för lokala datorer och virtuella Azure-datorer (VM).

Den här kursen visar hur du ställer in haveriberedskap för lokala fysiska Windows och Linux-servrar till Azure. I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Ställ in krav för Azure och lokalt
> * Skapa ett Recovery Services-valv för Site Recovery 
> * Konfigurera käll- och mål-replikering miljöer
> * Skapa replikeringsprincip
> * Aktivera replikering för en server

## <a name="prerequisites"></a>Krav

För att slutföra den här kursen behöver du:

- Se till att du förstår de [scenariots arkitektur och komponenter](concepts-physical-to-azure-architecture.md).
- Granska de [supportkrav](site-recovery-support-matrix-to-azure.md) för alla komponenter.
- Se till att de servrar som du vill replikera följer [krav för Azure VM](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).
- Förbered Azure. Du behöver en Azure-prenumeration, Azure-nätverk och ett lagringskonto.
- Förbered ett konto för automatisk installation av mobilitetstjänsten på varje server som du vill replikera.



### <a name="set-up-an-azure-account"></a>Konfigurera ett Azure-konto

Hämta en Microsoft [Azure-konto](http://azure.microsoft.com/).

- Du kan börja med en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
- Lär dig mer om [priserna för Site Recovery](site-recovery-faq.md#pricing), och få [prisinformationen](https://azure.microsoft.com/pricing/details/site-recovery/).
- Ta reda på vilka [regioner stöds](https://azure.microsoft.com/pricing/details/site-recovery/) för Site Recovery.

### <a name="verify-azure-account-permissions"></a>Kontrollera behörigheter för Azure-konto

Kontrollera att kontot har behörighet för replikering av virtuella datorer till Azure.

- Granska de [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) måste du replikera datorer till Azure.
- Kontrollera och ändra [rollbaserad åtkomst](../active-directory/role-based-access-control-configure.md) behörigheter. 



### <a name="set-up-an-azure-network"></a>Skapa ett Azure-nätverk

Konfigurera en [Azure-nätverk](../virtual-network/virtual-network-get-started-vnet-subnet.md).

- Virtuella Azure-datorer placeras i det här nätverket när de skapas efter växling vid fel.
- Nätverket måste finnas i samma region som Recovery Services-valvet


## <a name="set-up-an-azure-storage-account"></a>Skapa ett Azure-lagringskonto

Konfigurera en [Azure storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account).

- Site Recovery replikerar lokala datorer till Azure-lagring. Virtuella Azure-datorer skapas från lagringen efter redundansväxlingen.
- Lagringskontot måste vara i samma region som Recovery Services-valvet.
- Lagringskontot kan vara standard eller [premium](../virtual-machines/windows/premium-storage.md).
- Om du konfigurerar ett premium-konto behöver du även en ytterligare standardkonto för loggdata.



### <a name="prepare-an-account-for-mobility-service-installation"></a>Förbereda ett konto för installationen av Mobility

Mobilitetstjänsten måste installeras på varje server som du vill replikera. Site Recovery installerar den här tjänsten automatiskt när du aktiverar replikering för servern. Om du vill installera automatiskt, måste du förbereda ett konto som Site Recovery för att ansluta till servern.

- Du kan använda en domän eller lokalt konto
- För virtuella Windows-datorer, om du inte använder ett domänkonto, inaktivera kontroll av fjärråtkomst till användare på den lokala datorn. Du gör detta i registret under **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System**, lägga till DWORD-posten **LocalAccountTokenFilterPolicy**, med värdet 1.
- Om du vill lägga till en registerpost för att inaktivera inställningen från en CLI, skriver du:``REG ADD HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System /v LocalAccountTokenFilterPolicy /t REG_DWORD /d 1.``
- För Linux måste kontot vara rot på källservern för Linux.


## <a name="create-a-vault"></a>Skapa ett valv

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

## <a name="select-a-protection-goal"></a>Välj en skyddsmål

Välj vad att replikera och för att replikera den till.

1. Klicka på **Recovery Services-valv** > valvet.
2. Resurs-menyn klickar du på **Site Recovery** > **Förbered infrastrukturen** > **skyddsmål**.
3. I **skyddsmål**väljer **till Azure** > **inte virtualiserade/andra**.

## <a name="set-up-the-source-environment"></a>Konfigurera källmiljön

Ställ in konfigurationsservern, registrera i valvet och identifiera virtuella datorer.

1. Klicka på **Site Recovery** > **förbereda infrastrukturen** > **källa**.
2. Om du inte har en konfigurationsserver, klickar du på **+ konfigurationsservern**.
3. I **Lägg till Server**, kontrollera att **konfigurationsservern** visas i **servertyp**.
4. Hämta installationsfilen för enhetlig installationsprogram för Site Recovery.
5. Ladda ned valvregistreringsnyckeln. Du behöver detta när du kör installationsprogrammet för enhetlig. Nyckeln är giltig i fem dagar efter att du har genererat den.

   ![Konfigurera källan](./media/tutorial-physical-to-azure/source-environment.png)


### <a name="register-the-configuration-server-in-the-vault"></a>Registrera konfigurationsservern i valvet

Gör följande innan du börjar: 

- Kontrollera att systemklockan är synkroniserad med i configuration server-dator, en [tidsserver](https://technet.microsoft.com/windows-server-docs/identity/ad-ds/get-started/windows-time-service/windows-time-service). Den måste matcha. Installationen misslyckas om det är 15 minuter framför eller bakom.
- Kontrollera att datorn kan komma åt dessa webbadresser:[!INCLUDE [site-recovery-URLS](../../includes/site-recovery-URLS.md)]

- IP-adressbaserade brandväggsregler ska tillåta kommunikation till Azure.
- Tillåt [Azure Datacenter IP Ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) (IP-intervall för Azures datacenter) och HTTPS-port 443.
- Tillåt IP-adressintervall för Azure-regionen för din prenumeration och för USA, västra (används för åtkomstkontroll och identitetshantering).

Kör installationsprogrammet för enhetlig som en lokal administratör kan installera konfigurationsservern. Processervern och huvudmålservern installeras som standard på konfigurationsservern.

[!INCLUDE [site-recovery-add-configuration-server](../../includes/site-recovery-add-configuration-server.md)]

När registreringen är klar konfigurationsservern visas på den **inställningar** > **servrar** i valvet.


## <a name="set-up-the-target-environment"></a>Konfigurera målmiljön

Välj och kontrollera target-resurser.

1. Klicka på **Förbered infrastruktur** > **Mål** och välj den Azure-prenumeration som du vill använda.
2. Ange distributionsmodellen som mål.
3. Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk.

   ![mål](./media/tutorial-physical-to-azure/network-storage.png)


## <a name="create-a-replication-policy"></a>Skapa replikeringsprincip

1. Klicka för att skapa en ny replikeringsprincip **Site Recovery-infrastruktur** > **replikeringsprinciper** > **+ replikeringsprincip**.
2. I **skapa replikeringsprincip**, ange ett principnamn.
3. I **Återställningspunktmål tröskelvärdet**, ange mål (RPO) återställningspunktgränsen. Det här värdet anger hur ofta data återställningspunkter skapas. En avisering genereras om kontinuerlig replikering överskrider den gränsen.
4. I **kvarhållningstid för återställningspunkten**, ange hur lång tid (i timmar) är kvarhållningsperiod för varje återställningspunkt. Replikerade virtuella datorer kan återställas till valfri punkt i ett fönster. Upp till 24 timmar kvarhållning har stöd för datorer som replikeras till premium-lagring och 72 timmar för standardlagring.
5. I **frekvens av programkonsekventa ögonblicksbilder**, ange hur ofta (i minuter) återställningspunkter som innehåller programkonsekventa ögonblicksbilder ska skapas. Klicka på **OK** vill skapa principen.

    ![Replikeringsprincip](./media/tutorial-physical-to-azure/replication-policy.png)


Principen associeras automatiskt med konfigurationsservern. Som standard skapas automatiskt en matchande princip för återställning efter fel. Om exempelvis replikeringsprinciper är **rep princip** sedan en princip för återställning efter fel **rep-princip-återställning** skapas. Den här principen används inte förrän du har initierat en återställning från Azure.

## <a name="enable-replication"></a>Aktivera replikering

Aktivera replikering för varje server.

- Site Recovery installerar mobilitetstjänsten om replikering har aktiverats.
- När du aktiverar replikering för en server kan det ta 15 minuter eller längre för att ändringarna ska börja gälla och visas på portalen.

1. Klicka på **replikera program** > **källa**.
2. I **källa**, Välj konfigurationsservern.
3. I **datorn typen**väljer **fysiska datorer**.
4. Välj processerver (konfigurationsserver). Klicka sedan på **OK**.
5. I **mål**, Välj prenumerationen och resursgrupp som du vill skapa den virtuella Azure-datorer efter redundans. Välj distributionsmodell som du vill använda i Azure (klassiska eller resource management).
6. Välj Azure storage-konto som du vill använda för att replikera data. 
7. Välj det Azure-nätverk och undernät som virtuella Azure-datorer ska ansluta till efter en redundansväxling.
8. Välj **Konfigurera nu för valda datorer** om du vill använda nätverksinställningen på alla datorer som du väljer att skydda. Välj **Konfigurera senare** om du vill välja Azure-nätverket för varje dator. 
9. I **fysiska datorer**, och klicka på **+ fysisk dator**. Ange namn och IP-adress. Välj operativsystemet på den dator du vill replikera. Det tar några minuter för servrarna som ska identifieras och i listan. 
10. I **egenskaper** > **konfigurera egenskaper för**, Välj det konto som ska användas av processervern för att automatiskt installera mobilitetstjänsten på datorn.
11. I **replikeringsinställningarna** > **konfigurerar replikeringsinställningar**, kontrollera att rätt replikeringsprinciper har valts. 
12. Klicka på **Aktivera replikering**. Du kan följa förloppet för den **Aktivera skydd** jobb **inställningar** > **jobb** > **Site Recovery-jobb**. När jobbet **Slutför skydd** har körts är datorn redo för redundans.


Om du vill övervaka servrar som du lägger till, kan du identifierade senast för dem i **Konfigurationsservrar** > **senaste kontakt på**. Om du vill lägga till datorer utan att vänta på en identifiering av schemalagd tid, markera konfigurationsservern (inte på den), och klicka på **uppdatera**.

## <a name="next-steps"></a>Nästa steg

[Köra ett återställningstest](tutorial-dr-drill-azure.md)
