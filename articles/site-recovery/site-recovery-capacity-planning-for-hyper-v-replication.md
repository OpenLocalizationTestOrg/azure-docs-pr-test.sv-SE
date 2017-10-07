---
title: "aaaRun hello Hyper-V kapacitetsplaneringsverktyget för Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur toorun hello kapacitetsplaneringsverktyget för Hyper-V för Azure Site Recovery"
services: site-recovery
documentationcenter: na
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 2bc3832f-4d6e-458d-bf0c-f00567200ca0
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: nisoneji
ms.openlocfilehash: b853598e5cd290c48b59794ba48eefc72ac8ded6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-hello-hyper-v-capacity-planner-tool-for-site-recovery"></a>Kör hello kapacitetsplaneringsverktyget för Hyper-V för Site Recovery

Som en del av Azure Site Recovery-distributionen måste toofigure ut din replikering och krav på bandbredd. hello Hyper-V kapacitetsplaneringsverktyget för Site Recovery hjälper du toodo, för replikering av Hyper-V virtuella datorer.

Den här artikeln beskriver hur toorun hello kapacitetsplaneringsverktyget för Hyper-V. Det här verktyget kan användas tillsammans med hello information i [kapacitetsplanering för Site Recovery](site-recovery-capacity-planner.md).

## <a name="before-you-start"></a>Innan du börjar
Du kan köra hello verktyget på en Hyper-V-server eller kluster nod på din primära plats. toorun hello verktyget hello Hyper-V-värdservrar behöver:

* Operativsystem: Windows Server 2012 eller 2012 R2
* Minne: 20 MB (minst)
* : 5 procent processorbelastning (minst)
* Diskutrymme: 5 MB (minst)

Innan du kör verktyget hello måste tooprepare hello primär plats. Om du replikerar mellan lokala platser och du vill toocheck bandbredd måste du tooprepare en replikserver.

## <a name="step-1-prepare-hello-primary-site"></a>Steg 1: Förbered hello primär plats

1. Gör en lista över alla hello Hyper-V-datorer som du vill använda på hello primär plats, tooreplicate och där de är placerade hello Hyper-V-värdar/kluster. hello-verktyget kan köra flera fristående värdar eller ett enskilt kluster, men inte båda tillsammans. Det måste också toorun separat för varje operativsystem, så du bör samla in information om Hyper-V-servrar på följande sätt:

   * Windows Server 2012 fristående servrar
   * Windows Server 2012-kluster
   * Windows Server 2012 R2 fristående servrar
   * Windows Server 2012 R2-kluster
2. Aktivera fjärråtkomst tooWMI på alla hello Hyper-V-värdar och kluster. Kör det här kommandot på varje serverkluster anges toomake till brandväggsregler och behörigheter:

        netsh firewall set service RemoteAdmin enable
3. Aktivera övervakning av programprestanda på servrar och kluster, enligt följande:

   * Öppna hello Windows-brandväggen med hello **avancerad säkerhet** snapin-modulen, och sedan aktivera hello följande regler för inkommande trafik: **COM + nätverksåtkomst (DCOM-IN)** och alla regler i hello **fjärrhantering av händelseloggen Hanteringsgruppen**.

## <a name="step-2-prepare-a-replica-server-on-premises-tooon-premises-replication"></a>Steg 2: Förbered en replikserver (lokalt tooon lokal replikering)
Du behöver inte toodo detta om du replikerar tooAzure.

Vi rekommenderar att du ställer in en enskild Hyper-V-värd som en återställningsserver, så att en dummy virtuell dator kan vara replikerade tooit toocheck bandbredd.  Du kan hoppa över det här, men du inte kan toomeasure bandbredd om du inte gör det.

1. Om du vill toouse konfigurera koordinatortjänsten för Hyper-V i en klusternod som hello replik:

   * I **Serverhanteraren**öppnar **Klusterhanteraren**.
   * Ansluta toohello kluster, markera hello klusternamnet och klickar på **åtgärder** > **konfigurera roll** guiden för tooopen hello hög tillgänglighet.
   * I **Välj roll**, klickar du på **Hyper-V Replica Broker**. I hello guiden tillhandahåller en **NetBIOS-namnet** och hello **IP-adress** toobe används som hello anslutning punkt toohello klustret (kallas en klientåtkomstpunkt). Hej **Hyper-V Replica Broker** ska konfigureras, vilket resulterar i en klientåtkomstpunktens namn som du bör anteckna.
   * Kontrollera att hello Koordinatortjänsten för Hyper-V-rollen är online har och kan växlas mellan alla noder i klustret hello. toodo, högerklicka på hello rollen, peka för**flytta**, och klicka sedan på **Välj nod**. Välj en nod > **OK**.
   * Om du använder certifikatbaserad autentisering, se till att varje klusternod och hello klientåtkomstpunkt alla har hello certifikat installerat.
2. Aktivera en replikserver:

   * Öppna Hanteraren för fel för ett kluster, ansluta toohello klustret och klickar på **roller** > Välj rolltjänster > **replikeringsinställningarna** > **aktivera det här klustret som en replik Server**. Om du använder ett kluster som hello replik måste toohave hello Koordinatortjänsten för Hyper-V-rollen finns på hello-kluster i hello samt den primära platsen.
   * Öppna Hyper-V Manager för en fristående server. I hello **åtgärder** rutan klickar du på **Hyper-V-inställningar** hello-server du vill ha tooenable, och i **replikeringskonfiguration** klickar du på **aktiverar du datorn som en replikserver**.
3. Konfigurera autentisering:

   * I **autentisering och portar**, Välj hur tooauthenticate hello primära servern och hello autentiseringsportar. Om du använder ett certifikat klickar du på **Välj certifikat** tooselect en. Använda Kerberos om hello primära platsen och återställningsplatsen Hyper-V-värdar finns i hello samma domän eller betrodda domäner. Använd certifikat för olika domäner eller en arbetsgrupp-distribution.
   * I **auktorisering och lagring**, Tillåt **alla** autentiserade (primära) servrar toosend replikering data toothis replikservern.

     ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image1.png)
   * Kör **netsh http show servicestate**, toocheck som hello-lyssnaren körs för hello protocol/port som du angav:  
4. Konfigurera brandväggar. Under installationen av Hyper-V skapas brandväggsregler tooallow trafik på hello standardportarna (HTTPS på 443, Kerberos på 80). Vill du aktivera dessa regler på följande sätt:
  - Autentisering med datorcertifikat på klustret (443):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}``
  - Kerberos-autentisering på klustret (80):``Get-ClusterNode | ForEach-Object {Invoke-command -computername \$\_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}``
  - Autentisering med datorcertifikat på fristående server:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"``
  - Kerberos-autentisering på fristående server:``Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"``

## <a name="step-3-run-hello-capacity-planner-tool"></a>Steg 3: Kör hello kapacitetsplaneringsverktyget för
När du har förberett din primära plats och konfigurera en återställningsserver kan köra du hello-verktyget.

1. [Hämta](https://www.microsoft.com/download/details.aspx?id=39057) hello verktyget från hello Microsoft Download Center.
2. Kör hello verktyget från en av hello primära servrar (eller någon av noderna hello från hello primära kluster). Högerklicka på hello .exe-fil och välj sedan **kör som administratör**.
3. I **innan du börjar**, ange hur länge du vill toocollect data. Vi rekommenderar att du kör verktyget hello under produktion timmar tooensure att data är representativt. Om du försöker bara toovalidate nätverksanslutning, du kan samla in bara en minut.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image2.png)
4. I **primära platsinformation**, ange hello-servernamn eller FQDN för en fristående värd eller för ett kluster ange hello FQDN för hello klienten accepterar punkt, klusternamn eller en nod i klustret hello och klicka sedan på **nästa**. hello upptäcks automatiskt hello namnet på hello-server som den körs på. hello verktyget tar upp virtuella datorer som kan övervakas hello angivna servrar.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image3.png)
5. I **replik platsinformation**, om du replikerar tooAzure eller om du replikerar tooa sekundärt datacenter och inte har konfigurerat en replikserver väljer **hoppa över testerna som rör replikeringsplatsen**. Om du replikerar tooa sekundärt datacenter och du har konfigurerat en Repliktyp, anger FQDN för hello fristående server eller hello klientåtkomstpunkt för hello-kluster i **Server name (eller) Hyper-V Replica Broker CAP**.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image4.png)
6. I **utökad replik information**, aktivera **hoppa över hello tester som inbegriper den utökade repliken plats**. Dessa tester stöds inte av Site Recovery.
7. I **Välj virtuella datorer tooReplicate**, hello verktyg ansluter toohello server eller kluster och visar virtuella datorer och diskar som körs på hello primära servern, i enlighet med hello inställningar du angav i hello **primära platsinformation**  sidan. Virtuella datorer som redan har aktiverats för replikering eller som inte körs, visas inte. Välj hello virtuella datorer som du vill toocollect mått. Markera hello virtuella hårddiskar automatiskt samlas data in för hello virtuella datorer för.
8. Om du har konfigurerat en replikserver eller ett kluster i **nätverksinformation**, ange hello ungefärliga WAN-bandbredden du tror används mellan hello primära servern och repliken platser och välj hello certifikat om du har konfigurerat autentisering med datorcertifikat.

    ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image5.png)
9. I **sammanfattning**, kontrollera hello inställningar och klickar på **nästa** toobegin att samla in mått. Verktyget förlopp och status visas på hello **beräkna kapacitet** sidan. När verktyget hello är klar klickar du på **Visa rapport** tooview hello utdata. Som standard, rapporter och loggar lagras i **%systemdrive%\Users\Public\Documents\Capacity Planner**.

   ![](./media/site-recovery-capacity-planning-for-hyper-v-replication/image6.png)

## <a name="step-4-interpret-hello-results"></a>Steg 4: Tolka hello resultaten

Här följer hello viktiga mått. Du kan ignorera mått som inte visas här. De är inte relevanta för Site Recovery.

### <a name="on-premises-tooon-premises-replication"></a>Lokala tooon lokal replikering

* Effekten av replikering på hello primära värd beräkning, minne
* Effekten av replikering på hello primära recovery värdar diskutrymme för lagring, IOPS
* Totala bandbredd som krävs för deltareplikering (Mbps)
* Observerade nätverksbandbredden mellan hello primära värd och hello återställningsvärden (Mbps)
* Förslag på hello perfekt antalet aktiva parallella överförs mellan hello två värdar-kluster

### <a name="on-premises-tooazure-replication"></a>Lokala tooAzure replikering

* Effekten av replikering på hello primära värd beräkning, minne
* Effekten av replikering på hello primära värd disk lagringsutrymme IOPS
* Totala bandbredd som krävs för deltareplikering (Mbps)

## <a name="more-resources"></a>Fler resurser
* Läs hello-dokumentet som medföljer hello verktyget Hämta detaljerad information om hello-verktyget.
* Titta på en genomgång av hello-verktyget i Keith Mayers [TechNet-blogg](http://blogs.technet.com/b/keithmayer/archive/2014/02/27/guided-hands-on-lab-capacity-planner-for-windows-server-2012-hyper-v-replica.aspx).
* [Hello resultat](site-recovery-performance-and-scaling-testing-on-premises-to-on-premises.md) av våra prestandatestning för lokala tooon lokala Hyper-V-replikering

## <a name="next-steps"></a>Nästa steg

När du är klar kapacitetsplanering kan du börja distribuera Site Recovery:

* [Replikera virtuella Hyper-V-datorer i VMM-moln tooAzure](site-recovery-vmm-to-azure.md)
* [Replikera virtuella Hyper-V-datorer (utan VMM) tooAzure](site-recovery-hyper-v-site-to-azure.md)
* [Replikera virtuella Hyper-V-datorer mellan VMM-platser](site-recovery-vmm-to-vmm.md)
