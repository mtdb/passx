# PassX

PassX is an extension for [pass](https://www.passwordstore.org/) the standard unix password manager.

## Features

  - Support multiple fields per password file
  - Copy option for fields (not only for the master key)
  - Support for file attachments
  - Support for TOTP

### Ussage

    Passx extension usage:

        pass append pass-name key-name [value|file-path]
            Add a new key/value pair in the passx document
        pass [show] [-c] pass-name key-name
            Show existing key and optionally put it on the clipboard.
            If the key value is a file then it opens following the mailcap rules
            http://linux.die.net/man/4/mailcap
        pass edit pass-name key-name
            Edit an existing key.

#### Example output

```
i7@i7-desktop ~ » pass init D3000000
mkdir: created directory ‘/home/i7/.password-store/’
Password store initialized for D3000000
i7@i7-desktop ~ » pass insert Bank/One
mkdir: created directory ‘/home/i7/.password-store/Bank’
Enter password for Bank/One: 
Retype password for Bank/One: 
i7@i7-desktop ~ » pass
Password Store
└── Bank
    └── One
i7@i7-desktop ~ » pass show Bank/One 
123456
i7@i7-desktop ~ » pass append Bank/One number
write the vaue for `number`: 123456789
i7@i7-desktop ~ » pass append Bank/One cvv 123
i7@i7-desktop ~ » pass show Bank/One 
123456
⊥
cvv: 123
number: 123456789
⊤
i7@i7-desktop ~ » pass show Bank/One number
123456789
i7@i7-desktop ~ » pass -c Bank/One number
i7@i7-desktop ~ » pass append Bank/One avatar /home/i7/avatar.png
This looks like a system path, you want try to attach the file?  [Y/n]
You want to delete the source file?  [y/N]
i7@i7-desktop ~ » pass
Password Store
├── Bank
│   └── One
└── Files
    └── 6SDDITBP
i7@i7-desktop ~ » pass show Bank/One
123456
⊥
avatar: Files/6SDDITBP
cvv: 123
number: 123456789
⊤
i7@i7-desktop ~ » pass show Bank/One avatar
```

When you try to show a file this makes following the mailcap rules.

The idea is to have an valid yaml document within a `⊥` and `⊤` delimiters, is not relevant the content out of them, then you can edit the document with your editor following the yaml rules inside the delimiters or write without rules outside them.

### TOTP support

In order to read time based OTPs you need install **pyotp** (`sudo pip instal pyotp`) and the script will try to read the base32 secret from any field named **otp** in your passx document

#### Example

```
i7@i7-desktop ~ » pass append Bank/One otp
write the vaue for `otp`: base32secret3232
i7@i7-desktop ~ » pass show Bank/One otp
456123
i7@i7-desktop ~ » pass show Bank/One
123456
⊥
cvv: 123
number: 123456789
otp: base32secret3232
⊤
i7@i7-desktop ~ » pass show Bank/One otp
789456
```

### Installation

You need to have installed `pass` (`sudo apt-get install pass` in **Ubuntu/Debian**)

```
mkdir -p ~/opt/passx
wget https://raw.githubusercontent.com/individuo7/passx/master/pass -P ~/opt/passx
chmod +x ~/opt/passx/pass
```

After that you need add it in your `~/.profile`

```export PATH="$HOME/opt/passx:$PATH"```

Then do `source ~/.profile` and try `pass help` to check the changes.
