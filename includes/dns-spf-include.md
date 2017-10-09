Avsändaren princip Framework (SPF) poster är används toospecifying vilka e-postservrar tillåts toosend e-post för ett visst domännamn.  Korrekt konfiguration av SPF-poster är viktiga tooprevent mottagare Markera din e-post som} skräppost'.

hello DNS RFC ursprungligen infördes en ny 'SPF-posttypen toosupport det här scenariot. toosupport äldre namnservrar de heller hello användning av hello TXT-posttypen toospecify SPF-poster.  Den här tvetydigheten ledde tooconfusion som löstes genom [RFC 7208](http://tools.ietf.org/html/rfc7208#section-3.1).  Detta anger att SPF-poster ska skapas med enbart hello TXT-posttypen, och den hello SPF-posttypen är föråldrad.

**SPF-poster som stöds av Azure DNS och ska skapas med hjälp av hello TXT-posttypen.** hello föråldrade SPF-posttypen stöds inte. När [importerar en DNS-zonfilen](../articles/dns/dns-import-export.md), SPF-poster som använder hello SPF posttyp konverteras toohello TXT-posttypen.
