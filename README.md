# Experiment 2: Blockchain-Based Crowdfunding (Kickstarter Alternative)
## Aim:
To create a decentralized crowdfunding platform where donors contribute funds only if the campaign goal is met.

## Algorithm:
A project owner starts a campaign with a funding goal and deadline.


Contributors can send ETH to the campaign.


If the goal is met before the deadline, funds are released to the project owner.


If the goal is not met, contributors can withdraw their funds.


## Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract Crowdfunding {
    struct Campaign {
        address creator;
        uint256 goal;
        uint256 deadline;
        uint256 amountRaised;
        bool goalMet;
        mapping(address => uint256) contributions;
    }

    Campaign public campaign;

    constructor(uint256 _goal, uint256 _duration) {
        campaign.creator = msg.sender;
        campaign.goal = _goal;
        campaign.deadline = block.timestamp + _duration;
    }

    function contribute() public payable {
        require(block.timestamp < campaign.deadline, "Campaign ended");
        campaign.amountRaised += msg.value;
        campaign.contributions[msg.sender] += msg.value;
    }

    function withdrawFunds() public {
        require(msg.sender == campaign.creator, "Only creator can withdraw");
        require(campaign.amountRaised >= campaign.goal, "Goal not met");
        payable(msg.sender).transfer(campaign.amountRaised);
        campaign.goalMet = true;
    }

    function refund() public {
        require(block.timestamp > campaign.deadline, "Campaign still active");
        require(campaign.amountRaised < campaign.goal, "Goal was met");
        uint256 amount = campaign.contributions[msg.sender];
        campaign.contributions[msg.sender] = 0;
        payable(msg.sender).transfer(amount);
    }
}
```
# Expected Output:
Users can contribute ETH to the campaign.


If the goal is met, the creator can withdraw funds.
<img width="1915" height="831" alt="Screenshot 2025-10-23 093637" src="https://github.com/user-attachments/assets/bb86fc5a-2550-4af3-914f-e38637376bbc" />


If the goal is not met, contributors can claim a refund.
<img width="1918" height="830" alt="Screenshot 2025-10-23 214837" src="https://github.com/user-attachments/assets/48fd75c2-8ed1-46c3-b8aa-3eb41ea634e6" />


# High-Level Overview:
Teaches decentralized fundraising.

Avoids fraud by ensuring funds are only transferred if the goal is met.

# RESULT: 
Thus blochain-based crowdfunding is done successfully.
