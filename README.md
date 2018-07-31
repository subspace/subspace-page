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

The **subspace client** is a javascript library that can run in a **node.js**, **browser**, or **react-native** runtime.  It allows you to build client-side applications for web, mobile, or desktop apps using the subspace protocol and device network.

Subspace is a decentralized NoSQL database, or key-value store, that has a simple put(), get() API. Data hosted on subspace is sharded across a network of end user devices including mobile phones, tablets, desktops, and yes, server too.  Subspace hosts set aside local disk space in order to earn subspace credits, which they receive for hosting and serving data for subspace apps.  

To learn more about how the subspace protocol works you can review the architecture guide or [read the white paper](https://subspace.github.io/paper/).

#### Subspace allows you to build decentralized apps with having to worry about:

1. Designing your own cryptograhic database
2. Creating a P2P overlay network
3. Managing distributed consensus
4. Launching your own token
5. Bootstraping a peer network

#### Why build decentralized apps?  

1. They are more secure, using public key cryptography, encryption, and digital signatures.
2. They are more reliable, since data is highly replicated and self-healing.
3. Users can fully own their data since they can cover the hosting costs.
4. Users can pay you in credits they earn from devices they already own.
5. You don't have to manage infrastrucure, instead focus on making a great app. 

## Subspace Developer Console

Start by visiting the subspace console to setup your developer account and create a storage contract.  You will receive one subspace credit for signing up, which allows you to create a 1 GB / month storage contract for testing purposes purposes. 

1. Create an new subspace developer account
2. Safely store your key pair
3. Create a new app and storage contract

#### Can I test it out without creating a developer account?

Absolutely!  Install the subpace app on your mobile phone, setup a personal storage contract and get the contract id.  Now you can test your app on your own phone

## Include subspace in your app

> Please note, these libraries are not live yet

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

// generate a new ECDSA keypair for this user
// store this somewhere for safe keeping
const keyPair = subspace.profile.createKeyPair()

// or load an existing EDCSA keypair from disk
let publicPath = '../my_public_key.pem'
let privatePath = '../my_private_key.pem'
const keyPair = subspace.loadKeyPair(publicPath, privatePath)

// include your contract_id 
// contract space can only be written to by authorized users, more on this later
let contractId = 'a9993e364706816aba3e25717850c26c9cd0d89d' 


// your private key is not shared over the network, only used for signing requests locally and encrypting symmetric keys
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

subspace.put(data, (error, record) => {

  if (error) console.log(error)
  
  console.log(record)
  
  // the data value is encrypted by default using your private key 

  {
    key: '614da614...', // sha256 hash of unique public key for this record
    value: {
      content: 'hello subspace', // JSON encoded content, encrypted during storage and transit using symkey
      hash: '0b12e7e6...', // sha256 hash of content  
      revision: 0, // sequence number
      timeStamp: 1533067582, // unix timestamp for when record was published
      owner: '72ec8422...', // sha256 hash of owner public key
      symkey: '-----Begin PGP Message', // unique symkey for this record, encrypted with owner's private key 
      contract: '4a3b555a...', // id of the contract this data is stored under
      signature: '-----Begin PGP Signature...' // Detached openpgp signature with owner's public key
    }
  }
})
```

## Read some data from SSDB

```javascript

// get some data from SSDB
let key = '614da614...'

subspace.get(key, (error, value) => {

  if (error) console.log(error)
  
  console.log(value)

  // the data value is decrypted by default using your private key 

   {
    key: '614da614...', // sha256 hash of unique public key for this record
    value: {
      content: 'hello subspace', // JSON encoded content, encrypted during storage and transit using symkey
      hash: '0b12e7e6...', // sha256 hash of content  
      revision: 0, // sequence number
      timeStamp: 1533067582, // unix timestamp for when record was published
      owner: '72ec8422...', // sha256 hash of owner public key
      symkey: '-----Begin PGP Message', // unique symkey for this record, encrypted with owner's private key 
      contract: '4a3b555a...', // id of the contract this data is stored under
      signature: '-----Begin PGP Signature...' // Detached openpgp signature with owner's public key
    }
  }
})
```

## Manage Permissions

```javascript

// grant read-only access to data on subspace with some number of identites 
// identites are public key hashes and can belong to users, apps, or groups 
// the symkeys will be re-encrypted with these public keys, now they can decrypt the data on get() 

let key = 'ab3545e7cf80c3b8067ab00954510610d5744451'
let identities = ['4532ea9bfd3745f9a46d3d7b0e136b8f48daf87a', 'abdd468c57a4c58edb81dc0bc221f168a17421c6', '3929ff11d5e60ca74bc059b7086e78b4b8bd6098']

subspace.share(key, identities, (error, readGroup) => {

  if (error) console.log(error)
  
  console.log(readGroup)

  /*
  [
    '4532ea9bfd3745f9a46d3d7b0e136b8f48daf87a', 
    'abdd468c57a4c58edb81dc0bc221f168a17421c6', 
    '3929ff11d5e60ca74bc059b7086e78b4b8bd6098'
  ]
  */
 
})

// remove read-only access to data from some number of identities
// the data will be re-signed with the adjusted set of public keys
// since data is mutable any old identities can no longer decrypt the data they read from SSDB

subspace.unshare(key, identities, (error, readGroup) => {

  if (error) console.log(error)
  
  console.log(readGroup)

  /*
  []
  */
  
}) 

```

## Delete some data from SSDB

```javascript

// this will decrease the balance on your storage contract

let key = 'ab3545e7cf80c3b8067ab00954510610d5744451'
subspace.del(key, (error) => {
  if (error) console.log(error)
})

```

## Stay tuned for updates, this is a WIP, next steps include

* Developer console will be live
* Example web app (vue.js)
* Example mobile app (react native)
* Example desktop app (electron)
* How to manage write permissions
* How to register authorized contract users

