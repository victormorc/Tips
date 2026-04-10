---
project: "-"
brief: Notes related to Linux commands
author: Victor Moreno Calvo
date: 01-01-2016
version: 1.0
---

# BASIC LINUX COMMANDS:

## diff
- Compare two plain text files:
    $ diff <old_file> <new_file>

## echo
- Echo "string" without adding new line (\n) to the end.
    $ echo -n "string"

- Copy to a file hexadecimal chain "\xXX\xXX..\xXX"
    $ echo -e -n "\xXX\xXX..\xXX" > <file.hex>          # Introduce in file.hex a hexadecimal 

## file
- Give basic information of a <file_name>:
    $ file <file_name>
    
## find
- Find a file:
    $ find / -name file.txt   (Start locating since root directory. Locate file by name).
    
- Find recursively all the files with some extension:
    $ find <path_to_search> -type f -name ".extension"
    
- Modify in several files a string using commands 'find' and 'sed':
    $ find . -type f -exec sed -i 's/old_string/new_string/gI' {} \;
    
- Remove all files with some extension recursively in the current folder:
    $ find . -type f -name ".extension" -exec rm {} \;

- Rename the extension of several files:
    $ find . -name ".extension" -exec sh -c 'mv "$0" "${0%.extension}.newExtension"' {};


## grep
- Seek all the files which contains <string>:
    $ grep -rsl "<string>"            # Seek recursively in all files <string> and return those which contains it.

- Seek for <string> and show context N lines before:
    $ grep -B N "<string>"

- Seek for <string> and show context N lines after:
    $ grep -A  N "<string>"

- Seek for <string> and show context N lines before and after:
    $ grep -C N "<string>"

## hexdump

## nm

## readelf

## sed
- Add string every two characters:
    $ echo "original_string" | sed 's/../<string_to_add>&/g'     # This chain commands add a new string every two characters in the original_string

- Remove string:
    $ sed 's/<string_to_remove>//g'     # Remove in each line all the occurrences of the string

- Remove line which contains a string:
    $ sed '/<string_to_remove>/d'       # Remove line which contains the specified string

- Apply substitution only to those lines which starts with some regex:
    $ sed '/^<regex>/s/<old_string>/<new_string>/'

## objcopy

## objdump

## od
- Print binary of file:
    $ od <binary_file>

- Print hexadecimal of file:
    $ od -x <binary_file>

## openssl
- Show asn1 der format of file:
    $ openssl asn1parse -inform DER -in <file.der>
    $ openssl asn1parse -in <file.pem>

- Show content of an x509 certificate <cert.der>/<cert.pem>:
    $ openssl x509 -inform DER -in <cert.der> -text -noout
    $ openssl x509 -in <cert.pem> -text -noout

- Convert x509 certificate in pem format <cert.pem> to der format <cert.der>:
    $ openssl x509 -outform der -in <cert.pem> -out <cert.der>

- Verify <x509_to_verify.pem> against its issuer <issuer_x509.pem>:
    $ openssl verify -CAfile <issuer_x509.pem> <x509_to_verify.pem>     # Operation is done upon pem files.

- Extract public key from the cryptographic pair <private_key.key>:
    $ openssl pkey -in <private_key.key> -pubout > <public_key.pem>     # Operation is done upon pem files.

- Sign file using a key:
    $ openssl dgst -sha256 -sign <private_key.key> -out <file_signed> <file_to_sign>       # Data are digest first using sha256

## patch
- Apply patch created by diff:
    $ patch <original_file> < <patch_file>
    $ patch < <patch_file>.patch

## tar
- Compress a folder into an archive:
    $ tar -czvf <name_archive.tar.gz> <path_folder>
      # c: Create an archive.
      # z: Compress the archive with gzip.
      # v: Verbose
      # f: Allows to specify the filename of the archive.

    $ tar -cJvf <name_archive.tar.xz> <path_folder>
      # c: Create an archive.
      # J: Compress the archive with xz.
      # v: Verbose
      # f: Allows to specify the filename of the archive.

    $ tar -cjvf <name_archive.tar.bz2> <path_folder>
      # j: Compress the archive with bzip2.

- Uncompress (Instead of option -c use option -x):
    $ tar -xzvf <name_archive.tar.gz> <path_folder>


## tr
- Replace <old_character> with <new_character>:
    $ tr <old_character> <new_character>    # Data is gotten from standard input (command line)
    
- Remove <character>:
    $ tr -d <character>                     # Data is gotten from standard input (command line)

## truncate
- Remove latest bytes in file:
    $ truncate -s -N <file>         # N is the number of bytes to eliminate from file

## watch
- Execute <program> periodically, showing output:
    $ watch <program>               # Default is executing <program> every 2 seconds.

## xargs
- Change in several files a <old_string> for a <new_string>:
    $ grep -rsl "<old_string>" | xargs -I {} sed -i 's/<old_string>/<new_string>/g' {};

- Show the execution of the command perform by xargs.
    $ xargs -t <command>        # -t option is for trace.

- Input separated by 'null' character.
    $ xargs -0 <command>        # Input is treated as separated by 'null' character.

## xxd
- Print hexadecimal format of file:
    $ xxd <file>
    
- Print hexadecimal file all together:
    $ xxd -p <file>

- Copy hexadecimal string to a file:
    $ echo "string_hex" | xxd -r -p > <hex_file>        # -r (reverse) -> conversion from hex to binary, -p (plain) -> no new line, ...