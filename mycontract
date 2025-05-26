// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

// A simple will contract that transfers funds to a recipient after a long period of owner inactivity
contract SimpleWill {
    // State variables (stored on the blockchain)
    address public owner; // The person who creates the contract
    address public recipient; // The person who will receive the funds
    uint256 public lastPing; // Last time the owner confirmed they are active
    uint256 public constant INACTIVITY_PERIOD = 60 seconds; // Changed to 60 seconds for testing

    // Constructor runs when the contract is deployed
    constructor(address _recipient) payable {
        require(_recipient != address(0), "Recipient cannot be zero address");
        owner = msg.sender; // Set the deployer as the owner
        recipient = _recipient; // Set the recipient
        lastPing = block.timestamp; // Record the deployment time
    }

    // Owner calls this to show they are still active
    function ping() external {
        require(msg.sender == owner, "Only the owner can ping");
        lastPing = block.timestamp; // Update the last active time
    }

    // Recipient can claim funds if the owner has been inactive for too long
    function claim() external {
        require(msg.sender == recipient, "Only the recipient can claim");
        require(block.timestamp >= lastPing + INACTIVITY_PERIOD, "Owner is still active");
        
        // Send all funds to the recipient
        (bool success, ) = recipient.call{value: address(this).balance}("");
        require(success, "Transfer failed");
    }

    // Helper function to check the contract's balance
    function getBalance() public view returns (uint256) {
        return address(this).balance;
    }

    // Allow the contract to receive ETH (or tCORE on Core Chain)
    receive() external payable {}
}
