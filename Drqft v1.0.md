# IAMX X NFT MAKER X SELENE SYSTEMS IDENTIFICATION Specification Draft v0.1

# Abstract

This CIP proposes a solution to the issue of verifying Cardano NFT projects.


# Motivation

Bad actors are currently one of the biggest threats to the NFT space, with scams ranging from convincing victims to send ADA to a false address to stealing art for minting - this is something that we must work hard to stop. To do this, we plan to introduce a form of ‘trust score’ for every user on the platform. The initial metrics for which will be rated by two data points; their level of identification and their level of Stake.

For the most common NFT aftermarket scams, there are currently two main attack vectors:

**Fake** - List an NFT with an unverified POLICY ID and then use names and other metadata to make it appear like the real project with the hope that the lack of verification goes unnoticed.

**Man in middle** - Get a fake project verified (similar to getting an SSL Cert for a fake website), the idea being that people will then see the verification and not check that it is actually the correct project. Think of this like getting an SSL Cert for “G00GLE”, the padlock appears, as it really is G00GLE, but G00GLE is not GOOGLE.


The main protection against these attack vectors so far has been the verification process, the idea being that secondary markets can build up a database of POLICY ID’s (with corresponding projects) and then the default for the user is to only see verified projects. Verifying projects is on the surface, nothing more than matching up a unique POLICY ID with its respective creator, which, once complete, allows the aftermarket to show the potential buyers all of the related information and proof of that being the real project that the buyer is looking for.

Although the process for doing this so far has been accurate, it has relied on a handful of dedicated, centralised adjudicators which has resulted in delays and does not fully align with the decentralisation we all want to create.


# Specification and Rationale

Before we attempt to explain how this CIP will solve identity for the purpose of verifying NFTs, we need to first look at how it solves the identity problem for people and businesses.

# Key principles
* Public Key Encyption
* One-Way Processes
* Conceptually similar to SSL Certificate Authorities
* Layering

# Getting onboarded

The onboarding process is the initial step that people/businesses must take to create the first attributes in their **Master Record**
There are several methods by which a user can create thier initial verification

* Visiting an IAMX Kiosk
* Using the IAMX App
* Utilising the database of an exisiting company -> Physical letter with QR Code/2FA



# GETTING ONBOARDED

Lets say we are a new project, and we want to get onboarded.

Go to IAMX website,
Claim to be a certain project/company/artist (entity type is not relevant to this)

Systematically 
Add your projects Twitter
Add your projects Discord
Add your projects Github
Add your projects Google
Add your projects Facebook/Instagram

This is verified using Single Sign On using O-Auth2
(Any site that supports using O-Auth2 is theoretically viable, but these are the most commonly used in the NFT space)

At this point, these get added to my Master Record, this is stored on the blockchain because we WANT it to be public information.
It should be easy to verify this.

# The Master Record (only relevant to the IAMX KYC process)

For every attribute that a user proves is true about themselves, this is signed by a trusted authority's private key and the resulting checksum is stored in the users Master Record.

The users Master Record is stored on the users device -> heavily encrypted.

The clear text mappings of these attributes are never stored anywhere.


# Standardisation based on use cases

Conceptually, having this master DID file stored on the users device allows us perform runtime based subset DID's, based on the specific use case.
These Subset DIDs are NFTs minted to the Cardano Blockchain.

These can range from the simple downloads (requiring only your age),to mortgage applications or crypto exchange KYC (which require far more attributes).


# From an end user point of view

Lets take an example, I want to purchase some alcohol online.
I already have verified my name and address (needed for shipping).
By verified, I mean the "Getting Onboarded" phase has been completed for these attributes.

But I have not yet verified my DOB, thus I cannot prove my age yet to buy the alchol.

I can then use the App to follow a process similar to KYC, whereby I would show my passport, my face and then app would confirm that the passport is real, the DOB I have entered matches the passport and thus would sign the attribute and add it to the Master Record.

I then return to the website where I want to make the purchase, enter my details as normal into the form.
In additional to this, I also use the Master Record, to generate a subset DID for the specific use case of an online age verified purchase, which will pull all of the relevant Checksums for these attributes and mint them as an NFT.

The online store can then run the one-way checksum process on each attribute, to verify that they do match.

So essentially, for each value the user submits both:
* The cleartext value
* The corresponding checksum result

Notice here how the process is more about confirming that the information the user has inputted is actually their information.
The user still needs to enter it in cleartext for the shopping website to process, but the DID generated from the Master Record is used to confirm the information is correct.

# Using this for NFT Verification

## There are 3 scenarios in which we would need to implement this

- Brand New Project, Yet to mint and with an Open Policy
- A project that has already minted, but does have an Open Policy
    - (Backwards compatibility)
- A project that has already minted and the Policy has closed
    - (Backwards compatibility)


## We can also take this time to define keywords:
- MASTER RECORD
    -This is the full collection of verified identity information about an individual or entity
- DID
    - This signed collection of identity information about an individual or entity, it is created by signing data in the Master Record
    - Unique ID, for finding the Master Record
    - Like how a passport number, finds the passport
    - Its just a number, a very large number
- ARTIST
    - This is the project creator/publisher (the business or the group of people who consider themselves to be the creator)
- ISSUER
    - This is the issuer of the verification, they are the entity that compares the project information claimed to the corresponding information publisized about the project
    - This is the automated process mentioned in GETTING ONBOARDED 
- IDENTITY TX
    - This is the Transaction Metadata Minted under the Project Policy ID that contains information about the identity of the Artist
    - Their signed DID and Public Key are contained within
- ISSUER TX
    - This is the Transaction Metadata Minted under the Project Policy ID that contains information about the identity of the Issuer
    -Their signed DID and Public Key are contained within
- NFT-IDENTITY TX
    - This is the Transaction Metadata Minted under the Project Policy ID that contains information about the project in clear text
    - Such as the discord/insta/twitter/website and the PolicyID of the project
- CREDENTIAL HASH
    - This hash of the concatination of the Project PolicyID and the DID of the Artist


## New Projects
As with all new standards, they tend to be easiest to implement on new instances as you are starting fresh.
In this case, all new projects have the option to set this up in the optimal format, whereby the DID is included inside of each NFT.
Crucially, this NFT will be signed by a trusted party such as NFT Maker or IAMX.

Lets see this in action:

The **First TX** to get minted is the IDENTITY TX, this is the Identity of the artist and contains information about them such as their Signed DID and Public Key. This is minted under the Project's Policy ID. This TX is minted under the PROJECT Policy ID.
Because it contains signed information, the public key can be used to decrypt the data to reveal their information, knowing that they created it.

This TX is sent to the wallet address of the Artist, which also contains their own DID NFT and thus can be compared and correlated with the information claimed about the ownership.


The **Second TX** to get minted is the ISSUER TX, this is the Identity of the issuer (the org that technically is minting the NFTs) like NFT MAKER and contains information about them such as their Signed DID and Public Key.This TX is also minted under the PROJECT Policy ID.
Because it contains signed information, the public key can be used to decrypt the data to reveal their information, knowing that they created it.

This TX is sent to the wallet address of the Artist, which also contains their own DID NFT and thus can be compared and correlated with the information claimed about the ownership.

We are now in a positon, where the person who claims to be the real creator of the Project, has two TX's in their wallet that were minted by the Project Policy ID.

The **Third TX** to get minted is the NFT-IDENTITY TX, this is the Identity of the PROJECT and contains information about the project in CLEAR TEXT such as the discord/insta/twitter/website and the PolicyID of the project.

It also contains 3 additional groups of signature information

For the Issuer: The DID
A signed PolicyID + DID
The storage location of the ISSUER TX (This is the assetID of the ISSUER TX)

For the Project Creator:The DID
A signed DID
The storage location of the ISSUER TX (This is the assetID of the ISSUER TX)

For ????:The DID
A signed DID
The storage location of the ISSUER TX (This is the assetID of the ISSUER TX)


CRUCIALLY - every single NFT minted under this Policy ID will have the assetname of this NFT-IDENTITY TX inside its metadata, allong with the Credential Hash.



## Minted, with currently Open Policy
With the PolicyID still open, we are able to mint another NFT under that Policy and gain the inherint proof of authenticity through it.
We can therefore use this opportunity to mint a single new NFT containing the DID information for that collection.
Crucially, this NFT will be signed by a trusted party such as NFT Maker or IAMX.

## Minted, with a Closed Policy
In this case (as will be for many projects by now) the Policy is already closed.
This gives us more of a challenge as we can no longer point from inside the project, rather from the outside in.
To solve this, our solution would be for the creator to maintain another Policy, specifically for the purpose of pointing
