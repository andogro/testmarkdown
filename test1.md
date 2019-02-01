## Verifying a smart contract in blockscout

Once verified, a smart contract or token contract's source code becomes publically available and verifiable. This creates transparency and trust. Plus, it's easy to do!

1. Go to [blockscout.com](https://blockscout.com/), verify you are on the correct chain, and type the contract's address into the search bar. Your contract should come up.

2. Select the `Code` tab below the contract to view the bytecode.

<img 1>

3. Click the `Verify & Publish` button. 

<img 2>

4. Enter your smart contract details:
   1. **Contract Address:** The 0x address supplied on contract creation. 
   2. **Contract Name:** Name of the class whose constructor was called in the .sol file.  For `contract MyContract {`    MyContract is the Contract name.
   3. **Compiler:** derived from the first line in the contract   `pragma solidity X.X.X`. Use the corresponding compiler version rather than the nightly build.
   4. **Optimization:** If you enabled optimization during compilation, check yes.
   5. **Enter the Solidity Contract Code:** You may need to flatten your solidity code if it utilizes a library or inherits dependencies from another contract. We recommend the [POA solidity flattener](https://github.com/poanetwork/solidity-flattener) or the [truffle flattener](https://www.npmjs.com/package/truffle-flattener)
   6. Click the `Verify and Publish` button.
   
<img 3>   
   
 5. If all goes well, you will see a green checkmark next to the code, and an additional tab where you can read the contract. In addition, the contract name will appear in BlockScout with any transactions related to your contract.
 
## Troubleshooting:

If you receive the dreaded ‘There was an error compiling your contract’ error message this means the bytecode doesn't match the supplied sourcecode. Unfortunately, there are many reasons this may be the case. Here are a few things to try:

1. Double check the compiler version is correct.

2. Check that an extra space has not been added to the end of the contract. When pasting in, an extra space is sometimes added. Delete this and attempt to recompile.

3. Copy, paste and verify your source code in Remix. You may find exceptions there.

**Note**: Blockscout does not currently support constructor arguments. 


   
