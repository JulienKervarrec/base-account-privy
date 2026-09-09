# Chapitre 1 -- Presentation de base-account-privy

Ce depot est un starter Next.js officiel de Base qui montre comment integrer
Base Account comme option de portefeuille au sein de Privy, la plateforme
d authentification et de gestion de portefeuilles utilisee par de nombreuses
applications web3. L idee centrale : plutot que de cabler le SDK Base Account
directement, l application delegue toute la couche connexion/portefeuille a
Privy (`@privy-io/react-auth`), et configure Privy pour presenter Base
Account en premiere position parmi les methodes de connexion disponibles.

La demo est structuree en sections independantes, chacune illustrant une
capacite : authentification "Sign in with Base" (SIWE), gestion des Sub
Accounts, permissions de depense (Spend Permissions), operations de
portefeuille generiques (signature de message, de donnees typees, de hash
brut, envoi de transaction) pour des wallets EVM et Solana, liaison/
delaison de comptes sociaux (Google, email, passkey, etc.), et inscription
a l authentification multi-facteurs (MFA). Chaque section suit le meme
patron d interface : un composant `Section` reutilisable affichant un titre,
une description, le chemin du fichier source, et une liste de boutons
d action.

Le point commun a toutes les sections Base Account specifiquement est
qu elles s appuient sur les hooks Privy (`useBaseAccountSdk`, `useWallets`)
pour recuperer soit le SDK Base Account brut, soit le wallet Privy dont le
`walletClientType` vaut `base_account`, puis appellent les memes methodes RPC
que dans les autres depots de cette bibliotheque (`wallet_connect`,
`wallet_getSubAccounts`, `wallet_addSubAccount`) -- la difference etant que
Privy s interpose pour la decouverte et la connexion du wallet, pas pour les
appels RPC eux-memes une fois connecte.
