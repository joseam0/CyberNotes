72-77

presenta bloodhound

dice que hace mucho ruido y que para red team es mejor evitarlo salvo que sepas muy bien lo que haces

hace tanto ruido porque usa protocolos muy ruidososy hace muchas peticiones al DC


collect dat -> suply to bloodhound

enseña la ui de bloodhound
si se usa collectionmethods all es muy ruidoso

para hacerlo mas stealthy hay que ir quitando collection methods y excluir dcs

hay algunos collection methods muy ruidosos

los datos se ingestan con sharphound

SOAPHound es otro ingestor que es mas sigiloso que Sharphound, no usa ldap sino que se basa en Active Directory Web Services

los datos obtenidos por soaphound son integrables en bloodhound.

sharphound aporta mas info, SOAPHound solo si queremos ser silenciosos

si excluimos DC podremos enumerar todo menos enumeraciones de sesiones y ips de dcs (smb enumeration en los dc)



