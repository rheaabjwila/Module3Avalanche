// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract RheasCookieToken is ERC20, Ownable {
    uint256 public constant MAX_SUPPLY = 1000 * 10**18; // Maximum supply of 1000 tokens

    event CookiesBaked(address indexed to, uint256 amount);
    event CookiesMinted(address indexed to, uint256 amount);

    constructor(address initialOwner) ERC20("RheasCookieToken", "NCT") Ownable(initialOwner) {
        _bakeCookies(msg.sender, 100 * 10**18);
    }

    function bakeCookies(address to, uint256 amount) public onlyOwner {
        require(totalSupply() + amount <= MAX_SUPPLY, "Cannot exceed maximum cookie supply");
        _mint(to, amount);
        emit CookiesBaked(to, amount);
    }

    function mint(address to, uint256 amount) public onlyOwner {
        require(totalSupply() + amount <= MAX_SUPPLY, "Cannot exceed maximum cookie supply");
        _mint(to, amount);
        emit CookiesMinted(to, amount);
    }

    function transfer(address to, uint256 amount) public override returns (bool) {
        _transfer(msg.sender, to, amount);
        return true;
    }

    function eatCookies(uint256 amount) public {
        _burn(msg.sender, amount);
    }

    // Internal function to mint tokens during the contract deployment
    function _bakeCookies(address to, uint256 amount) internal {
        _mint(to, amount);
    }
}
