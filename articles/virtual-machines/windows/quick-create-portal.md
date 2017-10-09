---
title: aaaAzure Snabbstart - skapa Windows VM-portalen | Microsoft Docs
description: "Azure snabbstart – skapa virtuell Windows-dator med Portal"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/15/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5646ad51244db6d214c0121d1f7cc45c59f9a78b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-virtual-machine-with-hello-azure-portal"></a>Skapa en virtuell Windows-dator med hello Azure-portalen

Du kan skapa virtuella Azure-datorer via hello Azure-portalen. Den här metoden ger ett webbläsarbaserat användargränssnitt för att skapa och konfigurera virtuella datorer och alla relaterade resurser. Den här snabbstartsguide genom att skapa en virtuell dator och installera en webbserver på hello VM.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

## <a name="log-in-tooazure"></a>Logga in tooAzure

Logga in toohello Azure-portalen på http://portal.azure.com.

## <a name="create-virtual-machine"></a>Skapa en virtuell dator

1. Klicka på hello **ny** knappen hittades på hello övre vänstra hörnet av hello Azure-portalen.

2. Välj **Compute**, och välj sedan **Windows Server 2016 Datacenter**. 

3. Ange information om hello virtuella datorn. hello-användarnamn och lösenord som anges här är används toolog i toohello virtuella datorn. När du är klar klickar du på **OK**.

    ![Ange grundläggande information om den virtuella datorn i hello portalblad](./media/quick-create-portal/create-windows-vm-portal-basic-blade.png)  

4. Välj en storlek för hello VM. toosee flera storlekar, Välj **visa alla** eller ändra hello **stöds disktyp** filter. 

    ![Skärmbild som visar storlekar på virtuella datorer](./media/quick-create-portal/create-windows-vm-portal-sizes.png)  

5. På inställningsbladet för hello, hålla hello standardvärden och klicka på **OK**.

6. Klicka på sammanfattningssidan hello **Ok** toostart hello distribution av virtuella datorer.

7. hello VM kommer att fästa toohello Azure portalens instrumentpanel. När hello distributionen är klar öppnas automatiskt hello VM sammanfattning bladet.


## <a name="connect-toovirtual-machine"></a>Ansluta toovirtual datorn

Skapa en anslutning till fjärrskrivbord toohello virtuell dator.

1. Klicka på hello **Anslut** knappen hello egenskaper för virtuell dator. En protokollfil för fjärrskrivbord (.rdp-fil) skapas och hämtas.

    ![Portal 9](./media/quick-create-portal/quick-create-portal/portal-quick-start-9.png) 

2. tooconnect tooyour VM, öppna hello ned RDP-filen. Om du uppmanas till detta klickar du på **Anslut**. På en Mac, behöver du en RDP-klient som den här [fjärrskrivbordsklient](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12) från hello Mac App Store.

3. Ange hello användarnamn och lösenord som du angav när du skapar hello virtuella datorn och klicka sedan på **Ok**.

4. Du kan få en certifikatvarning under hello inloggningsprocessen. Klicka på **Ja** eller **Fortsätt** tooproceed med hello-anslutning.


## <a name="install-iis-using-powershell"></a>Installera IIS med PowerShell

Starta PowerShell-session på hello virtuella datorn och kör följande kommando tooinstall IIS hello.

```powershell
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```

När du är klar avslutar hello RDP-session och returnerar hello VM-egenskaper i hello Azure-portalen.

## <a name="open-port-80-for-web-traffic"></a>Öppna port 80 för webbtrafik 

En nätverkssäkerhetsgrupp (NSG) säkrar ingående och utgående trafik. När en virtuell dator skapas från hello Azure-portalen, skapas en regel för inkommande trafik på port 3389 för RDP-anslutningar. Eftersom den här virtuella datorn är värd för en webbserver, måste en NSG regel toobe som skapats för port 80.

1. Hello virtuell dator, klicka hello namnet på hello **resursgruppen**.
2. Välj hello **nätverkssäkerhetsgruppen**. hello NSG kan identifieras med hello **typen** kolumn. 
3. På hello vänstra menyn under inställningar, **inkommande säkerhetsregler**.
4. Klicka på **Lägg till**.
5. Skriv **http** i fältet **Namn**. Kontrollera att **portintervall** anges too80 och **åtgärd** har angetts för**Tillåt**. 
6. Klicka på **OK**.


## <a name="view-hello-iis-welcome-page"></a>Visa hello välkomstsidan för IIS

Med IIS installerat och port 80 öppna tooyour VM, kan nu komma åt hello webbserver från hello internet. Öppna en webbläsare och ange hello offentliga IP-adress hello VM. hello offentliga IP-adressen finns på hello VM-blad i hello Azure-portalen.

![Standardwebbplatsen i IIS](./media/quick-create-powershell/default-iis-website.png) 

## <a name="clean-up-resources"></a>Rensa resurser

Ta bort hello resursgrupp, virtuell dator och alla relaterade resurser när de inte längre behövs. toodo så Välj hello resursgruppen hello virtuella bladet och klicka på **ta bort**.

## <a name="next-steps"></a>Nästa steg

I den här snabbstarten har du distribuerat en virtuell dator och en regel för nätverkssäkerhetsgrupp samt installerat en webbserver. toolearn mer om Azure-datorer, fortsätta toohello självstudier för virtuella Windows-datorer.

> [!div class="nextstepaction"]
> [Självstudier om virtuella Azure Windows-datorer](./tutorial-manage-vm.md)
