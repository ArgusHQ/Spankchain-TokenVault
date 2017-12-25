# Analysis

- [TokenVault.sol](#TokenVault.sol)
  * [Description of contract system](#description-of-contract-system)
  * [Functions](#functions)
    + [TokenVault](#TokenVault)
    + [setInvestor](#setInvestor)
    + [lock](#lock)
    + [recoverFailedLock](#recoverFailedLock)
    + [getBalance](#getBalance)
    + [claim](#claim)
    + [getState](#getState)
    
- [Library Contracts](#library-contracts)

# TokenVault.sol
## Description of contract system
This contract is an interface for LoopringProtocolImpl.sol

## Functions
### TokenVault
``` 
function TokenVault(
  uint _freezeEndsAt, 
  HumanStandardToken _token, 
  uint _tokensToBeAllocated)
```

This function instantiates the vault. It accepts a UNIX timestamp to indicate when the tokens should be unlocked. It also accepts a token contract, which indicates which token we intend to lock up. Finally, it accpets the number of tokens that we expect the vault to hold.

### setInvestor
``` 
function setInvestor(
  address investor, 
  uint amount) 
  public onlyOwner
```

This function sets the allocation for an investor with a given amount. 

### lock
``` 
function lock() 
  onlyOwner
```

This function sets the `lockedAt` variable to the time that the function is called if two conditions are met. The first is that the vault should not already be locked. The second is that the balance of the vault contract (in the specified token) should equal the sum of amounts allocated in the setInvestor function. This sum is recorded in the `tokensAllocatedTotal` state variable. 

### recoverFailedLock
```
function recoverFailedLock() onlyOwner
```

This function allows the owner of the vault to transfer all the tokens back to him if the contract was not locked properly. 

### getBalance
```
function getBalance() public constant returns (
  uint howManyTokensCurrentlyInVault)
```

This function returns the balance of the vault. 

### claim
```
function claim()
```

This function allows an investor to claim their tokens. The conditions that are checked include checking that the vault was locked, the current time is after the specified end freeze time, the balance of this investor is positive (ensuring this investor is owed tokens), and the investor has not claimed their tokens. If the conditions are met the tokens are transferred to the investor. 

### getState
```
function getState() public constant returns(State)
```

This function returns the current state of the vault. The vault is `Loading` if it has not been locked, `Holding` if it has been locked and the time is before the freeze ends, and `Distributing` if the current time is after the freeze period. 

# Library Contracts
 The following contracts were used:
- HumanStandrardToken.sol
- StandardToken.sol
- Ownable.sol
