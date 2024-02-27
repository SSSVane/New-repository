# 智能合约文档: 随机数投票合约

## 简介
这个智能合约实现了一个简单的随机数投票合约，允许用户投票给候选者，并在投票结束后生成一个随机数来选择获胜者。智能合约的目标是提供一个公平的投票机制，确保每个候选者都有平等的机会获胜。

## 合约结构
这个智能合约由以下几个部分组成：

- `addCandidate(string name)`: 添加候选者的函数，允许管理员添加候选者。
- `vote(uint candidateId)`: 投票函数，允许用户投票给指定的候选者。
- `generateRandomNumber()`: 生成随机数的函数，用于选择获胜者。

## 使用方法
1. 管理员使用 `addCandidate` 函数添加候选者。
2. 用户使用 `vote` 函数为候选者投票。
3. 投票结束后，管理员调用 `generateRandomNumber` 函数来生成一个随机数，用于选择获胜者。

## 使用示例
以下是一个简单的合约示例：

```solidity
pragma solidity ^0.8.0;

contract RandomNumberVoting {
    struct Candidate {
        string name;
        uint votes;
    }

    Candidate[] public candidates;
    mapping(address => bool) public voters;

    function addCandidate(string memory name) public {
        candidates.push(Candidate(name, 0));
    }

    function vote(uint candidateId) public {
        require(candidateId < candidates.length, "Invalid candidate ID");
        require(!voters[msg.sender], "You have already voted");
        
        candidates[candidateId].votes++;
        voters[msg.sender] = true;
    }

    function generateRandomNumber() public view returns (uint) {
        uint randomNumber = uint(keccak256(abi.encodePacked(block.timestamp, block.difficulty))) % candidates.length;
        return randomNumber;
    }
}
