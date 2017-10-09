---
title: aaaManaging roller i Azure cloud services med Visual Studio | Microsoft Docs
description: "Lär dig hur tooadd och ta bort roller i Azure cloud services med Visual Studio."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5ec9ae2e-8579-4e5d-999e-8ae05b629bd1
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 131edc534d1110ba3d25cd00a3a24b643576875c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-roles-in-azure-cloud-services-with-visual-studio"></a>Hantera roller i Azure-molntjänster med Visual Studio
När du har skapat din Azure-molntjänst kan du lägga till nya roller tooit eller ta bort befintliga roller från den. Du kan också importera ett befintligt projekt och konvertera det tooa roll. Du kan till exempel importera ASP.NET-webbprogram och ange det som en webbroll.

## <a name="adding-a-role-tooan-azure-cloud-service"></a>Lägga till en roll tooan Azure-molntjänst
hello följande steg leder dig genom att lägga till ett webb eller arbetare rollen tooan Azure cloud service-projekt i Visual Studio.

1. Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.

1. I **Solution Explorer**, expandera hello projektnoden

1. Högerklicka på hello **roller** nod toodisplay hello snabbmenyn. Hello snabbmenyn, Välj **Lägg till**, Välj en befintlig webbroll eller worker-rollen från hello aktuella lösning eller skapa ett projekt för webb- eller worker-rollen. Du kan också Välj en lämplig, till exempel ett ASP.NET web application-projekt och associera det med ett rollprojekt.

    ![Menyn Alternativ tooadd en roll tooan Azure cloud service-projekt](media/vs-azure-tools-cloud-service-project-managing-roles/add-role.png)

## <a name="removing-a-role-from-an-azure-cloud-service"></a>Ta bort en roll från en Azure-molntjänst
hello hur följande steg du tar bort en webbplats eller arbetsprocess roll från ett Azure cloud service-projekt i Visual Studio.

1. Skapa eller öppna ett Azure cloud service-projekt i Visual Studio.

1. I **Solution Explorer**, expandera hello projektnoden

1. Expandera hello **roller** nod.

1. Högerklicka på hello nod du tooremove och, hello snabbmenyn väljer **ta bort**. 

    ![Menyn Alternativ tooadd roll-tooan Azure-molntjänst](media/vs-azure-tools-cloud-service-project-managing-roles/remove-role.png)

## <a name="readding-a-role-tooan-azure-cloud-service-project"></a>Readding en roll tooan Azure cloud service-projekt
Om du tar bort en roll från ditt molntjänstprojekt men vid ett senare tillfälle tooadd hello tillbaka rollen toohello projekt, endast hello rollen deklaration och grundläggande attribut, till exempel slutpunkter och diagnostik information har lagts till. Inga ytterligare resurser eller referenser läggs toohello `ServiceDefinition.csdef` fil eller toohello `ServiceConfiguration.cscfg` fil. Om du vill tooadd informationen måste toomanually lägga tillbaka det i dessa filer.

Till exempel kan du ta bort en webbtjänstrollen och du senare tooadd rollen tillbaka till din lösning. Om du gör det uppstår ett fel. tooprevent det här felet ska du har tooadd hello `<LocalResources>` element som visas i följande hello XML tillbaka till hello `ServiceDefinition.csdef` fil. Använd hello namnet på hello webbtjänstrollen som du har lagt till hello projekt som en del av hello namnattributet för hello  **<LocalStorage>**  element. I det här exemplet hello hello webbtjänstrollen heter **WCFServiceWebRole1**.

    <WebRole name="WCFServiceWebRole1">
        <Sites>
          <Site name="Web">
            <Bindings>
              <Binding name="Endpoint1" endpointName="Endpoint1" />
            </Bindings>
          </Site>
        </Sites>
        <Endpoints>
          <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
          <Import moduleName="Diagnostics" />
        </Imports>
       <LocalResources>
          <LocalStorage name="WCFServiceWebRole1.svclog" sizeInMB="1000" cleanOnRoleRecycle="false" />
       </LocalResources>
    </WebRole>

## <a name="next-steps"></a>Nästa steg
- [Konfigurera hello roller för en Azure-molntjänst med Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md)
