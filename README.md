# Basic Sample Re-Entrancy Attack Demo

This project demonstrates a Re-entrancy attack. 

The 'GoodContract.sol' is quite simple. The first function, 'addBalance' updates a mapping to reflect how much ETH has been deposited into this contract by another address. The second function, withdraw, allows users to withdraw their ETH back - but the ETH is sent before the balance is updated.

Now lets create another file inside the contracts directory known as 'BadContract.sol'

Within the constructor, this contract sets the address of 'GoodContract' and initializes an instance of it.

The attack function is a payable function that takes some ETH from the attacker, deposits it into 'GoodContract', and then calls the withdraw function in 'GoodContract'.

At this point, 'GoodContract' will see that BadContract has a balance greater than 0, so it will send some ETH back to BadContract. However, doing this will trigger the 'receive()' function in BadContract.

The 'receive()' function will check if 'GoodContract' still has a balance greater than 0 ETH, and call the withdraw function in 'GoodContract' again.

This will create a loop where 'GoodContract' will keep sending money to BadContract until it completely runs out of funds, and then finally reach a point where it updates BadContract's balance to 0 and completes the transaction execution. At this point, the attacker has successfully stolen all the ETH from 'GoodContract' due to re-entrancy.
