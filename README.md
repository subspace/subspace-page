<h1 align="center">
  <br>
  <img src="https://raw.githubusercontent.com/subspace/subspace.github.io/master/subspace.png" alt="SubSpace" width="150">
  <br>
  <a href="https://getsubspace.io">SubSpace</a>
  <br>
  <h4 align="center">A secure, persistent, highly available and <span style='font-style:italic'>decentralized</span> NoSQL database.</h4>
  <br>
</h1>
<hr>

## Subspace Client

The **subspace client** is a javascript library that can run in a **node.js** or **browser** runtime.  It allows you to build client-side applications for web, mobile, or desktop apps using the subspace protocol and device network.

Subspace is a decentralized NoSQL database, or key-value store, that has a simple get(), put() API. Data hosted on subspace is sharded across a network of persistent mobile devices.  Subspace hosts set aside local disk space in order to earn subspace credits, which they receive for hosting and serving data for subspace apps.  

#### Subspace allows you to build decentralized apps with having to worry about:

1. Designing your own cryptograhic database
2. Creating a P2P overlay network
3. Managing distributed consensus
4. Launching your own token
5. Bootstraping a peer network

#### Why build decentralized apps?  

1. They are more secure, using public key cryptography, encryption, and digital signatures.
2. They are more reliable, since data is highly replicated and self-healing.
3. Users can fully own their data sincey they can cover the hosting costs.
4. Users can pay you in credits they earn from devices they already own.
5. You don't have to manage infrastrucure, just focus on making a great gapp. 


To learn more about the how the subspace protocols works you can review the [architecture guide]() or read our [white paper]().

## Subspace Developer Console

Start by visiting the [subspace console](https://console.getsubspace.io) to setup your developer account and create a storage contract.  You will receive one subspace credit for signing up, which allows you to create a 1 GB / month storage contract for testing purposes purposes. 

1. Create an new subspace developer account
2. Safely store your key pair
3. Create a new app and storage contract

#### Can I test it out without creating a developer account?

Absolutely!  Install the subpace app on your mobile phone, setup a personal storage contract and get the contract id.  Now you can test your app on your own phone

## Include subspace in your app

Using NPM

```
npm install subspace-client
```

Via a CDN

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/subspace-client.js/1.0/subspace-client.min.js"></script>
```

## Connect to the subspace network

```javascript

import 'subspace' from subspace-client

// generate a new ECDSA keypair for a this user
// store this somewhere for safe keeping
const keyPair = subspace.createKeyPair()

// or load an existing EDCSA keypair from disk
let publicPath = '../my_public_key.pem'
let privatePath = '../my_private_key.pem'
const keyPair = subspace.loadKeyPair(publicPath, privatePath)

// include your contract_id 
// contract space can only be written to by authorized users, more on this later
let contractId = 'a9993e364706816aba3e25717850c26c9cd0d89d' 


// your private key is not shared over the network, only used for signing requests 
subspace.connect(keyPair, contractId)

subspace.on('connected', (stats) => {
  console.log(`connected to subspace!`)
  console.log(stats)
})
```

## Write some data to SSDB

```javascript
// put some data to SSDB 
let data = 'hello subspace'

subspace.put(data, (record) => {
  console.log(record)
  
  // the data value is encrypted by default using your private key 

  /*
  {
    key: 'ab3545e7cf80c3b8067ab00954510610d5744451'
    value: {
      data: 'hello subspace',
      timeStamp: 'unix timestamp for when record is published', 
      sequence: 'integer representing the version number',
      size: 'size of data in bytes',
      contract: 'id of the contract this data is stored under',
      signature: 'signature of record with my private key',
      ownerKey: 'hash of owner public key'
    }
  }
  */

})
```

## Read some data from SSB

```javascript
// get some data from SSDB
let key = 'ab3545e7cf80c3b8067ab00954510610d5744451'
subspace.get(key, (value) => {
  console.log(record)

  // the data value is decrypted by default using your private key 

  /*

  {
    key: 'ab3545e7cf80c3b8067ab00954510610d5744451'
    value: {
      data: 'hello subspace',
      timeStamp: 'unix timestamp for when record is published', 
      sequence: 'integer representing the version number',
      size: 'size of data in bytes',
      contract: 'id of the contract this data is stored under',
      signature: 'signature of record with my private key',
      ownerKey: 'hash of owner public key'
    }
  }

  */
})
```

## Full Example

## Todo

* What is a storage contract
* Architecture document

