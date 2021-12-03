# Defi-NFT-Nuit-de-l--nfo
Ce défi a été réalisé pour la Nuit de l'info 2021.

## Le concept
Les NFT(non fungible token), sont des token sur la technologie blockchain permettant d'être identifié et d'établir leur propriété numérique de façon immuable et décentralisé.

En lien avec le défi principal de la nuit c'est à dire celui en rapport avec les Secours en Mer nous avons décider de mettre en place un **système de donation** via l'achat de NFT. 

Les dons se font par palier de : 5(pas de NFT), 10, 25, 50 et 100 dollars.

## Mise en place

Après avoir réfléchi à la manière de procéder <insérer github de marcus> s'est chargé de la création du design de nos NFT.
Nous nous sommes renseignés sur la manière de les **minter** sur la blockchain.
Avec *rarepress* un système d'opération sur les NFT nous avons développer un script en javascript pour mettre nos récompenses de donation en ligne sur **Rarible**(un market place pour les NFT) grâce à la blockchain Etherum.

## Les étapes de la mise en ligne

Avant toute chose nous avons du créer un wallet **Metamask** et le lié a la blockchain Etherum Mainnet.

Ensuite avec *rarepress* nous avons écrit notre script:

Exemple du code utilisé pour mettre en ligne notre NFT "Bouée":
```
<html>
  <body>
    <pre id="token"></pre>
    <pre id="trade"></pre>
    <script src="https://unpkg.com/rareterm@0.0.9"></script>
    <script>
      const rarepress = new Rareterm();
      const mint = async () => {
        // 1. Ajouter le fichier à IPFS
        let cid = await rarepress.fs.add("https://i.imgur.com/ZRFGg2R.png");

        // 2. Créer et sauvegarder le token ERC1155 disponible en 50 exemplaires
        let token = await rarepress.token.create({
          type: "ERC1155",
          metadata: {
            name: "Soutiens niveau Bouée au SNSM !",
            description:
              "Ce NFT a été offert en remerciment d'un don de 25 dollars en soutien aux Services en Mer ",
            image: "/ipfs/" + cid,
          },
          supply: 50,
        });

        // 3. Publier l'image sur IPFS
        await rarepress.fs.push(cid);

        // 4. Publier les metadata sur IPFS
        await rarepress.fs.push(token.tokenURI);

        // 5. Publier le token sur Rarible
        let receipt = await rarepress.token.send(token);

        return {
          token,
          receipt,
        };
      };

      const sell = async (tokenId) => {
        let trade = await rarepress.trade.create({
          what: {
            type: "ERC1155",
            id: tokenId,
            value: 50,
          },
          with: {
            type: "ETH",
            value: 2.74072571873336,
          },
        });
        let receipt = await rarepress.trade.send(trade);
        return {
          trade,
          receipt,
        };
      };

      const mintAndSell = async () => {
        await rarepress.init({ host: "https://eth.rarenet.app/v1" });

        let tokenItem = await mint();
        document.querySelector("#token").innerHTML = JSON.stringify(
          tokenItem,
          null,
          2
        );
        let tradeItem = await sell(tokenItem.token.tokenId);
        document.querySelector("#trade").innerHTML = JSON.stringify(
          tradeItem,
          null,
          2
        );
      };

      mintAndSell();
    </script>
  </body>
</html>
```

Chaque NFT est donc disponible en quantité limité de 50 exemplaires.

Pour lancer ce programme nous avons eu besoin d'utiliser un serveur local en installant via node http-server et ainsi effectuer nos requêtes.
Aussi, nous avons du d'abord héberger les images de nos NFT sur un service de hosting _(https://imgur.com/)_.

Après avoir lancé notre script sur notre serveur http en local et y avoir lié notre wallet *metmask*  nous avons finalement pu publier et mettre en vente nos NFT sur le marketplace de *rarible*.

## Liens vers les NFT correspondants au palier de donation
[![Don niveau Ancre](https://i.imgur.com/XkPzQJM.png)](https://rarible.com/token/0xB66a603f4cFe17e3D27B87a8BfCaD319856518B8:79638617253701427051349581616089869356582801432216749563635906471392372094723)
[![Don niveau Bouée](https://i.imgur.com/UMxRHJG.png)](https://rarible.com/token/0xB66a603f4cFe17e3D27B87a8BfCaD319856518B8:79638617253701427051349581616089869356582801432216749563949483389126004738200)





