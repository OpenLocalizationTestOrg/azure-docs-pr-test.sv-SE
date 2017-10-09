---
title: aaaSet in StorSimple virtuell matris som filserver | Microsoft Docs
description: "Självstudierna tredje i virtuella StorSimple-matris distribution instruerar tooset upp en virtuell enhet som filserver."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: f609f6ff-0927-48bb-a68a-6d8985d2fe34
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 04/17/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 89cade37980f342695c0adee42c4ade0e1d53d2e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-storsimple-virtual-array---set-up-as-file-server-via-azure-portal"></a>Distribuera StorSimple virtuell matris - uppsättningen upp som filserver via Azure-portalen
![](./media/storsimple-virtual-array-deploy3-fs-setup/fileserver4.png)

## <a name="introduction"></a>Introduktion
Den här artikeln beskriver hur tooperform installationen, registrera din StorSimple-filserver, fullständig hello Enhetsinställningar och skapa och ansluta tooSMB resurser. Detta är hello senaste artikel i hello serien i kurserna om distribution krävs toocompletely distribuera virtuella matrisen som en filserver eller en iSCSI-server.

hello kan-installation och konfiguration ta ungefär 10 minuter toocomplete. hello informationen i den här artikeln gäller bara toohello distribution av hello virtuella StorSimple-matris. Hello distribution av StorSimple 8000-serien enheter, gå till: [distribuera StorSimple 8000-serien enheten kör uppdatering 2](storsimple-deployment-walkthrough-u2.md).

## <a name="setup-prerequisites"></a>Nödvändiga inställningar
Kontrollera följande innan du konfigurerar och konfigurera din virtuella StorSimple-matris:

* Du har etablerat en virtuell matris och anslutna tooit som anges i hello [etablera en virtuell StorSimple-matris på Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md) eller [etablera en virtuell StorSimple-matris på VMware](storsimple-virtual-array-deploy2-provision-vmware.md).
* Du har hello nyckel för tjänstregistrering från hello Enhetshanteraren för StorSimple-tjänsten som du skapade toomanage StorSimple virtuell matriser. Mer information finns i [steg 2: hämta hello nyckel för tjänstregistrering](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key) för virtuell StorSimple-matrisen.
* Om detta är hello andra eller senare virtuella matris som du registrerar med en befintlig StorSimple Device Manager-tjänst, bör du ha hello krypteringsnyckel för tjänstdata. Den här nyckeln har genererats när hello första enheten har registrerats med den här tjänsten. Om du har förlorat den här nyckeln kan se [Get hello krypteringsnyckel för tjänstdata](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) för din virtuella StorSimple-matrisen.

## <a name="step-by-step-setup"></a>Steg för steg-installationen
Använd hello följa instruktionerna tooset upp och konfigurera din virtuella StorSimple-matris.

## <a name="step-1-complete-hello-local-web-ui-setup-and-register-your-device"></a>Steg 1: Slutföra hello lokala web UI-installationen och registrera din enhet
#### <a name="toocomplete-hello-setup-and-register-hello-device"></a>toocomplete hello installationen och registrera hello enheten
1. Öppna ett webbläsarfönster och Anslut toohello lokala webbgränssnittet. Ange:
   
   `https://<ip-address of network interface>`
   
   Använd hello anslutnings-URL som anges i hello föregående steg. Du ser ett felmeddelande om att det finns ett problem med hello webbplatsens säkerhetscertifikat. Klicka på **fortsätta toothis webbsidan**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image2.png)
2. Logga in toohello Webbgränssnitt för din virtuella matrisen som **StorSimpleAdmin**. Ange hello enhetens administratörslösenord som du har ändrat i steg3: starta hello virtuella matris i [etablera en virtuell StorSimple-matris på Hyper-V](storsimple-virtual-array-deploy2-provision-hyperv.md) eller i [etablera en virtuell StorSimple-matris på VMware](storsimple-virtual-array-deploy2-provision-vmware.md).
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image3.png)
3. Du har vidtagit toohello **Start** sidan. Den här sidan beskrivs hello olika inställningar som krävs tooconfigure och registrera hello virtuella matris med hello StorSimple Device Manager-tjänsten. Hej **nätverksinställningar**, **Web proxy-inställningar**, och **tidsinställningar** är valfria. Hej krävs inställningarna är bara **Enhetsinställningar** och **Molninställningar**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image4.png)
4. I hello **nätverksinställningar** sidan **nätverksgränssnitt**, DATA 0 konfigureras automatiskt för dig. Varje nätverksgränssnitt är som standard tooget IP-adress automatiskt (DHCP). Därför måste tilldelas en IP-adress, nätmask och gateway automatiskt (för både IPv4 och IPv6).
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image5.png)
   
   Om du har lagt till fler än ett nätverksgränssnitt under hello etableringen av hello enhet kan konfigurera du dem här. Observera att du kan konfigurera din nätverksgränssnittet som IPv4 endast eller som både IPv4 och IPv6. Endast IPv6-konfigurationer stöds inte.
5. DNS-servrar krävs eftersom de används när enheten försöker toocommunicate med molntjänstleverantörer för lagring eller tooresolve enheten efter namn när konfigurerad som en filserver. I hello **nätverksinställningar** sidan under hello **DNS-servrar**:
   
   1. En primär och sekundär DNS-server konfigureras automatiskt. Om du väljer tooconfigure statiska IP-adresser, kan du ange DNS-servrar. För hög tillgänglighet rekommenderar vi att du konfigurerar en primär och en sekundär DNS-server.
   2. Klicka på **tillämpa** tooapply och validera hello nätverksinställningar.
6. I hello **Enhetsinställningar** sidan:
   
   1. Tilldela en unik **namn** tooyour enhet. Det här namnet kan vara 1 och 15 tecken och får innehålla bokstäver, siffror och bindestreck.
   2. Klicka på hello **filserver** ikonen ![](./media/storsimple-virtual-array-deploy3-fs-setup/image6.png) för hello **typen** av enhet som du skapar. En filserver kan du toocreate delade mappar.
   3. När enheten är en filserver, måste toojoin hello enheten tooa domän. Ange en **domännamn**.
   4. Klicka på **Använd**.
7. En dialogruta visas. Ange autentiseringsuppgifter för domänen i hello angivet format. Klicka på kryssikonen hello. hello domänautentiseringsuppgifter verifieras. Ett felmeddelande visas om hello autentiseringsuppgifterna är felaktiga.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image7.png)
8. Klicka på **Använd**. Detta gäller och validera hello Enhetsinställningar.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image8.png)
   
   > [!NOTE]
   > Kontrollera att din virtuella matris är i sin egen organisationsenhet (OU) för Active Directory och inga grupprincipobjekt (GPO) är tillämpade tooit eller ärvt. En Grupprincip kan installera program, till exempel antivirusprogram på hello virtuella StorSimple-matris. Installera ytterligare programvara stöds inte och kan leda toodata skadas. 
   > 
   > 
9. (Valfritt) Konfigurera din webbproxyserver. Även om webbproxykonfigurationen är valfri, Tänk på att om du använder en webbproxy, du kan bara konfigurera den här.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image9.png)
   
   I hello **Web proxy** sidan:
   
   1. Ange hello **Web proxy URL** i det här formatet: *http://&lt;värd-IP-adress eller FDQN&gt;: portnummer*. Observera att HTTPS-URL: er inte stöds.
   2. Ange **autentisering** som **grundläggande** eller **ingen**.
   3. Om autentisering med måste du också tooprovide en **användarnamn** och **lösenord**.
   4. Klicka på **Använd**. Detta validera och tillämpa hello konfigurerats web proxy-inställningar.
10. (Valfritt) Konfigurera hello tidsinställningar för din enhet, till exempel tidszon och hello primära och sekundära NTP-servrar. NTP-servrar krävs eftersom din enhet måste synkronisera tiden så att den kan autentisera med molntjänstleverantören.
    
    ![](./media/storsimple-virtual-array-deploy3-fs-setup/image10.png)
    
    I hello **tidsinställningar** sidan:
    
    1. Välj hello hello listrutan **tidszon** baserat på hello geografiska plats i vilka hello enheten ska distribueras. hello Standardtidszon för din enhet är PST. Enheten använder den här tidszonen för alla schemalagda åtgärder.
    2. Ange en **primär NTP-server** för din enhet eller godkänner time.windows.com hello standardvärdet. Se till att ditt nätverk tillåter att NTP-trafik toopass från ditt datacenter toohello Internet.
    3. Om du vill ange en **sekundära NTP-server** för din enhet.
    4. Klicka på **Använd**. Detta validera och tillämpa hello konfigurerats tidsinställningar.
11. Konfigurera hello molninställningar för din enhet. I det här steget kommer du att slutföra hello lokala enhetskonfiguration och registrera sedan hello enhet med StorSimple Device Manager-tjänsten.
    
    1. Ange hello **nyckel för tjänstregistrering** som du har fått [steg 2: hämta hello nyckel för tjänstregistrering](storsimple-virtual-array-deploy1-portal-prep.md#step-2-get-the-service-registration-key) för virtuell StorSimple-matrisen.
    2. Om det här är din första enhet som registreras med den här tjänsten visas med hello **krypteringsnyckel för tjänstdata**. Kopiera den här nyckeln och spara den på säker plats. Den här nyckeln är obligatorisk med hello service registrering viktiga tooregister ytterligare enheter med hello StorSimple enheten Manager-tjänsten. 
       
       Om detta inte är hello första enhet som du registrerar med den här tjänsten behöver tooprovide hello krypteringsnyckel för tjänstdata. Mer information finns i tooget hello [krypteringsnyckel för tjänstdata](storsimple-ova-web-ui-admin.md#get-the-service-data-encryption-key) på lokalt webbgränssnitt.
    3. Klicka på **registrera**. Hello enheten startas om. Du kan behöva toowait 2-3 minuter innan hello enheten har registrerats. Hello enheten har startats om, tas du toohello inloggning på sidan.
       
       ![](./media/storsimple-virtual-array-deploy3-fs-setup/image13.png)
12. Returnera toohello Azure-portalen. Gå för**alla resurser**, Sök efter StorSimple Device Manager-tjänsten.
    
    ![](./media/storsimple-virtual-array-deploy3-fs-setup/searchdevicemanagerservice1.png) 
13. I hello filtrerade väljer StorSimple Device Manager-tjänsten och sedan navigera för**Management > enheter**. I hello **enheter** bladet Kontrollera hello enheten har lyckats ansluta toohello service och har status hello **klar tooset in**.
    
    ![Konfigurera en filserver](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs2m.png)

## <a name="step-2-configure-hello-device-as-file-server"></a>Steg 2: Konfigurera hello-enhet som filserver
Utför följande steg i hello hello [Azure-portalen](https://portal.azure.com/) toocomplete hello krävs Enhetsinställningar.

#### <a name="tooconfigure-hello-device-as-file-server"></a>tooconfigure hello enheten som filserver
1. Gå tooyour StorSimple enheten Manager-tjänsten och sedan gå för **Management > enheter**. I hello **enheter** bladet, Välj hello-enhet som du nyss skapade. Den här enheten skulle visas som **klar tooset in**.
   
   ![Konfigurera en filserver](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs2m.png) 
2. Klicka på hello enheten och du kommer att se Banderollmeddelandet som visar hello enheten är klar toosetup.
   
    ![Konfigurera en filserver](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs3m.png)
3. Klicka på **konfigurera** i hello kommandofält. Detta öppnar hello **konfigurera** bladet. I hello **konfigurera** bladet hello följande:
   
    1. Hej filservernamnet fylls i automatiskt.
    
    2. Se till att hello molnet lagringskryptering anges för**aktiverad**. Detta kommer att kryptera alla hello-data som skickas toohello moln. 
    
    3. En 256-bitars AES-nyckel används med hello användardefinierade nyckeln för kryptering. Ange en nyckel med 32 tecken och sedan registrera hello viktiga tooconfirm den. Posten hello nyckeln i en app för hantering av nycklar för framtida bruk.
    
    4. Klicka på **konfigurera nödvändiga inställningar** toospecify lagringskonto autentiseringsuppgifter toobe används med din enhet. Klicka på **Lägg till ny** om det finns ingen lagringskontouppgifter som har konfigurerats. **Se till att hello storage-konto du använder stöder blockblobbar. Sidblobbar stöds inte.** Mer information om [blockerar blobbar och sidblobbar](https://docs.microsoft.com/rest/api/storageservices/understanding-block-blobs--append-blobs--and-page-blobs).
   
    ![Konfigurera en filserver](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs6m.png) 
4. I hello **lägga till en lagringskontouppgifter** bladet hello följande: 

    1. Välj aktuell prenumeration om hello storage-konto i hello samma prenumeration som hello-tjänst. Ange andra är hello storage-kontot är utanför hello prenumeration på tjänsten. 
    
    2. Välj ett befintligt lagringskonto hello listrutan. 
    
    3. hello plats automatiskt fylls i baserat på angivna hello storage-konto. 
    
    4. Aktivera SSL tooensure ett säkert nätverk kommunikationskanalen mellan hello enheten och hello molnet.
    
    5. Klicka på **Lägg till** tooadd lagringen konto autentiseringsuppgifter. 
   
        ![Konfigurera en filserver](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs8m.png)

5. När hello lagring kontoautentisering har skapats, hello **konfigurera** bladet uppdateras toodisplay hello angivna lagringskontouppgifter. Klicka på **Konfigurera**.
   
   ![Konfigurera en filserver](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs11m.png)
   
   Du ser en som en fil servern håller på att skapas. När hello filservern har skapats, du får ett meddelande.
   
   ![Konfigurera en filserver](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs13m.png)
   
   hello enhetens status ändras också för**Online**.
   
   ![Konfigurera en filserver](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs14m.png)
   
   Du kan fortsätta tooadd en resurs.

## <a name="step-3-add-a-share"></a>Steg 3: Lägg till en resurs
Utför följande steg i hello hello [Azure-portalen](https://portal.azure.com/) toocreate en resurs.

#### <a name="toocreate-a-share"></a>toocreate en resurs
1. Välj hello filen server-enhet som du konfigurerade i föregående steg hello och klicka på **...**  (eller högerklicka). Hello snabbmenyn Välj **Lägg till resursen**. Du kan också klicka **+ Lägg till resurs** i hello kommandofält för enheten.
   
   ![Lägg till en resurs](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs15m.png)
2. Ange följande delningsinställningar hello:

    1. Ett unikt namn för din resurs. hello namn måste vara en sträng som innehåller 3 too127 tecken.
    
    2. En valfri **beskrivning** för hello resurs. hello beskrivning hjälper dig att identifiera hello resurs-ägare.
    
    3. En **typen** för hello resurs. hello typ kan vara **skiktindelade** eller **lokalt Fäst**, med nivåindelade som hello standard. Arbetsbelastningar som kräver lokala garantier, låg latens och hög prestanda, Välj en **lokalt Fäst** delar. Alla andra data, Välj en **skiktindelade** delar.
    En lokalt Fäst resurs etableras tjockt, vilket och säkerställer att hello primära data på hello resursen förblir lokala toohello enhet och inte läcker över toohello moln. En skiktad resurs på hello andra sidan tunt allokerad. När du skapar en nivåindelad resurs 10% av hello utrymme har etablerats på hello lokala nivån och 90% av hello utrymme har etablerats i hello molnet. Till exempel om du har etablerat en volym 1 TB, 100 GB skulle finns i hello lokalt utrymme och 900 GB ska användas i hello molnet när hello datanivåer. I sin tur innebär detta att du inte kan etablera en skiktad resurs om du kör alla hello lokala slut på hello enhet.
   
    4. I hello **Ange standard fullständig behörighet** fältet, tilldela hello behörigheter toohello användare eller hello-grupp som har åtkomst till resursen. Ange hello namn hello användaren eller användargruppen för hello i  *john@contoso.com*  format. Vi rekommenderar att du använder en användare grupp (i stället för en enskild användare) tooallow admin privilegier tooaccess dessa resurser. När du har tilldelat hello behörigheter här kan kan du sedan använda Utforskaren toomodify dessa behörigheter.
   
    5. Klicka på **Lägg till** toocreate hello resursen. 
    
        ![Lägg till en resurs](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs18m.png)
   
        Du meddelas att hello resursen skapas.
   
        ![Lägg till en resurs](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs19m.png)
   
    När hello resursen har skapats med hello angetts inställningar, hello **resurser** bladet uppdateras tooreflect hello ny resurs. Som standard aktiveras övervakning och säkerhetskopiering för hello resursen.
   
    ![Lägg till en resurs](./media/storsimple-virtual-array-deploy3-fs-setup/deployfs22m.png)

## <a name="step-4-connect-toohello-share"></a>Steg 4: Anslut toohello resursen
Du måste nu tooconnect tooone eller flera resurser som du skapade i föregående steg i hello. Utför de här stegen på din Windows Server-värden som är anslutna tooyour virtuella StorSimple-matris.

#### <a name="tooconnect-toohello-share"></a>tooconnect toohello resursen
1. Tryck på ![](./media/storsimple-virtual-array-deploy3-fs-setup/image22.png) + R. Ange hello i hello körningsfönstret *&#92; &#92;&lt; filservernamnet&gt;*  som sökväg för hello ersätter *filservernamnet* namnet du tilldelade tooyour filservern med hello enhet. Klicka på **OK**.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image23.png)
2. Detta öppnar Utforskaren. Nu bör du kunna toosee hello resurser som du skapade som mappar. Välj och dubbelklicka på en resurs (mapp) tooview hello innehåll.
   
   ![](./media/storsimple-virtual-array-deploy3-fs-setup/image24.png)
3. Du kan nu lägga till toothese filresurser och gör en säkerhetskopia.

## <a name="next-steps"></a>Nästa steg
Lär dig hur toouse hello lokala webbgränssnittet för[administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).

