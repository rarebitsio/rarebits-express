# RareBits Express: Embeddable Marketplace for ERC721 DApps

We’re excited to provide a simple Javascript interface that allows any ERC721 to quickly deploy a set of powerful features to allow their users to easily buy and sell their ERC721 tokens.

## Demo

A live version of this code is available here: 

## Dependencies

RareBits requires that Web3 1.0+ is included on the page. It can be directly linked from our servers here, or self-hosted.

## Getting Started

**Note: You will need to reach out to us to whitelist your mainnet contract address before this library will work!**

Copy and paste the RareBits embed javascript code on your page to enable auction functionality. 

``` js
<script type='text/javascript'>
  (function() {
    function async(u, c) {
      var d = document, t = 'script',
          o = d.createElement(t),
          s = d.getElementsByTagName(t)[0];
      o.src = '//' + u + ( u.indexOf("?") >= 0 ? "&" : "?") + 'ref=' + encodeURIComponent(window.location.href);
      if (c) { o.addEventListener('load', function (e) { c(null, e); }, false); }
      s.parentNode.insertBefore(o, s);
    }
    async('https://rarebits.io/embed/buy.js', function() {

      // RareBitsExpress global object now available, can initialize and perform other actions
      RareBitsExpress.init({contractAddress: ">>Your Contract Address<<"});
    });
  })();
</script>

```

Replace **>>Your Contract Address<<** with the mainnet address of your published ERC721 contract.


## Auto-embedding

After the library is instantiated, it will search for any elements with data-rarebits="1" and automatically replace them with marketplace buttons.

The following tag will automatically be replaced with a <button> element that interacts #10 Token of your ERC721 contract.

``` html
<div data-rarebits="1" data-token-id="10" />
```

## Querying

Async function that will callback with all of tokens of your contract owned by a user.

``` js
RareBitsExpress.getTokensForAddress(page, "Users Wallet Address", (error, result) => {
	console.log(result);
});
```

Async function that will callback with all of the live auctions for your contract.

``` js
RareBitsExpress.getLiveAuctions(page, (error, result) => {
	console.log(result);
});
```

## Buying Existing Items

Instead of using automatic embeds, you can also manually create marketplace buttons.

``` js
RareBitsExpress.createBuyButton(domNode, "Token Id");
```

## Creating Auctions

``` js
RareBitsExpress.createSellButton(domId, "Your Contract Address", "Token Id");
```

