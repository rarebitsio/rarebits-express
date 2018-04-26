# RareBits Express: Embeddable Marketplace for ERC721 DApps
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fdraftparty%2Frarebits-express.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fdraftparty%2Frarebits-express?ref=badge_shield)


We’re excited to provide a simple Javascript interface that allows any ERC721 to quickly deploy a set of powerful features to allow their users to easily buy and sell their ERC721 tokens.

## Demo

[A live version of this code is available](https://rarebits-express.herokuapp.com)

## Dependencies

RareBits requires that Web3 1.0+ is included on the page. It can be directly linked from our servers using the tag below, or self-hosted.

``` html
<script src="https://rarebits.io/embed/web3.min.js" type="text/javascript"></script>
```

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
      var ts = Math.round((new Date()).getTime() / 1000);
      o.src = u + ( u.indexOf("?") >= 0 ? "&" : "?") + 'ref=' + encodeURIComponent(window.location.href) + "&_ts=" + ts;
      if (c) { o.addEventListener('load', function (e) { c(null, e); }, false); }
      s.parentNode.insertBefore(o, s);
    }
    async('https://rarebits.io/embed/express.js', function() {
      RareBitsExpress.init({contractAddress: ">>>Your Contract Address<<<"});
    });
  })();
</script>

```

Replace **>>Your Contract Address<<** with your ERC721 token contract address.


## Auto-embedding

After the library is instantiated, it will search for any elements with data-rarebits="1" and automatically replace them with marketplace buttons.

The following tag will automatically be replaced with a <button> element that interacts #10 Token of your ERC721 contract.

``` html
<div data-rarebits="1" data-token-id="10" />
```


## Manually creating auction buttons

Instead of using automatic embeds, you can also manually create marketplace buttons.

``` js
RareBitsExpress.createTokenButton(domNode, "tokenId");
```


## Querying

### Live Auctions

``` js
void getLiveAuctions(page : integer, callback : Function);
```

You can retrieve all of the live auctions for your DApp by using the getLiveAuctions function.

``` js
function handleAuctions(data) {
  // Print all tokens
  console.log(data.objects);
}
RareBitsExpress.getLiveAuctions(1, handleAuctions);
```

Data will contain an object with a list of ordered ids, page information and an array of tokens.

Return format: 
```  json
{
  "objects": [
    {
      "token_owner_address": "0xb2C3531F77ee0a7Ec7094A0bC87EF4a269E0BcFC",
      "token_id": "3",
      ... truncated ...
      "active_auction": {
        "ending_price": "20000000000000000",
        "duration": "259200",
        "current_price": "28015625000000000",
        ... truncated ...
      }
    },
  ],
  "display_lists": {
    "live_auction_items": {
      "total_pages": 2,
      "total_entries": 20,
      "page_size": 10,
      "page_number": 1,
      "ids": [
        "Mythereum-3",
      ]
    }
  }
}
```

## Callbacks 

We provide certain callbacks to let you customize the user experience even further. Simply add these arguments as additional properties for the RareBitsExpress.init() call.


### onError
The onError callback provides you a way to further message errors to your user.

``` js
function handleError(err) {
  alert(err);
}

async('https://rarebits.io/embed/express.js', function() {
  RareBitsExpress.init({contractAddress: ">>>Your Contract Address<<<", onError: handleError});
});
```

### onUpdateButton
The onUpdateButton callback provides you a way to customize the button further.
The TokenItem for the button is provided as well with additional information about the item.

``` js
function handleUpdateButton(button, tokenItem) {
  button.innerHTML = "Different Button Label";
}

async('https://rarebits.io/embed/express.js', function() {
  RareBitsExpress.init({contractAddress: ">>>Your Contract Address<<<", onUpdateButton: handleUpdateButton});
});
```

## Styling
Below is the stylesheet injected by RareBitsExpress. These styles can be overridden the HEAD tag of your page.

``` css
.rarebits-container {
  /** This is all just styling to make the font look nice, can safely remove */
  font-size: 100%;
  -webkit-text-size-adjust: 100%;
  font-variant-ligatures: none;
  -webkit-font-variant-ligatures: none;
  text-rendering: optimizeLegibility;
  -moz-osx-font-smoothing: grayscale;
  font-smoothing: antialiased;
  -webkit-font-smoothing: antialiased;
  text-shadow: rgba(0, 0, 0, .01) 0 0 1px;

  font-family: "Lato", "Gibson", "Helvetica", sans-serif;
  transition: background-color 250ms cubic-bezier(0.4, 0, 0.2, 1) 0ms,box-shadow 250ms cubic-bezier(0.4, 0, 0.2, 1) 0ms;
}

/** Button Styles **/
.rarebits-button {
  padding: 8px 16px;
  border-radius: 2px;
  min-width: 100px;
  font-size: 12px;
  text-transform: uppercase;
  font-weight: 500;
  transition: background-color 250ms cubic-bezier(0.4, 0, 0.2, 1) 0ms,box-shadow 250ms cubic-bezier(0.4, 0, 0.2, 1) 0ms;  
  box-shadow: 0px 1px 5px 0px rgba(0, 0, 0, 0.2), 0px 2px 2px 0px rgba(0, 0, 0, 0.14), 0px 3px 1px -2px rgba(0, 0, 0, 0.12);
  background-color: #3453B1;
  color: white;
  border: 0;
}

.rarebits-button:active, .rarebits-button:focus {outline: none;}
.rarebits-button:active {opacity: 0.5;}
.rarebits-button:hover {background-color: #303f9f; cursor: pointer;}
.rarebits-button:disabled {background-color: #ccc !important; cursor: inherit !important;}

.rarebits-success {background-color: #27ae60;}
.rarebits-success:hover {background-color: #2abc68;}

.rarebits-danger {background-color: rgb(217, 83, 79);}
.rarebits-danger:hover {background-color: #d83234;}

/** Message style, displays below button */
.rarebits-message {
  font-size: 12px;
  font-weight: bold;
  margin-top: 4px;
}

.rarebits-message-error {color: #e74c3c;}
.rarebits-message-info {color: #3498db;}

```




## License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fdraftparty%2Frarebits-express.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Fdraftparty%2Frarebits-express?ref=badge_large)