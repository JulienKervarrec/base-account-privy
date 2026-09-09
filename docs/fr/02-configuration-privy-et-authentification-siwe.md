# Chapitre 2 -- Configurer Privy pour Base Account et authentifier avec SIWE

La configuration se fait en un seul endroit, `src/providers/providers.tsx`,
qui enveloppe l application dans un `PrivyProvider`. Deux options sont
determinantes : `appearance.walletList: ['base_account']` restreint (ou met
en avant) Base Account comme methode de connexion, et
`appearance.showWalletLoginFirst: true` affiche l ecran de connexion par
portefeuille avant les methodes sociales. `defaultChain: base` (importe de
`@privy-io/chains`) fixe Base mainnet comme reseau par defaut pour les
wallets crees ou connectes.

Le flux d authentification, dans `authentication.tsx`, reproduit le patron
"Sign in with Ethereum" (SIWE, EIP-4361) deja vu dans d autres depots Base,
mais avec deux particularites propres a cette integration. D abord, le
provider RPC n est pas obtenu directement du SDK Base Account mais via
`useBaseAccountSdk().baseAccountSdk.getProvider()` -- c est Privy qui
instancie et expose le SDK sous-jacent. Ensuite, le bouton d interface
utilise le composant officiel `SignInWithBaseButton` du paquet
`@base-org/account-ui/react`, dans ses deux variantes de couleur (`dark` et
`light`), plutot qu un bouton personnalise.

Le flux lui-meme reste standard : recuperer un nonce aupres de
`/api/auth/nonce` (genere avec `crypto.randomBytes` et stocke dans un
`NonceStore` en memoire, `src/lib/nonce-store.ts`, qui se purge
automatiquement toutes les 10 minutes), appeler `wallet_connect` avec la
capability `signInWithEthereum` (nonce + `chainId: "0x2105"`, soit Base
mainnet en hexadecimal), recuperer l adresse, le message SIWE et la
signature retournes, puis les envoyer a `/api/auth/verify`. Cette route
serveur extrait le nonce du message SIWE par une cascade de patterns regex
(le format exact du message peut varier), verifie qu il n a pas deja ete
consomme (protection anti-rejeu), puis valide la signature avec
`viem.verifyMessage` -- une fonction qui gere nativement le standard
ERC-6492, necessaire pour verifier des signatures emises par un smart
wallet qui n est pas encore deploye on-chain.
