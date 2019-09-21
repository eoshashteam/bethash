# BetHash.io Fairness Verification

On 13-09-2019, a hacker attacked the EOS main net by sending massive transactions trying to manipulate the block ID of future blocks.

To ensure the security of our game without jeopardizing any fairness, we integrate the Digital Signature Algorithm (DSA).

The Digital Signature Algorithm (DSA) is the cornerstone of the blockchain world.

The DSA algorithm uses a key pair consisting of a public key and a private key. The private key is used to generate a digital signature for a message, and such a signature can be verified by using the signer's corresponding public key. The digital signature provides message authentication (the receiver can verify the origin of the message), integrity (the receiver can verify that the message has not been modified since it was signed) and non-repudiation (the sender cannot falsely claim that they have not signed the message).

There are two version of block ID hash method, because the the first edition is more customerized thus not easy to find opensource website to help verify easily (nevertheless, devs can always verify it writing simple codes). 

On 16-09-2019, 19:30 UTC time, we upgrade it to the second version.

## A Timeline of our Fairness Algorithm

### Before 13-09-2019
All BetHash.io games results are drawn from original EOS blockchain hashes. 

### Between 13-09-2019 and 16-09-2019
1. We use eos-ecc algorithm to sign original eos block hash
2. We take the SHA256 value of the content of the signature.

### How we do it NOW:
1. we use eos-ecc algorithm to sign original eos block hash
2. we take the SHA256 value of the signature.

### The difference between the 2 versions (attaching code snippet)
In version 1, signature.Content was used. In Version 2, []byte(signature.String()) was used. 

signature.String() = "SIG_K1_"+ base58Encode(signature.Content + calcSum(signature.Content))

To learn more about the eos-ecc algo: Elliptic curve cryptography functions: Private Key, Public Key, Signature, AES, Encryption, Decryption https://github.com/EOSIO/eosjs-ecc


edition 1:
```golang
import (
    "fmt"
    "crypto/sha256"

    "github.com/eoscanada/eos-go/ecc"
)

func encryptHash(privateKey *ecc.PrivateKey, blockID string) (string, string, error) {
    pre := sha256.Sum256([]byte(blockID))
  signature, err := privateKey.Sign(pre[:])
  if err != nil {
    return "", "", err
  }

    sum := sha256.Sum256(signature.Content))
    return fmt.Sprintf("%x", sum), signature.String(), nil
}
```

edition 2:
```golang
import (
    "fmt"
    "crypto/sha256"

    "github.com/eoscanada/eos-go/ecc"
)

func encryptHash(privateKey *ecc.PrivateKey, blockID string) (string, string, error) {
    pre := sha256.Sum256([]byte(blockID))
  signature, err := privateKey.Sign(pre[:])
  if err != nil {
    return "", "", err
  }

    sum := sha256.Sum256([]byte(signature.String()))
    return fmt.Sprintf("%x", sum), signature.String(), nil
}
```

The public key of our private key is EOS5UY8pMgdSyuy54AWV7i1BQrVkUG75rryQrobMRDDUjZFciPEar
