---
title: aaaUnderstand delade IP-adresser i Azure DevTest Labs | Microsoft Docs
description: "Lär dig hur Azure DevTest Labs som använder delade IP-adresser toominimize hello offentliga IP-adresser krävs tooaccess ditt labb virtuella datorer."
services: devtest-lab
documentationcenter: na
author: camsoper
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: casoper
ms.openlocfilehash: 8756410117a9d550d567d372174bf1ea96703c74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a>Förstå delade IP-adresser i Azure DevTest Labs

Azure DevTest Labs kan lab VMs resursen hello samma offentliga IP-adress toominimize hello antal offentliga IP-adresser krävs tooaccess labbet enskilda virtuella datorer.  Den här artikeln beskriver hur delade IP-adresser arbete och deras relaterade konfigurationsalternativ.

## <a name="shared-ip-setting"></a>Delade IP-inställning

När du skapar ett labb som den finns i ett undernät i ett virtuellt nätverk.  Det här undernätet skapas som standard med **aktivera delad offentlig IP** ställa in också*Ja*.  Den här konfigurationen skapas en offentlig IP-adress för hello hela undernätet.  Mer information om hur du konfigurerar virtuella nätverk och undernät finns [konfigurera ett virtuellt nätverk i Azure DevTest Labs](devtest-lab-configure-vnet.md).

![Nytt labb undernät](media/devtest-lab-shared-ip/lab-subnet.png)

För befintliga testlabb kan du aktivera det här alternativet genom att välja **konfiguration och principer > virtuella nätverk**. Välj ett virtuellt nätverk hello listan och väljer sedan **aktivera DELAD offentlig IP-** för valda undernätet. Du kan också inaktivera det här alternativet i en labb om du inte vill tooshare en offentlig IP-adress i labbet virtuella datorer.

Virtuella datorer skapas i den här övningen standard toousing en delad IP-adress.  När du skapar hello VM är den här inställningen kan observeras i hello **avancerade inställningar** bladet under **IP-adresskonfiguration**.

![Ny virtuell dator](media/devtest-lab-shared-ip/new-vm.png)

- **Delade:** alla virtuella datorer skapas som **delade** placeras i en resursgrupp (RG). En IP-adress har tilldelats för som RG och alla virtuella datorer i hello RG ska använda IP-adressen.
- **Publik:** varje VM som du skapar har sin egen IP-adress och skapas i sin egen resursgruppen.
- **Privat:** varje virtuell dator som du skapar använder en privat IP-adress. Du kommer inte att kunna tooconnect toothis VM direkt från hello internet med fjärrskrivbord.

När en virtuell dator med delade IP är aktiverat läggs toohello undernät, lägger till hello VM tooa belastningsutjämnare automatiskt i DevTest Labs och tilldelar TCP-portnummer på hello offentlig IP-adress och vidarebefordrar toohello RDP-porten på hello VM.  

## <a name="using-hello-shared-ip"></a>Med hjälp av hello delade IP

- **Linux-användare:** SSH toohello VM med hjälp av hello IP-adress eller fullständigt domännamn, följt av ett kolon följt av hello port. Till exempel i hello bilden nedan, hello RDP-adress tooconnect toohello VM är `doclab-lab13998814308000.centralus.cloudapp.azure.com:51686`.

  ![VM-exempel](media/devtest-lab-shared-ip/vm-info.png)

- **Windows-användare:** väljer hello **Anslut** på hello Azure portal toodownload förkonfigurerade RDP-filen och komma åt hello VM.

## <a name="next-steps"></a>Nästa steg

* [Definiera principer för labbet i Azure DevTest Labs](devtest-lab-set-lab-policy.md)
* [Konfigurera ett virtuellt nätverk i Azure DevTest Labs](devtest-lab-configure-vnet.md)





