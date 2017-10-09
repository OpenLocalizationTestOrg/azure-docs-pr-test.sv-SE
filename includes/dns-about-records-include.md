### <a name="record-names"></a>Registrera namn

DNS-poster i Azure anges med relativa namn. En *fullständigt kvalificerade* domännamn (FQDN) omfattar hello zonnamnet, medan en *relativa* namn inte. Till exempel ger hello relativa postnamnet www om du i hello zonen ”contoso.com” hello fullständigt kvalificerade postnamnet ”www.contoso.com”.

En *apex* posten är en DNS-post i roten för hello (eller *apex*) för en DNS-zon. Till exempel i hello DNS-zonen ”contoso.com” en post för apex också har hello fullständigt kvalificerade namnet ”contoso.com” (kallas ibland ett *asbestgaller* domän).  Brukligt hello relativa namnet ”@” är används toorepresent apex poster.

### <a name="record-types"></a>Typer av poster

Varje DNS-post har ett namn och en typ. Posterna är indelade i olika typer enligt toohello data de innehåller. hello vanligaste typen är en ”A” post som mappar ett namn tooan IPv4-adress. En annan vanliga typ är en 'MX-post som mappar ett namn tooa e-postserver.

Azure DNS stöder alla vanliga DNS-posttyper: A, AAAA, CNAME, MX, NS, PTR, SOA, SRV och TXT. Observera att [SPF-poster representeras med hjälp av TXT-poster](../articles/dns/dns-zones-records.md#spf-records).

### <a name="record-sets"></a>Postuppsättningar

Ibland behöver toocreate mer än en DNS-post med ett visst namn och typen. Anta exempelvis att hello ”www.contoso.com” webbplats finns på två olika IP-adresser. hello webbplatsen kräver då två olika poster, en för varje IP-adress. Här är ett exempel på en postuppsättning:

    www.contoso.com.        3600    IN    A    134.170.185.46
    www.contoso.com.        3600    IN    A    134.170.188.221

Azure DNS hanterar DNS-poster med hjälp av *postuppsättningar*. En uppsättning poster (även kallat en *resurs* postuppsättningen) är hello samling av DNS-poster i en zon som har hello samma namn och är av hello samma typ. De flesta postuppsättningar innehåller en enda post. Exempel som hello en ovan, i vilket en postuppsättning innehåller fler än en post, är inte ovanliga.

Till exempel anta att du redan har skapat en A-post ”www” i hello zonen ”contoso.com” pekar toohello IP-adress '134.170.185.46' (hello första posten ovan).  toocreate hello nästa post lägger du till posten toohello befintlig post, utan att skapa en ytterligare poster.

hello SOA- och CNAME-posttyper är undantag. hello DNS-standarden tillåter inte flera poster med hello samma namn för dessa typer, därför dessa postuppsättningar kan bara innehålla en enskild post.
