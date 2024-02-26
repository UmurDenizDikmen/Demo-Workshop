// SPDX-License-Identifier: MIT
pragma solidity ^0.8.7;

contract ProposalContract{
    uint256 private counter;
    address private  owner;
    mapping(uint256 => Proposal) private proposal_history;
    mapping (address => bool) private hasVoted;
    struct Proposal{
        string title;
        string description;
        uint256 approve;
        uint256 reject;
        uint256 pass;
        uint256 total_vote_to_end;
        bool is_active;
        bool is_accepted;

    }

    constructor(){
        owner = msg.sender;
    } 

    modifier onlyOwner(){
        require(msg.sender == owner , "You are not a owner" );
        _;
    }

    modifier activeProposal(){
            require(proposal_history[counter].is_active , "No active proposal");
            _;
    }

    modifier hasNotVoted(){
        require(!hasVoted[msg.sender] , "Address has already voted");
         _;
    }

    function createProposal(
        string calldata _title,
        string calldata _description,
        uint256   _total_vote_to_end
    ) public onlyOwner{
        require(!proposal_history[counter].is_active,"Proposal is still active");
        counter++;
        proposal_history[counter] = Proposal(
            _title,
            _description,
            0,
            0,
            0,
            _total_vote_to_end,
            true,
            false
        );
    }

    function vote(uint choice) public activeProposal hasNotVoted{
        Proposal storage proposal = proposal_history[counter];
        hasVoted[msg.sender] = true;
        if(choice == 1){
            proposal.approve++;
        }
        else if(choice ==2){
            proposal.reject++;
        }
        else if(choice == 0){
            proposal.pass++;
        }

        if(proposal.approve + proposal.reject + proposal.pass >= proposal.total_vote_to_end ){
            if(proposal.approve > proposal.reject + proposal.pass){
                proposal.is_accepted = true;
            }
            proposal.is_active = false;
        }
    }

    function terminateProposal() public onlyOwner activeProposal{
        proposal_history[counter].is_active = false;
    }

    function setOwner(address newOwner) public onlyOwner{
        owner=newOwner;
    }
    function getCurrentProposal () public view returns(Proposal memory){
        return  proposal_history[counter];
    }
    function getProposal (uint256 number ) public view returns (Proposal memory){
        return  proposal_history[number];
    }

  


}
