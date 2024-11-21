// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract MyToken is ERC20, Ownable {
    constructor() ERC20("MyToken", "MTK") {
        _mint(msg.sender, 10000000 * 10 ** decimals()); // Total supply of 10 million tokens
    }
}
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";
import "@openzeppelin/contracts/security/Pausable.sol";
import "@openzeppelin/contracts/token/ERC20/extensions/ERC20Burnable.sol";

contract MyToken is ERC20, Ownable, Pausable, ERC20Burnable {
    // Total supply of the token
    uint256 private constant INITIAL_SUPPLY = 10000000 * 10 ** decimals(); // 10 million tokens

    // Events
    event Mint(address indexed to, uint256 amount);
    event Burn(address indexed from, uint256 amount);

    constructor() ERC20("MyToken", "MTK") {
        _mint(msg.sender, INITIAL_SUPPLY); // Mint initial supply to the contract deployer
    }

    // Function to mint new tokens (only owner can mint)
    function mint(address to, uint256 amount) external onlyOwner {
        _mint(to, amount);
        emit Mint(to, amount);
    }

    // Function to burn tokens (override burn function from ERC20Burnable)
    function burn(uint256 amount) public override {
        super.burn(amount);
        emit Burn(msg.sender, amount);
    }

    // Function to pause token transfers (only owner can pause)
    function pause() external onlyOwner {
        _pause();
    }

    // Function to unpause token transfers (only owner can unpause)
    function unpause() external onlyOwner {
        _unpause();
    }

    // Override _beforeTokenTransfer to implement pausable functionality
    function _beforeTokenTransfer(address from, address to, uint256 amount) internal whenNotPaused override {
        super._beforeTokenTransfer(from, to, amount);
    }
}
