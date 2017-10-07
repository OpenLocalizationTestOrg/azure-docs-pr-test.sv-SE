---
title: "aaaLoad utjämning på flera IP-konfigurationer i Azure | Microsoft Docs"
description: "Belastningsutjämning mellan primära och sekundära IP-konfigurationer."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: na
ms.assetid: 244907cd-b275-4494-aaf7-dcfc4d93edfe
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: 8619493b8102e9d158d428fe6c59ecf3f32edc32
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-balancing-on-multiple-ip-configurations-using-hello-azure-portal"></a>Belastningsutjämning på flera IP-konfigurationer hello Azure-portalen

> [!div class="op_single_selector"]
> * [Portal](load-balancer-multiple-ip.md)
> * [PowerShell](load-balancer-multiple-ip-powershell.md)
> * [CLI](load-balancer-multiple-ip-cli.md)

Den här artikeln beskriver hur toouse Azure belastningsutjämnaren med flera IP-adresser på sekundärt nätverksgränssnitt (NIC). I det här scenariot har vi två virtuella datorer som kör Windows med en primär och sekundär NIC. Varje sekundär hello nätverkskort har två IP-konfigurationer. Varje virtuell värd för både webbplatser contoso.com och fabrikam.com. Varje webbplats är bundna tooone hello IP-konfigurationer på hello sekundära nätverkskortet. Vi använder Azure belastningsutjämnare tooexpose två klientdelens IP-adresser, en för varje webbplats toodistribute trafik toohello respektive IP-konfiguration för hello webbplats. Det här scenariot använder hello samma portnummer över både frontends som båda backend poolen IP-adresser.

![LB scenariot bild](./media/load-balancer-multiple-ip/lb-multi-ip.PNG)

##<a name="prerequisites"></a>Krav
Det här exemplet förutsätter att du har en resursgrupp med namnet *contosofabrikam* med hello följande konfiguration:
 -  innehåller ett virtuellt nätverk med namnet *myVNet*, två virtuella datorer kallas *VM1* och *VM2* hello respektive i samma tillgänglighetsuppsättning namngivna *myAvailset*. 
 - varje virtuell dator har en primär NIC och ett sekundärt nätverkskort. hello primära nätverkskort är namngivna *VM1NIC1* och *VM2NIC1* och hello sekundär nätverkskort namnges *VM1NIC2* och *VM2NIC2*. Mer information om hur du skapar virtuella datorer med flera nätverkskort finns [skapa en virtuell dator med flera nätverkskort som använder PowerShell](../virtual-network/virtual-network-deploy-multinic-arm-ps.md).

## <a name="steps-tooload-balance-on-multiple-ip-configurations"></a>Steg tooload saldot på flera IP-konfigurationer

Följ anvisningarna nedan tooachieve hello scenario som beskrivs i den här artikeln:

### <a name="step-1-configure-hello-secondary-nics-for-each-vm"></a>STEG 1: Konfigurera hello sekundära nätverkskort för varje virtuell dator

För varje virtuell dator i ditt virtuella nätverk, Lägg till ange IP-konfiguration för hello sekundära NIC på följande sätt:  

1. Från en webbläsare navigerar toohello Azure-portalen: http://portal.azure.com och logga in med ditt Azure-konto.
2. På hello övre vänstra sidan av hello-skärmen, klicka på ikonen för hello resursgrupp och klickar på hello resurs grupp hello virtuella datorer finns i (till exempel *contosofabrikam*). Hej **resursgrupper** blad som visar en lista över alla hello resurser tillsammans med hello nätverksgränssnitt för hello virtuella datorer visas nu.
3. toohello sekundära nätverkskort på varje virtuell dator, lägger du till en IP-konfiguration enligt följande:
    1. Välj hello nätverksgränssnitt som du vill tooadd hello IP-konfiguration.
    2. I hello bladet som visas för hello NIC som du har valt klickar du på **IP-konfigurationer**. Klicka på **Lägg till** mot hello överkant hello bladet som visas.
    3. I hello **lägga till IP-konfigurationer** bladet Lägg till en andra IP-konfiguration toohello NIC enligt följande: 
        1. Skriv ett namn för din sekundära IP-konfiguration (till exempel för VM1 och VM2 namnet hello IP-konfigurationer som *VM1NIC2 ipconfig2* och *VM2NIC2 ipconfig2* respektive).
        2. För **privata IP-adressen**, för **allokering**väljer **statiska**.
        3. Klicka på **OK**.
        4. När hello andra IP-konfiguration för hello sekundära NIC är klar visas den i hello **IP-konfigurationer** inställningsbladet för hello angivna nätverkskortet.

### <a name="step-2-create-a-load-balancer"></a>STEG 2: Skapa en belastningsutjämnare

Skapa en belastningsutjämnare enligt följande:

1. Från en webbläsare navigerar toohello Azure-portalen: http://portal.azure.com och logga in med ditt Azure-konto.
2. Hello övre vänstra sidan av hello-skärmen, klicka på **ny** > **nätverk** > **belastningsutjämnaren**. Klicka sedan på **skapa**.
3. I hello **skapa belastningsutjämnaren** bladet, ange ett namn för din belastningsutjämnare. Här kallas *mylb*.
4. Skapa en ny offentlig IP-adress anropas under offentliga IP-adress, **PublicIP1**.
5. Under resursgrupp, Välj hello befintlig resursgrupp för din virtuella dator (till exempel *contosofabrikam*). Sedan, Välj en lämplig plats och klicka sedan på **OK**. hello belastningsutjämnaren startar sedan toodeploy och tar några minuter toosuccessfully slutförd distribution.
6. När har distribuerats, visas hello belastningsutjämnaren som en resurs i resursgruppen.

### <a name="step-3-configure-hello-frontend-ip-pool"></a>STEG 3: Konfigurera hello klientdelens IP-pool

Konfigurera klientdelens IP-pool för varje webbplats (Contoso och Fabrikam) enligt följande:

1. I hello-portalen klickar du på **fler tjänster** > typen **offentliga IP-adressen** i hello filterfältet och klicka sedan på **offentliga IP-adresser**. Klicka på **Lägg till** mot hello överkant hello bladet som visas.
2. Konfigurera två offentliga IP-adresser (*PublicIP1* och *PublicIP2*) för båda webbplatserna (contoso och fabrikam) på följande sätt:
    1. Ange ett namn för din klientdelens IP-adress.
    2. För **resursgruppen**, Välj hello befintlig resursgrupp för hello virtuella datorer (till exempel *contosofabrikam*).
    3. För **plats**, Välj hello samma plats som hello virtuella datorer.
    4. Klicka på **OK**.
    5. När hello två offentliga IP-adresser har skapat de båda visas i hello **offentliga IP-Adressen** adresser bladet.
3. I hello-portalen klickar du på **fler tjänster** > typen **belastningsutjämnare** i hello filterfältet och klicka sedan på **belastningsutjämnare**.  
4. Välj hello belastningsutjämnare (*mylb*) som du vill tooadd hello klientdelens IP-poolen till.
5. Under **inställningar**väljer **klientdel pooler**. Klicka på **Lägg till** mot hello överkant hello bladet som visas.
6. Skriv ett namn för din klientdelens IP-adress (*farbikamfe* eller **contosofe*).
7. Klicka på **IP-adress** och på hello **Välj offentlig IP-adress** bladet, Välj hello IP-adresser för dina klientdel (*PublicIP1* eller *PublicIP2*).
8. Upprepa steg 3 too7 i det här avsnittet toocreate hello andra klientdelens IP-adressen.
9. När hello klientdelens IP-adresspool konfigurationen är klar visas båda klientdelens IP-adresser i hello **Klientdelens IP-Pool** bladet för din belastningsutjämnare. 
    
### <a name="step-4-configure-hello-backend-pool"></a>STEG 4: Konfigurera hello serverdelspool   
Konfigurera hello serverdelsadresspooler din belastningsutjämnare för varje webbplats (Contoso och Fabrikam) enligt följande:
        
1. I hello-portalen klickar du på **fler tjänster** > Skriv belastningsutjämnare i hello filtret och klicka sedan på **belastningsutjämnare**.  
2. Välj hello belastningsutjämnare (*mylb*) som du vill tooadd hello serverdelspooler till.
3. Under **inställningar**väljer **Serverdelspooler**. Skriv ett namn för din serverdelspool (till exempel *contosopool* eller *fabrikampool*). Klicka på hello **Lägg till** knappen i hello övre delen av hello bladet som visas. 
4. För **som är associerade med**väljer **tillgänglighetsuppsättning**.
5. För **tillgänglighetsuppsättning**väljer **myAvailset**.
6. Lägg till mål-IP-nätverkskonfigurationer, för både virtuella datorer på följande sätt (se bild 2):  
    1. För **mål virtuell dator**, Välj hello VM som du vill tooadd toohello serverdelspool (till exempel VM1 eller VM2).
    2. För **nätverks-IP-konfiguration**väljer hello sekundära nätverkskort IP-konfigurationen för den virtuella datorn (till exempel VM1NIC2 ipconfig2 eller VM2NIC2 ipconfig2).
    ![LB scenariot bild](./media/load-balancer-multiple-ip/lb-backendpool.PNG)
            
        **Bild 2**: Konfigurera hello belastningsutjämnare med backend-pooler  
7. Klicka på **OK**.
8. När hello backend poolen konfigurationen är klar visas båda serverdelsadresspooler i hello **Backend poolen bladet** av din belastningsutjämnare.

### <a name="step-5-configure-a-health-probe-for-your-load-balancer"></a>STEG 5: Konfigurera en hälsoavsökningen för din belastningsutjämnare
Konfigurera en hälsoavsökningen för din belastningsutjämnaren på följande sätt:
    1. I hello-portalen klickar du på fler tjänster > Skriv belastningsutjämnare i hello filtret och klicka sedan på **belastningsutjämnare**.  
    2. Välj hello belastningsutjämnare som du vill tooadd hello serverdelspooler till.
    3. Under **inställningar**väljer **hälsoavsökningen**. Klicka på **Lägg till** mot hello överkant hello bladet som visas.
    4. Skriv ett namn för hello hälsoavsökningen (till exempel HTTP) och klicka på **OK**.

### <a name="step-6-configure-load-balancing-rules"></a>STEG 6: Konfigurera regler för belastningsutjämning
Konfigurera regler för belastningsutjämning (*HTTPc* och *HTTPf*) för varje webbplats på följande sätt:
    
1. Under **inställningar**väljer **hälsoavsökningen**. Klicka på **Lägg till** mot hello överkant hello bladet som visas.
2. För **namn**, Skriv ett namn för regeln för belastningsutjämning hello (till exempel *HTTPc* för Contoso, eller *HTTPf* för Fabrikam)
3. Välj hello hello klientdelens IP-adress för Frontend-IP-adress (till exempel *Contosofe* eller *Fabrikamfe*)
4. För **Port** och **serverdelsport**, Behåll standardvärdet hello **80**.
5. För **flytande IP (direkt serverreturnering)**, klickar du på **aktiverad**.
6. Klicka på **OK**.
7. Upprepa steg 1 too6 i avsnittet toocreate hello andra belastningsutjämnare regeln.
8. Hello belastningsutjämning regler när konfigurationen är klar, både regler ((*HTTPc* och *HTTPf*) visas i hello **belastningsutjämningsregler** bladet för din belastningsutjämnare.

### <a name="step-7-configure-dns-records"></a>STEG 7: Konfigurera DNS-poster
Slutligen måste du konfigurera DNS-resurs registrerar toopoint toohello respektive klientdel IP-adressen för hello belastningsutjämnaren. Du kan vara värd för dina domäner i Azure DNS. Mer information om hur du använder Azure DNS med belastningsutjämnaren finns [med hjälp av Azure DNS med andra Azure-tjänster](../dns/dns-for-azure-services.md).

## <a name="next-steps"></a>Nästa steg
- Mer information om hur toocombine belastningsutjämning services i Azure i [med belastningsutjämning tjänster i Azure](../traffic-manager/traffic-manager-load-balancing-azure.md).
- Lär dig hur du kan använda olika typer av loggar i Azure toomanage och felsöka belastningsutjämnare i [logga analytics för Azure belastningsutjämnare](../load-balancer/load-balancer-monitor-log.md).
