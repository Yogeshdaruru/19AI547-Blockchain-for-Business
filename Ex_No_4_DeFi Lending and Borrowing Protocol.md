# Experiment 4: DeFi Lending and Borrowing Protocol
# Aim:
To build a decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral. This experiment introduces concepts like overcollateralization, liquidity pools, and interest accrual in DeFi.

# Algorithm:
Step 1: Initialize Lending and Borrowing Mechanism
Users deposit ETH into the smart contract to provide liquidity.

In return, depositors earn interest based on the amount and duration of their deposits.

Borrowers can request to borrow ETH by depositing collateral.

The collateral must exceed the loan amount (e.g., 150% of the borrowed ETH).

The interest rate for borrowers adjusts dynamically based on the utilization rate (borrowed liquidity / total available liquidity).

Step 2: Enforce Overcollateralization
Continuously monitor the value of each borrower’s collateral relative to their debt.

If the collateral value falls below the required minimum collateral ratio (e.g., 150%), the position becomes eligible for liquidation.

Step 3: Enable Liquidation Mechanism
When a borrower's collateral drops below the liquidation threshold:

Third-party liquidators can repay the borrower’s outstanding debt.

In exchange, they receive the borrower’s collateral at a discounted rate.

This ensures that the protocol remains solvent and depositors are protected.

Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DeFiLending {
    address public owner;
    uint256 public interestRate = 5; // 5% interest per cycle
    uint256 public liquidationThreshold = 150; // 150% collateralization
    mapping(address => uint256) public deposits;
    mapping(address => uint256) public borrowed;
    mapping(address => uint256) public collateral;

    event Deposited(address indexed user, uint256 amount);
    event Borrowed(address indexed user, uint256 amount, uint256 collateral);
    event Liquidated(address indexed user, uint256 debtRepaid, uint256 collateralSeized);

    constructor() {
        owner = msg.sender;
    }

    function deposit() public payable {
        require(msg.value > 0, "Deposit must be greater than zero");
        deposits[msg.sender] += msg.value;
        emit Deposited(msg.sender, msg.value);
    }

    function borrow(uint256 amount) public payable {
        require(msg.value >= (amount * liquidationThreshold) / 100, "Not enough collateral");
        borrowed[msg.sender] += amount;
        collateral[msg.sender] += msg.value;
        payable(msg.sender).transfer(amount);
        emit Borrowed(msg.sender, amount, msg.value);
    }

    function liquidate(address borrower) public {
        require(collateral[borrower] < (borrowed[borrower] * liquidationThreshold) / 100, "Not eligible for liquidation");
        uint256 debt = borrowed[borrower];
        uint256 seizedCollateral = collateral[borrower];

        borrowed[borrower] = 0;
        collateral[borrower] = 0;
        payable(msg.sender).transfer(seizedCollateral);
        emit Liquidated(borrower, debt, seizedCollateral);
    }
}

```
# Expected Output:

Users can deposit ETH and earn interest.

![Screenshot (76)](https://github.com/user-attachments/assets/7bf7f8de-bc90-4b4b-98a1-427f38091de6)

Users can borrow ETH by providing collateral.
![Screenshot (75)](https://github.com/user-attachments/assets/663b53ea-2492-47ef-8408-17a3c6cca60c)


If collateral < 150% of borrowed amount, liquidators can seize the collateral.
![Screenshot (74)](https://github.com/user-attachments/assets/3c42c983-8bd4-4aba-91ac-cc9c9d3af8f4)



# High-Level Overview:
Teaches key DeFi concepts: lending, borrowing, collateral, liquidation.


Introduces risk management: overcollateralization and liquidation.


Directly related to DeFi protocols like Aave and Compound.

# RESULT : 
Users deposit ETH to earn interest, borrowers take overcollateralized loans (e.g., 150%), and if collateral falls below a liquidation threshold, liquidators repay the debt and claim the collateral at a discount.









