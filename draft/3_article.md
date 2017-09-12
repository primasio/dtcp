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

*TODO:* Detailed asymmetric cryptography and hash algorithm and
their versions must be provided.

#### content_hash

Fingerprint represented by the hash of the content.

*TODO:* Detailed hash algorithm and version must be provided.

#### signature

Signature is calculated by representing all the DTCP data in a
[JSON](http://www.json.org) string,
except the signature field itself, and sign it with author's private key.

The fields in the JSON array must be sorted alphabetically before signing. 

Signature algorithm is recorded in the following field:

#### signature_algorithm

The algorithm used in signature.

#### similarity_score

Similarity score is a [locality sensitive hash](https://en.wikipedia.org/wiki/Locality-sensitive_hashing)
calculated using [MinHash](https://en.wikipedia.org/wiki/MinHash) algorithm.
It is used to get the estimated similarity between two articles quickly.
With the similarity index built from scores of a large set of articles,
we can identify similar articles in the set to a given article in a very short time.

*TODO:* Detailed algorithm description.