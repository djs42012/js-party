//Define Player class
class Player {
  constructor (name){
    this.name = name;
    this.playerNumber;
    this.boardPosition = 0;
    this.coinBalance = startingCoinBalance;
    this.inventory = {};
    this.starBalance = 0;
    this.spaceColor = blue;
  }
  //define static methods
  static rollDie = function (){
    return Math.ceil(Math.random() * dieSize);
  }
  static subtractFromAll(coins){
    let subtracted = 0;
    for(let i = 1; i <= playerCount; i++){
      let initialBalance = player[i].coinBalance;
      player[i].decreaseBalance(coins);
      player[i].updateSpace();
      subtracted += (initialBalance - player[i].coinBalance);
    }
    return subtracted;
  }
  //define instance methods
  announce (){
    if (board.space[this.boardPosition][0] === blue){
      confirm (`${coin} ${this.name} gains ${blueReward} coins! ${coin}`)
    }
    else if (board.space[this.boardPosition][0] === red){
      if(this.coinBalance < redPenalty){
        confirm (`${coin} ${this.name} loses ${this.coinBalance} coins! ${coin}`);
      }
      else confirm (`${coin} ${this.name} loses ${redPenalty} coins! ${coin}`);
    }
  }
  increaseBalance (coins){
    this.coinBalance += coins;
  }
  decreaseBalance (coins){
    if (this.coinBalance >= coins) {
      this.coinBalance -= coins;
    }
    else {
      this.coinBalance = 0;
    }
  }
  clearSpace(){
    board.space[this.boardPosition].forEach((ele, index) => {
      if (ele.includes(this.name)) board.space[this.boardPosition].splice(index, 1);
    });
  }
  fillSpace(){
    if(this.spacesRemaining){
    board.space[this.boardPosition].push(`${this.name} ${coin}: ${this.coinBalance} ${star}: ${this.starBalance} ${item} ${Object.keys(this.inventory).length} ${die}: ${this.spacesRemaining}`);
    }
    else {
      board.space[this.boardPosition].push(`${this.name} ${coin}: ${this.coinBalance} ${star}: ${this.starBalance} ${item} ${Object.keys(this.inventory).length}`);
    }
  }
  updateSpace(){
    this.clearSpace();
    this.fillSpace();
  }
  checkPosition(i, newRoll){
    if (i < newRoll){
      if (board.space[this.boardPosition][0] === star){
        refreshConsole();
        this.offerStar();
        this.updateSpace();
        refreshConsole();
      } 
      else if (board.space[this.boardPosition][0] === item) {
        refreshConsole();
        this.offerItem();
        this.updateSpace();
        refreshConsole();
      }   
    }
    else {
      refreshConsole();
      if (board.space[this.boardPosition][0] === star){
        this.offerStar();
      }
      else if (board.space[this.boardPosition][0] === item) {
        this.offerItem();
      }
      else if (board.space[this.boardPosition][0] === event) {
        GameBoard.chooseRandomEvent();
      }
      else if (board.space[this.boardPosition][0] === blue) {
        this.increaseBalance(blueReward);
      } 
      else if (board.space[this.boardPosition][0] === red) {
      this.decreaseBalance(redPenalty);
      } 
      this.updateSpace();
      this.announce();  
      refreshConsole();
    }
  }
  advancePlayer(i, newRoll){
    this.clearSpace();
    let newBoardPosition = (this.boardPosition + 1) % boardSize;
    this.boardPosition = newBoardPosition;
    this.spacesRemaining --;
    this.fillSpace();
    this.checkPosition(i, newRoll);
  }

  move(){
    confirm(`${die} ${this.name}'s turn ${die}`);
    if (Object.keys(this.inventory).length > 0) this.useItem();
    let newRoll = Player.rollDie();
    confirm(`${die} ${this.name} rolled a ${newRoll} ${die}`);
    this.spacesRemaining = newRoll;
    for (let i = 1; i <= newRoll; i ++){
      this.advancePlayer(i, newRoll);
    } 
  }
  
  offerStar(){
    if (this.coinBalance >= starCost){
      if (confirm(`${star} Congratulations, ${this.name}! You have reached the Star! Would you like to purchase it for ${starCost} coins? ${star}`)){
        this.coinBalance -= starCost;
        this.starBalance += 1;
        this.updateSpace();
        refreshConsole();
        confirm (`${star} ${this.name} has purchased a star! The board will be reset! ${star} `);
        board.randomize();
        refreshConsole();
        confirm (`The star is now at space ${board.starPosition}`)
      }
      else confirm ("Best of luck then!")
    }
    else confirm (`${star} You've reached the star, but you don't have enough coins to buy it yet! Keep playing! ${star}`)
  }
  offerItem(){
    if (this.coinBalance >= board.getMinimumItemCost() && Object.keys(this.inventory).length < 3){
      let playerWantsItem = confirm (`${item} You've found an item vendor. Would you like to purchase an Item? ${item}`);
      if (playerWantsItem){
        let validInputs = ["a", "b", "c"];
        let itemsForSale = Object.values(board.itemSet);
        let itemMenu ={}
        let playerInput;
        for (let i = 0; i < itemsForSale.length; i++){
          itemMenu[validInputs[i]] = itemsForSale[i];
        }
        function offerChoices(player){
          refreshConsole();
          playerInput = prompt(`Enter the letter of the Item you would like. Your choices are \n ${item} a: ${itemsForSale[0].name} ${coin} ${itemsForSale[0].cost} \n ${item} b: ${itemsForSale[1].name} ${coin} ${itemsForSale[1].cost} \n ${item} c: ${itemsForSale[2].name} ${coin} ${itemsForSale[2].cost}`);
          if (!validInputs.includes(playerInput)){
            refreshConsole();
            confirm ("Please input a valid letter");
            offerChoices(player);
          }
          else if(player.inventory[`${itemMenu[playerInput].name}`]){
            refreshConsole();
            confirm ("You already have one of those. Please choose something else!")
            offerChoices(player);
          }
        }
        offerChoices(this);
        if (validInputs.includes(playerInput) && !this.inventory[itemMenu[playerInput]] && this.coinBalance >= itemMenu[playerInput].cost){
          this.inventory[itemMenu[playerInput].name] = itemMenu[playerInput];
          this.coinBalance -= itemMenu[playerInput].cost;
          this.updateSpace();
          refreshConsole();
          confirm(`${item} ${this.name} bought: ${itemMenu[playerInput].name} ${item}`)
        }
        else if (this.coinBalance < itemMenu[playerInput].cost){
          confirm ("You do not have enough coins for that Item. Please choose another.")
          this.offerItem();
        } 
      }
      else confirm ("See you another time!");
    }
    else if (Object.keys(this.inventory).length ===3 ) confirm ("You've reached an item vendor, but you can't carry any more items!")
    else confirm("You've reached an item vendor, but you don't have enough coins to make a purchase! Come back later!");
  }
   
  useItem () {
    let wantsToUseItem = confirm ("Would you like to use an Item?");
    if (wantsToUseItem){
      let itemMenu = {};
      let playerInventory = Object.values(this.inventory);
      let playerInput;
      let validInputs = [];
      for (let i = 0; i < playerInventory.length; i ++){
        validInputs.push(numberToLetters(i));
        itemMenu[validInputs[i]] = playerInventory[i];
      }
      function offerChoices(){
        refreshConsole();
        if (playerInventory.length === 3){
          playerInput = prompt(`Enter the letter of the Item you would like to use. \n ${item} a: ${playerInventory[0].name} \n ${item} b: ${playerInventory[1].name}, \n ${item} c: ${playerInventory[2].name}`);
        }
        else if(playerInventory.length === 2){
          playerInput = prompt(`Enter the letter of the Item you would like to use. \n ${item} a: ${playerInventory[0].name} \n ${item} b: ${playerInventory[1].name}`);
        }
        else if(playerInventory.length === 1){
          playerInput = prompt(`Enter the letter of the Item you would like to use. \n ${item} a: ${playerInventory[0].name}`);
        }
        if(!validInputs.includes(playerInput)) {
          refreshConsole();
          confirm("Please enter a valid input!");
          offerChoices ();
        }
      }
      offerChoices();
      this.inventory[itemMenu[playerInput].name].use(player[`${this.playerNumber}`]);      
    }
    else confirm("Very well!");
  }  
}
//define Item Classes and Subclasses
class Item {
  constructor(){
    this.name = undefined;
    this.cost = undefined;
    this.effect = undefined;
    this.inventorySize = 1;
  }
}
class ExtraDie extends Item {
  constructor (){
    super();
    this.name = "Extra Die";
    this.cost = 10;
    this.effect = "Grants the player an additional Role";
    this.use = function (player){
      delete player.inventory["Extra Die"];
      refreshConsole();
      confirm(`${player.name} uses an extra die. They get to roll twice!`)
      player.updateSpace();
      refreshConsole();
      let extraRoll = Player.rollDie();
      confirm(`${die} ${player.name} rolled a ${extraRoll} ${die}`);
      player.spacesRemaining = extraRoll;
      for (let i = 1; i <= extraRoll; i ++){
        player.advancePlayer(i, extraRoll);
      } 
      confirm("Roll again!");
    }
  }
}
 
class PositionSwapper extends Item{
  constructor(){
    super();
    this.name = "Position Swapper";
    this.cost = 10;
    this.effect = "Shuffles the positions of all players";
    this.use = function (user){
      delete user.inventory["Position Swapper"];
      let choiceArray = [];
      let validInputs = [];
      let playerInput;
      for (let i = 1; i <= playerCount; i++){
        if(user.playerNumber !== i){
          choiceArray.push(`${player[i].playerNumber}: ${player[i].name}`);
          validInputs.push(`${player[i].playerNumber}`);
        }
      }
      user.updateSpace();
      refreshConsole();
      function askChoice(){
        refreshConsole();
        playerInput = prompt(`Enter the number of the player would you like to swap with? \n ${choiceArray}`);
        if(!validInputs.includes(playerInput)) {
          refreshConsole();
          confirm ("Please input a valid player number");
          askChoice();
        }
      }
      askChoice();
      let target = player[playerInput]
      let targetPosition = target.boardPosition;
      let userPosition = user.boardPosition;
      target.clearSpace();
      target.boardPosition = userPosition;
      target.fillSpace();
      user.clearSpace();
      user.boardPosition = targetPosition;
      user.fillSpace();
      refreshConsole();
      confirm(`${user.name} and ${target.name} have switched spaces!`);
    }
  }
}
class CoinSnatcher extends Item{
  constructor(){
    super();
    this.name = "Coin Snatcher";
    this.cost = 10;
    this.effect = "Steals 10 coins from each other player";
    this.use = function (user){
      let initialCoinBalance = user.coinBalance
      delete user.inventory["Coin Snatcher"];
      let loot = Player.subtractFromAll(10);
      user.coinBalance += loot;
      user.updateSpace();
      refreshConsole();
      confirm(`${user.name} steals ${user.coinBalance - initialCoinBalance} coins!`);
    }
  }
}


//Define Board class
class GameBoard{
  constructor (numStars, numEvents, numVendors){
    this.starCount = numStars;
    this.eventCount = numEvents;
    this.vendorCount = numVendors;
    this.space = {};
    //define items for the board
    this.itemSet = {
      "Extra Die" : new ExtraDie(),
      "Position Swapper" : new PositionSwapper(),
      "Coin Snatcher" : new CoinSnatcher(),
    }
  }
  //define static methods
  static initializePlayers(){
    let playerCount;
    function askPlayerCount(){
      playerCount = prompt("Enter the number of players (2 minimum)")
      if (playerCount < 2 || playerCount % 1 !== 0){
        confirm ("Please enter a valid number of players")
        askPlayerCount();
      } 
    }
    askPlayerCount();
    let player = {};
    let playerInputs = [];
    for(let i = 1; i <= playerCount; i++){
      let newPlayerName = prompt("Enter the name of Player " + i);
      playerInputs.forEach((ele, index) => {
        if (ele.includes(newPlayerName) || newPlayerName.includes(ele)){
          confirm("Please make sure that no player's name includes anothers.");
          GameBoard.initializePlayers();
        }
      })
      playerInputs.push(newPlayerName);
      player[i] = new Player(newPlayerName);
      player[i].playerNumber = i;
    }
  return player;     
  }   
  static setGameLength(){
    let gameLength;
    function askGameLength(){
      let playerInput = prompt("Enter the number of rounds would you like to play (1 minimum)");
      if (playerInput < 1 || playerInput % 1 !== 0){
        confirm ("Please enter a valid number of rounds");
        askGameLength();
      }
      else gameLength = playerInput;
    }
    askGameLength();
    return gameLength;
  }
  static createBoard(numStars, numEvents, numItems){
    let newBoard = new GameBoard (numStars,numEvents,numItems);
    newBoard.randomize();
    return newBoard;
  }  
  static initializeBoardState(){
    Object.keys(player).forEach(playerNumber =>{
      board.space[0].push(`${player[playerNumber].name} ${coin}: ${player[playerNumber].coinBalance} ${star}: ${player[playerNumber].starBalance} ${item} ${Object.keys(player[playerNumber].inventory).length}`);
    });
  }
  static closestGuessContest = function (){
    console.log(board.space);
    confirm (`${miniGame} All players deposit 5 coins and guess a number from 1 to 100. Winner takes all! ${miniGame} `)
    let prizePool = Player.subtractFromAll(5);
     for (let i = 1; i <= playerCount; i++){
      player[i].updateSpace();
    }
    refreshConsole("event");
    let target = Math.ceil(Math.random() * 100);
    let guessArray = [];
    let guessArrayValidity = true;
    let playerInput;
    function takeGuesses(){
      for(let i = 1; i <= playerCount; i++){
        playerInput = prompt(`${player[i].name}, enter your guess:`);
        guessArray.push(playerInput);
        for (let j = 0; j < guessArray.length; j++){
          if (guessArray[j] < 1 || guessArray[j] > 100 || playerInput % 1 !==0) guessArrayValidity = false;
        }
        if(!guessArrayValidity){
        guessArray =[];
        guessArrayValidity = true;
        confirm("Please enter a valid number");
        takeGuesses();
        break;
        }
      }  
    }
    takeGuesses();
    let smallestDifference = Infinity;
    let winningIndex;
    guessArray.forEach((ele, index) => {
      if (Math.abs(target - ele) < smallestDifference){
        smallestDifference = Math.abs(target - ele);
        winningIndex = index +1 ;
      }
    });
    
    confirm (`${miniGame} The number was ${target}. ${player[winningIndex].name} wins ${prizePool} coins! ${miniGame}`);
    player[`${winningIndex}`].increaseBalance(prizePool);
    for (let i = 1; i <= playerCount; i++){
      player[i].updateSpace();
    }
    refreshConsole("event");
  }
  static randomizePositions(){
    confirm (`Players positions will be shuffled!`)
    let positionArray = [];
    for (let i = 1; i <= playerCount; i++){
      positionArray.push(player[i].boardPosition);
    }
    let shuffledPositions = shuffleArray(positionArray);
    for (let i = 1; i <= playerCount; i++){
      player[i].clearSpace();
      player[i].boardPosition = shuffledPositions[i - 1];
      player[i].fillSpace();
    }
    refreshConsole();
  }
  static coinGiveaway (){
    confirm(`Everyone gets 10 coins!`);
    for (let i = 1; i <= playerCount; i++){
      player[i].increaseBalance(10);
      player[i].updateSpace();
    }
    refreshConsole();
  }
   static coinTakeaway (){
    confirm(`Everyone loses 10 coins!`);
    for (let i = 1; i <= playerCount; i++){
      player[i].decreaseBalance(10);
      player[i].updateSpace();
      refreshConsole();
    }
  }
  static chooseRandomEvent(){
    confirm (`${event}${event}${event} Event Time! ${event}${event}${event}`)
    let randInt = Math.ceil(Math.random()*100);
    if (randInt <= 50) GameBoard.randomizePositions();
    else if (randInt <= 80) GameBoard.coinGiveaway();
    else GameBoard.coinTakeaway();
  }
  static declareWinner(){
    let winner;
    let winnerStarCount = -Infinity;
    let winnerCoinBalance = -Infinity;
    for (let i = 1; i <= playerCount; i++){
      if(player[i].starBalance > winnerStarCount){
        winnerStarCount = player[i].starBalance
        winnerCoinBalance = player[i].coinBalance;
        winner = player[i];
      }
      else if(player[i].starBalance === winnerStarCount && player[i].coinBalance > winnerCoinBalance){
        winnerCoinBalance = player[i].coinBalance;
        winner = player[i];
      }
    }
    separator(trophy, 12,`${winner.name} has won the game! Congratulations!`);
  }
  //define instance methods
  randomize (){
    //calculate number of each kind of space
    let numStars = this.starCount;
    let numEvents = this.eventCount;
    let numVendors = this.vendorCount;
    let numBlue = Math.floor((boardSize - numStars - numEvents - numVendors) * (3 / 4));
    let numRed = boardSize - numStars - numEvents - numVendors -numBlue;
    //create input arrays for populateArray
    let spaceDistribution = [numStars, numEvents,numVendors, numBlue, numRed];
    let spaceLabels = [star, event, item, blue, red]
    //create and shuffle spaceArr
    let spaceArr = shuffleArray(populateArray(spaceDistribution,spaceLabels));
    //make space object from spaceArr
    spaceArr.forEach((element, index) => {
      if (this.space[index]) this.space[index][0] = element;
      else this.space[index] = [element];
    });
    this.getStarLocation();
  }
  getStarLocation(){
    for (let key in this.space){
        if (this.space[key][0].includes(`${star}`)){
        this.starPosition = key;
       }
    }
  }
  getMinimumItemCost(){
    let minimumItemCost = Infinity;
    for (let item in this.itemSet){
      if (this.itemSet[item].cost < minimumItemCost) {
        minimumItemCost = this.itemSet[item].cost; 
      }
    }
    this.minimumItemCost = minimumItemCost;
    return minimumItemCost;
  }
}
//useful helper functions
function shuffleArray (array){
  let output = [];
  function shuffler (array){
    if (array.length === 0) return output;
    let randInt = Math.floor(array.length * Math.random());
    output.push(array[randInt]);
    array.splice(randInt, 1);
    return shuffler(array);
  }
  return shuffler(array);
}
function populateArray (countArr, labelArr){
  let output = [];
  for (let i = 0; i < countArr.length; i++){
    for (let j = 0; j < countArr[i]; j++){
      output.push(labelArr[i])
    }
  }
  return output;
}
function numberToLetters(num) {
    let letters = ''
    while (num >= 0) {
        letters = 'abcdefghijklmnopqrstuvwxyz'[num % 26] + letters
        num = Math.floor(num / 26) - 1
    }
    return letters
}
function separator (character, count, message){
  output = character.repeat(count) + " " + message + " " + character.repeat(count);
  console.log(output);
}
function refreshConsole(phase){
  if (phase === "event"){
    console.clear();
    let message = "Event " + currentRound + `/${rounds}`;
    separator(miniGame, 20, message);
  }
  else{
    console.clear();
    let message = "Round " + currentRound + `/${rounds}`;
    separator(sparkle, 20, message);
  }
  console.log(board.space);
}

//set global variables
const red = `\u{1f534}`;
const blue = `\u{1f535}`;
const star = `\u{2b50}`;
const item = `\u{1f36c}`;
const event = `\u{2757}`;
const coin = `\u{1f4b0}`;
const die = ` \u{1f3b2}`;
const miniGame = `\u{26A1}`;
const boom = `\u{1f4a5}`;
const sparkle = `\u{2728}`;
const party = `\u{1f38a}`;
const check = `\u{2705}`;
const trophy = `\u{1F3c6}`
const boardSize = 30; //spaces are numbered from 0 up to boardSize - 1
const dieSize = 10; //maximum possible roll
const delay = 300;
const starCost = 30;
const blueReward = 5;
const redPenalty = 3;
const startingCoinBalance = 10;
const numStars = 1;
const numEvents = 3;
const numVendors = 2;

separator(party, 20, "Welcome to JS Party");
const board = GameBoard.createBoard(numStars, numEvents, numVendors);
const player = GameBoard.initializePlayers();
const rounds = GameBoard.setGameLength(); 
const playerCount = Object.keys(player).length;
const consent = confirm ("Ready? " +check);



//main game function
function beginGame (){
  GameBoard.initializeBoardState();
  for (let i = 1; i <= rounds; i++){
    currentRound = i;
    for (let i =1; i <= playerCount; i ++){
      refreshConsole();
      player[i].move();
    }
    confirm(`${miniGame} Mini Game Time! ${miniGame}`);
    console.clear();
    separator(`${miniGame}`, 20, "Event " + i + `/${rounds}`);
    GameBoard.closestGuessContest();
  }
  refreshConsole();
  GameBoard.declareWinner();
}
//start game
if (consent){
  beginGame();
}
else alert("OK...See you later!")
