---
layout: post
title: Creating my own substitution cipher application
description: Demonstrating my own substitution cipher application created in C# WPF using my own encoding method to encode the plaintext message into the encrypted strings.
keywords: substitution cipher, custom encoding method, heiswayi nrird cipher program, c# wpf, encryption and decryption
tags: [CSharp, WPF, Cryptography]
comments: true
---

> Please note that, this application is intended for experimental and learning purpose. Sometimes, I just do it for fun. There's no guarantee that this application would be practical for the real world use.

### What is cipher?

In cryptography, a cipher is an algorithm for performing encryption or decryption of information into some kind of secret codes. For a **substitution cipher**, it's a method of encoding characters from the plaintext message into something called ciphertext. There are few popular cipher algorithms such as steganography, ROT13, Caesar shift cipher, Vigenere, etc. which I can simply use. There are also the modern unbreakable of encryption algorithms available such as Triple DES, RSA, Blowfish, Twofish, AES, etc.

But to be epic and for the sake of fun, I decided to use a substitution method with my own designed keys and encoding algorithm. So, I created my own cipher application in C# WPF which obviously I named as _Heiswayi Nrird Cipher Program_. Not really a nice name huh?

### Screenshot of the application

![Heiswayi Nrird Cipher Program](http://i.imgur.com/AbUYj16.png)

### Here's how I implemented my encoding method

First, since this is a substitution cipher, so I have defined two my own dictionaries; one for encryption, another one for decryption.

Encryption dictionary:

```csharp
var encryptionDictionary = new Dictionary<string, string>(StringComparer.CurrentCultureIgnoreCase)
{
    { "a", "h01" },
    { "b", "e02" },
    { "c", "i03" },
    { "d", "s04" },
    { "e", "w05" },
    { "f", "a06" },
    { "g", "y07" },
    { "h", "i08" },
    { "i", "n09" },
    { "j", "r10" },
    { "k", "i11" },
    { "l", "r12" },
    { "m", "d13" },
    { "n", "h14" },
    { "o", "e15" },
    { "p", "i16" },
    { "q", "s17" },
    { "r", "w18" },
    { "s", "a19" },
    { "t", "y20" },
    { "u", "i21" },
    { "v", "n22" },
    { "w", "r23" },
    { "x", "i24" },
    { "y", "r25" },
    { "z", "d26" },
    { "0", "h27" },
    { "1", "e28" },
    { "2", "i29" },
    { "3", "s30" },
    { "4", "w31" },
    { "5", "a32" },
    { "6", "y33" },
    { "7", "i34" },
    { "8", "n35" },
    { "9", "r36" },
    { "~", "i37" },
    { "`", "r38" },
    { "!", "d39" },
    { "@", "h40" },
    { "#", "e41" },
    { "$", "i42" },
    { "%", "s43" },
    { "^", "w44" },
    { "&", "a45" },
    { "*", "y46" },
    { "(", "i47" },
    { ")", "n48" },
    { "-", "r49" },
    { "_", "i50" },
    { "=", "r51" },
    { "+", "d52" },
    { "[", "h53" },
    { "{", "e54" },
    { "]", "i55" },
    { "}", "s56" },
    { @"\", "w57" },
    { "|", "a58" },
    { ";", "y59" },
    { ":", "i60" },
    { "'", "n61" },
    { "\"", "r62" },
    { ",", "i63" },
    { "<", "r64" },
    { ".", "d65" },
    { ">", "h66" },
    { "/", "e67" },
    { "?", "i68" },
    { " ", "s69" },
    { "\n", "w70" },
};
```

Decryption dictionary:

```csharp
var decryptionDictionary = new Dictionary<string, string>(StringComparer.CurrentCultureIgnoreCase)
{
    { "h01", "a" },
    { "e02", "b" },
    { "i03", "c" },
    { "s04", "d" },
    { "w05", "e" },
    { "a06", "f" },
    { "y07", "g" },
    { "i08", "h" },
    { "n09", "i" },
    { "r10", "j" },
    { "i11", "k" },
    { "r12", "l" },
    { "d13", "m" },
    { "h14", "n" },
    { "e15", "o" },
    { "i16", "p" },
    { "s17", "q" },
    { "w18", "r" },
    { "a19", "s" },
    { "y20", "t" },
    { "i21", "u" },
    { "n22", "v" },
    { "r23", "w" },
    { "i24", "x" },
    { "r25", "y" },
    { "d26", "z" },
    { "h27", "0" },
    { "e28", "1" },
    { "i29", "2" },
    { "s30", "3" },
    { "w31", "4" },
    { "a32", "5" },
    { "y33", "6" },
    { "i34", "7" },
    { "n35", "8" },
    { "r36", "9" },
    { "i37", "~" },
    { "r38", "`" },
    { "d39", "!" },
    { "h40", "@" },
    { "e41", "#" },
    { "i42", "$" },
    { "s43", "%" },
    { "w44", "^" },
    { "a45", "&" },
    { "y46", "*" },
    { "i47", "(" },
    { "n48", ")" },
    { "r49", "-" },
    { "i50", "_" },
    { "r51", "=" },
    { "d52", "+" },
    { "h53", "[" },
    { "e54", "{" },
    { "i55", "]" },
    { "s56", "}" },
    { "w57", @"\" },
    { "a58", "|" },
    { "y59", ";" },
    { "i60", ":" },
    { "n61", "'" },
    { "r62", "\"" },
    { "i63", "," },
    { "r64", "<" },
    { "d65", "." },
    { "h66", ">" },
    { "e67", "/" },
    { "i68", "?" },
    { "s69", " " },
    { "w70", "\n" },
};
```

Next, I will convert the input string into character array, reverse it, then declare a new string variable.

```csharp
char[] charArrayReverse = data1.ToCharArray(); //data1 is input string
Array.Reverse(charArrayReverse);
string data2 = new string(charArrayReverse);
```

Next, I will loop through each input character, replace it with my substitution dictionary, and creat new string using `StringBuilder`.

```csharp
StringBuilder sb = new StringBuilder();
for (int i = 0; i < data2.Length; i++)
{
    foreach (KeyValuePair<string, string> kvp in encryptionDictionary)
    {
        if (data2[i].ToString() == kvp.Key)
        {
            sb.Append(kvp.Value);
        }
    }
}
string data3 = sb.ToString();
```

Finally, I will prepend the hash code of user password, then append the output characters by separating it using `$` character, then append again with my signature characters which is `$HN`.

```csharp
outputData = hashCode.ToUpper() + "$" + data4.ToUpper() + "$HN";
```

Example of the encrypted string for "Hello":

```
D7640066$80I50W21R21R51E$HN
```

`D7640066` is a password hash, `80I50W21R21R51E` is the encrypted string and `$HN` is my signature characters.

#### Decrypting the message

When decrypt the encrypted message, I will check for my signature characters `$HN` in the string, if it's not present, I will consider it as INVALID data. Then, I will validate the entered password for decryption with the hash code - e.g. `D7640066`. Once the password is correct, then I will proceed with the decoding method.

### Exporting as .HNC file

Another feature that I added to this application is a functionality to export into a compressed file in `.HNC` file format using `GZipStream`. This will make the file content unreadable by human, hardly to recognize what kind of content inside that file.

So simple huh?!

### Here's why this cipher is not practical for the real world use

Substitution cipher is weak, easily broken and the cryptanalyst can deduce the probable meaning of the most common symbols by analyzing the frequency distribution of the ciphertext.

### Source code and download

If you like to see the full source code of this application, you can download it from my [GitHub](https://github.com/heiswayi/HeiswayiNrirdCipherProgram) repository. If you want to try the compiled version, you can download the binary [here](https://www.dropbox.com/s/p9lc58cgw059mka/HeiswayiNrirdCipherProgramV1.zip?dl=0).

Prerequisite: [.NET Framework 4.5.2](https://www.microsoft.com/en-us/download/details.aspx?id=42642)
