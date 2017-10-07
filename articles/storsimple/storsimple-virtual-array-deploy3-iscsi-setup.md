---
title: aaaMicrosoft Azure StorSimple virtuell matris iSCSI-serverinstallation | Microsoft Docs
description: "Beskriver hur tooperform installationen, registrera StorSimple iSCSI-servern och slutför Enhetsinställningar."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 4db116d1-978b-48e8-b572-a719a8425dbc
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.openlocfilehash: b4ff6391cb2af69d4e83dcdac5e027f8498005b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array--set-up-as-an-iscsi-server-via-azure-portal"></a>Distribuera StorSimple virtuell Array – Set in som en iSCSI-server via Azure-portalen

![processflöde för iSCSI-installationen](./media/storsimple-virtual-array-deploy3-iscsi-setup/iscsi4.png)

## <a name="overview"></a>Översikt

Den här distributionen kursen gäller toohello Microsoft Azure StorSimple virtuell matris. Den här självstudiekursen beskrivs hur tooperform hello-installationen, registrera StorSimple iSCSI-serverns fullständiga hello Enhetsinställningar och sedan skapa, montera, initiera och formatera volymer på din virtuella StorSimple-matrisen som konfigurerats som en iSCSI-server. 

hello procedurerna som beskrivs ta här cirka 30 minuter too1 timme toocomplete. hello information som publicerats i den här artikeln gäller tooStorSimple virtuella matriser.

## <a name="setup-prerequisites"></a>Nödvändiga inställningar

Kontrollera följande innan du konfigurerar och konfigurera din virtuella StorSimple-matris:

* Du har etablerats virtuella matris och anslutna tooit enligt beskrivningen i [distribuera StorSimple virtuell matris - etablera en virtuell Hyper-V-matris](storsimple-ova-deploy2-provision-hyperv.md) eller [distribuera StorSimple virtuell matris - etablera en virtuell VMware-matris ](storsimple-virtual-array-deploy2-provision-vmware.md).
* Du har hello nyckel för tjänstregistrering från hello StorSimple enheten Manager-tjänsten som du skapade toomanage din virtuella StorSimple-matriser. Mer information finns i **steg 2: hämta hello nyckel för tjänstregistrering** i [distribuera StorSimple virtuell matris - förbereda hello portal](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key).
* Om detta är hello andra eller senare virtuella matris som du registrerar med en befintlig StorSimple Device Manager-tjänst, bör du ha hello krypteringsnyckel för tjänstdata. Den här nyckeln har genererats när hello första enheten har registrerats med den här tjänsten. Om du har förlorat den här nyckeln kan se **Get hello krypteringsnyckel för tjänstdata** i [Använd hello Webbgränssnittet tooadminister din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key).

## <a name="step-by-step-setup"></a>Steg för steg-installationen

Använd hello följa instruktionerna tooset upp och konfigurera din virtuella StorSimple-matris:

* [Steg 1: Slutföra hello lokala web UI-installationen och registrera din enhet](#step-1-complete-the-local-web-ui-setup-and-register-your-device)
* [Steg 2: Fullständig hello krävs Enhetsinställningar](#step-2-complete-the-required-device-setup)
* [Steg 3: Lägg till en volym](#step-3-add-a-volume)
* [Steg 4: Montera, initiera och formatera en volym](#step-4-mount-initialize-and-format-a-volume)

## <a name="step-1-complete-hello-local-web-ui-setup-and-register-your-device"></a>Steg 1: Slutföra hello lokala web UI-installationen och registrera din enhet

#### <a name="toocomplete-hello-setup-and-register-hello-device"></a>toocomplete hello installationen och registrera hello enheten

1. Öppna ett webbläsarfönster. tooconnect toohello web UI-Typ:
   
    `https://<ip-address of network interface>`
   
    Använd hello anslutnings-URL som anges i hello föregående steg. Du ser ett fel som får ett meddelande om att det finns ett problem med hello webbplatsens säkerhetscertifikat. Klicka på **Fortsätt toothis webbsida**.
   
    ![säkerhetscertifikat fel](./media/storsimple-virtual-array-deploy3-iscsi-setup/image3.png)
2. Logga in toohello Webbgränssnitt för din virtuella enhet som **StorSimpleAdmin**. Ange hello enhetens administratörslösenord som du har ändrat i steg3: starta hello-enhet i [distribuera StorSimple virtuell matris - etablera en virtuell enhet i Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md) eller [distribuera StorSimple virtuell matris - Etablera en virtuell enhet i VMware](storsimple-virtual-array-deploy2-provision-vmware.md).
   
    ![Inloggningssidan](./media/storsimple-virtual-array-deploy3-iscsi-setup/image4.png)
3. Du kommer att vidtas toohello **Start** sidan. Den här sidan beskrivs hello olika inställningar som krävs tooconfigure och registrera hello virtuella enheten med hello StorSimple Device Manager-tjänsten. Observera att hello **nätverksinställningar**, **Web proxy-inställningar**, och **tidsinställningar** är valfria. Hej krävs inställningarna är bara **Enhetsinställningar** och **Molninställningar**.
   
    ![Startsida](./media/storsimple-virtual-array-deploy3-iscsi-setup/image5.png)
4. På hello **nätverksinställningar** sidan **nätverksgränssnitt**, DATA 0 konfigureras automatiskt för dig. Varje nätverksgränssnitt är som standard tooget en IP-adress automatiskt (DHCP). Därför kan kommer en IP-adress, nätmask och gateway att tilldelas automatiskt (för både IPv4 och IPv6).
   
    När du planerar toodeploy enheten som en iSCSI-server (tooprovision blocklagring) rekommenderar vi att du inaktiverar hello **hämta IP-adress automatiskt** och konfigurera statiska IP-adresser.
   
    ![Inställningssidan för nätverk](./media/storsimple-virtual-array-deploy3-iscsi-setup/image6.png)
   
    Om du har lagt till fler än ett nätverksgränssnitt under hello etableringen av hello enhet kan konfigurera du dem här. Observera att du kan konfigurera din nätverksgränssnittet som IPv4 endast eller som både IPv4 och IPv6. Endast IPv6-konfigurationer stöds inte.
5. DNS-servrar krävs eftersom de används när enheten försöker toocommunicate med molntjänstleverantörer för lagring eller tooresolve enheten efter namn om den är konfigurerad som en filserver. På hello **nätverksinställningar** sidan under hello **DNS-servrar**:
   
   1. En primär och sekundär DNS-server konfigureras automatiskt. Om du väljer tooconfigure statiska IP-adresser, kan du ange DNS-servrar. För hög tillgänglighet rekommenderar vi att du konfigurerar en primär och en sekundär DNS-server.
   2. Klicka på **Använd**. Detta gäller och validera hello nätverksinställningar.
6. På hello **Enhetsinställningar** sidan:
   
   1. Tilldela en unik **namn** tooyour enhet. Det här namnet kan vara 1 och 15 tecken och får innehålla bokstäver, siffror och bindestreck.
   2. Klicka på hello **iSCSI-server** ikonen ![iSCSI Serverikon](./media/storsimple-virtual-array-deploy3-iscsi-setup/image7.png) för hello **typen** av enhet som du skapar. En iSCSI-server kan du tooprovision blocklagring.
   3. Ange om du vill att den här enheten toobe ansluten till domänen. Om enheten är en iSCSI-server, är koppla hello domän valfritt. Om du väljer toonot ansluta till iSCSI-server tooa-domän, klickar du på **tillämpa**, vänta tills hello inställningar toobe tillämpas och hoppa över toohello nästa steg.
      
       Om du vill toojoin hello enheten tooa domän. Ange en **domännamn**, och klicka sedan på **tillämpa**.
      
      > [!NOTE]
      > Är tillämpade tooit om ansluta till iSCSI-server tooa domänen, se till att virtuella matrisen är i sin egen organisationsenhet (OU) för Microsoft Azure Active Directory och inga grupprincipobjekt (GPO).
      > 
      > 
   4. En dialogruta visas. Ange autentiseringsuppgifter för domänen i hello angivet format. Klicka på kryssikonen hello ![kryssikon](./media/storsimple-virtual-array-deploy3-iscsi-setup/image15.png). hello domänautentiseringsuppgifter kontrolleras. Ett felmeddelande visas om hello autentiseringsuppgifterna är felaktiga.
      
       ![autentiseringsuppgifter](./media/storsimple-virtual-array-deploy3-iscsi-setup/image8.png)
   5. Klicka på **Använd**. Detta gäller och validera hello Enhetsinställningar.
7. (Valfritt) Konfigurera din webbproxyserver. Även om webbproxykonfigurationen är valfri, Tänk på att om du använder en webbproxy, du kan bara konfigurera den här.
   
    ![Konfigurera webbproxy](./media/storsimple-virtual-array-deploy3-iscsi-setup/image9.png)
   
    På hello **Web proxy** sidan:
   
   1. Ange hello **Web proxy URL** i det här formatet: *http://host-IP adress* eller *FDQN:Port nummer*. Observera att HTTPS-URL: er inte stöds.
   2. Ange **autentisering** som **grundläggande** eller **ingen**.
   3. Om du använder autentisering måste du också tooprovide en **användarnamn** och **lösenord**.
   4. Klicka på **Använd**. Detta validera och tillämpa hello konfigurerats web proxy-inställningar.
8. (Valfritt) Konfigurera hello tidsinställningar för din enhet, till exempel tidszon och hello primära och sekundära NTP-servrar. NTP-servrar krävs eftersom din enhet måste synkronisera tiden så att den kan autentisera med molntjänstleverantören.
   
    ![Tidsinställningar](./media/storsimple-virtual-array-deploy3-iscsi-setup/image10.png)
   
    På hello **tidsinställningar** sidan:
   
   1. Välj hello hello nedrullningsbara listan **tidszon** baserat på hello geografiska plats i vilka hello enheten ska distribueras. hello Standardtidszon för din enhet är PST. Enheten använder den här tidszonen för alla schemalagda åtgärder.
   2. Ange en **primär NTP-server** för din enhet eller godkänner time.windows.com hello standardvärdet. Se till att ditt nätverk tillåter att NTP-trafik toopass från ditt datacenter toohello Internet.
   3. Om du vill ange en **sekundära NTP-server** för din enhet.
   4. Klicka på **Använd**. Detta validera och tillämpa hello konfigurerats tidsinställningar.
9. Konfigurera hello molninställningar för din enhet. I det här steget kommer du att slutföra hello lokala enhetskonfiguration och registrera sedan hello enhet med StorSimple Device Manager-tjänsten.
   
   1. Ange hello **nyckel för tjänstregistrering** som du har fått **steg 2: hämta hello nyckel för tjänstregistrering** i [distribuera StorSimple virtuell matris - förbereda hello Portal](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key).
   2. Om detta inte är hello första enhet som du registrerar med den här tjänsten måste tooprovide hello **krypteringsnyckel för tjänstdata**. Den här nyckeln är obligatorisk med hello service registrering viktiga tooregister ytterligare enheter med hello StorSimple enheten Manager-tjänsten. Mer information finns för[Get hello krypteringsnyckel för tjänstdata](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) på lokalt webbgränssnitt.
   3. Klicka på **registrera**. Hello enheten startas om. Du kan behöva toowait 2-3 minuter innan hello enheten har registrerats. Hello enheten har startats om, tas du toohello inloggning på sidan.
      
      ![Registrera enhet](./media/storsimple-virtual-array-deploy3-iscsi-setup/image11.png)
10. Returnera toohello Azure-portalen.
11. Navigera toohello **enheter** bladet för din tjänst. Om du har många resurser, klickar du på **alla resurser**på ditt tjänstnamn (Sök efter det om det behövs) och klicka sedan på **enheter**.
12. På hello **enheter** bladet Kontrollera hello enheten har lyckats ansluta toohello tjänsten genom att leta upp hello status. hello enhetens status ska vara **klar tooset in**.
    
    ![Registrera enhet](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis1m.png)

## <a name="step-2-configure-hello-device-as-iscsi-server"></a>Steg 2: Konfigurera hello-enhet som iSCSI-server

Utföra hello följa stegen i hello Azure portal toocomplete hello krävs Enhetsinställningar.

#### <a name="tooconfigure-hello-device-as-iscsi-server"></a>tooconfigure hello enheten som iSCSI-server

1. Gå tooyour StorSimple enheten Manager-tjänsten och sedan gå för**Management > enheter**. I hello **enheter** bladet, Välj hello-enhet som du nyss skapade. Den här enheten skulle visas som **klar tooset in**.
   
    ![Konfigurera enhet som iSCSI-server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis1m.png) 
2. Klicka på hello enheten och du kommer att se Banderollmeddelandet som visar hello enheten är klar toosetup.
   
    ![Konfigurera enhet som iSCSI-server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis2m.png)  
3. Klicka på **konfigurera** i hello kommandofält för enheten. Detta öppnar hello **konfigurera** bladet. I hello **konfigurera** bladet hello följande:
   
   * hello iSCSI-servernamnet fylls i automatiskt.
   * Se till att hello molnet lagringskryptering anges för**aktiverad**. Detta säkerställer att hello data som skickas från hello enhet toohello moln är krypterad.
   * Ange en krypteringsnyckel med 32 tecken och registrera den på en app för hantering av nycklar för framtida bruk.
   * Välj en storage-konto toobe som används med din enhet. I den här prenumerationen kan du välja ett befintligt lagringskonto eller klicka **Lägg till** toochoose ett konto från en annan prenumeration.
     
     ![Konfigurera enhet som iSCSI-server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis4m.png)
4. Klicka på **konfigurera** toocomplete konfigurera hello iSCSI-server.
   
    ![Konfigurera enhet som iSCSI-server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis5m.png) 
5. Du meddelas att hello iSCSI-servern skapas. När hello iSCSI-server har skapats, hello **enheter** bladet uppdateras och hello motsvarande enhetens status är **Online**.
   
    ![Konfigurera enhet som iSCSI-server](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis9m.png)

## <a name="step-3-add-a-volume"></a>Steg 3: Lägg till en volym

1. I hello **enheter** bladet, Välj hello-enhet som du precis har konfigurerat som en iSCSI-server. Klicka på **...**  (du kan också högerklicka på den här raden) och hello snabbmenyn, Välj **Lägg till volymen**. Du kan också klicka på **+ Lägg till volymen** från hello kommandofältet. Detta öppnar hello **Lägg till volymen** bladet.
   
    ![Lägg till en volym](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis10m.png)
2. I hello **Lägg till volymen** bladet hello följande:
   
   * I hello **volymnamn** , ange ett unikt namn för volymen. hello namn måste vara en sträng som innehåller 3 too127 tecken.
   * I hello **typen** listrutan anger om toocreate en **skiktindelade** eller **lokalt Fäst** volym. Arbetsbelastningar som kräver lokala garantier, låg latens och hög prestanda, Välj **lokalt Fäst** **volym**. Alla andra data, Välj **skiktindelade** **volym**.
   * I hello **kapacitet** anger hello storleken på hello volym. En nivåindelad volym måste vara mellan 500 GB och 5 TB och en lokalt Fäst volym måste vara mellan 50 och 500 GB.
     
     En lokalt Fäst volym etableras tjockt, vilket och säkerställer att hello primära data i hello volymen förblir på enheten hello och inte läcker över toohello moln.
     
     En nivåindelad volym på hello andra sidan tunt allokerad. När du skapar en nivåindelad volym cirka 10% av hello utrymme har etablerats på hello lokala nivån och 90% av hello utrymme har etablerats i hello molnet. Till exempel om du har etablerat en volym 1 TB, 100 GB skulle finns i hello lokalt utrymme och 900 GB ska användas i hello molnet när hello datanivåer. Detta innebär i sin tur är att om du kör utanför alla hello lokalt utrymme på enheten hello, du kan inte etablera en skiktad resurs (eftersom hello 10% är inte tillgänglig).
     
     ![Lägg till en volym](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis12.png)
   * Klicka på **anslutna värdar**, Välj en access control post (ACR) motsvarande toohello iSCSI-initierare du vill tooconnect toothis volymen och klickar sedan på **Välj**. <br><br> 
3. tooadd en ny anslutna värddator klickar du på **Lägg till ny**, ange ett namn för hello värden och dess iSCSI-kvalificerade namn (IQN) och klicka sedan på **Lägg till**. Om du inte har hello IQN, går för[bilaga A: hämta hello IQN för en Windows Server-värd](#appendix-a-get-the-iqn-of-a-windows-server-host).
   
      ![Lägg till en volym](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis15m.png)
4. När du är klar med att konfigurera volymen, klickar du på **OK**. En volym skapas med hello angetts inställningar och du ser ett meddelande. Som standard aktiveras övervakning och säkerhetskopiering för hello volym.
   
     ![Lägg till en volym](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis18m.png)
5. tooconfirm som hello volymen har har skapats, gå toohello **volymer** bladet. Du bör se hello volym som visas.
   
   ![Lägg till en volym](./media/storsimple-virtual-array-deploy3-iscsi-setup/deployis20m.png)

## <a name="step-4-mount-initialize-and-format-a-volume"></a>Steg 4: Montera, initiera och formatera en volym

Hello utför följande steg toomount, initiera och formatera dina StorSimple-volymer på en Windows Server-värd.

#### <a name="toomount-initialize-and-format-a-volume"></a>toomount, initiera och formatera en volym

1. Öppna hello **iSCSI-initieraren** app på hello lämplig server.
2. I hello **iSCSI-Initieraregenskaper** fönstret på hello **identifiering** klickar du på **identifiera Portal**.
   
    ![identifiera portal](./media/storsimple-virtual-array-deploy3-iscsi-setup/image22.png)
3. I hello **identifiera målportal** dialogrutan Ange hello IP-adressen för din iSCSI-aktiverade nätverksgränssnitt och klicka sedan på **OK**.
   
    ![IP-adress](./media/storsimple-virtual-array-deploy3-iscsi-setup/image23.png)
4. I hello **iSCSI-Initieraregenskaper** fönstret på hello **mål** , letar du reda hello **upptäckta mål**. (Varje volym vara ett upptäckta mål.) hello enhetens status borde visas som **inaktiv**.
   
    ![upptäckta mål](./media/storsimple-virtual-array-deploy3-iscsi-setup/image24.png)
5. Välj en målenhet och klicka sedan på **Anslut**. När hello-enheten är ansluten hello bör statusen ändras för**ansluten**. (Mer information om hur du använder hello Microsoft iSCSI-initieraren finns [installera och konfigurera Microsoft iSCSI Initiator][1].
   
    ![Välj målenheten](./media/storsimple-virtual-array-deploy3-iscsi-setup/image25.png)
6. Tryck på hello Windows-tangenten + X på din Windows-värd och klicka sedan på **kör**.
7. I hello **kör** skriver **Diskmgmt.msc**. Klicka på **OK**, och hello **Diskhantering** visas dialogrutan. hello högra panelen visar hello volymer på värden.
8. I hello **Diskhantering** hello monterade volymer visas fönster, som visas i följande illustration hello. Högerklicka på hello identifierade volymen (klicka på hello diskens namn) och klicka sedan på **Online**.
   
    ![Diskhantering](./media/storsimple-virtual-array-deploy3-iscsi-setup/image26.png)
9. Högerklicka och välj **initiera Disk**.
   
    ![Initiera disk 1](./media/storsimple-virtual-array-deploy3-iscsi-setup/image27.png)
10. Välj hello diskarna tooinitialize hello i dialogrutan och klicka sedan på **OK**.
    
    ![initiera disken 2](./media/storsimple-virtual-array-deploy3-iscsi-setup/image28.png)
11. hello startas ny enkel volym. Välj storleken för en disk och klicka sedan på **nästa**.
    
    ![guiden Skapa ny volym 1](./media/storsimple-virtual-array-deploy3-iscsi-setup/image29.png)
12. Tilldela en enhet bokstav toohello volym och klicka sedan på **nästa**.
    
    ![guiden Skapa ny volym 2](./media/storsimple-virtual-array-deploy3-iscsi-setup/image30.png)
13. Ange hello parametrar tooformat hello volym. **Endast NTFS stöds på Windows Server.** Ange hello allokering enhet storlek too64K. Ange en etikett för volymen. Det är rekommenderad praxis för toobe identiska toohello volymnamnet du angav på din virtuella StorSimple-matrisen. Klicka på **Nästa**.
    
    ![guiden Skapa ny volym 3](./media/storsimple-virtual-array-deploy3-iscsi-setup/image31.png)
14. Kontrollera hello värdena för volymen och klicka sedan på **Slutför**.
    
    ![guiden Skapa ny volym 4](./media/storsimple-virtual-array-deploy3-iscsi-setup/image32.png)
    
    hello volymer visas som **Online** på hello **Diskhantering** sidan.
    
    ![volymer online](./media/storsimple-virtual-array-deploy3-iscsi-setup/image33.png)

## <a name="next-steps"></a>Nästa steg

Lär dig hur toouse hello lokala webbgränssnittet för[administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).

## <a name="appendix-a-get-hello-iqn-of-a-windows-server-host"></a>Bilaga A: Get hello IQN för en Windows Server-värd

Utför följande steg tooget hello iSCSI hello kvalificerade namn (IQN) för en Windows-värd som kör Windows Server 2012.

#### <a name="tooget-hello-iqn-of-a-windows-host"></a>tooget hello IQN för en Windows-värd

1. Starta hello Microsoft iSCSI-initieraren på din Windows-värd.
2. I hello **iSCSI-Initieraregenskaper** fönstret på hello **Configuration** , markera och kopiera hello sträng från hello **Initierarnamn** fältet.
   
    ![iSCSI-initieraregenskaper](./media/storsimple-virtual-array-deploy3-iscsi-setup/image34.png)
3. Spara den här strängen.

<!--Reference link-->
[1]: https://technet.microsoft.com/library/ee338480(WS.10).aspx



