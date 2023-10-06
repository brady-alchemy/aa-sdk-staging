---
outline: deep
head:
  - - meta
    - property: og:title
      content: ISmartAccountProvider • withGasEstimator
  - - meta
    - name: description
      content: Overview of the withGasEstimator method on ISmartAccountProvider
  - - meta
    - property: og:description
      content: Overview of the withGasEstimator method on ISmartAccountProvider
---

# withGasEstimator

Override the `gasEstimator` middleware which is used for setting the `gasLimit` fields on the `UserOperation` prior to execution.

## Usage

::: code-group

```ts [example.ts]
import { provider } from "./provider";

// [!code focus:99]
provider.withGasEstimator(async () => ({
  callGasLimit: 0n,
  preVerificationGas: 0n,
  verificationGasLimit: 0n,
}));
```

<<< @/snippets/provider.ts
:::

## Returns

### `SmartAccountProvider`

The provider with the gasEstimator middleware override.

## Parameters

### `override: GasEstimatorMiddleware`

A function for overriding the default gas estimator middleware
