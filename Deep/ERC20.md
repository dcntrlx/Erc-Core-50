# [ERC20](https://eips.ethereum.org/EIPS/eip-20)
**Created:** 2015-11-19  
**Description:** A standard interface created to standaritze tokens on Ethereum. It requires implementations to provide basic functionality to:
- Transfer tokens.
- Allow tokens to be approved so they can be spent by another on-chain third party.

### Methods
#### Global
- Callers **MUST** handle true and false from returns (bool success)

#### Required Methods
1.  **`totalSupply`**
    ```solidity
    function totalSupply() public view returns (uint256)
    ```
    Returns the total token supply.

2.  **`balanceOf`**
    ```solidity
    function balanceOf(address _owner) public view returns (uint256 balance)
    ```
    Returns the account balance of another account with address _owner.

3.  **`transfer`**
    ```solidity
    function transfer(address _to, uint256 _value) public returns (bool success)
    ```
    Transfers _value amount of tokens to address _to, and MUST emit the Transfer event.
    - The function SHOULD throw if the message caller’s account balance does not have enough tokens to spend.
    - Note: Transfers of 0 values MUST be treated as normal transfers and emit the Transfer event.

4.  **`transferFrom`**
    ```solidity
    function transferFrom(address _from, address _to, uint256 _value) public returns (bool success)
    ```
    Transfers _value amount of tokens from address _from to address _to, and MUST emit the Transfer event.
    - Used for a withdraw workflow, allowing contracts to transfer tokens on your behalf.
    - The function SHOULD throw unless the _from account has deliberately authorized the sender of the message via some mechanism.
    - Note: Transfers of 0 values MUST be treated as normal transfers and emit the Transfer event.

5.  **`approve`**
    ```solidity
    function approve(address _spender, uint256 _value) public returns (bool success)
    ```
    Allows _spender to withdraw from your account multiple times, up to the _value amount. If this function is called again it overwrites the current allowance with _value_.
    - Security Note: To prevent attack vectors (front-running approval changes), clients SHOULD make sure to create user interfaces in such a way that they set the allowance first to 0 before setting it to another value for the same spender. The contract itself shouldn’t enforce it to allow backwards compatibility.

6.  **`allowance`**
    ```solidity
    function allowance(address _owner, address _spender) public view returns (uint256 remaining)
    ```
    Returns the amount which _spender_ is still allowed to withdraw from _owner_.


#### Optional Methods
These methods can be used to improve usability, but no required by the standard, thus interfaces and other contracts MUST NOT expect these values to be present.

1.  **`name`**
    ```solidity
    function name() public view returns (string)
    ```
    Returns the name of the token.

2.  **`symbol`**
    ```solidity
    function symbol() public view returns (string)
    ```
    Returns the symbol of the token.

3.  **`decimals`**
    ```solidity
    function decimals() public view returns (uint8)
    ```
    Returns the number of decimals the token uses.
    Thats required because Solidity doesn't support floating point numbers. decimals allow to represent token amounts in floating point numbers, e.g:
    for decimals = 6; 1 whole token will be represented as 10^6

### Events

1.  **`Transfer`**
    ```solidity
    event Transfer(address indexed _from, address indexed _to, uint256 _value)
    ```
    - MUST trigger when tokens are transferred, including zero value transfers.
    - A token contract which creates new tokens SHOULD trigger a Transfer event with the _from address set to 0x0 when tokens are created.

2.  **`Approval`**
    ```solidity
    event Approval(address indexed _owner, address indexed _spender, uint256 _value)
    ```
    - MUST trigger on any successful call to approve(address _spender, uint256 _value).

## Implementation
There are many ERC20-compliant tokens deployed on the Ethereum network. Different implementations have different trade-offs (gas saving vs improved security).

## OpenZeppelin Implementation
Openzeppelin provides:
- IERC20 interface describing the interface for all required methods from this standard
- IERC20Metadata interface describing the interface for all optional methods from this standard
- ERC20 implementation provides an implementation of the IERC20 interface + IERC20Metadata interface