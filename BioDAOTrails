/// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

// Minimal ERC20 interface
interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
}

contract BioDAOTrials {
    address public admin;
    IERC20 public rewardToken;

    uint public trialCount = 0;

    enum TrialStatus { Pending, Approved, Completed }

    struct Trial {
        uint id;
        address researcher;
        string title;
        string description;
        uint rewardPerParticipant;
        uint maxParticipants;
        address[] participants;
        mapping(address => bool) completed;
        TrialStatus status;
    }

    mapping(uint => Trial) public trials;

    event TrialProposed(uint trialId, address researcher);
    event TrialApproved(uint trialId);
    event Enrolled(uint trialId, address participant);
    event Completed(uint trialId, address participant, uint reward);

    modifier onlyAdmin() {
        require(msg.sender == admin, "Only admin can call this");
        _;
    }

    constructor(address _rewardToken) {
        admin = msg.sender;
        rewardToken = IERC20(_rewardToken);
    }

    function proposeTrial(
        string memory _title,
        string memory _description,
        uint _rewardPerParticipant,
        uint _maxParticipants
    ) public {
        Trial storage t = trials[trialCount];
        t.id = trialCount;
        t.researcher = msg.sender;
        t.title = _title;
        t.description = _description;
        t.rewardPerParticipant = _rewardPerParticipant;
        t.maxParticipants = _maxParticipants;
        t.status = TrialStatus.Pending;

        emit TrialProposed(trialCount, msg.sender);
        trialCount++;
    }

    function approveTrial(uint _trialId) public onlyAdmin {
        Trial storage t = trials[_trialId];
        require(t.status == TrialStatus.Pending, "Already approved or completed");
        t.status = TrialStatus.Approved;
        emit TrialApproved(_trialId);
    }

    function enroll(uint _trialId) public {
        Trial storage t = trials[_trialId];
        require(t.status == TrialStatus.Approved, "Trial not approved");
        require(t.participants.length < t.maxParticipants, "Participant limit reached");

        for (uint i = 0; i < t.participants.length; i++) {
            require(t.participants[i] != msg.sender, "Already enrolled");
        }

        t.participants.push(msg.sender);
        emit Enrolled(_trialId, msg.sender);
    }

    function markCompleted(uint _trialId, address _participant) public {
        Trial storage t = trials[_trialId];
        require(msg.sender == t.researcher || msg.sender == admin, "Unauthorized");
        require(!t.completed[_participant], "Already rewarded");

        // In real-world usage, integrate oracle or zero-knowledge proof here
        t.completed[_participant] = true;

        rewardToken.transfer(_participant, t.rewardPerParticipant);
        emit Completed(_trialId, _participant, t.rewardPerParticipant);
    }

    function getParticipants(uint _trialId) public view returns (address[] memory) {
        return trials[_trialId].participants;
    }
}
/// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

// Minimal ERC20 interface
interface IERC20 {
    function transfer(address recipient, uint256 amount) external returns (bool);
}

contract BioDAOTrials {
    address public admin;
    IERC20 public rewardToken;

    uint public trialCount = 0;

    enum TrialStatus { Pending, Approved, Completed }

    struct Trial {
        uint id;
        address researcher;
        string title;
        string description;
        uint rewardPerParticipant;
        uint maxParticipants;
        address[] participants;
        mapping(address => bool) completed;
        TrialStatus status;
    }

    mapping(uint => Trial) public trials;

    event TrialProposed(uint trialId, address researcher);
    event TrialApproved(uint trialId);
    event Enrolled(uint trialId, address participant);
    event Completed(uint trialId, address participant, uint reward);

    modifier onlyAdmin() {
        require(msg.sender == admin, "Only admin can call this");
        _;
    }

    constructor(address _rewardToken) {
        admin = msg.sender;
        rewardToken = IERC20(_rewardToken);
    }

    function proposeTrial(
        string memory _title,
        string memory _description,
        uint _rewardPerParticipant,
        uint _maxParticipants
    ) public {
        Trial storage t = trials[trialCount];
        t.id = trialCount;
        t.researcher = msg.sender;
        t.title = _title;
        t.description = _description;
        t.rewardPerParticipant = _rewardPerParticipant;
        t.maxParticipants = _maxParticipants;
        t.status = TrialStatus.Pending;

        emit TrialProposed(trialCount, msg.sender);
        trialCount++;
    }

    function approveTrial(uint _trialId) public onlyAdmin {
        Trial storage t = trials[_trialId];
        require(t.status == TrialStatus.Pending, "Already approved or completed");
        t.status = TrialStatus.Approved;
        emit TrialApproved(_trialId);
    }

    function enroll(uint _trialId) public {
        Trial storage t = trials[_trialId];
        require(t.status == TrialStatus.Approved, "Trial not approved");
        require(t.participants.length < t.maxParticipants, "Participant limit reached");

        for (uint i = 0; i < t.participants.length; i++) {
            require(t.participants[i] != msg.sender, "Already enrolled");
        }

        t.participants.push(msg.sender);
        emit Enrolled(_trialId, msg.sender);
    }

    function markCompleted(uint _trialId, address _participant) public {
        Trial storage t = trials[_trialId];
        require(msg.sender == t.researcher || msg.sender == admin, "Unauthorized");
        require(!t.completed[_participant], "Already rewarded");

        // In real-world usage, integrate oracle or zero-knowledge proof here
        t.completed[_participant] = true;

        rewardToken.transfer(_participant, t.rewardPerParticipant);
        emit Completed(_trialId, _participant, t.rewardPerParticipant);
    }

    function getParticipants(uint _trialId) public view returns (address[] memory) {
        return trials[_trialId].participants;
    }
}
