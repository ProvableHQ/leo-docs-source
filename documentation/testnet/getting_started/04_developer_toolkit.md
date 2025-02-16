---
id: developer_toolkit
title: Leo Developer Toolkit
---
### Requirements

This toolkit works best with a MacOS machine from 2021 or newer. You can check your MacOS version [here](https://support.apple.com/en-us/HT201260).

## Leo
### Download and install Leo
- You can find the latest version of Leo [here](https://github.com/ProvableHQ/leo/releases/latest).
- After downloading the version that works for your machine, double-click the zip file to open it.
- Then install Leo by moving it to `/usr/local/bin` on your machine, which you can do by dragging it to that location or via terminal (`sudo mv leo /usr/local/bin`); in both cases, you will need to enter your computer password.

### The `leo help` command

`leo --help`

Open a new terminal window and type `leo --help` to confirm that Leo is installed. You should see the following output:

   ```
    CLI Arguments entry point - includes global parameters and subcommands

    Usage: leo [OPTIONS] [API] <COMMAND>

    Commands:
    account  Create a new Aleo account
    new      Create a new Leo package in a new directory
    example  Create a new Leo example package in a new directory
    build    Compile the current package as a program
    clean    Clean the output directory
    run      Run a program with input variables
    execute  Execute a program with input variables
    update   Update the Leo CLI
    help     Print this message or the help of the given subcommand(s)

    Arguments:
    [API]  Custom Aleo PM backend URL [env: APM_URL=]

    Options:
    -d                 Print additional information for debugging
    -q                 Suppress CLI output
    --path <PATH>  Optional path to Leo program root folder
    -h, --help         Print help
    -V, --version      Print version
   ```

###### 💡Note: If you do not see the output above, you can build Leo from source by following the guide [here](https://github.com/ProvableHQ/leo#%EF%B8%8F%EF%B8%8F-build-guide) or download the [latest `.zip`](https://github.com/ProvableHQ/leo/releases/latest) file directly and then move the Leo binary to any location; we recommend `/usr/local/bin`.

### The `leo account` command

`leo account new`

- Continue in your existing terminal window and type `leo account new`, you should see something similar to the following output:

   ```
  Private Key  APrivateKey1zkpAeeSTY6CuV2ahuJsHLqBFEChK3NnKxLqDyFoMQVmi1Ru
  View Key  AViewKey1cuV5NU4WgkHezc6fFFduECwrpGg8knqyb78YFJVmhhMT
  Address  aleo14hmf2rfh5nne6m7wsqlklqzcagh2tjydecvjycgj80snnns9svqsyjzsll
  ```

- Once you have generated your private key, view key, and wallet address, save them in a safe place such as a text editor, IDE, or password manager.

### The `leo example` command

`leo example $NAME`

- Continue in your existing terminal window and type `leo example`. You should see the following output:

  ```
  Create a new Leo example package in a new directory

  Usage: leo example [OPTIONS] <COMMAND>

  Commands:
    lottery    A public lottery program
    tictactoe  A standard tic-tac-toe game program
    token      A transparent & shielded custom token program
    help       Print this message or the help of the given subcommand(s)

  Options:
    -d                 Print additional information for debugging
    -q                 Suppress CLI output
        --path <PATH>  Optional path to Leo program root folder
    -h, --help         Print help
  ```

- Choose one of the examples, and create it using the `leo example` command like so: `leo example tictactoe`.

### The `leo run` command

`leo run $FUNCTION`

- Navigate to the root of your Leo project's folder using the `cd` command, i.e., `cd tictactoe`.
- To run your new Leo example, refer to the included `README.md` file, which you can find in the root of your newly created project's folder.
    - An example `leo run` command for the `tictactoe` application is `leo run new` which creates a new game board.

### Push your Leo app to GitHub
This section refers to the guide created by GitHub [here](https://docs.github.com/en/migrations/importing-source-code/using-the-command-line-to-import-source-code/adding-locally-hosted-code-to-github). You can find additional information there as well as alternatives means to add your Leo project to GitHub such as the [GitHub CLI](https://cli.github.com/), [GitHub Desktop](https://desktop.github.com/), or uploading your project folder to an existing repository directly.

#### Initializing a Git repository

1. Continue in your existing terminal window; confirm you are in your Leo project's root folder with `pwd`, if not, type:

   `cd tictactoe`

2. Initialize your new Leo project example as a Git repository. By default, the initial branch is called main.

   `git init -b main`

4. Add the files in your new local repository. This stages them for the first commit. You can add them all at once with `.` or individually by typing each file name, i.e., `git add README.md build inputs program.json src`

   `git add .`

5. Commit the files that you've staged in your local repository.

   `git commit -m "First commit"`

###### 💡Note: At any point, you can check the status of your Git commands with `git status`. You can learn more about `git status` [here](https://github.com/git-guides/git-status).

#### Adding a local repository to GitHub using Git

1. Create a new repository on [GitHub.com](https://github.com/new). To avoid errors, do not initialize the new repository with README, license, or gitignore files. You can add these files after your project has been pushed to GitHub. For more information, see ["Creating a new repository."](https://docs.github.com/en/repositories/creating-and-managing-repositories/creating-a-new-repository)
2. At the top of your repository on GitHub.com's Quick Setup page, click  to copy the remote repository URL
   ![ ](https://docs.github.com/assets/cb-48149/mw-1440/images/help/repository/copy-remote-repository-url-quick-setup.webp)
3. Continue in your existing terminal window and type

   ```
   git remote add origin <REMOTE_URL>
   git remote -v
   ```
4. Push the changes in your local repository to GitHub.com.

   `git push -u origin main`

###### 💡Note: You can check which remote you are using with `git remote -v`. You can learn more about `git remote` [here](https://github.com/git-guides/git-remote#what-does-git-remote-do).

## Continue your Leo journey

Check out the following resources:

- [Our developer docs](https://developer.aleo.org/getting_started/)
- See the SDK in action at [provable.tools](https://www.provable.tools/)
- Play around with Leo in the browser with [Leo Playground](https://play.leo-lang.org/)
- Learn Leo syntax, functions, and best practices with the [Leo's language guide](https://developer.aleo.org/leo/language)
- Deploy and Execute Leo applications on-chain with our [Deploy and Execute Demo](https://developer.aleo.org/testnet/getting_started/deploy_execute_demo)
<!-- markdown-link-check-disable -->
- See Aleo's testnet live and other Leo developer's applications via an explorer such as [Haruka's Program Registry](https://explorer.hamp.app/programs) or [aleo.network](https://www.aleo.network/).
<!-- markdown-link-check-enable -->

## FAQ

### What the "New Developer Toolkit" *is*
- Getting started with Aleo's tools is easier than ever. This guide will explain how to install Leo, Aleo's programming language for writing private applications, create an example Leo app (or your own), and some GitHub basics so your work is accessible from anywhere.
- This "New Developer Toolkit" is a simple set of instructions you can copy-paste into your terminal, line-by-line, without any required dependencies other than the latest MacOS. After completing this guide, you'll have a working Leo application, be able to run, compile, and generate Leo examples, and have an example of Leo code on your GitHub profile that you can share with friends, family, or future employers.

### What the "New Developer Toolkit" *is not*
<!-- markdown-link-check-disable -->
The "The New Developer Toolkit" is not exhaustive in that it does not go over the basics of Leo syntax, folder structure, or any aspects of zero-knowledge. For that, check out the "Continue your Leo journey" section above.
<!-- markdown-link-check-enable -->
