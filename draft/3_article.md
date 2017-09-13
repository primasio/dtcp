# Decentralized Trusted Content Protocol

## Article metadata fields

#### type

For articles, the type field is fixed to "article".

#### language

The main language used in this article.
Language name representation follows the definition in
[RFC4646](http://www.ietf.org/rfc/rfc4646.txt).
For example "zh-CN", "en-US".

#### title

Title of the article.

#### abstract

A short description of the subject of the article.

#### category

A comma separated list of words describing the categories.
For example, "news,economy".

This field should be provided by the author when creating the article for accuracy.
However categorization algorithms could be used for assistance
or in the absence of author's decision.

#### created

The time when this article is created.

This field is a UNIX timestamp representing time in milliseconds starts from
Jan 1, 1970 in UTC.

This field can be provided as a proof of article creation time,
or [Trusted Timestamp](https://en.wikipedia.org/wiki/Trusted_timestamping),
due to the inalterability provided by Blockchain.

#### creator

Author's name.

#### creator_id

The author's unique id.

Author's identity is represented by a pair of
[public and private keys](https://en.wikipedia.org/wiki/Public-key_cryptography).
For security reasons only the hash of the public key is published on Blockchain to
verify the author's identity.

The algorithm used to create the key pair is [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)).
Key length should be longer than 2048 bit.

The creator_id is an [SHA1](https://en.wikipedia.org/wiki/SHA-1) hash of the public key generated.
The hash should be encoded in [Base36](https://en.wikipedia.org/wiki/Base36) format.

#### content_hash

Fingerprint represented by the hash of the content.

The content_hash is an [SHA1](https://en.wikipedia.org/wiki/SHA-1) hash of the public key generated.
The hash should be encoded in hex digits.

#### signature

Signature is calculated by representing all the DTCP data in a
[JSON](http://www.json.org) string,
except the signature field itself, calculate the [SHA1](https://en.wikipedia.org/wiki/SHA-1)
hash of the JSON string, and then sign the hash with private key.

The fields in the JSON array must be sorted alphabetically before signing. 

Signature algorithm used is [RSA](https://en.wikipedia.org/wiki/RSA_(cryptosystem)).

#### similarity_score

Similarity score is a [locality sensitive hash](https://en.wikipedia.org/wiki/Locality-sensitive_hashing)
calculated using [MinHash](https://en.wikipedia.org/wiki/MinHash) algorithm.
It is used to get the estimated similarity between two articles quickly.
With the similarity index built from scores of a large set of articles,
we can identify similar articles in the set to a given article in a very short time.

MinHash algorithm utilizes an array of hash functions (or permutation functions).
In DTCP we choose 126 hash functions, and hashing them into 9 components,
which makes our similarity score a 9 dimensional vector.

The selection of permutation functions are not fixed, however,
for best performance it is better selected according to some rules like [Universal Hashing](http://en.wikipedia.org/wiki/Universal_hashing).

There's another fixed hash function to produce the hash component.
The packing algorithm, together with the parameters used, are given in the following python codes:

```python

    # digests are 126 numbers from the MinHash algorithm
    
    b = 9
    r = 14

    hashes = []

    large_prime = int(18446744073709551557)

    for i in range(0, b):

        hs = int(0)

        for j in range(0, r):
            current = int(digests[i * r + j])
            hs += current * int(7 ** j)
            hs %= large_prime

        hashes.append(hs)

    return hashes
    
    # we got 9 hash components here in the hashes list

```