# BasicEscrow
BasicEscrow.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract BasicEscrow {
    address public buyer;
    address public seller;
    uint256 public amount;
    bool public isReleased;

    constructor(address _seller) payable {
        buyer = msg.sender;
        seller = _seller;
        amount = msg.value;
    }

    function release() public {
        require(msg.sender == buyer, "Only buyer can release");
        require(!isReleased, "Already released");
        isReleased = true;
        payable(seller).transfer(amount);
    }

    function refund() public {
        require(msg.sender == seller, "Only seller");
        require(!isReleased, "Already released");
        payable(buyer).transfer(amount);
    }
}
