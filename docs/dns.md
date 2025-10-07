## Documentation DNS

| Zone | Serveur | Role | Adresse IP |
|-----------|-----------|-----------|-----------|
|LAN        |Unbound|Résolveur Interne|192.168.110.80|
|DMZ        |Bind9      |DNS PUBLIC (Autorité)|192.168.18.70|
|Firewall   |PAT UDP:TCP 53|Redirection DNS vers DMZ|192.168.18.1|
|Internet   |Clients externes|Accès au DNS public|           

## Configuration du PAT sur le Firewall 





## Configuration des zones

`view "interne" {
     match-clients { localhost; 172.28.128.0/24; };
     recursion yes;
     zone "bourges.sportludique.fr" {
     type master;
     file "/etc/bind/db.interne";
     };
};`

`view "externe" {
     match-clients {any;};
     recursion no;
     zone "bourges.sportludique.fr" {
     type master;
     file "/etc/bind/db.externe";
     };
};`

     






