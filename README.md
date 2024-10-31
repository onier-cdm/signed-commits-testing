# signed-commits-testing

My own documentation for testing and configured signed and verified commits using gpg on macOS.

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

# Important detail to make commits signed works on github

As I have in my global config my personal email onier0217@gmail.com I need to set in each future project the git config user.email to oniercdm@gmail.com to make github accept the verified commit so this verify the email from the GPG Key match with the git account as well.
**Conclusion** Setting the user.email to your work email ensures that the email matches the GPG key associated with that email.

1. Set the email to that specific work project directory

```
git config user.email oniercdm@gmail.com
```

2. To check is the commit was signed correctly

```
git log --show-signature -1
```

3. To check git configuration list of that specific project (not global)

```
git config --list
```

# Possible error during make a commit

`Error: onier@Oniers-MacBook-Pro signed-commits-testing % git commit -m "[update] gpg added and readme updated"
error: gpg failed to sign the data
fatal: failed to write commit object`

**Solution**

1. Ensure You Have GPG Installed
   First, let's confirm that you have the correct version of GPG installed and ensure it’s properly linked:
   Upgrade GPG (if you haven't already):

```
brew update
```

```
brew upgrade gnupg
```

2. Link GPG:

```
brew link --overwrite gnupg
```

3. Install Pinentry
   Make sure you have the pinentry-mac program, which helps with entering your GPG passphrase:

```
brew install pinentry-mac
```

4. Configure GPG to Use Pinentry
   Next, configure GPG to use the pinentry-mac program:
   Open or create the gpg-agent.conf file:

```
nano ~/.gnupg/gpg-agent.conf
```

Add the following line (if it’s not already there):
`pinentry-program /opt/homebrew/bin/pinentry-mac`

5. Kill the GPG Agent
   After updating the configuration, kill the GPG agent to apply changes:

```
killall gpg-agent
```

6. Set the GPG Program in Git Configuration
   Ensure that Git knows where to find GPG:

```
git config --global gpg.program gpg
```

7. Test GPG Signing
   Now, test GPG signing again to see if it works:

```
echo "test" | gpg --clearsign
```

If this command is successful and does not show any errors the prompt to set the passphrase will appear and proceed to the next step.

8. Try Committing Again (remember to use Conventional Commits extension)
