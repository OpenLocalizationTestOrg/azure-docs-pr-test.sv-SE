---
title: aaaHow toomanage service konfigurationer och profiler | Microsoft Docs
description: "Lär dig hur toowork med tjänsten konfigurationer och profiler konfigurationsfiler | som lagrar inställningar för hello distributionsmiljöer och publiceringsinställningar för molntjänster."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: 7da8c551-fb06-4057-b5c7-c77f4b39d803
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/11/2017
ms.author: kraigb
ms.openlocfilehash: 1dba9df2fa57fe94dacc90ae74b05ccdc28270c1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomanage-service-configurations-and-profiles"></a>Hur toomanage service konfigurationer och profiler
## <a name="overview"></a>Översikt
När du publicerar en tjänst i molnet, Visual Studio lagrar konfigurationsinformation i två typer av konfigurationsfiler: tjänsten konfigurationer och profiler. Tjänstkonfiguration (.cscfg-filer) sparar inställningar för hello distributionsmiljöer för en Azure-molntjänst. Azure använder konfigurationsfilerna vid hantering av dina molntjänster. Hej på andra sidan, profiler (.azurePubxml-filer) store publiceringsinställningar för molntjänster. Dessa inställningar är en post på vad du väljer när du använder hello Publiceringsguiden och används lokalt av Visual Studio. Det här avsnittet beskrivs hur toowork med båda typerna av konfigurationsfiler.

## <a name="service-configurations"></a>Tjänstkonfiguration
Du kan skapa flera service konfigurationer toouse för varje distributionsmiljöer. Du kan till exempel skapa en tjänstkonfiguration för hello lokala miljön om du använder toorun och testa ett Azure-program och en annan tjänstkonfigurationen för din produktionsmiljö.

Du kan lägga till, ta bort, byta namn på och ändra dessa konfigurationer baserat på dina krav. Du kan hantera dessa konfigurationer från Visual Studio som visas i följande illustration hello.

![Hantera konfigurationer](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-service-config.png)

Du kan också öppna hello **hantera konfigurationer** dialogruta från hello rollen egenskapssidor. tooopen hello egenskaper för en roll i projektet Azure öppna hello snabbmenyn för rollen och välj sedan **egenskaper**. På hello **inställningar** fliken, expandera hello **tjänstkonfiguration** listan och välj sedan **hantera** tooopen hello **hantera konfigurationer**dialogrutan.

### <a name="tooadd-a-service-configuration"></a>tooadd en tjänstkonfiguration
1. Öppna hello snabbmenyn för hello Azure-projekt i Solution Explorer och markera **hantera konfigurationer**.
   
    Hej **hantera tjänstkonfiguration** dialogrutan visas.
2. tooadd en tjänstkonfiguration, måste du skapa en kopia av en befintlig konfiguration. toodo, Välj hello konfiguration som du vill toocopy hello namnet listan och väljer sedan **Skapa kopia**.
3. (Valfritt) toogive hello tjänstkonfiguration ett annat namn, Välj hello ny tjänstkonfiguration hello namnlistan och välj sedan **Byt namn på**. I hello **namn** textruta, hello typnamn som du vill använda toouse för den här tjänstkonfigurationen och välj sedan **OK**.
   
    En ny service konfigurationsfil som heter ServiceConfiguration. [Nytt namn] .cscfg läggs toohello Azure-projekt i Solution Explorer.

### <a name="toodelete-a-service-configuration"></a>toodelete en tjänstkonfiguration
1. Öppna hello snabbmenyn för hello Azure-projekt i Solution Explorer och markera **hantera konfigurationer**.
   
    Hej **hantera tjänstkonfiguration** dialogrutan visas.
2. toodelete en tjänstkonfiguration Välj hello konfiguration som du vill toodelete från hello **namn** och väljer sedan **ta bort**. En dialogruta visas tooverify som du vill toodelete den här konfigurationen.
3. Välj **Ta bort**.
   
     Hej tjänstkonfigurationsfilen tas bort från hello Azure-projekt i Solution Explorer.

### <a name="toorename-a-service-configuration"></a>toorename en tjänstkonfiguration
1. Öppna hello snabbmenyn för hello Azure-projekt i Solution Explorer och markera sedan **hantera konfigurationer**.
   
    Hej **hantera tjänstkonfiguration** dialogrutan visas.
2. toorename en tjänstkonfiguration Välj hello ny tjänstkonfiguration hello **namn** listan och välj sedan **Byt namn på**. I hello **namn** textruta, hello namn du vill använda toouse för den här tjänstkonfigurationen och sedan väljer **OK**.
   
    hello namnet på hello tjänstkonfigurationsfilen ändras i hello Azure-projekt i Solution Explorer.

### <a name="toochange-a-service-configuration"></a>toochange en tjänstkonfiguration
* Om du vill toochange en tjänstkonfiguration, öppna hello snabbmenyn för hello serverroll du vill toochange i hello Azure-projekt och välj sedan **egenskaper**. Se [så här: Konfigurera hello roller för en Azure Cloud Service med Visual Studio](https://docs.microsoft.com/azure/vs-azure-tools-configure-roles-for-cloud-service) för mer information.

## <a name="make-different-setting-combinations-by-using-profiles"></a>Se annan inställning kombinationer med hjälp av profiler
Med hjälp av en profil kan du automatiskt fylla i hello **Publiceringsguiden** med olika kombinationer av inställningar för olika ändamål. Till exempel kan du ha en profil för felsökning och en annan för versionen bygger. I så fall din **felsöka** profil skulle ha **IntelliTrace** aktiverad och hello **felsöka** konfiguration som har valts och dina **versionen** profil skulle ha **IntelliTrace** inaktiverad och hello **versionen** konfiguration som valdes. Du kan också använda olika profiler toodeploy en tjänst med ett annat lagringskonto.

När du kör guiden hello för hello första gången, skapas en standardprofil. Visual Studio lagrar hello profil i en fil med filnamnstillägget .azurePubXml läggs tooyour Azure-projekt under hello **profiler** mapp. Om du manuellt ange olika alternativ när du kör guiden hello senare uppdateras automatiskt hello-filen. Innan du kör hello så bör du redan har publicerat din molntjänst minst en gång.

### <a name="tooadd-a-profile"></a>tooadd en profil
1. Öppna hello snabbmenyn för din Azure-projekt och välj sedan **publicera**.
2. Nästa toohello **mål profil** listan, Välj hello **spara profil** knappen som hello följande illustration visas. Detta skapar en profil för dig.
   
    ![Skapa en ny profil](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/create-new-profile.png)
3. Efter hello profilen har skapats, Välj **< hantera... >** i hello **mål profil** lista.
   
    Hej **hantera profiler** dialogruta visas som hello följande illustration visas.
   
    ![Hantera profiler dialogrutan](./media/vs-azure-tools-service-configurations-and-profiles-how-to-manage/manage-profiles.png)
4. I hello **namn** listan, väljer du en profil och väljer sedan **Skapa kopia**.
5. Välj hello **Stäng** knappen.
   
    hello nya profilen visas i hello mållistan profil.
6. I hello **mål profil** listan, Välj hello-profil som du nyss skapade. hello guiden Publicera inställningar fylls i med hello alternativ från hello profil du valt.
7. Välj hello **föregående** och **nästa** knappar toodisplay varje sida i hello publicera guiden och sedan anpassa hello inställningar för den här profilen. Se [Publiceringsguiden för Azure-program](http://go.microsoft.com/fwlink/p/?LinkID=623085) information.
8. När du är klar med inställningarna för hello Välj **nästa** toogo tillbaka toohello inställningssidan. hello profil sparas när du publicerar hello-tjänsten med hjälp av dessa inställningar eller om du väljer **spara** nästa toohello listan över profiler.

### <a name="toorename-or-delete-a-profile"></a>toorename eller ta bort en profil
1. Öppna hello snabbmenyn för din Azure-projekt och välj sedan **publicera**.
2. I hello **mål profil** väljer **hantera**.
3. I hello **hantera profiler** dialogrutan, Välj hello profil du vill toodelete och välj sedan **ta bort**.
4. I hello bekräftelsedialogrutan som visas väljer du **OK**.
5. Välj **Stäng**.

### <a name="toochange-a-profile"></a>toochange en profil
1. Öppna hello snabbmenyn för din Azure-projekt och välj sedan **publicera**.
2. I hello **mål profil** listan, Välj hello-profil som du vill toochange.
3. Välj hello **föregående** och **nästa** knappar toodisplay hello av varje sida i guiden Publicera och ändra hello inställningar som du vill. Se [Publiceringsguiden för Azure-program](http://go.microsoft.com/fwlink/p/?LinkID=623085) information.
4. När du har ändrat hello inställningar, väljer du **nästa** toogo tillbaka toohello **inställningar** sidan.
5. (Valfritt) Markera **publicera** toopublish hello tjänst i molnet med hjälp av hello nya inställningar. Om du inte vill toopublish Molntjänsten vid denna tidpunkt, och Stäng Hej guiden Publicera, Visual Studio frågan om du vill toosave hello ändringar toohello profil.

## <a name="next-steps"></a>Nästa steg
toolearn om hur du konfigurerar andra delar av din Azure-projekt från Visual Studio finns [konfigurera ett Azure-projekt](http://go.microsoft.com/fwlink/p/?LinkID=623075)

