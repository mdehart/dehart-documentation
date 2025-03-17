<h1 align="center">
  <img src="./assets/bb-3.png" alt="BBLogo" width="250" /></br></br>
  <strong style="font-size:60px;">Typescript Installation</strong>
</h1></br>

This doc will outline the steps to install and configure node and typescript. Node is a prerequisite to installing .ts. 

# Node
To install node run the following command:
`brew install node`

To confirm installation run the following command:
`node -v`

You can also install a specific version of node by running the following:
`nvm install <<version>>`
`nvm use <<version>>`

Once verified, you should be able to proceed with the typescript installation. 

# .ts
If you are using npm or another package manager simply replace nvm with with your package manager. Installation starts by entering:
`nvm install --global typescript` to install it globally. 

or 

`nvm install --global typescript@4.x.x` to install a specific version. 

Verify installation by running:
`tsc -v`

# .ts Configuration
To work on or create a new project in ts, run the following:
`npm init` 
inside your directory. This will create the `package-lock.json` lockfile used for tracking the exact versions of every package and its dependencies installed. 

Run 
`nvm install typescript` 
to install TypeScript in that folder only. This allows you to use different version of ts over different projects.