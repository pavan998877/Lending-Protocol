Examine the following Solidity code for a basic lending protocol. Identify any security vulnerabilities in the code, particularly related to flash loan attacks, and rewrite it to prevent such vulnerabilities.

pragma solidity ^0.8.0;

contract LendingProtocol {
    mapping(address => uint) public balances;
    uint public totalLent;

    function deposit() public payable {
        balances[msg.sender] += msg.value;
        totalLent += msg.value;
    }

    function borrow(uint amount) public {
        require(amount <= totalLent, "Insufficient funds");
        balances[msg.sender] += amount;
        totalLent -= amount;
        (bool sent, ) = msg.sender.call{value: amount}("");
        require(sent, "Transfer failed");
    }

    function repay() public payable {
        balances[msg.sender] -= msg.value;
        totalLent += msg.value;
    }
}
