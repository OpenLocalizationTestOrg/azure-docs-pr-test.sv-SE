---
title: Application Insights tootroubleshoot anpassade principer - Azure AD B2C | Microsoft Docs
description: "hur toosetup Application Insights tootrace hello körning av anpassade principer"
services: active-directory-b2c
documentationcenter: 
author: saeedakhter-msft
manager: krassk
editor: parakhj
ms.assetid: 658c597e-3787-465e-b377-26aebc94e46d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 08/04/2017
ms.author: saeda
ms.openlocfilehash: c02d7178512c7f9e022385371c3effd4f8cb7726
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-collecting-logs"></a>Azure Active Directory B2C: Insamling av loggar

Den här artikeln innehåller steg för att samla in loggar från Azure AD B2C så att du kan felsöka problem med din anpassade principer.

>[!NOTE]
>För närvarande hello detaljerad aktivitetsloggar som beskrivs här är utformade **bara** tooaid i utvecklingen av anpassade principer. Använd inte utvecklingsläge i produktion.  Loggar samla in alla anspråk skickas tooand från hello identitetsleverantörer under utveckling.  Om används i produktionen ansvarar hello developer för personligt identifierbar information (privat identifierbar Information) som samlas in i hello App Insights logg som de äger.  Dessa detaljerade loggar samlas endast in när hello princip placeras på **UTVECKLINGSLÄGE**.


## <a name="use-application-insights"></a>Använda Application Insights

Azure AD B2C stöder en funktion för att skicka data tooApplication insikter.  Application Insights ger ett sätt toodiagnose undantag och visualisera prestandaproblem i programmet.

### <a name="setup-application-insights"></a>Installera Application Insights

1. Gå toohello [Azure-portalen](https://portal.azure.com). Se till att du är i hello-klient med din Azure-prenumeration (inte din Azure AD B2C-klient).
1. Klicka på **+ ny** i hello vänstra navigeringsmenyn.
1. Söka efter och välja **Programinsikter**, klicka på **skapa**.
1. Slutför hello formuläret och klickar på **skapa**. Välj **allmänna** för hello **programtyp**.
1. När hello resursen har skapats, öppnar du hello Application Insights-resurs.
1. Hitta **egenskaper** hello i vänster-menyn och klicka på den.
1. Kopiera hello **Instrumentation nyckeln** och spara den för hello nästa avsnitt.

### <a name="set-up-hello-custom-policy"></a>Ställ in hello anpassad princip

1. Öppna hello RP-filen (till exempel SignUpOrSignin.xml).
1. Lägg till följande attribut toohello hello `<TrustFrameworkPolicy>` element:

  ```XML
  DeploymentMode="Development"
  UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  ```

1. Om den inte redan finns, lägger du till en underordnad nod `<UserJourneyBehaviors>` toohello `<RelyingParty>` nod. Det måste finnas omedelbart efter hello`<DefaultUserJourney ReferenceId="YourPolicyName" />`
2. Lägg till följande nod som underordnad till hello hello `<UserJourneyBehaviors>` element. Se till att tooreplace `{Your Application Insights Key}` med hello **Instrumentation nyckeln** som du fick från Application Insights i hello föregående avsnitt.

  ```XML
  <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
  ```

  * `DeveloperMode="true"`Anger ApplicationInsights tooexpedite hello telemetri via hello process-pipelinen bra för utveckling, men begränsad vid stora volymer.
  * `ClientEnabled="true"`skickar hello ApplicationInsights klientskript för spårning av klientsidan och visa sidfel (behövs inte).
  * `ServerEnabled="true"`skickar hello befintliga UserJourneyRecorder JSON som en anpassad händelse tooApplication insikter.
Exempel:

  ```XML
  <TrustFrameworkPolicy
    ...
    TenantId="fabrikamb2c.onmicrosoft.com"
    PolicyId="SignUpOrSignInWithAAD"
    DeploymentMode="Development"
    UserJourneyRecorderEndpoint="urn:journeyrecorder:applicationinsights"
  >
    ...
    <RelyingParty>
      <DefaultUserJourney ReferenceId="YourPolicyName" />
      <UserJourneyBehaviors>
        <JourneyInsights TelemetryEngine="ApplicationInsights" InstrumentationKey="{Your Application Insights Key}" DeveloperMode="true" ClientEnabled="false" ServerEnabled="true" TelemetryVersion="1.0.0" />
      </UserJourneyBehaviors>
      ...
  </TrustFrameworkPolicy>
  ```

3. Överföra hello princip.

### <a name="see-hello-logs-in-application-insights"></a>Se hello loggar i Application Insights

>[!NOTE]
> Det finns en kort fördröjning (mindre än fem minuter) innan du kan se nya loggar i Application Insights.

1. Öppna hello Application Insights-resurs som du skapade i hello [Azure-portalen](https://portal.azure.com).
1. I hello **översikt** -menyn klickar du på **Analytics**.
1. Öppna en ny flik i Application Insights.
1. Här är en lista över frågor som du kan använda toosee hello loggar

| Fråga | Beskrivning |
|---------------------|--------------------|
spår | Visa alla hello-loggar som genereras av Azure AD B2C |
spår \| där tidsstämpel > ago(1d) | Visa alla hello-loggar som genereras av Azure AD B2C för hello sista dagen

hello poster kan vara långt.  Exportera tooCSV för en närmare titt.

Du kan lära dig mer om hello Analytics verktyget [här](https://docs.microsoft.com/azure/application-insights/app-insights-analytics).

>[!NOTE]
>hello community har utvecklat en användare resa viewer toohelp identitet utvecklare.  Den är inte stöds av Microsoft och göras tillgänglig strikt som-är.  Det läser från Application Insights-instans och innehåller en korrekt struktur för hello användaren resa händelser.  Du skaffar hello källkoden och distribuerar den i din egen lösning.

>[!NOTE]
>För närvarande hello detaljerad aktivitetsloggar som beskrivs här är utformade **bara** tooaid i utvecklingen av anpassade principer. Använd inte utvecklingsläge i produktion.  Loggar samla in alla anspråk skickas tooand från hello identitetsleverantörer under utveckling.  Om används i produktionen ansvarar hello developer för personligt identifierbar information (privat identifierbar Information) som samlas in i hello App Insights logg som de äger.  Dessa detaljerade loggar samlas endast in när hello princip placeras på **UTVECKLINGSLÄGE**.

[Github-lagringsplatsen för exempel på stöds inte anpassade princip och relaterade verktyg](https://github.com/Azure-Samples/active-directory-b2c-advanced-policies)



## <a name="next-steps"></a>Nästa steg

Utforska hello data i Application Insights toohelp du förstår hur hello identitet upplevelse Framework underliggande B2C fungerar toodeliver inträffar i din egen identitet.
