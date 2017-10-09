hello Domain Name System (DNS) är används toolocate saker på hello internet. Till exempel när du anger en adress i webbläsaren eller på en länk på en webbsida, används DNS tootranslate hello-domän i en IP-adress. hello IP-adressen är till exempel en gatuadress, men det är inte mycket mänsklig eget. Till exempel, är det mycket enklare tooremember ett DNS-namn som **contoso.com** än den som den är tooremember en IP-adress, till exempel 192.168.1.88 eller 2001:0:4137:1f67:24a2:3888:9cce:fea3.

hello DNS-systemet är baserad på *poster*. Poster associera en specifik *namn*, som **contoso.com**, med en IP-adress eller ett annat DNS-namn. När ett program, till exempel en webbläsare, letar efter ett namn i DNS, den söker efter hello-post och använder det pekar hello tooas adress. Om hello värde den punkter toois en IP-adress, använder hello webbläsare det värdet. Om den pekar tooanother DNS-namn, sedan hello programmet har toodo upplösning igen. Slutligen går all namnmatchning ut om en IP-adress.

När du skapar en Azure-webbplats kan tilldelas automatiskt toohello plats i ett DNS-namn. Det här namnet tar hello form av  **&lt;yoursitename&gt;. azurewebsites.net**. När du lägger till webbplatsen som en Azure Traffic Manager-slutpunkt webbplatsen sedan är tillgängligt via hello  **&lt;yourtrafficmanagerprofile&gt;. trafficmanager.net** domän.

> [!NOTE]
> När webbplatsen har konfigurerats som en Traffic Manager-slutpunkt, använder du hello **. trafficmanager.net** adressen när du skapar DNS-poster.
> 
> Du kan bara använda CNAME-poster med Traffic Manager
> 
> 

Det finns flera typer av poster med sina egna funktioner och begränsningar, men för webbplatser som är konfigurerade tooas Traffic Manager-slutpunkter, vi bara hand om en; *CNAME* poster.

### <a name="cname-or-alias-record"></a>Posten CNAME eller Alias
En CNAME-post mappar en *specifika* DNS-namn, exempelvis **mail.contoso.com** eller **www.contoso.com**, tooanother (kanoniska) domännamn. I Azure-webbplatser med Traffic Manager hello fallet hello kanoniska domännamnet är hello  **&lt;myapp >. trafficmanager.net** domännamnet för Traffic Manager-profilen. När du skapat hello CNAME skapas ett alias för hello  **&lt;myapp >. trafficmanager.net** domännamn. hello CNAME-post ska matcha toohello IP-adressen för din  **&lt;myapp >. trafficmanager.net** domännamn automatiskt, så om hello IP-adressen för webbplatsen hello ändras, behöver du inte tootake någon åtgärd.

När trafik anländer på Traffic Manager, dirigerar sedan hello trafik tooyour webbplats som använder hello belastningsutjämningsmetod som den är konfigurerad för. Detta är helt transparent toovisitors tooyour webbplats. Användaren ser endast hello domännamn i webbläsaren.

> [!NOTE]
> Vissa domän-registratorer endast tillåter att du toomap underdomäner när du använder en CNAME-post som **www.contoso.com**, och inte rot-namn som **contoso.com**. Mer information om CNAME-poster i dokumentationen hello tillhandahålls av din registrator <a href="http://en.wikipedia.org/wiki/CNAME_record">hello Wikipedia transaktionen på CNAME-post</a>, eller hello <a href="http://tools.ietf.org/html/rfc1035">IETF domännamn - implementering och specifikation</a> dokumentet.
> 
> 

