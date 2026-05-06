# votingsystem.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;
contract votingsystem{
//candidate structure
struct candidate{
    uint id;
    string name;
    string party;
    uint age;
    uint voteCount;
}
//store canditates
mapping ( uint => candidate ) public candidates;

//to prevent double voting
mapping ( address => bool) public hasvoted;
//this should show the total amount of people that voted
uint public totalvotes;
//track if a wallet has voted
mapping ( address => bool) public hasVoted;

uint public candidatescount;
address public admin = 0x5B38Da6a701c568545dCfcB03FcB875f56beddC4;

constructor(){

    
}
//only admin can add candidates
function addcandidate(string memory _name, string memory _party, uint _age) public {
    require(msg.sender == admin, "Only admin can add candidates");
    candidatescount++;
    candidates[candidatescount] = candidate(candidatescount, _name, _party, _age, 0);
}
//vote function
function vote(uint _candidateId) public {
    require(!hasVoted[msg.sender], "you already voted");
    require(_candidateId > 0 && _candidateId <= candidatescount,"invalid candidate");
    hasVoted[msg.sender] = true;
    candidates[_candidateId].voteCount++;
}
//view results
function getvotes(uint _candidateId) public view returns (uint) {
    return candidates[_candidateId].voteCount;
}

}
