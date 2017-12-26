# Findings

- [General Findings](#general-findings)
    + [Testing](#testing)
- [Significant Findings](#significant-findings)
  * [High](#high)
    + [```TokenVault.sol: claim()```](#---TokenVaultsol--claim-----)
  * [Medium](#medium)
    + [```dummyContract.sol: dummyFunction()```](#---dummyContractsol--dummyFunction-----)
  * [Low](#low)
    + [```dummyContract.sol: dummyFunction()```](#---dummyContractsol--dummyFunction-----)


# General Findings 

### Testing 

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

### ```dummyContract.sol: dummyFunction()```

# Low
### ```dummyContract.sol: dummyFunction()```