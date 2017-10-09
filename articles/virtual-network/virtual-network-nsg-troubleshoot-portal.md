---
title: "aaaTroubleshoot Nätverkssäkerhetsgrupper - Portal | Microsoft Docs"
description: "Lär dig hur tootroubleshoot Nätverkssäkerhetsgrupper i hello Azure Resource Manager distribution modellen med hello Azure-portalen."
services: virtual-network
documentationcenter: na
author: AnithaAdusumilli
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: a54feccf-0123-4e49-a743-eb8d0bdd1ebc
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/23/2016
ms.author: anithaa
ms.openlocfilehash: 0d3d2110fe1507f36e3b933de924a0876db2747a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-network-security-groups-using-hello-azure-portal"></a>Felsöka Nätverkssäkerhetsgrupper med hello Azure-portalen
> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-nsg-troubleshoot-portal.md)
> * [PowerShell](virtual-network-nsg-troubleshoot-powershell.md)
> 
> 

Om du konfigurerade Nätverkssäkerhetsgrupper (NSG: er) på den virtuella datorn (VM) och har problem med anslutningen VM, för den här artikeln innehåller en översikt över diagnostikfunktionerna för NSG: er toohelp fortsätta felsökningen.

NSG: er kan du toocontrol hello typer av trafik som flöde till och från virtuella datorer (VM). NSG: er kan vara tillämpade toosubnets i ett Azure Virtual Network (VNet), nätverksgränssnitt (NIC) eller båda. hello effektiva reglerna tooa NIC är en sammanställning av hello regler som finns i hello NSG: er används tooa NIC och hello undernät, det är anslutet till. Regler för dessa NSG: er kan ibland står i konflikt med varandra och påverka nätverksanslutning för en virtuell dator.  

Du kan visa alla hello effektiva säkerhetsregler från dina NSG: er som tillämpas på den Virtuella datorns nätverkskort. Den här artikeln visar hur tootroubleshoot VM anslutningsproblem med dessa regler i hello Azure Resource Manager-distributionsmodellen. Om du inte är bekant med principerna för VNet och NSG läsa hello [för virtuella nätverk](virtual-networks-overview.md) och [Nätverkssäkerhetsgrupper](virtual-networks-nsg.md) översikt artiklar.

## <a name="using-effective-security-rules-tootroubleshoot-vm-traffic-flow"></a>Med hjälp av effektiva säkerhetsregler tootroubleshoot VM trafikflöde
hello-scenariot som följer är ett exempel på ett vanligt anslutningsproblem:

En virtuell dator med namnet *VM1* är en del av ett undernät med namnet *Undernät1* inom ett VNet med namnet *WestUS VNet1*. Ett försök tooconnect toohello VM som använder RDP över TCP-port 3389 misslyckas. NSG: er som tillämpas på båda hello NIC *VM1 NIC1* och hello undernät *Undernät1*. Trafik tooTCP port 3389 tillåts i hello NSG som är kopplad till nätverksgränssnittet hello *VM1 NIC1*, men TCP pinga tooVM1's port 3389 misslyckas.

När det här exemplet används TCP-port 3389, hello följande steg kan vara används toodetermine inkommande och utgående anslutningsfel via alla portar.

### <a name="vm"></a>Visa effektiva säkerhetsregler för en virtuell dator
Fullständig hello följande steg tootroubleshoot NSG: er för en virtuell dator:

Du kan visa en fullständig lista över hello effektiva säkerhetsregler på ett nätverkskort från hello Virtuella datorn. Du kan också lägga till, ändra och ta bort både nätverkskort och undernät NSG-regler från hello effektiva regler bladet, om du har behörigheter tooperform dessa åtgärder.

1. Inloggningen toohello Azure-portalen på https://portal.azure.com.
2. Klicka på **fler tjänster**, klicka på **virtuella datorer** i hello-lista som visas.
3. Markera en VM-tootroubleshoot hello listan som visas och en VM-bladet med alternativ visas.
4. Klicka på **diagnostisera & lösa problem** och välj sedan ett vanligt problem. I det här exemplet **jag kan inte ansluta toomy Windows VM** är markerad. 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image1.png)
5. Stegen visas under hello problem, som visas i följande bild hello: 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image2.png)
   
    Klicka på *effektiva regler för nätverkssäkerhetsgrupper* i hello lista över rekommenderade steg.
6. Hej **hämta effektiva säkerhetsregler** bladet visas som i följande bild hello:
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image3.png)
   
    Observera följande avsnitt i hello bild hello:
   
   * **Omfattning:** ställa in också*VM1*, hello VM valde i steg 3.
   * **Nätverksgränssnitt:** *VM1 NIC1* är markerad. En virtuell dator kan ha flera nätverksgränssnitt (NIC). Varje nätverkskort kan ha unika effektiva säkerhetsregler. När du felsöker kan behöva du tooview hello effektiva säkerhetsregler för varje nätverkskort.
   * **Associerade NSG: er:** NSG: er kan vara tillämpade tooboth hello NIC och hello undernät hello nätverkskortet är anslutet till. Hello bilden har en NSG tillämpad tooboth hello NIC och hello undernät den är ansluten till. Du kan klicka på hello NSG namn toodirectly ändra reglerna i hello NSG: er.
   * **Fliken VM1 nsg:** hello listan över regler som visas i bild hello är för hello NSG tillämpas toohello NIC. Flera standardregler skapas av Azure när en NSG skapas. Du kan inte ta bort hello standardregler, men du kan åsidosätta dem med regler med högre prioritet. Mer om standardregler läsa hello toolearn [NSG översikt](virtual-networks-nsg.md#default-rules) artikel.
   * **Målkolumnen:** vissa hello regler har text i hello kolumn, medan andra har adressprefix. hello text är standard märkningar toohello säkerhetsregeln hello namn när den skapades. hello-taggar är systemdefinierade identifierare som representerar flera prefix. Att markera en regel med en tagg som *AllowInternetOutBound*, visar hello prefix i hello **-adressprefix** bladet.
   * **Hämta:** hello listan över regler som kan vara långa. Du kan hämta en CSV-fil med hello regler för offlineanalys genom att klicka på **hämta** och spara hello-fil.
   * **AllowRDP** inkommande regel: den här regeln kan RDP-anslutningar toohello VM.
7. Klicka på hello **Undernät1 NSG** fliken tooview hello effektiva regler från hello NSG tillämpas toohello undernät, som visas i följande bild hello: 
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image4.png)
   
    Meddelande hello *denyRDP* **inkommande** regeln. Regler för inkommande trafik tillämpas på hello undernät utvärderas före regler som används vid hello nätverksgränssnitt. Eftersom hello neka-regel används på hello undernät, misslyckas hello begäran tooconnect tooTCP 3389, eftersom hello Tillåt-regel på hello NIC utvärderas aldrig. 
   
    Hej *denyRDP* regeln är hello orsak varför hello RDP-anslutning inte. Ta bort den bör lösa hello.
   
   > [!NOTE]
   > Om hello VM som är associerade med hello NIC är inte i ett körtillstånd eller NSG: er har inte tagits tillämpade toohello nätverkskort eller ett undernät, visas inga regler.
   > 
   > 
8. tooedit NSG-regler, klicka på *Undernät1 NSG* i hello **associerade NSG: er** avsnitt.
   Då öppnas hello **Undernät1 NSG** bladet. Du kan redigera hello regler direkt genom att klicka på **inkommande säkerhetsregler**.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image7.png)
9. När du tar bort hello *denyRDP* regel för inkommande trafik i hello **Undernät1 NSG** och lägga till en *allowRDP* regel hello effektiva regellistan ser ut hello följande bild:
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image8.png)
   
    Kontrollera att TCP-port 3389 är öppen genom att öppna en RDP-anslutning toohello VM eller hello PsPing verktyget. Du kan lära dig mer om PsPing genom att läsa hello [PsPing hämtningssidan](https://technet.microsoft.com/sysinternals/psping.aspx).

### <a name="nic"></a>Visa effektiva säkerhetsregler för ett nätverksgränssnitt
Om din VM trafikflöde påverkas för ett specifikt nätverkskort, kan du visa en fullständig lista över hello effektiva regler för hello NIC från hello nätverk gränssnitt kontexten genom att slutföra hello följande steg:

1. Inloggningen toohello Azure-portalen på https://portal.azure.com.
2. Klicka på **fler tjänster**, klicka på **nätverksgränssnitt** i hello-lista som visas.
3. Välj ett nätverksgränssnitt. I följande bild hello, ett nätverkskort med namnet *VM1 NIC1* är markerad.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image5.png)
   
    Observera att hello **omfång** anges toohello nätverksgränssnittet som valts. Mer om toolearn hello ytterligare information som visas, läs steg 6 i hello **felsöka NSG: er för en virtuell** i den här artikeln.
   
   > [!NOTE]
   > Om en NSG tas bort från ett nätverksgränssnitt hello undernät NSG är fortfarande gälla för hello angivna nätverkskortet. I det här fallet skulle hello utdata visar endast regler från hello undernät NSG. Reglerna visas bara om hello NIC bifogade tooa VM.
   > 
   > 
4. Du kan redigera regler direkt för NSG: er som är associerad med ett nätverkskort och ett undernät. toolearn hur, läsa steg 8 i hello **visa effektiva säkerhetsregler för en virtuell dator** i den här artikeln.

## <a name="nsg"></a>Visa effektiva säkerhetsregler för nätverkssäkerhetsgruppen (NSG)
När du ändrar NSG-regler kan du kanske vill tooreview hello effekten av hello regler som läggs till på en viss virtuell dator. Du kan visa en fullständig lista över hello effektiva säkerhetsregler för alla hello nätverkskort som en given NSG tillämpas på, utan att behöva tooswitch kontext från hello angivna NSG-bladet. tootroubleshoot effektiva regler inom en NSG fullständig hello följande steg:

1. Inloggningen toohello Azure-portalen på https://portal.azure.com.
2. Klicka på **fler tjänster**, klicka på **Nätverkssäkerhetsgrupper** i hello-lista som visas.
3. Välj en NSG. I följande bild hello, har en NSG som heter VM1 nsg valts.
   
    ![](./media/virtual-network-nsg-troubleshoot-portal/image6.png)
   
    Observera följande avsnitt i hello föregående bild hello:
   
   * **Omfattning:** ange toohello NSG som valts.
   * **Virtuell dator:** när en NSG är tillämpade tooa undernät, är det tillämpade tooall gränssnitt bifogade tooall VMs anslutna toohello undernät. I listan visas alla virtuella datorer som den här NSG tillämpas på. Du kan välja alla VM hello-listan.
     
     > [!NOTE]
     > Om en NSG tillämpad tooonly ett tomt undernät, visas inte virtuella datorer. Om en NSG tillämpad tooa nätverkskort som inte är associerad med en virtuell dator, visas dessa nätverkskort också inte. 
     > 
     > 
   * **Nätverksgränssnitt:** en virtuell dator kan ha flera nätverksgränssnitt. Du kan välja ett nätverksgränssnitt bifogade toohello valt VM.
   * **AssociatedNSGs:** när som helst ett nätverkskort kan ha upp tootwo effektiva NSG: er, en tillämpas toohello NIC och hello andra toohello undernät. Även om hello scope är markerad som VM1 nsg om hello NIC har en effektiv undernät NSG, visar hello utdata både NSG: er.
4. Du kan redigera regler direkt för NSG: er som är associerad med ett nätverkskort eller ett undernät. toolearn hur, läsa steg 8 i hello **visa effektiva säkerhetsregler för en virtuell dator** i den här artikeln.

Mer om toolearn hello ytterligare information som visas, läs steg 6 i hello **visa effektiva säkerhetsregler för en virtuell dator** i den här artikeln.

> [!NOTE]
> Även om ett undernät och ett nätverkskort kan ha en enda NSG tillämpas toothem, kan det vara en NSG associerade toomultiple nätverkskort och flera undernät.
> 
> 

## <a name="considerations"></a>Överväganden
Överväg följande punkter när du felsöker problem med nätverksanslutningen hello:

* Standard NSG-regler som blockerar inkommande åtkomst från hello internet och bara bevilja VNet inkommande trafik. Regler som uttryckligen läggas tooallow inkommande åtkomst från Internet, vid behov.
* Om det finns inga NSG säkerhetsregler som orsakar en virtuell dators network connectivity toofail, kan hello problemet bero på följande:
  * Brandväggsprogram körs i hello VM-operativsystem
  * Vägar som konfigurerats för virtuella installationer eller lokal trafik. Internet-trafiken kan vara omdirigerade tooon lokala via Tvingad tunneltrafik. En RDP/SSH-anslutning från hello Internet tooyour VM kanske inte fungerar med den här inställningen, beroende på hur hello lokalt nätverksmaskinvara hanterar den här trafiken. Läs hello [felsökning vägar](virtual-network-routes-troubleshoot-powershell.md) artikel toolearn hur toodiagnose väg problem som kan hindra hello trafikflödet i och ut ur hello VM. 
* Om du har peerkoppla Vnet, som standard, expandera hello VIRTUAL_NETWORK taggen automatiskt tooinclude prefix för peerkoppla Vnet. Du kan visa dessa prefix i hello **ExpandedAddressPrefix** lista, tootroubleshoot alla problem relaterade tooVNet peering anslutning. 
* Effektiva säkerhetsregler visas bara om det finns en NSG som är associerade med hello Virtuella nätverkskort och undernät. 
* Om det finns inga NSG: er som är associerade med hello NIC eller undernät och att du har en offentlig IP-adress som tilldelats tooyour VM, vara alla portar öppna för inkommande och utgående åtkomst. Om hello VM har en offentlig IP-adress, rekommenderas tillämpa NSG: er toohello NIC eller undernät.

