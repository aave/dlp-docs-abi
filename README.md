## Aave Decentralized Lending Protocol documentation

This is the initial version of the Decentralized Lending Pool (DLP) protocol documentation. It details the smart contract interfaces and the API Rest endpoint. Further details will be added to this documentation during the upcoming weeks.

### Smart contract structure


[TBD]


### Testnet release

The smart contracts are currently live on the Kovan testnet. A list of the involved addresses follows:

#### LendingPool

0x5c94e75316CAD5f5e0eF2E8A460Bd4aBAd6222Ee

#### LendingPoolCore

0x7BF52810F9Ff7F13c99F12D5512e840850eDb3b2

#### Mock price Oracle

0x68556C42b33df0F39fBcB8877eF0FA7E9c5a4282

### Market Rate Oracle



*** NOTE *** Current testnet uses a mock version of the Aave Price Oracle, where prices are currently submitted manually. The Aave price oracle is currently being tested separately and will be integrated accordingly.




### ABI Details

The LendingPool smart contract is the primary access to the protocol. Here is a list of functions that are available currently available. Note during the testnet period, these interfaces will likely change.


#### Actions

```
    function deposit(address _reserve, uint _amount) public payable

```
Allows the deposit of the asset identified by the _reserve_ address with the specified amount.

```
    function withdraw(address _reserve, uint _amount) public 
    
```

Allows the withdrawal of the asset identified by _reserve_ with the specified amount.

```
    function borrow(address _reserve, uint _amount, uint _interestRateMode) public
```

InterestRateMode is defined with the following Enumerative:


```
   enum InterestRateMode{
        FIXED,
        VARIABLE
    }
```



Allows to borrow the specified asset with the specified amount and the specified interest rate mode, as long as the caller previously deposited enough collateral to cover the borrowing.

```
    function repay(address _reserve, uint _amount, address _onBehalfOf) public payable
```

Allows the caller to repay (either for himself or on behalf of another address) a specific borrowing. Note: _onBehalfOf_ needs to be equal to msg.sender when the caller wants to repay for himself.

```
    function swapBorrowRateMode(address _reserve) public 
    
```

Allows the caller to swap his borrowing interest rate mode from fixed to variable, or vice versa.

```
    function collateralCall(address _collateral, address _user, uint _purchaseAmount, address _reserve) public payable

```

Execute collateral call on a specific asset for a specific account, is the collateral price in ETH is below the liquidation threshold.


#### Accessors

```
    function getReserveData(address _reserve) external view returns(
        uint totalLiquidity,
        uint availableLiquidity,
        uint totalBorrowsFixed, //total amount borrowed on a fixed rate
        uint totalBorrowsVariable, //total amount borrowed on a variable rate
        uint liquidityRate, //depositors APR (in rays)
        uint variableBorrowRate, //variable borrows APR (in rays)
        uint fixedBorrowRate, //current fixed borrow rate (in rays)
        uint averageFixedBorrowRate, //weighted average rate of all the fixed rate borrows  
        uint utilizationRate,
        uint liquidityIndex, //cumulation index of the depositors interest for the whole reserve
        uint variableBorrowIndex) //cumulation index of the variable borrowers interest for the whole reserve
```

Allows to fetch all the data related to a specific reserve.

```

    function getUserGlobalData(address _user) external view returns(
        uint totalLiquidityETH, //total liquidity deposited by _user, in ETH
        uint totalBorrowsETH, //total amount borrowed by _user, in ETH
        uint currentLiquidationRatio, //average liquidation ratio on the borrows
        uint ltv, //current average LTV based on the collaterals deposited by the user
        bool isBelowLiquidationThreshold) //if the borrowing is below liquidation threshold 
```

Fetches the user global data (data across all the reserves)
 
 ```
 
    function getUserReserveData(address _reserve, address _user) external view returns(
        uint currentLiquidityBalance, //total deposits for _reserve (includes compounded interest)
        uint currentBorrowBalance, //total borrows for _reserve (includes compounded interest)
        uint principalLiquidityBalance, //original amount deposited by _user
        uint principalBorrowBalance, //original amount borrowed by _user
        uint borrowRateMode, //if the current borrow rate mode is fixed or variable
        uint borrowRate, //current rate (might be fixed or variable depending on borrowratemode)
        uint liquidityRate, //current liquidity rate on the deposits
        uint originationFee, //origination fee cumulated from the borrows
        uint liquidityIndex, //cumulation index at the moment of the deposit
        uint variableBorrowIndex)  //cumulation index at the moment of the borrow (only for variable borrowings)
        
 ```
 
 Fetches the user data for a specific reserve.
 
 
 ### REST API
 
 A set of REST API has been developed to support the integration of the protocol. These endpoints allow to fetch the data from the smart contracts in a easy way and automatically generate the transactions to perform actions. Here is a list of the endpoints:
 
 
 
 
