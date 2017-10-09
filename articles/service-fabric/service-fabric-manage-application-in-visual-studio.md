---
title: aaaManage dina program i Visual Studio | Microsoft Docs
description: "Använda Visual Studio toocreate, utveckla, paket, distribuera och felsöka din Service Fabric-program och tjänster."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: timlt
editor: 
ms.assetid: c317cb7e-7eae-466e-ba41-6aa2518be5cf
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/07/2017
ms.author: mikkelhegn
ms.openlocfilehash: b2d5803d85e4f9645dcbece33a2208bc0955498d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-visual-studio-toosimplify-writing-and-managing-your-service-fabric-applications"></a>Använd Visual Studio toosimplify skrivning och hantera dina Service Fabric-program
Du kan hantera dina Azure Service Fabric-program och tjänster via Visual Studio. När du har [ställa in din utvecklingsmiljö](service-fabric-get-started.md), du kan använda Visual Studio toocreate Service Fabric-program, Lägg till tjänster eller paket, registrera och distribuerar program i klustret för lokal utveckling.

## <a name="deploy-your-service-fabric-application"></a>Distribuera Service Fabric-program
Som standard kombinerar hello följa stegen i en enkel åtgärd om du distribuerar ett program:

1. Skapa hello programpaket
2. Överför hello programmet paketet toohello avbildningsarkivet
3. Registrera hello programtyp
4. Att ta bort alla instanser av programmet körs
5. Skapa en instans av programmet

I Visual Studio, trycka på **F5** distribuerar ditt program och bifoga hello felsökare tooall programinstanser. Du kan använda **Ctrl + F5** toodeploy ett program utan felsökning eller du kan publicera lokala tooa eller kluster med hjälp av hello Publicera profil. Mer information finns i [publicera ett program tooa kluster med hjälp av Visual Studio](service-fabric-publish-app-remote-cluster.md).

### <a name="application-debug-mode"></a>Programmet felsökningsläge
Visual Studio tillhandahåller en egenskap som kallas **programmet felsökningsläge**, som styr hur du vill att Visual Studios toohandle programdistribution som en del av felsökning.

#### <a name="tooset-hello-application-debug-mode-property"></a>tooset hello programmet felsökningsläge egenskapen
1. På hello Service Fabric application projektets (*.sfproj) snabbmenyn väljer **egenskaper** (eller tryck på hello **F4** nyckel).
2. I hello **egenskaper** fönster, ange hello **programmet felsökningsläge** egenskapen.

![Egenskapen programmet Debug-läge][debugmodeproperty]

#### <a name="application-debug-modes"></a>Programmet felsökningslägen

1. **Uppdatera program** detta läge kan du ändra tooquickly och felsöka din kod och stöd för redigering av statiska filer när du felsöker. Det här läget fungerar bara om lokal utveckling klustret är i [1 nod läge](/service-fabric-get-started-with-a-local-cluster.md#one-node-and-five-node-cluster-mode).
2. **Ta bort programmet** orsaker hello programmet toobe tas bort när hello debug-sessionen avslutas.
3. **Automatisk uppgradering** hello program fortsätter toorun när hello debug-sessionen avslutas. hello behandlar nästa felsökningssessionen hello distribution som en uppgradering. hello uppgraderingsprocessen bevarar alla data som du angav i föregående felsökningssessionen.
4. **Hålla program** hello programmet håller körs i hello klustret när hello debug konsolsessionen avslutas. Hello början av hello nästa felsökningssessionen, tas hello programmet bort.

För **automatiskt uppgradera** bevaras data genom att använda hello-programfunktioner uppgradering av Service Fabric. Mer information om hur du uppgraderar program och hur du kan utföra en uppgradering i en verklig miljö finns [uppgradering av Service Fabric-programmet](service-fabric-application-upgrade.md).

## <a name="add-a-service-tooyour-service-fabric-application"></a>Lägg till en service tooyour Service Fabric-program
Du kan lägga till nya tjänster tooyour programmet tooextend dess funktioner.  tooensure att hello service ingår i ditt programpaket, lägga till hello tjänsten via hello **nya Fabric-tjänsten...**  menyalternativ.

![Lägg till en ny Service Fabric-tjänst][newservice]

Välj ett Service Fabric-projekt typen tooadd tooyour program och ange ett namn för hello-tjänsten.  Se [att välja ett ramverk för din tjänst](service-fabric-choose-framework.md) toohelp som du bestämmer dig för vilken tjänst skriver toouse.

![Välj ett Service Fabric-projektet typen tooadd tooyour tjänstprogram][addserviceproject]

hello ny tjänst läggs tooyour lösningen och befintliga programpaket. referenser för hello och en standardinstans för tjänsten kommer att tillagda toohello programmanifestet, orsakar hello service toobe skapas och igång hello nästa gång du distribuerar hello program.

![hello ny tjänst läggs tooyour programmanifestet][newserviceapplicationmanifest]

## <a name="package-your-service-fabric-application"></a>Paketera Service Fabric-program
toodeploy hello programmet och dess tjänster tooa kluster, måste toocreate programpaket.  hello paketet organiserar hello programmanifestet service manifest och andra nödvändiga filer i en viss layout.  Visual Studio konfigurerar och hanterar hello-paket i projektet hello-program-mappen i hello-pkg-katalogen.  Klicka på **paketet** från hello **programmet** snabbmenyn skapar eller uppdateringar hello programpaket.

## <a name="remove-applications-and-application-types-using-cloud-explorer"></a>Ta bort program och programtyper med Cloud Explorer
Du kan utföra grundläggande klusterhanteringsåtgärder inifrån Visual Studio med Cloud Explorer som du kan starta från hello **visa** menyn. Du kan till exempel ta bort program och avetablera programtyper på lokala eller fjärranslutna kluster.

![Ta bort ett program][removeapplication]

> [!TIP]
> Rikare hanteringsfunktioner för klustret, se [visualisera ditt kluster med Service Fabric Explorer](service-fabric-visualizing-your-cluster.md).
>
>

<!--Every topic should have next steps and links toohello next logical set of content tookeep hello customer engaged-->
## <a name="next-steps"></a>Nästa steg
* [Service Fabric programmodell](service-fabric-application-model.md)
* [Distribution av Service Fabric](service-fabric-deploy-remove-applications.md)
* [Hantera programparametrar för miljöer med flera](service-fabric-manage-multiple-environment-app-configuration.md)
* [Felsöka ditt Service Fabric-program](service-fabric-debugging-your-application.md)
* [Visualisera ditt kluster med hjälp av Service Fabric Explorer](service-fabric-visualizing-your-cluster.md)

<!--Image references-->
[addserviceproject]:./media/service-fabric-manage-application-in-visual-studio/addserviceproject.png
[manageservicefabric]: ./media/service-fabric-manage-application-in-visual-studio/manageservicefabric.png
[newservice]:./media/service-fabric-manage-application-in-visual-studio/newservice.png
[newserviceapplicationmanifest]:./media/service-fabric-manage-application-in-visual-studio/newserviceapplicationmanifest.png
[debugmodeproperty]:./media/service-fabric-manage-application-in-visual-studio/debugmodeproperty.png
[removeapplication]:./media/service-fabric-manage-application-in-visual-studio/removeapplication.png