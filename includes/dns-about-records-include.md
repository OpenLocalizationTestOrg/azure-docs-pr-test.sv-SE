### <a name="record-names"></a>Registrera namn

DNS-poster i Azure anges med relativa namn. Ett *fullständigt kvalificerat* domännamn (FQDN), inkluderar zonnamnet, medan ett *relativt* namn inte gör det. Det relativa postnamnet ”www” i zonen ”contoso.com” skapar till exempel det fullständigt kvalificerade postnamnet ”www.contoso.com”.

En *topp*post är en DNS-post vid roten (eller *toppen*) av en DNS-zon. I till exempel DNS-zonen "contoso.com" har en toppost också det fullständigt kvalificerade namnet "contoso.com" (kallas ibland en *naken* domän).  Enligt konventionen används det relativa namnet '@' för att representera topposter.

### <a name="record-types"></a>Typer av poster

Varje DNS-post har ett namn och en typ. Posterna är indelade i olika typer beroende på den data de innehåller. Den vanligaste typen är en ”A”-post som mappar ett namn till en IPv4-adress. En annan vanlig typ är en ”MX”-post som mappar ett namn till en e-postserver.

Azure DNS stöder alla vanliga DNS-posttyper: A, AAAA, CAA, CNAME, MX, NS, PTR, SOA, SRV och TXT. Observera att [SPF-poster representeras med hjälp av TXT-poster](../articles/dns/dns-zones-records.md#spf-records).

### <a name="record-sets"></a>Postuppsättningar

Ibland måste du skapa fler än en DNS-post av ett visst namn och typ. Anta exempelvis att webbplatsen ”www.contoso.com” finns på två olika IP-adresser. Webbplatsen kräver då två olika A-poster, en för varje IP-adress. Här är ett exempel på en postuppsättning:

    www.contoso.com.        3600    IN    A    134.170.185.46
    www.contoso.com.        3600    IN    A    134.170.188.221

Azure DNS hanterar DNS-poster med hjälp av *postuppsättningar*. En postuppsättning (även kallat en *resurs*postuppsättning)är en samling DNS-poster i en zon som har samma namn och är av samma typ. De flesta postuppsättningar innehåller en enda post. Men exempel som det ovanstående, där en postuppsättning innehåller fler än en post, är inte ovanliga.

Anta till exempel att du redan har skapat en A-post "www" i zonen "contoso.com" som pekar på IP-adressen ”134.170.185.46” (första posten ovan).  För att skapa den andra posten skulle du lägga till posten i den befintliga postuppsättningen i stället för att skapa ytterligare en post.

Postuppsättningarna SOA och CNAME är undantag. DNS-standarden tillåter inte flera poster med samma namn för dessa typer, därför kan dessa postuppsättningar endast innehålla en enda post.