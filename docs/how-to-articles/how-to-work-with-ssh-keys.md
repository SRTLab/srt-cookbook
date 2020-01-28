# How to Work with SSH Keys

SSH keys can be used to automate access to servers. They are commonly used in scripts, backup systems, configuration management tools, and by developers and sysadmins. They also provide single sign-on, allowing the user to move between his/her accounts without having to type a password every time. This works even across organizational boundaries, and is highly convenient.

For more details, please read the following [article](https://www.ssh.com/ssh).


## Generating SSH Key and Copying It to Remote Server

### 1. [Check for existing SSH key pair](https://help.github.com/en/github/authenticating-to-github/checking-for-existing-ssh-keys) or generate a new one on your local machine

Enter `ls -al ~/.ssh` to see if existing SSH keys are present:
```
ls -al ~/.ssh
# Lists the files in your .ssh directory, if they exist
```

If you don't have an existing public and private key pair, or don't wish to use any that are available to connect to remote server, then [generate a new SSH key](https://www.ssh.com/ssh/keygen):

```
ssh-keygen -f ~/.ssh/mykey -t rsa -b 4096
```

This generates a public/private rsa key pair. The algorithm is selected using the `-t` option and key size using the `-b` option. The filename is specified using the `-f <filename>` option.

Then you will be promted to enter a passphrase. The passphrase is used for encrypting the key, so that it cannot be used even if someone obtains the private key file. The passphrase should be cryptographically strong. [The online random password generator](https://www.ssh.com/iam/password/generator) is one possible tool for generating strong passphrases.

### 2. [Copy the public key to the remote server](https://www.ssh.com/ssh/keygen#copying-the-public-key-to-the-server)

To use public key authentication, the public key must be copied to a server and installed in an [authorized_keys](https://www.ssh.com/ssh/authorized_keys) file. This can be conveniently done using the [ssh-copy-id](https://www.ssh.com/ssh/copy-id) tool. Like this:
```
ssh-copy-id -i ~/.ssh/mykey user@host
```

Once the public key has been configured on the server, the server will allow any connecting user that has the private key to log in. During the login process, the client proves possession of the private key by digitally signing the key exchange.

The copying may ask for a password or other authentication for the server.

It's important to note that only the public key is copied to the server. The private one should never be copied to remote machine.

Now try logging into the server with `ssh user@host` and check to make sure that only the key you wanted was added:
```
cat ~/.ssh/authorized_keys
```

### 3. Test the new key

Try logging into the server with
```
ssh -i ~/.ssh/mykey user@host
```

The login should now complete without asking for a password. Note, however, that the command might ask for the passphrase you've specified for the key.


## Adding the Key to SSH Agent

`ssh-agent` is a program that can hold a user's private key, so that the private key passphrase only needs to be supplied once. A connection to the agent can also be forwarded when logging into a server, allowing [SSH commands](https://www.ssh.com/ssh/command) on the server to use the agent running on the user's desktop. For more information on using and configuring the SSH agent, see the [ssh-agent](https://www.ssh.com/ssh/agent) page.

The detailed instructions of running ssh-agent on Mac/Windows/Linux can be found on the following [GitHub Help page](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent).

### Mac

When adding your SSH key to the agent, use the default macOS `ssh-add` command, and not an application installed by `macports`, `homebrew`, or some other external source.

1. Start the ssh-agent in the background
```
$ eval "$(ssh-agent -s)"
> Agent pid 59566
```

2. If you're using macOS Sierra 10.12.2 or later, you will need to modify your `~/.ssh/config` file to automatically load keys into the ssh-agent and store passphrases in your keychain
```
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/mykey
```

3. Add your SSH private key to the ssh-agent and store your passphrase in the keychain
```
$ ssh-add -K ~/.ssh/mykey
```

**Note:** The `-K` option is Apple's standard version of `ssh-add`, which stores the passphrase in your keychain for you when you add an ssh key to the ssh-agent.

If you don't have Apple's standard version installed, you may receive an error. For more information on resolving this error, see ["Error: ssh-add: illegal option -- K."](https://help.github.com/en/github/authenticating-to-github/error-ssh-add-illegal-option----k)

### Windows

<!-- Check and fill in -->

Please visit the following [GitHub Help page](https://help.github.com/en/github/authenticating-to-github/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) for the detailed instructions of running ssh-agent on Windows.

### Linux

1. Start the ssh-agent in the background
```
$ eval "$(ssh-agent -s)"
> Agent pid 59566
```

2. Add your SSH private key to the ssh-agent
```
$ ssh-add ~/.ssh/mykey
```

## Automation

1. It is possible to turn off password authentication when SSH to a remote server using one of the following options
```
-o PasswordAuth=no
-o BatchMode=yes
```

It is recommended to use `-o BatchMode=yes` option to disable any kind of prompt. In this case the script will immediately die if ssh-agent has not been started manually before running the script.

2. In case of running the script within CI (TeamCity, etc.), it is recommended to use public SSH key without passphrase.