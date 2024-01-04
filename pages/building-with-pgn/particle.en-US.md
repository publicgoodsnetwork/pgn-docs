
# Particle Network Smart Wallet-as-a-Service


[**Particle Network**](https://particle.network) is the Intent-Centric, Modular Access Layer of Web3. Using MPC-TSS for key management, Particle facilitates social logins yielding embedded EOA/AA wallets.
  
Particle Network's Smart Wallet-as-a-Service supports both PGN Mainnet and PGN Sepolia through either a standard EOA, or a smart account (supported by natively deployed account abstraction infrastructure on PGN), both resulting from a social login.

## Integrating Particle's Smart Wallet-as-a-Service

To integrate Particle's Smart Wallet-as-a-Service within your own application and therefore enable social logins (leading to a non-custodial smart account), you'll need to begin by installing a number of libraries, as is shown below.

```shell
yarn add @particle-network/auth-core-modal @particle-network/aa @particle-network/chains

# OR

npm install @particle-network/auth-core-modal @particle-network/aa @particle-network/chains
```

With these three libraries installed, `@particle-network/auth-core-modal`, `@particle-network/aa`, and `@particle-network/chains`, you're ready to begin integrating Particle Network within your application; in this case, that'll be a standard React application. You can get started with the basics through the following steps.

1. Configure Particle Auth Core (`@particle-network/auth-core-modal`) within `index.ts`
You'll need to start by heading into your `index.ts` (or `.js` depending on your project setup) and configuring Particle Auth Core through wrapping your primary `App` component (or it's equivalent) in `AuthCoreContextProvider`. Within `AuthCoreContextProvider`, you'll need to have a `projectId`, `clientKey`, and `appId` ready; all three of these values can be retrieved by setting up an application on the [Particle dashboard](https://dashboard.particle.network).

```js
import React from 'react'
import ReactDOM from 'react-dom/client'
import { PGN } from '@particle-network/chains';
import { AuthCoreContextProvider, PromptSettingType } from '@particle-network/auth-core-modal';
import App from './App'

import('buffer').then(({ Buffer }) => {
  window.Buffer = Buffer;
});

ReactDOM.createRoot(document.getElementById('root') as HTMLElement).render(
  <React.StrictMode>
    <AuthCoreContextProvider
      options={{
        projectId: process.env.REACT_APP_PROJECT_ID,
        clientKey: process.env.REACT_APP_CLIENT_KEY,
        appId: process.env.REACT_APP_APP_ID,
        themeType: 'dark', // Optional
        fiatCoin: 'USD', // Optional
        language: 'en', // Optional
        erc4337: {
          name: 'SIMPLE',
          version: '1.0.0',
        },
        promptSettingConfig: { // Optional, determines the security settings that a user has to configure
          promptPaymentPasswordSettingWhenSign: PromptSettingType.first,
          promptMasterPasswordSettingWhenLogin: PromptSettingType.first,
        },
        wallet: { // Optional, streamlines the wallet modal popup
          visible: true,
          customStyle: {
            supportChains: [PGN],
          }
        },
      }}
    >
    <App />
      </AuthCoreContextProvider>
  </React.StrictMode>
)
```

2. Setup a smart account within your primary `App` component
Now that you've configured Particle Auth Core, if you'd like to use a smart account (derived from the social login you'll initiate in a moment), it first needs to be configured within your primary application component (`App` by default), such as is shown below.

```js
import { useEthereum } from '@particle-network/auth-core-modal';
import { PGN } from '@particle-network/chains';
import { SmartAccount, AAWrapProvider } from '@particle-network/aa';
import { ethers } from 'ethers';

const { provider } = useEthereum();

const smartAccount = new SmartAccount(provider, {
	projectId: process.env.REACT_APP_PROJECT_ID,
    clientKey: process.env.REACT_APP_CLIENT_KEY,
    appId: process.env.REACT_APP_APP_ID,
    aaOptions: {
      simple: [{ chainId: PGN.id, version: '1.0.0' }] // PGN is only supported on 'simple', although 'biconomy' and 'cyberConnect' are also options
	}
});

// Optionally, you can setup a custom Ethers provider
const customProvider = new ethers.providers.Web3Provider(new AAWrapProvider(smartAccount), "any");
```

3. Initiate social login
With a smart account setup, you're ready to have a user login to your application through their social account, and thus have a smart account assigned. To do this, you'll need to use the `connect` function on the `useConnect` hook from `@particle-network/auth-core-modal`.

```js
import { useConnect } from '@particle-network/auth-core-modal';
import { PGN } from '@particle-network/chains';

const userInfo = await connect({
    socialType: authType,
    chain: PGN,
});
```

From here, you can facilitate interaction with the wallet natively through the `smartAccount` object, through functions on the `useEthereum` hook, or through a custom EIP-1193 ethers/Web3.js object derived from `smartAccount`.
  

## Learn More

- [Particle Dashboard](https://dashboard.particle.network/)
- [Particle Documentation](https://docs.particle.network/)
- [PGN Example Repository](https://github.com/TABASCOatw/particle-pgn-demo)
