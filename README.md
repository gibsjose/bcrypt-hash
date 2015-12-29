# Generate and verify secure cryptographic hashes from the command-line using `bcrypt`

`bcrypt-hash` generates a secure cryptographic hash using the PHP 5.5 `password_hash` function by specifying `bcrypt` as the algorithm.

This was written as a command-line utility to experiment with generating `bcrypt` hashes in a similar way that is possible with the SHA family by using the `shasum` or `md5` utilities.

## Compatibility
This utility will only work with PHP 5.5+. See the comment [here](http://stackoverflow.com/a/17073604) for options in regards to using older versions of PHP.

## Usage
```bash
# Example: Hash the plaintext 't3rr1bl3_p4$$w0rd' with a cost factor of 12
bcrypt-hash -c 12 't3rr1bl3_p4$$w0rd'
$2y$12$UzKl7mitlZJt52PAMemYmeb9YUC9XhvX6DlbtbaVtdqI32TCPPCj6

# Example: Hash the plaintext 'Look! Here is some plaintext...' with the default cost factor of 10
bcrypt-hash 'Look! Here is some plaintext...'
$2y$10$k8pe9htFbLrJD/EjOE3In.RPOFpPz2WZ44lwQVt8RJRmUgXNnfnSC

# Example: Check the plaintext 'test' against a correct hash
bcrypt-hash check 'test' '$2y$10$5ixGI4bAKbWI4bdlzbXi9uqaOrysHRuqbBLP4N8HhgPL6c5yIuS2a'
Verified

# Example: Check the plaintext 'test' against an incorrect hash
bcrypt-hash check 'test' '$2y$10$8zcwWCamJ3a.w.D3Y82cWOfyeQygxG9HHBCOpXy7w18I2cbsN9IC2'
No match

# Example: Show the help
bcrypt-hash -h

# Example: Show the version
bcrypt-hash -v
```
>**Note:** Your hashes will be different, since `bcrypt` generates it's own salt.

The cost factor must be between `04` and `32` as specified [here](https://secure.php.net/manual/en/function.crypt.php), or else it will default to `10`.

The cost factor indicates the number of expansion rounds performed during the main loop of the hash function:
```
number of rounds = 2^cost
```

Currently, a cost factor of 12 or 13 (4096 or 8192 rounds) is recommended as a good balance between responsiveness and security.

## `bcrypt` Hash Format
A `bcrypt` hash follows the following standard format:
```
$2y$cc$ssssssssssssssssssssssHHHHHHHHHHHHHHHHHHHHHHHHHHHHHHH
```

Where:
* `$2y$` is the standard `bcrypt` prefix
* `cc` is the two-digit representation of the cost factor, from `04` to `32`
* `sss...sss` is a 128-bit salt encoded as 22 base-64 digits
* `HHH...HHH` is the 184-bit hash encoded as 31 base-64 digits

## More Information
For more information on `bcrypt`, see the [Wikipedia article](https://en.wikipedia.org/wiki/Bcrypt).

See [here](https://secure.php.net/manual/en/function.password-hash.php) for more information on PHP's `password_hash` function, and [here](https://secure.php.net/manual/en/function.crypt.php) for more information on the `cost` and `salt` parameters.

## Thanks
Thanks to the incredible [`docopt`](https://github.com/docopt/docopt.php) PHP library, which made the documentation and command-line argument processing a breeze.
