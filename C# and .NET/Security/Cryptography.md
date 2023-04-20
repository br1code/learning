# Cryptography

## Simple Explanation

Cryptography is the practice of securing information by transforming it into an unreadable format, so that it can only be accessed by authorized parties who possess the necessary keys or passwords to decrypt the information. In C# and .NET, there are a number of cryptographic classes and methods available that allow developers to implement various encryption and decryption algorithms.

## Deep Explanation

Cryptography is a crucial aspect of modern data security. It is used to protect data and communications from unauthorized access, interception, and tampering. There are two main types of cryptography: symmetric and asymmetric.

Symmetric cryptography involves using the same key for both encryption and decryption. This means that anyone who has the key can both encrypt and decrypt data. Common symmetric encryption algorithms include Advanced Encryption Standard (AES) and Data Encryption Standard (DES).

Asymmetric cryptography involves using two different keys, one for encryption and another for decryption. This is also known as public key cryptography. The encryption key is publicly available and can be used by anyone to encrypt data, but only the holder of the corresponding private key can decrypt the data. Asymmetric encryption algorithms include RSA and Elliptic Curve Cryptography (ECC).

In C# and .NET, there are a number of classes and methods available for implementing cryptography. These include:

- System.Security.Cryptography.SymmetricAlgorithm: This is an abstract class that provides a base implementation for symmetric encryption algorithms. To use it, you would need to create an instance of a derived class such as AesManaged or TripleDES.

- System.Security.Cryptography.AsymmetricAlgorithm: This is an abstract class that provides a base implementation for asymmetric encryption algorithms. To use it, you would need to create an instance of a derived class such as RSACryptoServiceProvider or ECDiffieHellmanCng.

- System.Security.Cryptography.HashAlgorithm: This is an abstract class that provides a base implementation for hashing algorithms. Hashing is a one-way encryption method that transforms data into a fixed-length string of characters. It is often used to verify the integrity of data. To use it, you would need to create an instance of a derived class such as SHA256Managed or MD5CryptoServiceProvider.

- System.Security.Cryptography.RNGCryptoServiceProvider: This class provides a cryptographically secure way to generate random numbers.

## Examples

Some basic examples of cryptography in C# and .NET include:

1 - Encrypting and decrypting a string using symmetric encryption:

```C#
string plaintext = "This is a secret message!";
using (AesManaged aes = new AesManaged())
{
    byte[] key = aes.Key;
    byte[] iv = aes.IV;

    byte[] encrypted = CryptographyUtils.EncryptStringToBytes(plaintext, key, iv);
    string decrypted = CryptographyUtils.DecryptStringFromBytes(encrypted, key, iv);
    Console.WriteLine("Original: {0}", plaintext);
    Console.WriteLine("Encrypted: {0}", Convert.ToBase64String(encrypted));
    Console.WriteLine("Decrypted: {0}", decrypted);
}
```

2 - Generating a random salt for password hashing:

```C#
byte[] salt = new byte[32];
using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
{
    rng.GetBytes(salt);
}
```

3 - Hashing a password with salt:

```C#
string password = "mypassword";
byte[] salt = new byte[32];
using (RNGCryptoServiceProvider rng = new RNGCryptoServiceProvider())
{
    rng.GetBytes(salt);
}
string hashedPassword = CryptographyUtils.HashPassword(password, salt);
Console.WriteLine("Password: {0}", password);
Console.WriteLine("Salt: {0}", Convert.ToBase64String(salt));
Console.WriteLine("Hashed password: {0}", hashedPassword);
```