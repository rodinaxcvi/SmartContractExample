# SmartContractExample


**This smart contract manages an escrow system between a buyer, seller, and an escrow agent. Let's break it down:**

1. License & Version:


**// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;**

- Specifies the license as MIT.
- Declares the version of Solidity that the contract is written for (0.8.0 or higher).


2. Contract Declaration:

**contract Escrow {**

-The contract is named Escrow.



3. State Variables:

**address public buyer;
address public seller;
address public escrowAgent;
uint256 public amount;
bool public fundsDeposited;
bool public buyerApproval;
bool public sellerApproval;**


- buyer, seller, and escrowAgent are the involved parties (all are addresses).
- amount stores the amount of funds involved in the transaction.
- fundsDeposited, buyerApproval, and sellerApproval are boolean flags tracking the transaction's state.



4. Events

**event Deposit(address indexed buyer, uint256 amount);
event Approval(address indexed party, string role);
event ReleaseFunds(address indexed seller, uint256 amount);
event Refund(address indexed buyer, uint256 amount);**

- Deposit: Triggered when the buyer deposits funds.
- Approval: Triggered when either the buyer or seller approves the transaction.
- ReleaseFunds: Triggered when the funds are released to the seller.
- Refund: Triggered when the funds are refunded to the buyer.



5. Modifiers

**modifier onlyBuyer() { ... }
modifier onlyEscrowAgent() { ... }
modifier onlySeller() { ... }**

- These modifiers enforce access control. Only the specified party (buyer, seller, or escrow agent) can call certain functions.



6. Constructor


**constructor(address _buyer, address _seller, address _escrowAgent, uint256 _amount) { ... }**

- The constructor initializes the contract with the buyer, seller, escrow agent, and the transaction amount. It also validates that the addresses are not zero and the amount is positive.



7. Buyer Deposit Function


**function depositFunds() external payable onlyBuyer { ... }**

- The buyer deposits the agreed amount into the contract. This function ensures that only the buyer can call it and the correct amount is deposited.
- It also triggers the Deposit event when funds are deposited.



8. Buyer Approval


**function approveByBuyer() external onlyBuyer { ... }**

- The buyer approves the release of the funds. If the buyer approves, it checks both parties' approvals and releases the funds if all conditions are met.
- It triggers the Approval event.



9. Seller Approval

**function approveBySeller() external onlySeller { ... }**

- Similar to the buyer's approval, the seller approves the release of funds. The contract checks both approvals and 
  releases the funds if everything is in order.
- It triggers the Approval event.



10. Internal Fund Release Logic

**function _checkApprovalAndRelease() internal { ... }**

- This internal function checks if both parties have approved and the funds have been deposited. If all conditions are met,   it releases the funds to the seller and triggers the ReleaseFunds event.
- After the transaction is completed, it resets the state.



11. Refund Function

**function refundToBuyer() external onlyEscrowAgent { ... }**

- If either party does not approve, the escrow agent can refund the buyer.
- The function checks that funds are deposited and that not both parties have approved. If conditions are met, it refunds     the buyer and triggers the Refund event.



12. State Reset Function

**function _resetState() internal { ... }**

- After each transaction, whether successful or refunded, this function resets the state variables (fundsDeposited,       
  buyerApproval, sellerApproval) to their initial values.
- In summary, this contract provides a secure way for a buyer, seller, and escrow agent to handle a transaction. The buyer 
  deposits funds, the buyer and seller approve, and the escrow agent releases the funds to the seller. If anything goes 
  wrong, the escrow agent can refund the buyer.
