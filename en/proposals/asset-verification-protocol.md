# Waves Ticker Protocol

## Motivation

Waves Platform allows to create and transfer any asset. The only threshold for the creator is to pay a fee (currently 1 waves). This lowers the threshold for entering the platform but opens up a lot of opportunities to send spam at a relatively low price. Also scam assets could be used for fraudulent purposes, for example by duplicating, fully or in part, the name of an existing cryptocurrency or a well-known company with the aim of misleading users.

We want to protect our users from suspecious assets and ensure verified assets are used. It could be possible with some kind of listings generated by token holders with voting system or by a trusted organizations in compliance with established rules, access to which is public.

Here we present a standard that allows the exchange of information about tokens between data providers and applications using the Waves blockchain.

Any company or individual provider can provide information about tokens, verify/certify projects that have issued a token in the Waves blockchain. Any application using the Waves blockchain can use the information provided by the providers.

An example of such provider is [bettertokens](http://bettertokens.org/) - Tokenization Standards Association, a non-profit organization, the main function of them is development of due diligence standards for companies engaged in the tokenization of assets and initial coin offerings (ICO)/ token generation events (TGE). 

## Specification

Our protocol is supposed to serve as an interaction between providers and the blockchain. A provider can be both a centralized organization with a developed methodology and a decentralized service for ranking tokens by voting or multisignature signing. 

Each provider has its own address on Waves blockchain. It can be created specially for the purpose of asset verification or used for other goals as well.

![](/_assets/waves_ticker_1.png)

Each provider has its own Waves address. Before starting to supply information about tokens, providers must fill information about themselves as one data entry of DataTransaction. Then, for each token, the provider fills another data entry in the same DataTransaction, with status and detailed information. DataTransaction is sent to smart account address and could be read then as account state. Example of such transaction in JSON format:

```
{
  "version" : 1,
  "sender": "3FjTpAg1VbmxSH39YWnfFukAUhxMqmKqTEZ", 
  "data": [
    {"key": "data_provider_name", // provider initialization key
     "type": "string", 
     "value": "provider_name", // company name 
     }, {
     "key": "data_provider_link", 
     "type": "string", 
     "value": "http://example.com", // provider’s site 
     }, {
     "key": "data_provider_email", 
     "type": "string", 
     "value": "example@gmail.com", // contact email 
     }, {
     "key": "data_provider_logo_meta", 
     "type": "string", 
     "value": "data:image/png;base64", // information about a logo type (svg, png, jpg … ) 
     }, {
     "key": "data_provider_logo", 
     "type": "binary", 
     "value": "base64:__base64_image_code__", // provider's logo 
     },  {
      "key": "data_provider_lang_list", 
      "type": "string",
      "value": "en" // a list of available languages for provider's description
      },
      {
      "key": "data_provider_description_<en>", // provider initialization key
      "type": "string", 
      "value": "Verification provider", // provider info 
      }, 
      {"key": "status_id_<2L3hRkSJpmaytgSfKLSNgC1vcoUvGGAv2353c6V9hPKC>, // token ID
     "type": "integer", 
     "value": "1", // status
     }, {
     "key": "link_<2L3hRkSJpmaytgSfKLSNgC1vcoUvGGAv2353c6V9hPKC>", 
     "type": "string", 
     "value": "http://example.com", // project site 
     }, {
     "key": "email_<2L3hRkSJpmaytgSfKLSNgC1vcoUvGGAv2353c6V9hPKC>", 
     "type": "string", 
     "value": "example@gmail.com", // project contact email
     }, {
     "key": "description_<en>_<2L3hRkSJpmaytgSfKLSNgC1vcoUvGGAv2353c6V9hPKC>", 
     "type": "string", 
     "value": "Founded in June of 2012, Company is a digital currency wallet and platform where merchants and consumers can transact with new digital currencies like bitcoin, ethereum, and litecoin. Based in San Francisco, California.", // brief project description
     }, {
     "key": "ticker_<2L3hRkSJpmaytgSfKLSNgC1vcoUvGGAv2353c6V9hPKC>", 
     "type": "string", 
     "value": "TKR", // assigned project ticker
     }, {
     "key": "logo_meta_<2L3hRkSJpmaytgSfKLSNgC1vcoUvGGAv2353c6V9hPKC>", 
     "type": "string", 
     "value": "data:image/png;base64", // project logo 
     }, {
     "key": "logo_<2L3hRkSJpmaytgSfKLSNgC1vcoUvGGAv2353c6V9hPKC>", 
     "type": "string", 
     "value": "base64:__base64_image_code__", // project logo 
     }
  ],
  "fee": 100000
}
```
Where `version` is a DataTransaction version of transaction (this information you can find in our documentation),  `sender` is a provider address, `data` consists of data entries each of which contains fields `key`, `type` and `value`, and `fee` is a fee amount in Waves. The token's status can be "scam", "suspicious", "with detailed description", "proven" or "gateway". More detailed information about parameters you can find in the tables below.


Table 1. Tokens statuses


| Status | Description |
| :--- | :--- |
| -2 | Scam token |
| -1 | Suspicious token |
| 0 | Token with detailed description |
| 1 | Proven tokens |
| 2 | Gateways |

Table 2. Parameters for the provider description 


| Key | Required | Value |
| :--- | :--- | :--- |
| data_provider_name | true | Provider's name | 
| data_provider_link | true | A link to the provider's website |
| data_provider_email | false | Contact email | 
| data_provider_description_<language> | true | Provider service Information in a language from "data_provider_lang_list"|
| data_provider_lang_list | true | A list of available language which can be added to the provider and its tokens in comma separated format|
| data_provider_logo | false, default:en | Provider's logo in any format in base64, must be accompanied by a correctly filled "data_provider_logo_meta" field |
| data_provider_logo_meta | true | A logo format description | 

Table 3. Parameters to describe the token

| Key | Required | Value |
| :--- | :--- | :--- |
| status_id_<ASSET_ID> | true | Status |
| link_<ASSET_ID> | false | A link to the project’s website |
| email_<ASSET_ID> | false | Project contact email |
| description_<LANG>_<ASSET_ID> | false | Description of the project in one of "data_provider_lang_list" language |
| ticker_<ASSET_ID> | false | Assigned project ticker |
| logo_<ASSET_ID> |  Provider's logo in any format in base64, must be accompanied by a correctly filled "logo_meta_<ASSET_ID>" field  |
| logo_meta_<ASSET_ID> | A logo format description |


## Example of a blacklist smart-contract

Here we present an example of a smart-contract which blacklists all transactions with suspicious and scam tokens:
```
let address = …

match tx {
    case tx: BurnTransaction | TransferTransaction | MassTransferTransaction | ReissueTransaction | ExchangeTransaction | SetAssetScriptTransaction => extract(getInteger(address, tx.assetId)) >= 0
    case _ => false
}

```

### Interaction of Waves applications with the protocol

The providers' accounts are specified in the settings so the ranking can be read from the corresponding account's state. So that the user has the opportunity to choose a "good" provider. It is also planned to organize a rating of the providers, determined by users’ voting.


Table 4. Values of token parameters in the UI

<table>
  <tr>
    <td>Parameter</td>
    <td>Status</td>
    <td>Content</td>
  </tr>
  <tr>
    <td rowspan="4">Link</td>
    <td>-1</td>
    <td>Not displayed</td>
  </tr>
  <tr>
    <td>0</td>
    <td>A link to the project’s website</td>
  </tr>
  <tr>
    <td>1</td>
    <td>A link to the project’s website</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Link to the personal account of the gateway</td>
  </tr>
   <tr>
    <td rowspan="4">Email</td>
    <td>-1</td>
    <td>Not displayed</td>
  </tr>
  <tr>
    <td>0
    <td>Project contact email
  </tr>
  <tr>
    <td>1
    <td>Project contact email
  </tr>
  <tr>
    <td>2
    <td>Gateway support email
  </tr>
  <tr>
    <td rowspan="4">Details</td>
    <td>-1</td>
    <td>The reason for getting into the filter of suspicious projects (optional)</td>
  </tr>
  <tr>
     <td>0</td>
     <td>Description of the project</td>
  </tr>
  <tr>
    <td>1</td>
    <td>Description of the project</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Description of I / O conditions through gateway</td>
   </tr>
   <tr>
    <td rowspan="4">Ticker symbol</td>
    <td>-1</td>
    <td>Not displayed</td>
  </tr>
  <tr>
    <td>0</td>
    <td>Not displayed</td>
  </tr>
  <tr>
    <td>1</td>
    <td>Assigned project ticker</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Not displayed</td>
  </tr>
  <tr>
    <td rowspan="4">Logo</td>
    <td>-1</td>
    <td>Not displayed</td>
  </tr>
  <tr>
    <td>0</td>
    <td>Project logo</td>
  </tr>
  <tr>
    <td>1</td>
    <td>Project logo</td>
  </tr>
  <tr>
    <td>2</td>
    <td>Project logo</td>
  </tr>
</table>

## Settings

The default settings indicate the addresses of bettertokens and the community spam-list.
The user has the ability to add the addresses of their providers.

If a tokenId is present in the several proveders' lists then the user is able to choose the most trusted provider and use this information in the future.


### UI change depending on status

Table5. UI Change 

<table>
  <tr>
    <td>Status</td>
    <td>UI Change</td>
  </tr>
  <tr>
    <td rowspan="3">-1 (Suspicious tokens)</td>
    <td>Portfolio page <ul><li>Hiding the token name.</li><li>Falling into the “Suspicious” filter.</li><li>Displays the label “Suspicious” instead of the name in the list.</li></ul></td>
  </tr>
  <tr>    
    <td>Transaction page <ul><li>Hiding the token name.</li><li>Displays the label “Suspicious” instead of the name in the list.
</li><li>Change the colors of the data in the block to muted.</li></ul></td>
  </tr>
  <tr>    
    <td>Token Details Window <ul><li>Display label “Suspicious” next to the token name.</li><li>Hiding the attachment specified when creating the token. Instead, the reason for entering the filter, if specified.</li><li>Link to information provider site.</li></ul></td>
  </tr>
  <tr>
    <td>0 (Token info)</td>
    <td>Token Details Window<ul><li>Add links to the project site</li><li>Add project contact email.</li><li>Project description.</li><li>Project logo.</li><li>Link to information  provider site. </li></ul></td>
  </tr>
  <tr>
    <td rowspan="4">1 (Verification)</td>
    <td>Portfolio page <ul><li>Getting into the filter of suspicious projects.</li><li>Displaying the label “Qualified Issuer” next to the token name.</li><li>Project logo instead of the standard icon.</li><li>Add a quick jump to DEX</li></ul></td>
  </tr>
  <tr>    
    <td>Token Details Window <ul><li>Displaying the label “Qualified Issuer” next to the token name.</li><li>Add links to the project site.</li><li>Add project contact email.</li><li>Hiding the attachment specified when creating the token. Instead of it - the essence of the project, specified in the data-transaction from the provider.</li><li>Project logo instead of the standard icon.</li><li>Ticker.</li><li>Link to information provider site.</li></ul></td>
  </tr>
  <tr>    
    <td>Search<ul><li>Priority search by ticker.</li></ul></td>
  </tr>
  <tr>    
    <td>DEX<ul><li>Ticker output instead of name in whatchlist.</li><li>Priority search by ticker</li></ul></td>
  </tr>
  <tr>
    <td rowspan="4">2 (Gateway)</td>
    <td>Portfolio page <ul><li>Getting into the “Gateways” filter.</li><li>Displaying the “Gateway” label next to the token name.</li><li>Project logo instead of the standard icon.</li><li>Add a quick jump to DEX</li></ul></td>
  </tr>
  <tr>    
    <td>Shipping Window<ul><li>The appearance of the tabs on the type http://joxi.ru/KAxLe4UM05KDA8.</li><li>Project logo instead of the standard icon</li></ul></td>
  </tr>
  <tr>    
    <td>Receiving window<ul><li>The appearance of the tabs on the type http://joxi.ru/V2VOnZCxOo8MAv.</li><li>Project logo instead of the standard icon</li></ul></td>
  </tr>
 <tr>    
    <td>Token Details Window <ul><li>Displaying the “Gateway” label next to the token name.</li><li>Add links to the project site.</li><li>Add project contact email.</li><li>Hiding the attachment specified when creating the token. Instead of it - the essence of the project, specified in the data-transaction from the provider.</li><li>Project logo instead of the standard icon.</li><li>Ticker.</li><li>Link to information provider site.</li></ul></td>
  </tr>


