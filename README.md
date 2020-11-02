# Profit_splitter_contract

This project involves building smart contracts to automate some company finances to make everyone's lives easier, increase transparency, and to make accounting and auditing practically automatic!

The profit splitter contracts will do the following:
  * Pay Associate-level employees quickly and easily.
  * Distribute profits to different tiers of employees.
  * Distribute company shares for employees in a "deferred equity incentive plan" automatically.
  
This project was created in Remix IDE and uses the Ganache development chain and MetaMask (localhost:8545)

## Associate Profit Splitter Contract

This contract has two main functions:
 
  * Balance -- This function should is set to `public view returns(uint)`, and must return the contract's current balance. Since we should always be sending Ether to the beneficiaries, this function should always return 0. If it does not, the deposit function is not handling the remainders properly and should be fixed. This will serve as a test function of sorts.
  
  * Deposit -- This function is set to `public payable check`, ensuring that only the owner can call the function and transfers an amount of Ether to three different employees.

It is important to note that `uint` only contains positive whole numbers, and Solidity does not fully support float/decimals, so we must deal with a potential remainder at the end of this function since amount will discard the remainder during division.
We do this by transfering the `msg.value - amount * 3` back to `msg.sender`. This will re-multiply the amount by 3, then subtract it from the `msg.value` to account for any leftover wei, and send it back to Human Resources.

### Testing the Contract

The contract is compiled and then deployed by connecting to Injected Web3. During deployment, we assign employee addresses.

![image](https://user-images.githubusercontent.com/65314799/97828696-78f1a180-1c8d-11eb-902e-4058e3ec9f4c.png)

**Deposit of 40 Ether is executed through MetaMask:**

![image](https://user-images.githubusercontent.com/65314799/97828832-d8e84800-1c8d-11eb-95db-8f119f7fedb5.png)

**Original Balances shown in Ganache:**

![image](https://user-images.githubusercontent.com/65314799/97828931-1b118980-1c8e-11eb-966e-4e9f202dd3b2.png)

**Balances After Deposit shown in Ganache:**

![image](https://user-images.githubusercontent.com/65314799/97829058-6a57ba00-1c8e-11eb-8dcc-ea7a895c8bbd.png)
 
**Calling Balance Function:**

Notice it returns a value of 0 which indicates are deposit function is handling the remainders successfully.

![image](https://user-images.githubusercontent.com/65314799/97829133-abe86500-1c8e-11eb-948b-3855792f19de.png)

## Tiered Profit Splitter Contract

In this contract, rather than splitting the profits between Associate-level employees, you will calculate rudimentary percentages for different tiers of employees (CEO, CTO, and Bob).

The deposit function has these differences:

* Calculation of the number of points/units is done by dividing `msg.value` by 100. This allows us to multiply the points with a number representing a percentage.
* The `uint amount` variable is used to store the amount to send each employee temporarily. For each employee, the amount is set equal to the number of points multiplied by the percentage 
  - For employee_one, distribute points * 60.
  - For employee_two, distribute points * 25.
  - For employee_three, distribute points * 15.
* After calculating the amount for the first employee, the amount is added to the total to keep a running total of how much of the `msg.value` has distributed.
* After transfering the amount, the remainder is sent to the employee with the highest percentage by subtracting `total` from `msg.value`.

## Deferred Equity Plan Contract
In this contract, we managed an employee's "deferred equity incentive plan" in which 1000 shares will be distributed over 4 years to the employee. 
We didn't work with Ether in this contract, but we store and set the amounts that represent the number of distributed shares the employee owns and enforcing the vetting periods automatically.














