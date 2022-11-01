<h2>
    <span>@signatures-pub/signatures-verify</span>
    <span><a href="https://www.npmjs.com/package/@signatures-pub/signatures-verify"><img src="https://badgen.net/npm/v/@signatures-pub/signatures-verify?color=black&labelColor=black"></a></span>
</h2>
Solidity smart contract library for verifying signature

### Installation
Use the following command to install `@signatures-pub/signatures-verify`:
```sh
npm install @signatures-pub/signatures-verify
```

### Usage
Once installed, you can use the contracts in the library by importing them:
```solidity
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
import "@signatures-pub/signatures-verify/Signature.sol";

contract MyNFT is ERC721 {
    address private signerAddress;

    mapping(address => uint256) public claimed;

    constructor(address _signerAddress) ERC721("MyNFT", "MNFT") {
        signerAddress = _signerAddress;
    }

    function whitelistMint(
        uint256 quantity, 
        uint256 maxQuantity,
        bytes memory signature
    ) external 
    {
        require(Signature.verify(maxQuantity, msg.sender, signature) == signerAddress, "Address is not whitelisted");
        require(claimed[msg.sender] + quantity <= maxQuantity, "Exceed current max minted");
        claimed[msg.sender] += quantity; 
        _safeMint(msg.sender, quantity);
    }
}
```
