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
npm install subspace
```

Via a CDN

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/subspace.js/1.0/subspace.min.js"></script>
```

## Todo

* What is a storage contract
* Architecture document


### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
