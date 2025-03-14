<h1 align="center">
  <img src="./assets/bb-1.png" alt="BBLogo" width="250" /></br></br>
  <strong style="font-size:60px;">Package Manager</strong>
</h1></br>

This doc outlines the steps I found easiest for installing a package manager. The top 2 package managers of choice are nvm and npm. I recommend nvm just due to ease of use. 

To install NVM run the following command:

`brew install nvm`

It will likely be required to source nvm in your shell profile, so do that by adding the following lines in your `~/.bashrc`, `~/.zshrc`, or `~/.bash_profile`:

```
export NVM_DIR=~/.nvm
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

Restart is required. 

To verify installation run the following command:

`nvm --version`

You can proceed to the javascript or typescript setup. 