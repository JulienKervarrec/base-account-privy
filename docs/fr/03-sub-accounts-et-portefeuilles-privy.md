# Chapitre 3 -- Retrouver et creer des Sub Accounts a travers l abstraction Privy

La section Sub Accounts (`sub-accounts.tsx`) illustre comment un concept
propre a Base Account (les Sub Accounts) reste accessible sans effort meme
lorsque le portefeuille est gere par une couche tierce comme Privy. Le
composant utilise `useWallets()` de Privy pour obtenir la liste des wallets
connectes, puis filtre celui dont `walletClientType === 'base_account'` --
c est la seule adaptation necessaire par rapport a une integration directe
du SDK Base Account.

Une fois ce wallet Privy identifie, deux methodes exposees par l objet
wallet lui-meme font la jonction avec le SDK sous-jacent :
`baseAccount.switchChain(8453)` change le reseau actif (Base mainnet), et
`baseAccount.getEthereumProvider()` retourne le meme provider EIP-1193 que
celui qu on obtiendrait directement du SDK Base Account. A partir de ce
provider, les appels sont identiques a ceux vus dans le depot
sub-account-demo : `wallet_getSubAccounts` (avec le compte principal et le
domaine courant) pour lister les Sub Accounts existants, et
`wallet_addSubAccount` (avec `account: { type: 'create' }`) pour en creer un
nouveau.

Cette section n implemente pas de flux de depense ou de transaction groupee
avec le Sub Account cree -- elle se limite a la creation et a la
consultation, laissant l utilisation avancee (transferts, permissions de
depense appliquees au Sub Account) aux exemples des chapitres suivants et
au depot sub-account-demo qui traite ce sujet plus en profondeur.
