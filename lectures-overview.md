# Lecture 1

## Kerckhoff's principle

A cryptosystem should be secure even if everything about the system, except the key, is public knowledge.

## Three basic types of cryptography

- Steganography: Disguise there is a message
- Codes: Look up in a secret table
- Ciphers: Use a general algorithm with a secret parameter known only to a selected few

## Codes

Tables list every possible plaintext for encryption
and every possible ciphertext for decryption. Items not in the table are sent in clear.

## Ciphers

Uses alphabets (anything defined as a finite set), plaintext and ciphertext do not have to use the same alphabet.

### Caesar Cipher

#### Encryption

Choose a fixed number that determines how many positions each letter in the plaintext is shifted. For example, with a key of 3, 'A' becomes 'D', 'B' becomes 'E', and so on. Wrap around to the beginning of the alphabet if the shift goes beyond 'Z'.

Example with a shift of 3:
Plain:    ABCDEFGHIJKLMNOPQRSTUVWXYZ
Cipher:   DEFGHIJKLMNOPQRSTUVWXYZABC

#### Decryption

To decrypt a message encrypted with the Caesar cipher, use the opposite shift.

If the encryption used a shift of 3, then decryption would use a shift of -3.

Cipher:   DEFGHIJKLMNOPQRSTUVWXYZABC
Plain:    ABCDEFGHIJKLMNOPQRSTUVWXYZ

Breaking the Caesar cipher is relatively simple, as there are only 26 possible keys.

### Strengthen the Caesar Cipher

The Affine cipher uses a second key integer j and encrypts using: c = jm + k modulo n.

The affine cipher has 312 = 26 x 12 key values.

### Simple substitution (= code)

Create a table of plaintext characters and their corresponding crypto characters. Crypto characters can be just ordinary letters, but also anything
else. Each crypto character must occur only once in the table to enable unique decryption.

#### Breaking simple substitution

Every crypto letter will occur exactly as often as its plaintext counterpart occurs in plaintext. Count how often letters, bigrams and trigrams occur in the
cryptogram, and try to identify the ones corresponding to common plaintext letters and common letter combinations.

Single letter distribution of English is uneven, for example, 'E' is the most used letter followed by 'T'.

### Vigenere

#### Encryption

Choose a keyword, which is a word or phrase that determines the shift for the repeated keyword. Use the caesar cipher for each letter with the shift determined by the letter of the keyword.

Example:

MESSAGE: HELLOTHERE
KEYWORD: KEYKEYKEYK
CRYPTOGRAM: RIJVSRRIPO

#### Decryption

Breaking the Vigenère cipher can be more challenging than the Caesar cipher, but it is still vulnerable to certain attacks:

Create a Vigenère tableau, a grid of Caesar ciphers based on the alphabet and the keyword.

##### Kasiski Examination

Look for repeated sequences of characters in the ciphertext. The distance between these repetitions may provide clues about the length of the keyword.

##### Vigenere Tableau

Use the tableau to decrypt the message by finding the intersection of the row (keyword letter) and column (plaintext letter).

### Playfair

#### Encryption

Choose a keyphrase (omitting duplicate letters) and arrange it in a 5x5 matrix called the Playfair square.

Fill the remaining cells of the matrix with the remaining letters of the alphabet, excluding any duplicates and the letters in the key.

K E Y B C 
D F G H I 
J L M N O 
P Q R S T
U V W X Y

#### Decryption

Use digram frequencies, e.g., both “re” and “er” are common in English, and they encrypt to a digram and its reverse. Each plaintext letter has only eight alternatives. Final part of the table is predictable.

### Enigma

The key is:
- what rotors are used
- their starting position
- what letters are exchanged

#### Breaking the Enigma

Each Enigma operator was given a codebook with the daily settings for the next month.

Each message starts with a three-letter message key, encrypted with the setting for the day.

The message key is repeated to avoid transmission errors. Statistical analysis won’t work.

The code book contained the daily settings:
- Rotor order (later also: rotors used, three out of several)
- Plugboard settings
- Initial rotor positions

Repeated parts of messages (“ANX” meaning “To:”)

An operator sends a dummy message containing “LLLLL. . . ” which gave the English the wiring of the new rotor.

# Lecture 2

## Methods to break a cipher

- Try all possible keys (exhaustive search)
- Use plaintext alphabet statistics
- Use both single letter statistics, and digram, trigram, and word statistics
- Do calculations adjusted to the algorithm

### Attack possibilities

- Ciphertext only: Use properties of the plaintext such as statistics of the language
- Known plaintext: Allows simple deduction of the key in some ciphers, but not in others
- Chosen plaintext: In some ciphers, there are weak messages that reveal the key. In other cases, pairs of chosen plaintexts together reveal properties of the key
- Chosen ciphertext: Adds the reverse transformation, say in some systems that let you test decryption of a number of encrypted texts

### Possible results

- Find the key: Complete break, the final goal of cryptanalysis
- Finding more plaintext than you already have: Sometimes a complete break is not possible, but a partial break can be very useful
- Finding correct cryptograms for some plaintexts: Important in authentication schemes

## Shannon Entropy

See lecture slides.

## OTP (One Time Pad)

Suppose you have a cryptogram and the complete statistics for every possible plaintext of the same length. For each possible plaintext there is a corresponding key encrypting that plaintext into the given cryptogram. Every key is exactly as likely as another; thus you have no clue to which plaintext is the more likely one, except what you already knew before getting the cryptogram.

- Never, ever reuse a key!
- If the key sequence is not truly random, it is NOT OTP.
- You must generate a truly random key sequence equally long as the message, and then find a secure channel for transportation of that key to the intended message recipient. . .

- Useful for sending secret data without preserved data integrity
- Uses a long random bitsequence as key
- Adds this to the plaintext to form the next cryptogram
- No error propagation

# Lecture 3 (About Stream Ciphers)

## Pseudo-Random Number Generator (PRNG)

- Sender and receiver must generate exactly the same key stream from the seed, so the generator is deterministic
- Real-world generators must be finite, having a fixed total number of states
- Sooner or later the generator is in a state it has been in before
- From then on, the internal states and the output repeats, exactly as the previous time the state was used
- Therefore, the output will be periodic, directly or after some initial output part

## Key stream generators

- You cannot use a true random sequence of sufficient length (∼OTP), since this would require a(nother) secure channel
- Alternative: use a pseudorandom sequence, created through a known (deterministic) procedure that is “seeded” with a shorter key
- Since the generator is deterministic and finite, the sequence is periodic
- It is not enough that the output “seems” random. If the seed (key) is unknown, every output bit (byte, . . . ) should be unpredictable given the already generated sequence

## LFSR

See slides

## RNG testing

The idea is to use a “hypothesis test,” where the hypothesis is that the tested RNG is good. The test looks for deviations that should not occur in the output of a good RNG. 

Examples:
- Bias
- Correlation over distance
- Long strings of the same number
- Length of increasing sequences
- Output distribution from known functions of the output
- Poker hands
- Monkey-on-a-typewriter
- Length to repetition

### RNG test suites

- Diehard: good for simpler PRNGs, but not so well-documented
- NIST STS: well documented but from NIST
- ENT: simple teaching tool with limited documentation
- SPRNG: for parallell implementations
- Dieharder: easy to use and well documented
- TestU01: large and very well documented

## Random numbers

Humans are bad random number generators. People try to avoid patterns in a supposedly random output.

In principle, any stream cipher can function as a Pseudo Random Number Generator. The reverse is not always true.

- Stream ciphers are used for speed, PRNGs can be slow
- Failing statistical tests is not always fatal for stream ciphers
- A PRNG may pass statistical tests, but still be reverse-engineered
- Re-keying a stream cipher is well-understood and used to avoid key-stream repetition, while re-seeding a PRNG is more of an art.

## Stream ciphers overview

Properties of stream ciphers:
- Benefits: fast, no fixed block size, errors do not propagate
- Drawbacks: No message integrity check, the devil is in the details

They do need to be used with care. If you don’t follow the algorithm to the letter, things can go very bad.

A stream cipher expands a short key into a long keystream, to be XORed with the plaintext.

- LFSR-based: simple mathematical structure, needs nonlinear combination of several (A5/1, A5/2, E0, SNOW 3G)
- Many other variants, RC4 used to be popular but is now deprecated
- Another alternative is a block cipher in OFB or CTR mode

# Lecture 4

## Defence against breaking a cipher through exhaustive search

Make sure there are too many possible keys for exhaustive search

## Block cipher criteria

- Diffusion: If a plaintext character changes, several ciphertext characters should change. This is a basic demand on a block cipher, and ensures that the statistics used need to be block statistics (as opposed to letter statistics)
- Confusion: Every bit of the ciphertext should depend on several bits in the key. This can be achieved by ensuring that the system is nonlinear

## Diffusion: the avalanche effect

A change in one bit in the input should propagate to many bits in the output.

## DES

See slides for details.

There was a lot of controversy surrounding the S-box construction. People were worried there were backdoors in the system. But in the late eighties it was found that even small changes in the S-boxes gave a weaker system.

Each S-box has 6 input bits and four output bits (1970’s hardware limit). The S-boxes should not be linear functions, or even close to linear. Each row of an S-box contains all numbers from 0 to 15. Two inputs that differ by 1 bit should give outputs that differ by at least 2 bits. Two inputs that differ in the first 2 bits but are equal in the last 2 bits should give unequal outputs. There are 32 pairs of inputs with a given XOR. No more than eight of the corresponding outputs should have equal XORs. A similar criterion involving three S-boxes.

### Meet-in-the-middle attacks

- A meet-in-the-middle attack is a known plaintext attack
- Make a list of all 2^56 possible (single-DES) encryptions of the plaintext, and of all 2^56 (single-DES) decryptions of the ciphertext
- Match the two lists. The key(s) that give the same middle value is (are) the key (candidates)
- Attack is of complexity 2^57

# Lecture 5

## AES

See lecture slides for details.

### Weaknesses

- There are (used to only be) known attacks for 7-round AES-128, 8-round AES-196 and 9-round AES-256
- A “related-key” attack can break the full 12-round AES-196 using 2^176 operations, and 14-round AES-256 in 2^119 operations
- The problem is the key schedule, which seems weaker in AES-196 and AES-256 than in AES-128
- The first proper break of full AES appeared 2011. The attack is based on “bicliques”, and recovers the key from AES-128 in 2^126.1 operations, from AES-192 in 2^189.7 op., and from AES-256 in 2^254.4 op.

### ECB (Electronic Code Book)

- One block at a time
- No feedback, no preserved state
- Repeated plaintext blocks produce repeated cipher blocks
- Don’t use ECB

### CBC (Cipher Block Chaining)

- Most common mode to preserve data integrity
- One whole block at a time
- Feedback of previous cipher block
- Propagates transmission errors (one bit transmission error destroys one whole block plus one more bit)

### CTR (Counter Mode)

- Useful for sending secret data without preserved data integrity
- Encrypts one block at a time
- Adds this output to the plaintext to form the next cryptogram
- Uses a counter as input, so uses block cipher as pseudorandom generator
- No error propagation

## Message Authentication Code

- A MAC is a function that takes two arguments, a message and a key, and generates a short tag
- These are generated and verified with the same key
- If no secret key is involved, you have a simple checksum, not a MAC (nor a signature)

Examples:

- HMAC: Hash-function-based MACs
- CBC-MAC: Use (part of) the last ciphertext block of a block cipher in CBC mode

### CBC-MAC, Cipher Block Chaining-MAC

- Run through CBC, use (part of) last block
- Vulnerable to collision (birthday) attacks
- Also vulnerable if used with variable-length messages
- Don’t use the same key for encryption and authentication

MACs achieve the following:

- They ensure data integrity
- They ensure data origin down to the level of owners of the (symmetric, secret) key, but not down to one single individual
- If you are one member of a pair that owns the key, and if the MAC is correct, and you know that you have not created this message, then the originator must be the other member of your pair

### Requirements on a MAC

- An ideal MAC behaves (from the point of view of Eve) as a random mapping from all possible inputs to all possible tags
- This requires using a secret key
- Note that the security is related to the tag length rather than the key length
- “Related” is used because the security is often given by half the tag length, rather than the entire tag length, because of the birthday paradox

#### The birthday paradox

How many people must there be in a room so that the probability of two of them having the same birthday is larger than 50%?

One tends to be selfish in these cases and think: ”The chance that another person has the same birthday as me is 1/365. The chance that two other person has the same birthday as me is (almost) 2/365. So, close to 183.”

But this is wrong! This calculation is correct when looking for matches to one specific person.

The correct calculation is the following: The chance that a second person does not have the same birthday as the first is 364/365.

If the two first do not have the same birthday, the chance that a third person does not have the same birthday as the two first is 363/365.

If these three do not share a birthday, ...

## The Secure Channel

If we want to do both Encryption and Authentication, which order is best?

1. Encrypt-then-Authenticate
2. Authenticate-then-Encrypt
3. Encrypt-and-Authenticate

Use different keys for encryption and authentication in the first two cases.

### The Horton principle

- Authenticate what is meant, not what is said
- For example message content and length
- If the recipient needs extra information on how to interpret the message, that information needs to be authenticated as well
- An example is varying data field lengths in internet protocols, another example is protocol version
- In fact, in SSL 3.0, the protocol version field (in the record layer) is not authenticated, neither are (some) message boundaries; these are minor problems

# Lecture 6 

## Symmetric key cryptography

systems where the encryption key is the same as the decryption

## Asymmetric key cryptography

Diffie and Hellman proposed the use of different keys for encryption and decryption

### Public key cryptography

Asymmetric key systems can be used in public key cryptography

### One-way functions

A one-way function is a function that is easy to compute but computationally hard to reverse

- Easy to calculate f(x) from x
- Hard to invert: to calculate x from f (x)

There is no proof that one-way functions exist, or even real evidence that they can be constructed

A trapdoor one-way function has one more property, that with certain knowledge it is easy to invert, to calculate x from f(x)
- Easy to calculate (x^e mod n) from x
- Hard to invert: to calculate x from (x^e mod n)

There is no proof that trapdoor one-way functions exist, or even real evidence that they can be constructed.

The trapdoor is that with another exponent d it is easy to invert, to calculate x = (x^e mod n)^d mod n

There is no proof that this is a true trapdoor one-way function, but we think it is

### Greatest Common Divisor

The Euclidean algorithm

576 = 4 x 135 + 36
135 = 3 x 36 + 27
36 = 1 x 27 + 9
27 = 3 x 9 + 0

Theorem (the extended Euclidean algorithm): Given nonzero a and b, there exist x and y such that:
ax + by = gcd(a, b)

### Arithmetic mod n

- Numbers mod n are equal (congruent) if their difference is a multiple of n
- Addition, subtraction, and multiplication mod n works as usual:
  - 5 = 27 mod 11 because 27 − 5 = 2 x 11
  - 5 + 7 = 1 mod 11 because (5 + 7) − 1 = 11
  - 5 − 7 = 9 mod 11 because 9 − (5 − 7) = 11
  - 5 · 7 = 2 mod 11 because (5 x 7) − 2 = 3 x 11
- But division is not always possible

### Division mod n

If gcd(a, n) = 1, then you can divide by a, because of the following theorem:

Theorem: If gcd(a, n) = 1 there exists an x such that ax = 1 mod n

Proof: The extended Euclidean algorithm gives us x and y so that ax + ny = 1. Now,

ax + ny = ax mod n

so

ax = 1 mod n

### Fermat’s little theorem

Having learnt how division works (mod p), we can prove

Theorem: If p is a prime and p does not divide a, then a^(p−1) = 1 mod p

### Euler’s theorem

See slides

### Square roots mod n

x^2 = 1 mod 7 has the solutions ±1 (as for all odd primes)

x^2 = 1 mod 15 has the solutions ±1, ±4

The last seems simple enough (±1 mod 3 and ±1 mod 5), but how do we find solutions in general?

Chinese remaindering: see slides.

### Rivest Shamir Adleman (1977)

- Bob chooses secret primes p and q, and sets n = pq
  - Choose primes p and q using, say, the Miller-Rabin test
  - Choose primes of slightly different size
  - Choose p and q so that p − 1 and q − 1 has at least one large
prime factor
- Bob chooses e with gcd(e, φ(n)) = 1
- Bob computes d so that de = 1 mod φ(n)
- Bob makes n and e public but keeps p, q and d secret
- Alice encrypts m as c = m^e mod n
- Bob decrypts c as m = c^d mod n

# Lecture 7

Examples on attacks. See lecture slides

## The right kind of problem?

- NP (Nondeterministic Polynomial time)-problems are an important concept in complexity theory
- The necessary effort to solve any problem is usually approximated as some expression involving the size of a parameter
- In cryptography we want to know that there is no better attack method than exhaustive search, which grows exponentially with the size of the key
- Problems that can be solved in polynomial time lie in a smaller class of problems, P
- The problems believed to be in NP but not in P do not have efficient solutions, the known algorithms grow faster than any polynomial expression of the size of a problem parameter

## The class of NP-complete problems

- NP-complete problems are the hardest problems in the NP complexity class: any other NP problem can be rewritten as a NP-complete problem in polynomial time
- It is unknown whether P̸=NP. If an NP-complete problem can be solved in polynomial time, then P=NP
- One example of a NP-complete problem is “the knapsack problem”
  
## The knapsack problem, original

- A travelling salesman wants to pack as many items as possible in his knapsack (=bag)
- All items have different sizes
- How can he find the subset that maximizes the total size into the knapsack?
- A physical knapsack is 3D, so let us simplify into one dimension

### The one-dimensional knapsack

You are given a set of numbers, all different,

D = {d1 , d2 , ... , dn },

and the sum c of the elements in a subset M of D, but the subset M is unknown to you

The knapsack problem is now to deduce what elements are in M (what bits are set in the message)

If D is “superincreasing”, the problem is simple to solve, but the knapsack problem in its general version is NP-complete

### Weakness of the knapsack

- You can recreate Ds from D using u and s
- The weakness is that any u and s that creates a superincreasing knapsack makes the problem simple, it is irrelevant if this is the original one or not
- And such values are easy to find, if D is constructed from a superincreasing knapsack
- Remember, in general, the knapsack problem is NP-complete, but (as it turns out) if the knapsack is constructed from a superincreasing one, the problem is much simpler

## A different idea: double one-way functions

Use two one-way functions f and g that satisfy the symmetry:

g(f(a), b) = g(f(b), a)

This cannot be used for encryption/signing because one does not necessarily recover a or b

But it can be used for key exchange:
- Alice takes a secret random a and makes f(a) public
- Bob takes a secret random b and makes f(b) public
- Both can now create k = g(f(b), a) = g(f(a), b)

### Diffie-Hellman key exchange

Use exponentiation mod p:

g(x, y) = x^y mod p
f(x) = g(α, x) = α^x mod p

where α is a “primitive root of numbers mod p”

The symmetry is:

g(f(a), b) = (α^a)^b = (α^b)^a = g(f(b), a) mod p

This can be used for key exchange: parameters p and α:
- Alice takes a secret random a and makes α^a mod p public
- Bob takes a secret random b and makes α^b mod p public
- Both can now create k = (α^a)^b = (α^b)^a mod p

Read more about security of DH in slides...

### ElGamal encryption

Choose a large prime p, and a primitive root α mod p. Also, take a random integer a and calculate β = α^a mod p.

The public key is the values of p, α, and β, while the secret key is the value a.

Encryption uses a random integer k, and the ciphertext is the pair: (α^k, β^k m)
- α^k is used to transmit the “one-time secret” k
- β^k is the “one-time pad” for m

Decryption is done with a, by calculating

(α^k)^(−a) (β^k m) = (α^(−ak))(α^(ak) m) = m mod p

Read more about security of ElGamal in slides...

# Lecture 8

## A MAC is what you get from symmetric cryptography

A MAC is used to prevent Eve from creating a new message and inserting it instead of Alice’s message

## Signature vs MAC

- A MAC, Message Authentication Code, preserves data integrity, i.e., it ensures that creation and any changes of the message have been made by authorised entities
- Only the authorised entities can check a MAC, and all who can check can also change the data
- In most legally interesting cases, you want to be able to verify that one single individual wrote something
- Also, in many situations it is good if everyone is able to check the signature

### Digital signatures

- In asymmetric ciphers, one single individual holds the private key, while everyone can get the public key
- So if you encrypt with the private key, and send both cryptogram and message, anyone can check that “decryption” with the public key does indeed create the message
- Note that some public key systems do not allow “encryption” with the private key
- Most systems can be modified to generate and verify signatures
- A digital signature should not only be tied to the signing user, but also to the message
- The example of encrypting with the private key does this: only Alice can create it, and it is valid only if the decryption and the plaintext coincide
- No attempt is made to hide the information (unless it is encrypted using another method)

### RSA signatures

- Alice sets up RSA as usual
- In order to sign a message m, Alice uses her private key d (and not Bob’s public key) to create the signature:
  - s = m^d mod n
- Alice now gives both m and s to Bob
- He uses Alice’s public key to verify the signature by comparing:
  - m and s^e mod n

#### Variation: Blind RSA signatures

- Bob wants to prove that he has created a document at a certain time, but keep it secret, and Alice agrees to help him. She sets up standard RSA, keeping d for herself.
- Bob chooses a random integer k, and gives Alice the message:
  - t = k^e m mod n
- The number t is random to Alice, but she signs the message and gives the signature to Bob:
  - s = t^d = k^(ed) m^d = km^d mod n
- Bob can now divide by k and retrieve m^d, Alice’s signature for m.

### ElGamal signatures

- Choose a large prime p, and a primitive root α mod p. Also, take a random integer a and calculate β = α^a mod p
- The public key is the values of p, α, and β, while the secret key is the value a

More details in slides

### The need for hashing

- Unfortunately, all known signature algorithms (RSA, ElGamal, ...) are slow
- Also, all known signature algorithms generate output with the same size as the input
- Therefore, it is much better to shorten (hash) the message first, and sign the short hash:
  - (m, sig(m)) becomes (m, sig(h(m)))
- A typical hash function has 160-512 bit output (giving 80-256 ”bits” of security)

### Digital Signature Algorithm (∼ElGamal)

- This is a modification to the ElGamal signature scheme adopted as standard by NIST in 1994
- Some debate followed, comparing DSA and RSA signatures
- The most serious problem was parameter size, which is better in later versions
- The main change from ElGamal is to choose p so that p − 1 has a 160-bit prime factor q, and exponents are mod q

### DSA signatures

See lecture slides

## Weakly collision-free hash functions

A weakly collision-free hash function is a function that is easy to compute but computationally hard to find a second preimage for:
- Easy to calculate h(x) from x
- Hard to find a second preimage: to calculate x' from x so that h(x') = h(x)

There is no proof that weakly collision-free hash functions exist, or even real evidence that they can be constructed

## Strongly collision-free hash functions

A strongly collision-free hash function is a function that is easy to compute but computationally hard to find a collision for:
- Easy to calculate h(x) from x
- Hard to find a collision: to find x and x' (x' /= x) such that h(x') = h(x)

There is no proof that strongly collision-free hash functions exist, or even real evidence that they can be constructed

## Birthday attacks on message hash signatures

- Fred (the Fraudster) knows that Alice will sign a contract. His goal is to get a signature from Alice on a different contract.
- Fred takes the original contract and produces small variations in it. He can add spaces at the line ends, change the wording slightly, add nonprinting data, and so on. Thirty changes will give 2^30 different documents.
- He now does the same with the fraudulent contract, and attempts to find a match for the hash values of the two lists. The same signature will be valid for those two contracts
- If the hash values are shorter than 60 bits, the probability of a match is very high.

## Theoretical security: The Random Oracle Model

- A Random Oracle is a function that gives fixed length output. If it is the first time that it receives a particular input, it gives random
output. If it already has received the input, it will repeat the corresponding output
- This is the ideal strongly collision resistant function
- It can be used to prove security of encryption/signing under the assumption that the functions used behave like a random oracle
- This is usually treated as strong evidence rather than a real proof. The evidence falls if weaknesses are found in the functions used

## Desirable properties of a practical hash function

- Transformation of messages of any length to a fixed length block
- Dependence on every bit of message
- Everyone should be able to check the validity of a hash of a message
- You could still use a secret parameter, but this means you restrict the group of people that can verify to the group that knows the secret

## Iterative hash functions

- Mapping any-length input into fixed-length output is complicated
- A simple way is to use an iterative hash function, to divide the message into blocks

More details in slides

## HMAC (Hash-function-based MAC, FIPS standard)

- Applies h twice to avoid length-extension attacks on h(k||m), collision attacks on h(m||k), and more complicated attacks on h(k1||m||k2)
- The values used to change the key in the two steps are not critical, but security is better if they differ in at least one bit
- Since SHA-3 is not vulnerable to length-extension attacks, h(k||m) could be used — but HMAC is standard

## Constructing cryptographic hash functions

- Be wary of inventing a new hash function
  - double hashing
  - concatenating hashes
  - XORing the results
- Adding a few operations to a hash function or combining existing functions is a tempting do-it-yourself fix, but usually adds only an
insignificant amount of complexity to the attack, and can easily reduce the complexity of attack
- It was long thought that: H(M) = H1(M)||H2(M) would be much stronger than either component—no such luck—it
is only slightly better than the best component
- There is a reason for the counters in MD1-5, SHA0-3

## MD5

- 128-bit hash
- Very widespread (SSL/TLS, IPsec, ...) but not secure (X.509 collisions, rouge CA, ...)

See more on slide

## SHA-1

- 160-bit hash
- Very widespread, but being phased out
- Weaknesses found in 2005
- Collision attack in 2017 needs 2^63.1 SHA-1 evaluation

See more on slide

## SHA-2

- 224, 256, 384, 512-bit hash
- Widespread, taking over from SHA-1
- The two longer ones are 10% slower

See more on slide

## SHA-3 contest

- NIST started a contest 2007 to find (future) replacements for SHA-1 and SHA-2
- Finalists were chosen for reasons of speed, security, available analysis, and diversity
- NIST selected five SHA-3 candidates for the final evaluation: BLAKE, Grøstl, JH, Keccak, and Skein
- The decision was made Oct 3 2012, and the winner was Keccak
- The standard was published 2015, but there was some
controversy as NIST “tuned the hash function for speed”

### SHA-3

- 224, 256, 384, 512-bit hash
- 12.5 cycles per byte

See more on slide

## The sponge construction

- Mapping any-length input into fixed-length output is complicated
- The sponge construction is used to absorb r bits in each iteration
- The unused capacity c should be twice the desired resistance to collision or preimage attacks
- Still needs secure padding of the last block

## Other uses of cryptographic hash functions

- They hide what input to use in order to get a specific output, which is needed in for example password storage
- Cryptographic hash functions can be used to encrypt, basically running it as a block cipher in OFB mode
- Other applications of hash functions in computer science (hash tables, file integrity, ...) often has weaker requirements than the ones used here
- It is therefore important to remember that hash functions as used in computer science often do not fulfil the requirements of a cryptographic hash function

## Example: (Early) UNIX passwords

- Passwords are assumed to be 8 character 7-bit ASCII
- In total 56 bits
- Encrypt the message 0 using this DES key

### Weaknesses

- Dictionary attacks
  - Use a salt to make dictionary attacks more difficult
  - The book mentions 12-bit salts
- DES is fast, and there is dedicated hardware
  - Don’t have the salt in the message
  - Instead use it to change the DES algorithm
  - This makes hardware attacks more difficult and slows down an attack

## Example: Modern unixes

- Stronger algorithms (proper hash functions)
- Whole password significant
- Longer salts (up to 16 characters)
- Use “key stretching”: many many rounds
- Only root can read the passwd file

## Hash function life cycles

Historically, popular cryptographic hash functions have a useful lifetime of around 10 years

# Lecture 9

## Key management

- The first key in a new connection or association is always delivered via a courier
- Once you have a key, you can use that to send new keys
- If Alice shares a key with Trent and Trent shares a key with Bob, then Alice and Bob can exchange a key via Trent (provided they both trust Trent)

## Key distribution center

- If Alice shares a key with Trent and Trent shares a key with Bob, then Alice and Bob can exchange a key via Trent (provided they both trust Trent)

### Key server

- If Alice shares a key with Trent and Trent shares a key with Bob, then Alice and Bob can receive a key from Trent (provided they both trust Trent)

### Blom key pre-distribution

- If Alice shares a key with Trent and Trent shares a key with Bob, and Alice and Bob each have a public id rA , rB , they can recieve key-generation info from Trent (provided they both trust Trent)

### Station-To-Station (STS) protocol

- If Alice shares a key with Trent and Trent shares a key with Bob, then Alice and Bob can use Trent to verify that they exchange key with the right person

### Replay attacks

- But perhaps Eve has broken a previously used key, and intercepts Alice’s request
- Then she can fool Bob into communicating with her

### Wide-mouthed frog

- Alice and Trent add time stamps to prohibit the attack
- But now, Eve can pretend to be Bob and make a request to Trent

### Needham-Schroeder key agreement

- Another variation is to use nonces to prohibit the replay attack
- If Eve ever breaks one session key, she can get Bob to reuse it

## Kerberos

- A client, Cliff
- An authentication server, Trent
- An authorization server, Grant
- A service server, Serge
- They share keys KC , KG , KS
  
1. Cliff sends Trent IDC||IDG
2. Trent responds width EKC(KCG)||TGT where TGT = IDG||EKG(IDC||t1||KGC)
3. Cliff sends Grant EKCG(IDC||t2)||TGT
4. Grant responds with EKCG(KCS)||ST where ST = EKS (IDC||t3||texpir.||KCS)
5. Cliff sends Serge EKCS (IDC||t4)||ST and can then use Serge’s services

## Public key distribution

- Public key distribution uses a Public Key Infrastructure (PKI)
- Alice sends a request to a Certification Authority (CA) who responds with a certificate, ensuring that Alice uses the correct key to communicate with Bob

### Using X.509 certificates

- The CAs often are commercial companies, that are assumed to be trustworthy
- Many arrange to have the root certificate packaged with IE, Mozilla, Opera, ...
- They issue certificates for a fee
- They often use Registration Authorities (RA) as sub-CA for efficiency reasons

### Using web of trust

- No central CA
- Users sign each other’s public key (hashes)
- This creates a “web of trust”
- Each user keeps a keyring with the keys (s)he has signed
- The secret key is stored on a secret keyring, on h{er,is} computer
- The public key(s) and their signatures are uploaded to key servers

## Secure Sockets Layer (SSL) and Transport Layer Security (TLS)

- This is a client-server handshake procedure to establish key
- The server (but not the client) is authenticated (by its certificate)
- SSL 1.0 (no public release), 2.0 (1995), 3.0 (1996), originally by Netscape
- TLS 1.0 (1999), changes that improve security, among other things how random numbers are chosen
  - Sensitive to CBC vulnerability discovered 2002, demonstrated by BEAST attack 2011
  - Current problem: TLS 1.0 is fallback if either end does not support higher versions
- TLS 1.1 (2006), added protection against CBC attacks by explicit IV specification
- TLS 1.2 (2008), e.g., change MD5-SHA1 to SHA256
- Never fall back to SSL 2.0 (2011)
- TLS 1.3 (August 2018), many improvements
  - CBC is gone, beacuse of BEAST
  - Static-RSA-key exchange is removed (!), no forward secrecy
  - MD5 (!), RC4, SHA1 and so-called “Export” algorithms removed
  - More efficient session startup, less TCP packets

## Forward Secrecy

- Forward secrecy: Even if the key-distribution cipher is broken, only the current and possibly future sessions are broken, not the previous sessions
- RSA as key transport does not give forward secrecy, while RSA as signing algorithm may give forward secrecy
- STS (DH) gives forward secrecy if new secrets a and b are used for each session (so-called “Ephemeral DH”, but beware of reusing the prime p)
- This property does not only depend on the cipher suite used, but on the details of how it is used

## Elliptic curves

An elliptic curve is the set of solutions to the equation: y^2 = x^3 + ax^2 + bx + c

These solutions are not ellipses, the name elliptic is used for historical reasons and has do to with the integrals used when calculating arc length in ellipses.

An elliptic curve is the set:

E = {(x, y) : y^2 = x^3 + ax^2 + bx + c}

Most of the time a “depressed” cubic is enough:

E = {(x, y) : y^2 = x^3 + bx + c}

You do not want “singular curves” with double or triple roots:

E = {(x, y) : y^2 = x^3 + bx + c}

Previously we have used the multiplicative group of integers mod p

We need a group operation on points of E , we’ll call it “addition”

### Addition on elliptic curves

- Given two elements in the group, construct a third
- Draw a straight line through the two points, it will intersect the elliptic curve in a third point. Mirror that in the x-axis
- There is one special case: if the line through the two points is vertical, it will not intersect the elliptic curve again
- We add the point (∞, ∞) to E
  - This is the neutral element, the “0”
  - That is, (∞, ∞) + (x, y) = (x, y)
  - This also means that −(x, y) is (x, −y)
- If adding a point to itself, use the tangent line

See lecture slides for more details

### Multiplication on elliptic curves

- Multiplication with an integer is defined through repeated addition:

3(x, y) = (x, y) + (x, y) + (x, y)

- We want to have a discrete set of points. We arrange this by having coordinates mod p

E = {(x, y) : y^2 = x^3 + bx + c mod p}

- This is not so easy to draw in a diagram, remember, it is y^2 mod p

### Discrete Logarithms on elliptic curves

- The discrete logarithm for elliptic curves: given two points A and B on an elliptic curve, find k so that:

B = kA = A + A + ... + A

- There is an analog for the Polig-Hellman algorithm
- The baby step-giant step algorithm is impractical
- But most importantly, there is no analog for the index calculus
  - Integer mod p index calculus is based on using small base numbers (not small exponents as in Polig-Hellman)
  - But there are no points on E that are closer to “0” than any other points, the distance to (∞, ∞) is the same for all other points

## Trapdoor one-way functions

- A trapdoor one-way function is a function that is easy to compute but computationally hard to reverse
  - Easy to calculate xA from x
  - Hard to invert: to calculate x from xA
- A trapdoor one-way function has one more property, that with certain knowledge it is easy to invert, to calculate x from xA
- There is no proof that trapdoor one-way functions exist, or even real evidence that they can be constructed

## Standard (m mod p) ElGamal encryption

See lecture slides

## Representing plaintext on elliptic curves

- Unfortunately, it is not simple to represent a given plaintext as a point on E
- Even worse, there is actually no polynomial time algorithm that can write down all points of an elliptic curve
- There is a method that will work with high probability:
  - The message m should be in the x-coordinate, but there is no guarantee that m^3 + bm + c is a square mod p
  - Each number x has a probability of about 1/2 that x^3 + bx + c is a square, so put a few bits at the end of m and run through all possible values
  - If the number of possible values is K , the risk of failure is 2^(−K)

## Standard (integer mod p) Diffie-Hellman key exchange

See lecture slides

# Lecture 10

## Key distribution is a problem in cryptography

- Public key transfer rests on the (unproven) hardness of certain mathematical problems such as factoring
- Another solution: Transfer the key secretly, and use symmetric key cryptography

## Quantum key distribution

Task: to transfer (share) secret key
Idea: Content on a quantum channel changes when Eve listens (The classical channel in the scheme is not encrypted)

A lot of pictures and details in slides.

### Attack possibilities for Eve

- Intercept-resend (Heisenberg)
- Entangling probe (Monogamy of entanglement)
- Cloning (No-cloning theorem)
- Coherent attacks (more advanced versions of the above)
- Side channel attacks
  - Photon-number splitting
  - Trojan horse
  - Weaknesses of the equipment

### Quantum Key Distribution, version 1

- Generate raw key
- Sift the key
- Check the noise level

#### Problem 1

- A real-life quantum channel has noise even without Eve

### Quantum Key Distribution, version 2

- Generate raw key
- Sift the key
- Reduce and check the noise level

#### Reconciliation (Error correction)

- Bob takes two random bit values (e.g., nr 137 and 501)
- He calculates their XOR and sends the bit indices and the XOR value to Alice
- Alice compares with her XOR value
- If the XOR values are the same, keep the first bit value, otherwise none of them

#### Problem 2

- A real-life quantum channel has noise even without Eve
- Eve might have better technology than Alice and Bob (less noisy quantum channel)
- In that case, she can change to her quantum channel and also eavesdrop, up to the former noise level

### Quantum Key Distribution, version 3

- Generate raw key
- Sift the key
- Reduce and check the noise level
- Reduce Eve’s information on the new key

#### Privacy amplification

- Bob takes two random bit indices (e.g., nr 43 and 212)
- He sends the bit indices to Alice (but not the XOR value)
- Alice and Bob individually computes the XOR value
- They remove their bit values and insert the XOR value (without having sent them on the classical channel)

#### Noise limit

- BB84 can manage a QBER of 11%

#### Problem 3

- Messages on real-life classical channels can be modified

### Man-in-the-middle

Eve can pretend to be Bob when she speaks to Alice and pretend to be Alice when she speaks to Bob

### Quantum Key Distribution, final version

- Generate raw key
- Sift the key
- Reduce and check the noise level
- Reduce Eve’s information on the new key
- Authenticate the messages on the classical channel

### Quantum Key Distribution = Quantum Key Expansion

- Raw key generation
- Sifting
- Reconciliation
- Privacy amplification
- Authentication

- Information-theoretically secure auth uses secret key
- The system needs secret key to start

# Lecture 11

## Bit commitment

- Alice wants to commit to a value of a bit b, such as the outcome of a Hockey game the next weekend
- Her intent is to show Bob she can predict the outcome, but she does not want to tell Bob how, or what the result is (so that he won’t reduce Alice’s winnings)
- But nonetheless she wants to give Bob something, that he can use to verify that she knew, after the game has been played

## Zero knowledge

- A “zero-knowledge” scheme is intended to convince someone that you have certain knowledge, without giving the knowledge to this someone
- The standard example is that Peggy claims to know how to open a door inside a tunnel system
- She wants to convince Victor that she can do this, without telling Victor how to open the door
- If Peggy exits from the correct path enough times, Victor is convinced that Peggy can open the door
- But Victor has not learnt how to open the door
- And a third person cannot be convinced that Peggy can open the door
- It doesn’t even help to have a video of everything that Victor sees
- This is known as an interactive protocol

1. Peggy enters, while Victor waits outside
2. Victor goes to the intersection and calls out “left” or “right”
3. Peggy exits from the correct path
4. If this happens enough times, Victor is convinced that Peggy can open the door

### Zero-knowledge cut-and-choose

The classic cut-and-choose

1. Peggy cuts the cake in half
2. Victor chooses one of the halfs for himself
3. Peggy takes the remainders

The cave&door protocol works because Peggy cannot guess which path Victor will ask for, so she only has 50% chance of fooling him.

Repeating 10 times reduces Peggy’s chance to fool Victor to 2^(−10) ≈ 1/1000

### Zero-knowledge, formal setup

Peggy knows the solution to a hard mathematical problem, such as the pre-image to a one-way function output

1. Peggy uses a random number to transform the problem into a different, but equally hard mathematical problem. She commits to the solution of the new instance (using bit commitment) and reveals it to Victor
2. Victor asks Peggy to either a) prove that the two problems are isomorphic, or b) provide the solution of the second problem
3. If Peggy can do this enough times, Victor will conclude that Peggy can solve the original problem

Peggy is known as the prover, while Victor is known as the verifier

It is important that Peggy’s and Victor’s choices is random (to the other participant), otherwise cheating is possible

### Digital signatures are not Zero-knowledge

- Either Alice chooses the message, and gives Bob message and signature. In this case, there is no (random) challenge from Bob.
  - Bob is not convinced. Messages have no structure here, so how does he know Alice did not select the signature first and used the public key to generate the message?
- Or Bob chooses the message and asks Alice to sign, but then, there is no (random) commitment by Alice
  - Here the danger is that Bob could carefully select messages to extract information on the secret
- In proper zero knowledge protocols, new randomness is added at each instance, so that no information on the secret is revealed to the verifier

## Secret sharing

- Imagine you make a lot of money on the latest Iphone app you wrote
- You live happily, but make plans for the future: at some point your (less trustworthy) children are going to inherit
- You want to force them to cooperate to open the safe
- This is easily done by giving them each one number in the three-number combination
- If you happen to have, say, seven children, or want to give them less information (not making exhaustive search easier), you need to be more sophisticated

### Secret splitting

- You want to split a secret message m between Alice and Bob so that they need to cooperate to read it
- The simplest way to do this is to take a random r , give r to Alice and m − r to Bob
- If you want to do this among n people, give n − 1 of them random numbers, and the last one m − r1 − r2 − ... − rn−1
- They must now add their numbers to find m

### More sophisticated secret sharing

See slides

### Lagrange interpolation

See slides

### Blakley secret sharing

- Another way to generalize the linear function we started with is to go to higher dimension
- Use intersecting hyperplanes in t dimensions instead
- Almost the same as Shamir’s polynomial secret sharing

More details in slides

## Coin tossing over the phone

- Alice and Bob want to toss coins over telephone to decide who gets something they both want. To set up the system, Alice chooses p and q prime (congruent to 3 mod 4) and sends Bob n = pq
- Bob chooses a random x < n/2 and sends Alice u = x^2 mod n
- Alice computes the four roots (easy if you know p and q, hard if not), at random chooses v as one of the two < n/2 and sends v to Bob
- If v = ±x mod n, Bob sends “OK, you win” to Alice. If v /= x, Bob sends “I win” and x to Alice, to prove he knew it (he can now factor n; Alice should check that x^2 = u mod n)

## Poker over the telephone, discrete log

- Alice and Bob want to play Poker over the telephone; they agree on a large prime p, represent the cards using 52 numbers, and choose secrets α and β so that gcd(α, p − 1) = 1 = gcd(β, p − 1)
  - Bob encrypts each card by raising it to the power β and sends the set to Alice
  - Alice chooses five cards for herself and encrypts them with α, and five cards for Bob. She sends all ten cards to Bob
  - Bob decrypts the cards by raising it to the power β^(−1) mod p − 1, and sends back Alice’s cards
  - Alice decrypts her cards
- Both keep the encrypted version of the other’s cards
- Discarded cards are encrypted and sent to the other
- To end the game, they claim their hands and prove them by revealing their keys
- Encryption as c = m^k mod p and decryption as m = c ^(1/k) mod p
- This is a system with private keys only
- Security depends on discrete log complexity

### With RSA

- Alice and Bob want to play Poker over the telephone; they both set up for RSA
  - Alice encrypts each card with her public key and sends the set to Bob
  - Bob chooses five cards for himself and encrypts them with his public key, and five cards for Alice. He sends all ten cards to Alice
  - Alice decrypts the cards using her private key, and sends back Bob’s cards
  - Bob decrypts his cards
- Both keep the encrypted version of the other’s cards
- Discarded cards are encrypted and sent to the other
- To end the game, they claim their hands and prove them by revealing their private keys

#### Cheating

- Alice and Bob want to play Poker over the telephone; they both set up for RSA
- Alice’s encryption as c = m^ea mod na and decryption as m = c^da mod na
- Security depends on complexity of factoring

## Summary

- Bit commitment is used to convince someone that you have certain knowledge, without giving the knowledge to this someone — uses one-way functions 
- A zero-knowledge scheme is also used to convince someone that you have certain knowledge, without giving the knowledge to this someone — uses a random commitment and a random challenge, not one-way functions
- Secret sharing is used to divide a secret into pieces so that all (or at least t) pieces are needed to read the secret
- Coin tossing or poker over the phone uses bit (or card) commitment to convince participants that it is a fair game

# Lecture 12

This lecture includes a lot of symbols and is hard to summarize in an .md-file. See lecture slides.