---
sidebar_position: 12
---

# Inter-canister calls

## Overview

One of the most important features of ICP for developers is the ability to call functions in one canister from another canister. This capability to make calls between canisters, also sometimes referred to as **inter-canister calls**, enables you to reuse and share functionality in multiple dapps.

For example, you might want to create a dapp for professional networking, organizing community events, or hosting fundraising activities. Each of these dapps might have a social component that enables users to identify social relationships based on some criteria or shared interest, such as friends and family or current and former colleagues.

To address this social component, you might want to create a single canister for storing user relationships then write your professional networking, community organizing, or fundraising application to import and call functions that are defined in the canister for social connections. You could then build additional applications to use the social connections canister or extend the features provided by the social connections canister to make it useful to an even broader community of other developers.

This example will showcase a simple way to configure inter-canister calls that can be used as the foundation for more elaborate projects and use-cases such as those mentioned above.

## Example

Consider the following code for `canister1`:

```motoko no-repl
import Canister2 "canister:canister2";

actor {
    public func main() : async Nat {
        return await Canister2.getValue();
    };
};
```

Then, consider the following code for `canister2`:

```motoko
import Debug "mo:base/Debug";

actor {
    public func getValue() : async Nat {
        Debug.print("Hello from canister 2!");
        return 10;
    };
};
```


To make an inter-canister call from `canister1` to `canister2`, you can use the `dfx` command:

```
dfx canister call canister1 main
```

The output should resemble the following:

```
2023-06-15 15:53:39.567801 UTC: [Canister ajuq4-ruaaa-aaaaa-qaaga-cai] Hello from canister 2!
(10 : nat)
```

Alternatively, you can also use a canister id to access a previously deployed canister by using the following piece of code in `canister1`:

```motoko
actor {
    public func main(canisterId: Text) : async Nat {
        let canister2 = actor(canisterId): actor { getValue: () -> async Nat };
        return await canister2.getValue();
    };
};
```

Then, use the following call, replacing `canisterID` with the principal ID of a previously deployed canister:

```
dfx canister call canister1 main "canisterID"
```

<img src="https://github.com/user-attachments/assets/844ca364-4d71-42b3-aaec-4a6c3509ee2e" alt="Logo" width="150" height="150" />