# gov-authority
![Build Status](https://github.com/indefibank/gov-authority/actions/workflows/.github/workflows/tests.yaml/badge.svg?branch=master)

Custom authority for allowing GOV to govern GOV.

Intentionally simple. Once set as the GOV token's authority, if the GOV token's `owner` field is subsequently set to
zero, the following properties obtain:
* any user may call GOV's `burn` function
* only authorized users (according to the GovAuthority's `ward`s) can call GOV's `mint()` function
* only the `root` user set in the authority can call other `auth`-protected functions of the GOV contract
* only the `root` user can modify the GovAuthority's `ward`s or change the `root`

Though this contract could be used in different ways, it was designed in the context of an overall design for control 
of the GOV token via GOV governance as illustrated below.

```
<~~~ : points to source's authority
<=== : points to source's root or owner

-------    -------    ------------    --------------    -----
|Chief|<~~~|Pause|<===|PauseProxy|<===|GovAuthority|<~~~|GOV|===>0
-------    -------    ------------    --------------    -----
```

Such a structure allows governance proposals voted in on the Chief to make arbtirary changes to the GOV token
and its permissions subject to a delay. (See DappHub contracts
[DSChief](https://github.com/dapphub/ds-chief) and [DSPause](https://github.com/dapphub/ds-pause)
for implementations of the voting contract and the delay contract, respectively.)

Note that the GovAuthority allows for upgrading of the GOV token's `authority` or `owner` by the `root`.

K specifications of the contract's functionality can be found in: https://github.com/indefibank/k-gov-authority/
