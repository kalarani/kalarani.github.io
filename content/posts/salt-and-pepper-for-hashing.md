+++
date = '2026-01-12T10:04:25+05:30'
title = 'Seasoning Hashes: Why Salt and Pepper Matter'
+++

Any application that processes and stores data must be aware of and comply with the data protection laws of the region it serves. Hashing is one of the (many) tools that *helps reduce risk* when handling sensitive data.

## Hashing

Hashing is a deterministic mathematical function: the same input produces the same output every time. This property makes hashing useful, but also risky if used incorrectly. While it is difficult to infer the original input from a hash, hashes are still vulnerable to brute-force attacks. If an attacker gets hold of a set of hashes, they can try many possible inputs, hash them, and look for matches. Precomputed dictionaries and rainbow tables make this especially effective when raw hashes are used.

{{< figure src="/images/basic-hash-function.png" title="Basic Hash Function" >}}

|Input|Output|
|---|---|
|asdf|f0e4c2f76c58916ec258f246851bea|
|qwer|f6f2ea8f45d8a057c9566a33f99474|

## Salting

The common practice to counter this attack is called **salting**. A salt is a random value added to the input before hashing. Each hash gets its own unique salt, which is stored along with the hash so that it can be verified later. This makes precomputed dictionary attacks ineffective and forces an attacker to resort to brute-force each hash independently, thus making the process more expensive. In practice, salting is most effective when combined with slow, purpose-built hash functions for sensitive data.

{{< figure src="/images/salted-hash.png" title="Salted Hash" >}}

|Input|Salt|Output|
|---|---|---|
|asdf|1234|91d14d4247a2fc3e18694461b1816e13b|
|asdf|0123|e21wB6BIloxOrHYSgRHNL8Q4bBlXgND/u|
|qwer|5678|98BkyHo+7UpyUmP9CeG+RKlGNyFw0/JTE|

## Uniqueness check

There are scenarios, however, where hashes are used not for verification but for **uniqueness checks**. For example, the original data may be encrypted, and a hash is used only to ensure that the same logical value is not stored twice. In such cases, per-record salts break determinism, making uniqueness checks impossible.

## Pepper

This is where **pepper** comes into the picture. A pepper is a secret value shared at the application (or sometimes column) level and added to the input before hashing. Unlike salts, peppers are not stored alongside each hash and remain hidden from the database. This preserves determinism, allowing uniqueness checks, while still protecting against generic dictionary attacks as long as the pepper remains secret.

{{< figure src="/images/pepper-hash.png" title="Peppered Hash" >}}

|Input|Output (Pepper: 1234)|
|---|---|
|asdf|da2e5c6a6604d736121650e2730c6fb0a3|
|asdf|da2e5c6a6604d736121650e2730c6fb0a3|
|qwer|a3jESxavpOAKe61ofHTSohKVJCNqP4TGUB|


## A word of caution

Pepper is not a replacement for salting, and its security depends entirely on secrecy. If the pepper is compromised, an attacker can construct a custom dictionary tailored to the application. For this reason, peppers should be treated like keys: stored securely, access-controlled, and rotated regularly. Rotation typically requires supporting multiple peppers or re-deriving hashes over time, and should be done with a clear operational plan.

{{< callout emoji="⚡️" text="Salting protects individual hashes at scale, pepper adds a shared layer of defense where determinism is required, and both must be used with the right hash functions and threat model in mind." >}}