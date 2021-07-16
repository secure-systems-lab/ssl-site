---

layout: article
title: "Who is signing your Christmas cards: The importance of signature verification"
subnav: blog
comments: true
tagline: 'Why cryptographic signatures are only useful when paired with secure key distribution and verification.'
author: '<a href="/people#marina_moore">Marina Moore</a>'
categories:
  - Informational

---

Recently, there has been greater awareness of the importance of cryptographic signatures during distribution of software and metadata. This is a big win for the overall security of software ecosystems, but on their own, signatures do not enhance software security. Anyone can sign a holiday card as “Santa Claus,” but if a kid wants to be sure of the source of that present, he or she would need to verify it against a source of truth. Assuming the kid has some hacking or pickpocketing skills, this source of truth could be a driver’s license with the real Santa’s signature.

For cryptographic signatures, the source of truth lies in a verification strategy using a trusted public key infrastructure (PKI) system. I won’t go into all of the details of PKI systems here, though for an example of how to distribute keys you should check out [The Update Framework (TUF)](https://theupdateframework.io). But, basically, the PKI system is like Santa’s driver’s license. It is a trusted source that affirms what Santa’s signature should look like on your Christmas card. Thus, it’s not the signature alone that assures little Timmy or Jane that Santa left them a card, but the evidence that it was the real signature.

In this post, I want to spend some time talking about the importance of signature verification and understanding what is and isn’t achieved by validating a signature. To start, let’s define what we mean by a cryptographic signature. A signature is a bit of cryptography attached to an asset that allows whoever controls a given [private key](https://en.wikipedia.org/wiki/Public-key_cryptography) to attest to the validity of that asset. Anyone with a computer can generate a signature for an asset using their own private key. The benefit of signatures is they provide assurance of the veracity of the data associated with them. To verify a signature, another entity can use a public key associated with the private key to ensure that the signature was made by a particular trusted party. However, this requires prior knowledge of who is trusted to sign a particular asset, and with what private key. Lacking that prior knowledge, an entity that downloads this software will not know whether it was signed by a trusted developer or by a malware author.

The only way to trust that the jolly old elf from the North Pole actually signed the card is access to some key contextual information, including knowing who owns the private key used. For example, a software updater may send a signed package, along with the public key necessary to verify that package all in the same protocol. Yet, if an attacker is able to interrupt network traffic or gain access to the software repository, that attacker can then change both the package and the key used to sign the package. It would be the equivalent of the Grinch signing a letter with Santa’s name, and attaching a fake driver’s license with a matching signature. This leaves the recipient no better off than when they download unsigned data.

<img align="right" src="/img/signature-verification-exploit.jpg" style="margin: 0px 20px"/>

In order to ensure that software is not only signed, but valid, you must ensure that you are using a public key that was communicated over a secure channel. For cryptographic signatures, a secure channel can be a trusted PKI system such as TUF, or an offline mechanism. These mechanisms act like government issued ids and allow you to verify the identity of an individual. By ensuring that the public key is communicated securely, you can ensure that when it is used to verify data, that data actually came from the expected trusted signer.

Cryptographic signatures are a key building block for secure software distribution, but on their own they do not provide needed security properties. Relying on just a signature allows anyone to sign as Santa, but verification is needed to ensure that the signature actually came from Santa. In the same way, cryptographic signatures must be combined with effective key distribution, limitations of trust, and rigorous verification.

In a future post, I will focus on other important considerations when using cryptographic signatures, including ensuring that signatures are valid at the time they are verified, ensuring that the revocation of keys or signatures is communicated to the verifier, and effectively communicating signing algorithms.
