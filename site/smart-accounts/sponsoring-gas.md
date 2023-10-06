---
outline: deep
head:
  - - meta
    - property: og:title
      content: Sponsoring Gas
  - - meta
    - name: description
      content: Guide to Sponsor Gas of an account
  - - meta
    - property: og:description
      content: Guide to Sponsor Gas of an account
prev:
  text: Choosing a Signer
  link: /smart-accounts/signers/overview
---

# Sponsoring Gas

Gas fees are a significant barrier to entry for a new user in your dapp. With Account Kit you can remove this barrier by sponsoring gas fees for transactions via the [Gas Manager](https://docs.alchemy.com/docs/gas-manager-services). In this guide you will learn how to create a gas policy, configure spending limits, and send sponsored user operations from a smart account.

## How to Sponsor Gas

After [installing `aa-sdk`](/getting-started#install-the-packages) in your project, follow these steps to sponsor gas.

### 1. Set Up the Provider

First, create an `AlchemyProvider`. You'll use this to send user operations and interact with the blockchain.

<<< @/snippets/provider.ts

Remember to replace `ALCHEMY_API_KEY` with your Alchemy API key. If you don't have one yet, you can create an API key on the [Alchemy dashboard](https://dashboard.alchemy.com/).

### 2. Create a Gas Manager Policy

A gas manager policy is a set of rules to define which user operations are eligible for gas sponsorship. You can configure the following
* Spending rules:
* Allowlist:
* Blocklist:
* Policy duration:

To learn more refer to the guide on [setting up a gas manager policy](https://docs.alchemy.com/docs/setup-a-gas-manager-policy).

Once you've decided on policy rules for your app, [create a policy](https://dashboard.alchemy.com/gas-manager/policy/create) in the Gas Manager dashboard. 

### 3. Link the Policy to your Provider

Next, you need to link the gas policy to your provider. You will see your Policy ID at the top of the dashboard. Copy it and then replace the `GAS_MANAGER_POLICY_ID` in the snippet below. 

::: code-group

```ts [sponsor-gas.ts]
import { provider } from "./provider.ts";

// Find your Gas Manager policy id at: // [!code focus:10]
//dashboard.alchemy.com/gas-manager/policy/create
const GAS_MANAGER_POLICY_ID = "YourGasManagerPolicyId";

// Link the provider with the Gas Manager. This ensures user operations
// sent with this provider get sponsorship from the Gas Manager.
provider.withAlchemyGasManager({
  policyId: GAS_MANAGER_POLICY_ID,
  entryPoint: entryPointAddress,
});

// Here's how to send a sponsored user operation from your smart account:
const { hash } = await provider.sendUserOperation({
  target: "0xTargetAddress",
  data: "0xCallData",
  value: 0n, // value in bigint or leave undefined
});
```

<<< @/snippets/provider.ts

:::

You've created a gas manager policy and linked it to the provider. This guarantees that user operations sent with this provider receive sponsorship if and only the user operation satisfies the rules defined in your gas policy.

### 4. Send the Sponsored UserOperation

Now you're ready to send sponsored user operations! You can send a user operation by calling `sendUserOperation` on the provider. The Gas Manager will check if this user operation satisfies the policy rules defined above and sponsor the gas costs if the rules are met. If the user operation does not meet the policy rules, [TODO]

::: code-group

```ts [sponsor-gas.ts]
import { provider } from "./provider.ts";

// Your Gas Manager policy id is available at: //
//dashboard.alchemy.com/gas-manager/policy/create
const GAS_MANAGER_POLICY_ID = "YourGasManagerPolicyId";

// Link the provider with the Gas Manager so the user operations
// sent with this provider get sponsorship from the Gas Manager.
provider.withAlchemyGasManager({
  policyId: GAS_MANAGER_POLICY_ID,
  entryPoint: entryPointAddress,
});

// Send a sponsored user operation from your smart account like this: // [!code focus:6]
const { hash } = await provider.sendUserOperation({
  target: "0xTargetAddress",
  data: "0xCallData",
  value: 0n, // value in bigint or leave undefined
});
```

<<< @/snippets/provider.ts

:::

Congratulations! You've successfully sponsored gas for a user operation by creating a Gas Manager Policy, defining policy rules, linking your policy to the provider, and submitting a user operation.
