# Chapitre 4 -- Permissions de depense avec la librairie navigateur de Base Account

Contrairement au flux de sub-account-demo qui signe une structure EIP-712
"a la main" via `eth_signTypedData_v4`, cette demo utilise les fonctions
utilitaires de haut niveau exposees par `@base-org/account/spend-permission/
browser` : `requestSpendPermission`, `fetchPermissions`,
`prepareSpendCallData`, et `getPermissionStatus`. C est la meme
fonctionnalite (une permission de depense EIP-712 geree par le contrat
`SpendPermissionManager`), mais accedee via une interface plus abstraite qui
masque la construction manuelle du typed-data.

`requestSpendPermission` prend en parametres le compte proprietaire, le
spender autorise, le token (ici l adresse USDC sur Base,
`0x833589fCD6eDb6E08f4c7C32D4f71b54bdA02913`), le montant autorise
(`allowance`, en unites du token) et une periode en jours (`periodInDays`),
puis retourne l objet permission signe pret a etre stocke. `fetchPermissions`
interroge l etat existant pour un compte et un spender donnes, utile pour
retrouver les permissions deja accordees sans les reconstruire. Avant
d utiliser une permission, `getPermissionStatus` verifie qu elle est
toujours active (`isActive`) et calcule le solde restant (`remainingSpend`)
pour la periode courante -- une verification cote client qui evite de
tenter une depense vouee a l echec.

Enfin, `prepareSpendCallData(permission, amount)` construit les appels de
contrat necessaires pour effectuer la depense (typiquement
`approveWithSignature` puis `spend` sur le `SpendPermissionManager`), sans
les soumettre : la demo se contente de les afficher dans la console, en
notant explicitement que dans une vraie application, ces appels seraient
soumis via le compte du spender. Cette demo se concentre donc sur le cote
"proprietaire" du flux (accorder et inspecter des permissions) plutot que
sur le cote "spender" (les executer), ce qui la rend complementaire de
sub-account-demo, qui documente le chemin complet d execution.
