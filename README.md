# signed-commits-testing

For testing signed and verified commits using gpg.

# Second commit will be unverified

Just for testing

# Installations

1. Install gpg for my Macbook

```
brew install gnupg
```

2. Generate a GPG Key for my work email (remember to set a passphrase)

```
gpg --full-generate-key
```

3. Command to see the list of gpg generated keys

```
gpg --list-secret-keys --keyid-format=long
```

4. Copy the key to your clipboard with this command (display the long key)

```
gpg --armor --export <your_key_id>
```

5. Go to GitHub > Settings > SSH and GPG keys, select New GPG key, and paste the previous key copied.

6. Make sure to configure your global git account with the new gpg key

```
git config --global user.signingkey <gpg-ID>
git config --global commit.gpgSign true
git config --global tag.gpgSign true
```

7. To check global GIT config list run this command

```
git config --global --list
```

# Steps to handle SSH on work projects

1. Configure SSH for these all work projects (Generate a new SSH key)

```
ssh-keygen -t ed25519 -C "oniercdm@gmail.com"
```

2. Add the SSH key to the SSH agent

```
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

3. Add the SSH key to your GitHub work account: Copy your new SSH key to your clipboard:

```
pbcopy < ~/.ssh/id_ed25519.pub
```

Then go to your github account and paste the SSH key

4. Update your work repository to use SSH:

To use SSH :

```
git remote set-url origin git@github.com:onier-cdm/repository-name.git
```

To use HTTPS:

```
git remote add origin <https-link-url-from-repository>
```

# Possible error during make a commit

`Error: onier@Oniers-MacBook-Pro signed-commits-testing % git commit -m "[update] gpg added and readme updated"
error: gpg failed to sign the data
fatal: failed to write commit object`

Solution: Testing new commit after paste the gpg on github again.
