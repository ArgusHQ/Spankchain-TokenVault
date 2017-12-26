# Findings

- [General Findings](#general-findings)
    + [Testing](#testing)
- [Significant Findings](#significant-findings)
  * [High](#high)
    + [```TokenVault.sol: claim()```](#tokenvaultsol-claim)
  * [Medium](#medium)
    
  * [Low](#low)
    + [```TokenVault.sol: claim()```](#tokenvaultsol-claim)
    + [```TokenVault.sol```](#tokenvaultsol-constructors)


# General Findings 

### Testing 
There is a missing dependency `csv-load-sync` required to run the tests.

Out of the 10 total tests provided in `vaultTest.js` 7 failed. After further inspection, the logic of the tests was correct, however Solidity would throw before the assert statements were reached. For example in the second test: `"Non-owner can't load the participants"` the `setInvestor()` method is given invalid parameters and the following call to `tokensAllocatedTotal()` correctly fails. However, this triggers an EVM exception which marks the test as failed before the assert statement is reached even though the contract's behavior was correct. 


# Significant Findings

## Severity explanation:
- High: Affects functionality of protocol 
- Medium: Affects intended functionality, but does not break protocol
- Low: Design discrepancies, optimization opportunities, minor issues

## High

### ```TokenVault.sol: claim()```

**Description of the Exploit**:

The `token.transfer` call is not checked for failure, so an investor may not receive their tokens if the transfer fails but the vault will record that they have claimed their tokens. This locks the investor's tokens in the vault forever.

**Recommendation**:

Wrap the call in a `require()`.


## Medium



## Low
### ```TokenVault.sol: claim(), TokenVault.sol: setInvestor()```


**Description of the Exploit**:

There is a possibility, albeit unlikely, of an unsigned integer overflow.

**Recommendation**:

Use SafeAdd to ensure that addition operates without undefined behavior.

### ```TokenVault.sol: Constructors```

**Recommendation**:

It is good practice to initialize the state variables to some default values in the constructor even if the default is zero. (e.g. 'lockedAt')


