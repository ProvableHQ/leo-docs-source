---
id: deploy_execute_demo
title: Aleo Deploy and Execute Demo
sidebar_label: Deploy and Execute Demo
---

These changes support the first iteration of deploying and executing programs on the Aleo network.

Bugs, usability suggestions, and feedback in general would be greatly appreciated.

## Overview

Learn how to deploy and execute a basic "hello world!" program on Aleo's network using Leo and snarkOS.

## Usage guide

### 1. Prerequisites

Make sure you have both Leo and snarkOS installed on your machine.

* To verify if you have Leo, open your terminal and type `leo`. If you don't see something like `zsh: command not found: leo`, you're good

* To verify if you have snarkOS, do the same thing you did with Leo but replace `leo` with `snarkos`

**Optional**: Install the [JSON Beautifier & Editor](https://chrome.google.com/webstore/detail/json-beautifier-editor/lpopeocbeepakdnipejhlpcmifheolpl) Chrome extension.

**Note**:

* You can find instructions to install Leo on your machine [here](https://github.com/ProvableHQ/leo) and snarkOS [here](https://github.com/ProvableHQ/snarkos)
* Make sure to pull the latest versions of `snarkos` and `leo` from GitHub to your local machine

### 2. Generate your test keys and wallet address

* In your favorite browser, navigate to [provable.tools](https://www.provable.tools/) and click the **Generate** button

* Save your **Address**, **View Key**, and **Private Key** in a safe place, you'll need them later

### 3a. Seeding your wallet with credits
<!-- markdown-link-check-disable -->

To seed your wallet, you'll need to request credits from Aleo's faucet at [faucet.aleo.org](https://faucet.aleo.org/) ⛲️.

<!-- markdown-link-check-enable -->

**Note**:

* It can take up to 5-minutes for the faucet to send your credits, to bide the time, concurrently move on to step 3b below.
* ⚠️ International requests are not supported by the faucet at the moment (a solution is coming soon). In the meantime, if you need credits and are testing internationally, reach out to the Aleo team on Discord or Twitter for support.

### 3b. Create a Leo application

We'll need something to deploy, so let's create a simple test Leo application.

Open your terminal and enter the following commands consecutively:

* Create a directory to store your Leo application - feel free to use a different name for this directory or location

```
cd $HOME/Desktop
mkdir demo_deploy_Leo_app && cd demo_deploy_Leo_app
```

* ⚠️ Assign $WALLETADDRESS to the wallet address you saved

```
WALLETADDRESS=""
```

* Generate a unique application name using part of your wallet address

```
APPNAME=helloworld_"${WALLETADDRESS:4:6}"
```

* Create a new test Leo application

```
leo new "${APPNAME}"
```

* Run your Leo application to make sure things are working

```
cd "${APPNAME}" && leo run && cd -
```

* Save the path of your application - this is important later

```
PATHTOAPP=$(realpath -q $APPNAME)
```

### 4. Confirm the Aleo faucet ⛲️ has sent your wallet credits and obtain your ciphertext record value

By this point, the Aleo faucet should have sent your wallet credits. Next, you'll need to verify your credit balance by decrypting the ciphertext record for the execute transfer that was sent to you.

If you requested credits by texting with your phone number, you should also receive a confirmation with a URL that has a prefix of `explorer.aleo.org/transaction...`

<!-- markdown-link-check-disable -->

Alternatively, you can find your execute transaction confirmation by going to [the faucet](https://faucet.aleo.org/) and searching the table provided (supported on desktop only currently) for your address. Once a result is returned, click on the `Transaction ID` field. If you do not see a result in the table, your credits have not yet been sent.

<!-- markdown-link-check-enable -->

* You should be presented with a JSON object in a new browser window. If you haven't already, we highly recommend you install the [JSON Beautifier & Editor](https://chrome.google.com/webstore/detail/json-beautifier-editor/lpopeocbeepakdnipejhlpcmifheolpl) Chrome extension.
* Navigate to `object.execution.transitions[0].outputs[0].value` and copy the ciphertext stored there

### 5. Obtain your records plaintext

* Navigate to [provable.tools](https://www.provable.tools/) and click the **Record** tab in the nav bar at the top of the page
* Place the record ciphertext you copied in the previous step in the `Record (Ciphertext)` field
* Place your view key in the `View Key` field
* Copy the plaintext record provided. If you do not see it, it's likely you copied the wrong ciphertext record in step 4. Consider revisiting or reach out to `hello@aleo.org`.
* Save your plaintext record in the same place as you did your address, view key, and private key. You will need it later.

### 6. Deploy your test application

Now that we have all the details required, we can deploy your first Leo application.

Open the same terminal instance as before and enter the following commands consecutively:

* Navigate to the path of your app

```
cd $PATHTOAPP && cd ..
```

* ⚠️ Assign $PRIVATEKEY to the private address you saved earlier

```
PRIVATEKEY=""
```

* ⚠️ Assign $RECORD to the plaintext record you saved earlier

```
RECORD=""
```

* Deploy your Leo application (if all your variables were assigned correctly, you should be able to copy/paste the following

```
snarkos developer deploy "${APPNAME}.aleo" --private-key "${PRIVATEKEY}" --query "https://api.explorer.aleo.org/v1" --path "./${APPNAME}/build/" --broadcast "https://api.explorer.aleo.org/v1/testnet/transaction/broadcast" --fee 1000000 --record "${RECORD}"
```

You should have seen a confirmation that your Aleo application was deployed in the form of a transaction ID that looks like the following `at1rkkpqu5k4rt86zzccczw6cxeyvrl7hxydvvv7dhl7zr7p9w40c8s70kwm8`. Make sure to copy this string as you'll need it for the last step.

### 7. Execute your test application
<!-- markdown-link-check-disable -->

Finally, it is time to execute the application you just deployed!

* You'll need to update the `--record` flag with the latest transaction linked to your wallet balance. In this case, you can obtain that by going to the following URL: https://api.explorer.aleo.org/v1/testnet/transaction/$DEPLOY_TX_ID but replace $DEPLOY_TX_ID with the transaction ID provided to you once your application was deployed (or from the most recent transaction linked to your wallet address). An example URL looks like so: https://api.explorer.aleo.org/v1/testnet/transaction/at1rkkpqu5k4rt86zzccczw6cxeyvrl7hxydvvv7dhl7zr7p9w40c8s70kwm8
* In the JSON object provided at https://api.explorer.provable.com/v1/testnet/transaction/$DEPLOY_TX_ID, navigate to: `object.fee.transition.outputs[0].value` and copy the record ciphertext value.
* Head to [provable.tools](https://www.provable.tools/) and navigate to the **Record** tab and paste the record ciphertext you just copied as well as your wallet's view key
* Similar to the steps we followed for a deploy transaction, update your `RECORD` variable with the record plaintext you just decrypted by doing the following:

<!-- markdown-link-check-enable -->

⚠️ Assign $RECORD to the plaintext record you saved earlier

```
RECORD=""
```

Then just paste the following command in your terminal

```
snarkos developer execute "${APPNAME}.aleo" "main" "1u32" "2u32" --private-key "${PRIVATEKEY}" --query "https://api.explorer.provable.com/v1" --broadcast "https://api.explorer.provable.com/v1/testnet/transaction/broadcast" --fee 1000000 --record "${RECORD}"
```

Awesome! You have successfully deployed and executed a Leo application to Testnet, how exciting 🎉
