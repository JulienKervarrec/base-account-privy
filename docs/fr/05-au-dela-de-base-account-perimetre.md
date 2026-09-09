# Chapitre 5 -- Au-dela de Base Account : ce que Privy ajoute, et limites du parcours

Le reste de la demo illustre des capacites qui ne sont pas specifiques a
Base Account mais montrent comment il cohabite avec le reste de la
plateforme Privy. La section Wallet Actions (`wallet-actions.tsx`) expose
un ensemble d operations generiques (signer un message, des donnees
typees, un hash brut, envoyer une transaction) disponibles a la fois pour
des wallets EVM et Solana, via les hooks paralleles
`@privy-io/react-auth` et `@privy-io/react-auth/solana` -- une demonstration
que la meme interface Privy s applique uniformement quel que soit le type
de wallet connecte, Base Account inclus. La signature de hash brut
(`secp256k1_sign`) est reservee aux wallets embarques Privy (`walletClientType
=== "privy"`), pas aux wallets externes comme Base Account.

Les sections Link/Unlink Accounts (`link-accounts.tsx`) exposent la liste
complete des methodes de liaison Privy (email, telephone, wallet, et une
quinzaine de fournisseurs sociaux dont Google, Apple, Discord, Farcaster,
Telegram, passkey), permettant d associer plusieurs identites a un meme
utilisateur Privy independamment de Base Account. La section MFA
(`mfa.tsx`) declenche simplement la modale d inscription Privy
(`showMfaEnrollmentModal`), qui gere TOTP, SMS et passkey en interne.

Ce parcours couvre donc precisement : la configuration du provider Privy
pour prioriser Base Account, le flux d authentification SIWE via
`wallet_connect` et verification backend, la consultation et creation de
Sub Accounts a travers l abstraction wallet de Privy, et les Spend
Permissions via la librairie navigateur haut niveau. Restent hors champ :
le detail interne du SDK Privy lui-meme (gestion de session, stockage des
tokens), l implementation des routes API de liaison de comptes sociaux
tierces, et le contrat `SpendPermissionManager` (deja documente comme
partage par tout l ecosysteme Base dans les autres parcours de cette
bibliotheque). L objectif est de comprendre specifiquement le point de
jonction entre Base Account et une plateforme d authentification tierce,
plutot que de redocumenter Privy dans son ensemble.
