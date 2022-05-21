const chai = require('chai');
const expect = chai.expect;
const Card = require('../src/Card');
const Turns = require('../src/Turns');

describe('Turns', () => {

    it('should be a function', () => {
        expect(Turns).to.be.a('function');
    });

    it('should instantiate of Turns', () => {
        const turns = new Turns();
        expect(turns).to.be.an.instanceof(Turns);
    });

    it('should have a user’s guess to the question and object for the current card in play', () => {
        const card = new Card(1,"What allows you to define a set of related information using key-value pairs?",["object", "array", "function"],"object"); //instance of card
        const turns = new Turns("array", card);
        expect(turns.guess).to.be.a('string');
        expect(turns.guess).to.equal("array");
        expect(turns.currentCard).to.be.an('object');

    });

    it('it should return the guess', () => {
        const card = new Card(1,"What allows you to define a set of related information using key-value pairs?",["object", "array", "function"],"object"); //instance of card
        const turns = new Turns("array", card);

        turns.returnGuess();

        expect(turns.returnGuess()).to.equal("array")

      });

      it('it should be able to return the current card', () =>{
        const card = new Card;
        const turns = new Turns('guess',card)

        turns.returnCard();

        expect(turns.returnCard()).to.equal(card);

      })

      it('should be able to return a boolean indicating if the user’s guess matches the correct answer on the card', () => {
        const card = new Card(1,"What allows you to define a set of related information using key-value pairs?",["object", "array", "function"],"object"); //instance of card
        const turns1 = new Turns("object", card);
        const turns2 = new Turns("array",card);

        turns1.evaluateGuess();
        turns2.evaluateGuess();

        expect(turns1.evaluateGuess()).to.equal(true);
        expect(turns2.evaluateGuess()).to.equal(false);

      })

      it('should tell if the guess is correct or incorrect!', () => {
        const card = new Card(1,"What allows you to define a set of related information using key-value pairs?",["object", "array", "function"],"object"); //instance of card
        const turns1 = new Turns("object", card);
        const turns2 = new Turns("array",card);

        turns1.evaluateGuess();
        turns1.giveFeedback();
        turns2.evaluateGuess();
        turns2.giveFeedback();

        expect(turns1.giveFeedback()).to.equal('correct!');
        expect(turns2.giveFeedback()).to.equal('incorrect!');

      })
})










const chai = require("chai");
const expect = chai.expect;
const Round = require("../src/Round");
const Deck = require("../src/Deck");
const Card = require("../src/Card");
const Turns = require("../src/Turns");

describe("Round", () => {
  let card1, card2, card3, card4, deck, round, turn;

  beforeEach(() => {
    card1 = new Card(
      1,
      "What is Robbie's favorite animal",
      ["sea otter", "pug", "capybara"],
      "sea otter"
    );
    card2 = new Card(
      14,
      "What organ is Khalid missing?",
      ["spleen", "appendix", "gallbladder"],
      "gallbladder"
    );
    card3 = new Card(
      12,
      "What is Travis's favorite stress reliever?",
      ["listening to music", "watching Netflix", "playing with bubble wrap"],
      "playing with bubble wrap"
    );
    card4 = new Card(
      3,
      "What is Betul's pets' names?",
      ["Togo-Lily", "Daisy-Lucy", "Sun-Moon"],
      "Togo-Lily"
    );

    deck = new Deck([card1, card2, card3, card4]);
    round = new Round(deck);
    // turn = new Turns(guess, currentCard);
  });

  it("should be a function", () => {
    expect(Round).to.be.a("function");
  });

  it("should store a Deck", () => {
    expect(round.deck.cards).to.be.an("array");
    expect(round.deck).to.equal(deck);
  });

  it("should be able to return the current card", () => {
    expect(round.returnCurrentCard()).to.equal(deck.cards[0]);
  });

  it("should updates turns count", () => {
    round.takeTurn();
    round.takeTurn();

    expect(round.turns).to.equal(2);
  });

  it("should have an incorrect guesses", () => {
    round.takeTurn("Fritz");

    expect(round.incorrectGuesses).to.be.an("array");
  });

  it("should store incorrect guess", () => {
    round.takeTurn("Fritz");

    expect(round.incorrectGuesses).to.deep.equal([1]);
  });

  it("should evaluate the current guess", () => {
    round.takeTurn("sea otter");

    expect(round.takeTurn("sea otter")).to.equal("correct!");
  });

  it("should evaluate the current guess if it is incorrect", () => {
    round.takeTurn("spleen");

    expect(round.takeTurn("spleen")).to.equal("incorrect!");
  });

  it.skip("should be able to calculate the percentage of correct guesses", () => {
    round.takeTurn("capybara");
    round.takeTurn("appendix");
    expect(round.calculatePercentCorrect()).to.equal(0);

    round.takeTurn("playing with bubble wrap");
    round.takeTurn("Togo-Lily");

    expect(round.calculatePercentCorrect()).to.equal(50);
  });

  it('should be able to game over', () => {
    round.takeTurn("capybara");
    round.takeTurn("appendix");
    round.takeTurn("playing with bubble wrap");
    round.takeTurn("Togo-Lily");
    expect(round.endRound()).to.equal("** Round over! ** You answered 50% of the questions correctly!")
  })
});





const chai = require("chai");
const expect = chai.expect;

const Card = require("../src/Card");

describe("Card", function () {
  let card;

  beforeEach(() => {
    card = new Card(
      1,
      "What allows you to define a set of related information using key-value pairs?",
      ["object", "array", "function"],
      "object"
    );
  })
  it("should be a function", function () {
    expect(Card).to.be.a("function");
  });

  it("should be an instance of Card", function () {
    expect(card).to.be.an.instanceof(Card);
  });

  it("should store a question", function () {
    expect(card.question).to.equal(
      "What allows you to define a set of related information using key-value pairs?"
    );
  });

  it("should store a list of possible answers", function () {
    expect(card.answers).to.deep.equal(["object", "array", "function"]);
  });

  it("should store the correct answer", function () {
    expect(card.correctAnswer).to.equal("object");
  });
});





const chai = require('chai');
const expect = chai.expect;
const Deck = require('../src/Deck');
const Card = require('../src/Card');

describe('Deck', () => {
    let card1, card2, card3, card4, deck;

    beforeEach(() => {
        card1 = new Card(1, 'What is Robbie\'s favorite animal', ['sea otter', 'pug', 'capybara'], 'sea otter');
        card2 = new Card(14, 'What organ is Khalid missing?', ['spleen', 'appendix', 'gallbladder'], 'gallbladder');
        card3 = new Card(12, 'What is Travis\'s middle name?', ['Lex', 'William', 'Fitzgerald'], 'Fitzgerald');
        card4 = new Card(3,'What is Betul\'s pets\' names?', ['Togo-Lily', 'Daisy-Lucy', 'Sun-Moon'], 'Togo-Lily');

        deck = new Deck([card1, card2, card3, card4]);
    })
    it('should be a function', () => {
        expect(Deck).to.be.a('function');
    });

    it('should be initialized with an array of Card objects', () => {

        expect(deck.cards).to.be.an('array');
        expect(deck.cards).to.deep.equal([card1, card2, card3, card4]);
    })

    it('should know how many Cards are in the Deck', () => {

        expect(deck.countCards()).to.equal(4);
    })

});
