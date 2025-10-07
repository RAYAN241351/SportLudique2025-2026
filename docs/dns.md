## Documentation DNS

| Zone | Serveur | Role | Adresse IP |
|-----------|-----------|-----------|-----------|
|LAN        |Unbound|Résolveur Interne|192.168.110.80|
|DMZ        |Bind9      |DNS PUBLIC (Autorité)|192.168.18.70|
|Firewall   |PAT UDP:TCP 53|Redirection DNS vers DMZ|192.168.18.1|
|Internet   |Clients externes|Accès au DNS public|           

## Configuration du PAT sur le Firewall 





## 

`test`
