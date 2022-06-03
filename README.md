# ICRC-1 Token Standard

The ICRC-1 is a standard for Fungible Tokens on the [Internet Computer](https://internetcomputer.org).

## Data

### account

A `principal` can have multiple accounts. Each account of a `principal` is identified by a 32-byte string called `subaccount`. Therefore an account corresponds to a pair `(principal, subaccount)`.

The account identified by the subaccount with all bytes set to 0 is the _default account_ of the `principal`.

## Methods

### name

Returns the name of the token, e.g. `MyToken`.

```
name: () -> (text) query;
```

### symbol

Returns the symbol of the token, e.g. `ICP`.

```
symbol: () -> (text) query;
```

### decimals

Returns the number of decimals the token uses, e.g. `8`, means to divide the token amount by `100000000` to get its user representation.

```
decimals: () -> (nat32) query;
```

### totalSupply

Returns the total token supply.

```
totalSupply: () -> (nat32) query;
```

### balanceOf

Returns the balance of the account given as argument.

```
balanceOf: (record { Principal; SubAccount; }) -> (nat64) query;
```

### transfer

Transfers `amount` of tokens from the account `(caller, from_subaccount)` to the account `(to_principal, to_subaccount)`.

```
type TransferArgs = record {
    from_subaccount: opt SubAccount;
    to_principal: Principal;
    to_subaccount: opt SubAccount;
    amount: nat64;
};

transfer: (TransferArgs) -> (variant { Ok: nat64; Err: TransferError; });
```

The result is either the block index of the transfer or an error.

### approve

Allows `spender` to withdraw from the account `(caller, from_subaccount)` multiple times, up to the `amount`. If this function is called again it overwrites the current allowance with `amount`.

```
type ApproveArgs = record {
    from_subaccount: opt SubAccount;
    spender: principal;
    amount: nat64;
};

approve: (ApproveArgs) -> (variant { Ok: nat64; Err: ApproveError; });
```

The result is either the block index of the approve or an error.

### transferFrom

Transfers `amount` of tokens from the account `(from_principal, from_subaccount)` to the account `(to_principal, to_subaccount)`.

```
type TransferFromArgs = record {
    from_principal: principal;
    from_subaccount: opt SubAcccount;
    to_principal: principal;
    to_subaccount: opt SubAccount;
    amount: nat64;
};

transferFrom: (TransferFrom) -> (variant { Ok: nat64; Err: TransferFromError; });
```

The result is either the block index of the approve or an error.

### allowance

Returns the `amount` which `spender` is still allowed to withdraw from the account `(owner, owner_subaccount)`.

```
type AllowanceArgs = record {
    owner: principal;
    owner_subaccount: opt SubAcccount;
    spender: principal;
};

allowance: (AllowanceArgs) -> (nat64) query;
```