pragma solidity ^0.7.5;

contract Voting {
    enum VoteStates{absent,yes,no}

    event ProposalCreated(uint);
    event VoteCast(uint, address);

    struct Proposal {
        address creator;
        string question;
        uint yesCount;
        uint noCount;
        mapping (address => VoteStates) voteStates;
    }

    Proposal[] public proposals;

    mapping(address => bool) members;

    constructor(address[] memory _members) public {
        for(uint i = 0; i < _members.length; i++) {
            members[_members[i]] = true;
        }
        members[msg.sender] = true;
    }

    function newProposal(string calldata _question) external {
        require(members[msg.sender]);
        emit ProposalCreated(proposals.length);
        Proposal storage proposal = proposals.push();
        proposal.creator = msg.sender;
        proposal.question = _question;
    } 

    function castVote(uint proposalId, bool _vote) external {
        require(members[msg.sender]);
        Proposal storage proposal = proposals[proposalId];

        if (proposal.voteStates[msg.sender] == VoteStates.yes) {
            proposal.yesCount--;
        }
        else if (proposal.voteStates[msg.sender] == VoteStates.no) {
            proposal.noCount--;
        }

        if (_vote == true) {
            proposal.yesCount++;
            proposal.voteStates[msg.sender] = VoteStates.yes;
        }
        else {
            proposal.noCount++;
            proposal.voteStates[msg.sender] = VoteStates.no;
        }

        emit VoteCast(proposalId, msg.sender);
    }
}
