pragma solidity ^0.8.0;

contract TexasHoldemPoker {
    address public owner;
    uint256 public buyInAmount;
    uint256 public pot;
    
    enum Phase { Setup, Betting, Flop, Turn, River, Showdown }
    Phase public currentPhase = Phase.Setup;
    
    struct Player {
        address payable wallet;
        uint256 balance;
        bool isActive;
    }
    
    Player[] public players;
    address public dealer;
    uint256 public currentPlayerIndex;
    uint256 public lastToActIndex;
    
    constructor(uint256 _buyInAmount) {
        owner = msg.sender;
        buyInAmount = _buyInAmount;
    }
    
    modifier onlyOwner() {
        require(msg.sender == owner, "Only the owner can call this function");
        _;
    }
    
    modifier onlyDealer() {
        require(msg.sender == dealer, "Only the dealer can call this function");
        _;
    }
    
    modifier inPhase(Phase _phase) {
        require(currentPhase == _phase, "Invalid phase");
        _;
    }
    
    function joinGame() external payable {
        require(msg.value == buyInAmount, "Incorrect buy-in amount");
        require(currentPhase == Phase.Setup, "Game is not in the setup phase");
        players.push(Player({
            wallet: payable(msg.sender),
            balance: msg.value,
            isActive: true
        }));
    }
    
    function startGame() external onlyOwner {
        require(players.length >= 2, "At least 2 players are required to start the game");
        dealer = players[0].wallet;
        currentPhase = Phase.Betting;
        currentPlayerIndex = 1; // Start with the player to the left of the dealer
    }
    
    function bet(uint256 amount) external inPhase(Phase.Betting) {
        Player storage player = players[currentPlayerIndex];
        require(player.isActive, "Player is not active");
        require(player.balance >= amount, "Not enough balance");
        require(amount > 0, "Bet amount must be greater than 0");
        
        player.balance -= amount;
        pot += amount;
        
        currentPlayerIndex = (currentPlayerIndex + 1) % players.length;
        if (currentPlayerIndex == lastToActIndex) {
            // All players have acted, move to the next phase or end the game
            // Implement the logic for each phase (Flop, Turn, River, Showdown) here
        }
    }
    
    // Add functions for handling other phases (Flop, Turn, River, Showdown) and determining the winner.
}
