## Summary

1. The main ideas of RSC Valve SC is to redistribute tokens (whether they are ERC-20 or native cryptocurrency), to the participants based on the percentages assigned to them.
    * Percentages may have up to 5 decimal points. 
    * The sum of all percentages must always be equal to 100% percentageSum == 10000000
1. Every native cryptocurrency sent to this contract address will be redistributed according to the rules either in real-time or manually, as determined by setting the isAutoNativeCurrencyDistribution to true of false.
1. Every ERC-20 token must be manually redistributed using the redistributeToken() method.
1. The contract must always redistribute 100% of the tokens between recipients, and it is not possible to have a contract without recipients or with less than 100% shares assigned.
1. The distribution of native cryptocurrency and ERC-20 tokens can only be done by the one of the distributors. Distributors can be added or removed by the owner.  However, native cryptocurrency distribution can be done by anyone if isAutoNativeCurrencyDistribution is true.
1. The recipients can only be changed by the controller. If controller is zero address, then recipients cannot be changed. In this case we refer to it as immutable recipients, however the contract itself does not have immutability attribute for recipients. 
1. Controller can be changed by the owner of the Valve contract only if (both statements below should pass):
    * Controller is NOT zero address
    * isImmutableController is FALSE

## Actors and use cases

* owner → Address that has the capability to set the distributor / controller;
* recipients →  Addresses which will receive redistributed currency or ERC-20 tokens according to percentage;
* distributors → Addresses which can distribute ERC-20 tokens locked in contract;
* controller → Address which can set recipients. If none (assigned to 0 address) then contract is immutable;
* factory → Address of the factory that was used for contract creation. It is used for getting platformWallet which receives Fee from contract usage;

## Functions

### Read functions
| Function  | Description |
| ------------- |:-------------:|
| controller() -> address      | This function returns the controller’s wallet address. |
| distributors(address distributor) -> bool      | This function (mapping) helps to identify if the address provided to this function is a distributor or not (true/false).     |
| factory() -> address      | This function returns the address of the factory contract.     |
| isAutoNativeCurrencyDistribution() -> bool | This function returns a boolean value indicating whether the native tokens distributions are automatic or not. If the native tokens distribution is automatic, isAutoNativeCurrencyDistribution() returns true, otherwise it returns false. |
| isImmutableController() -> bool      | This function returns a boolean value indicating whether the controller is immutable or not. If the controller address can NOT be changed, isImmutableController() returns true, otherwise it returns false.     |
| minAutoDistributionAmount() -> uint256      | This function returns the minimum number of automatic distribution amount in denominated wei.      |
| numberOfRecipients()       | Returns number of recipients in the contract.      |
| owner() -> address      | Returns the owner of the contract.     |
| platformFee() -> uint256      | This function returns the platform fee in integers. PlatformFee cannot be more than 10000000, which represents 100%     |
| recipients(uint256 number) -> address | Accepts ordinal numbers starting from zero and returns recipient address (which is saved in the array in specified number as a key). |
| recipientsPercentage(address recipient) -> uint256 |  address → number representing percentage with 5 decimal numbers. Thus the minimum share you can allocate is 0.00001% |

### Write functions

| Function  | Description |
| ------------- |:-------------:|
| initialize  | Initialises smart contract with initial settings. See Constructor. |
| redistributeNativeCurrency() | Allows to manually distribute native tokens in the contract to participants based on their percentage. |
| redistributeToken(address token) | Accepts token address and will control whether RSC contract have any balance for this token. If yes it will redistribute the token according to percentages assigned to them. |
| renounceOwnership() -> bool | Leaves the contract without owner. It will not be possible to call onlyOwner functions anymore.  |
| setAutoNativeCurrencyDistribution(bool _isAutoNativeCurrencyDistribution) | This function allows setting an auto native currency(token) distribution to true/false.  |
| setController(address _controller) | This function allows setting a new controller address. If the address is set to address(0), the contract becomes immutable. If contract have immutableController, you cannot change the controller address. |
| setDistributor(address distributor, bool isDistributor) | Enables setting of distributor status to either true or false. A value of true indicates that the address holds the distributor role, while a value of false indicates that the address no longer has the distributor role. |
| setMinAutoDistributionAmount(uint256 _minAutoDistributionAmount) | This function allows setting a minimum auto distribution amount(numerically). |
| setRecipients(address[] recipients, uint256[] percentages)| This function enables to change recipients. |
| transferOwnership(address newOwner) -> bool | Transfers ownership of the contract to a new account (newOwner). |
| fallback() | This function is activated when native tokens are directly sent to the contract or when a non-existent function is invoked. |
| receive() | This function is called when native cryptocurrency is sent directly to the contract. It automatically redistributes received cryptocurrency to the recipients. If the isAutoNativeCurrencyDistribution is set to true, this function will distribute native tokens received to the recipients. However, for this to occur, the contract's balance must exceed the minimum amount set for automatic distribution, minAutoDistributionAmount. |
