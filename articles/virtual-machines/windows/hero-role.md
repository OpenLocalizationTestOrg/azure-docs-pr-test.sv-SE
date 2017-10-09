---
title: "aaaInstall IIS på din första virtuella Windows-dator | Microsoft Docs"
description: "Experimentera med först virtuella Windows-dator genom att installera IIS och öppna port 80 med hello Azure-portalen."
keywords: 
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 6334ea45-db6b-4e08-abb5-1f98927ebc34
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/06/2016
ms.author: cynthn
ms.openlocfilehash: 7cfed6197df058c4569d111ee88961da7c6fe0b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a>Experimentera med att installera en roll på Windows VM
När du har din första virtuella dator (VM) in och körs, kan du gå vidare tooinstalling program och tjänster. Den här självstudiekursen kommer vi toouse Serverhanteraren på hello Windows Server VM tooinstall IIS. Sedan skapar vi en Nätverkssäkerhetsgrupp (NSG) med hjälp av hello Azure portal tooopen port 80 tooIIS trafik. 

Om du inte har skapat din första virtuella dator du bör gå tillbaka för[skapa din första Windows virtuell dator i hello Azure-portalen](../virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) innan du fortsätter med den här kursen.

## <a name="make-sure-hello-vm-is-running"></a>Se till att hello virtuella datorn körs
1. Öppna hello [Azure-portalen](https://portal.azure.com).
2. Hej hubbmenyn, klicka på **virtuella datorer**. Välj hello virtuella hello-listan.
3. Om hello status är **Stoppad (Deallocated)**, klicka på hello **starta** hello-knappen **Essentials** bladet för hello VM. Om hello status är **kör**, kan du flytta på toohello nästa steg.

## <a name="connect-toohello-virtual-machine-and-sign-in"></a>Ansluta toohello virtuella datorn och logga in
1. Hej hubbmenyn, klicka på **virtuella datorer**. Välj hello virtuella hello-listan.
2. Klicka på hello bladet för den virtuella datorn hello **Anslut**. Detta skapar och laddar ned en fil med Remote Desktop Protocol (RDP-fil) som fungerar som en genväg tooconnect tooyour dator. Du kanske vill toosave hello filen tooyour skrivbordet för enkel åtkomst. **Öppna** den här filen tooconnect tooyour VM.
   
    ![Skärmbild av hello Azure portal som visar hur tooconnect tooyour VM](./media/hero-role/connect.png)
3. Du får en varning om att hello RDP-filen kommer från en okänd utgivare. Detta är normalt. På hello fjärrskrivbordet i fönstret **Anslut** toocontinue.
   
    ![Skärmbild med ett varning som meddelar att utgivaren är okänd](./media/hero-role/rdp-warn.png)
4. I fönstret säkerhet för Windows hello hello typen hello användarnamn och lösenord för lokala hello-kontot som du skapade när du skapade VM. hello användarnamnet anges som *vmname*&#92; *användarnamnet*, klicka på **OK**.
   
    ![Skärmbild av att ange hello VM-namn, användarnamn och lösenord](./media/hero-role/credentials.png)
5. Du får en varning hello certifikatet kan inte verifieras. Detta är normalt. Klicka på **Ja** tooverify hello hello virtuella datorns identitet och slutföra inloggning.
   
   ![Skärmbild som visar ett meddelande möts verifierar hello identitet hello VM](./media/hero-role/cert-warning.png)

Om du kör i tootrouble när du försöker tooconnect, se [Felsöka fjärrskrivbord anslutningar tooa Windows-baserade virtuella Azure-datorn](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="install-iis-on-your-vm"></a>Installera IIS på den virtuella datorn
Nu när du är inloggad i toohello VM, kommer vi installera en serverroll så att du kan experimentera mer.

1. Öppna **Serverhanteraren** om den inte redan är öppen. Klicka på hello **starta** -menyn och klicka sedan på **Serverhanteraren**.
2. I **Serverhanteraren**väljer **lokal Server** från hello till vänster. 
3. Välj menyn hello **hantera** > **Lägg till roller och funktioner**.
4. I hello Lägg till roller och funktioner, på hello **installationstyp** väljer **rollbaserad eller funktionsbaserad installation**, och klicka sedan på **nästa**.
   
    ![Skärmbild som visar hello guiden Lägg till roller och funktioner på fliken för installationstyp](./media/hero-role/role-wizard.png)
5. Välj hello VM hello-serverpoolen och klicka på **nästa**.
6. På hello **serverroller** väljer **webbserver (IIS)**.
   
    ![Skärmbild som visar hello guiden Lägg till roller och funktioner på fliken för serverroller](./media/hero-role/add-iis.png)
7. I hello popup om att lägga till funktioner som behövs för IIS, se till att **ta med hanteringsverktyg** är markerad och klicka sedan på **Lägg till funktioner**. När popup-hello stängs klickar du på **nästa** i hello guiden.
   
    ![Skärmbild som visar popup-tooconfirm lägger till hello IIS-rollen](./media/hero-role/confirm-add-feature.png)
8. På hello funktioner klickar du på **nästa**.
9. På hello **webbserverroll (IIS)** klickar du på **nästa**. 
10. På hello **rolltjänster** klickar du på **nästa**. 
11. På hello **bekräftelse** klickar du på **installera**. 
12. När hello installationen är klar klickar du på **Stäng** för hello guiden.

## <a name="open-port-80"></a>Öppna port 80
Inkommande trafik via port 80 för VM-tooaccept måste du tooadd en inkommande regel toohello nätverkssäkerhetsgruppen. 

1. Öppna hello [Azure-portalen](https://portal.azure.com).
2. I **virtuella datorer** Välj hello VM som du skapade.
3. Markera hello inställningar för virtuella datorer **nätverksgränssnitt** och sedan väljer hello befintliga nätverksgränssnittet.
   
    ![Skärmbild som visar hello virtuella datorns inställning för hello nätverksgränssnitt](./media/hero-role/network-interface.png)
4. I **Essentials** hello nätverksgränssnitt, klickar du på hello **nätverkssäkerhetsgruppen**.
   
    ![Skärmbild som visar hello Essentials avsnittet för hello nätverksgränssnittet](./media/hero-role/select-nsg.png)
5. I hello **Essentials** bladet för hello NSG bör du ha en befintlig standard för inkommande trafik för **standard Tillåt rdp** vilket gör att du toolog i toohello VM. Du lägger till en annan regel för inkommande trafik tooallow IIS-trafik. Klicka på **Ingående säkerhetsregel**.
   
    ![Skärmbild som visar hello Essentials avsnittet hello NSG](./media/hero-role/inbound.png)
6. Klicka på **Lägg till** i **Ingående säkerhetsregler**.
   
    ![Skärmbild som visar hello knappen tooadd en säkerhetsregel](./media/hero-role/add-rule.png)
7. Klicka på **Lägg till** i **Ingående säkerhetsregler**. Typen **80** i hello portintervall och se till att **Tillåt** är markerad. Klicka på **OK** när du är klar.
   
    ![Skärmbild som visar hello knappen tooadd en säkerhetsregel](./media/hero-role/port-80.png)

Mer information om NSG: er, regler för inkommande och utgående, finns [Tillåt extern åtkomst tooyour VM med hjälp av hello Azure-portalen](nsg-quickstart-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

## <a name="connect-toohello-default-iis-website"></a>Ansluta toohello IIS-standardwebbplatsen
1. I hello Azure-portalen klickar du på **virtuella datorer** och välj sedan den virtuella datorn.
2. I hello **Essentials** bladet, kopiera ditt **offentliga IP-adressen**.
   
    ![Skärmbild som visar där toofind hello offentliga IP-adressen för den virtuella datorn](./media/hero-role/ipaddress.png)
3. Öppna en webbläsare och i hello adressfältet skriver du in din offentliga IP-adress så här: http://<publicIPaddress> och på **RETUR** toogo toothat adress.
4. Din webbläsare ska öppna hello standardwebbsidan för IIS. Det ser ut ungefär så här:
   
    ![Skärmbild som visar vilka IIS standardsida hello ser ut i en webbläsare](./media/hero-role/iis-default.png)

## <a name="next-steps"></a>Nästa steg
* Du kan också experimentera med [bifogar en datadisk](attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) tooyour virtuella datorn. Hårddiskar ökar den virtuella datorns lagringsutrymme.

