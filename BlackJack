import java.util.ArrayList;
import java.awt.event.*;
import javax.swing.*;
import java.awt.Font;
import java.io.FileInputStream;
import java.io.*;

public class BlackJack {
    ArrayList<Card> dealerHand; //this is the arraylist for the dealer's hand.
    ArrayList<Card> playerHand; //this is the arraylist for the player's hand.

    public boolean faceDown; //this boolean value will tell the program if have the card facedown or faceup.
    public boolean dealerWon; //this boolean value will tell the program if dealer won the round.
    public volatile boolean roundOver; //this boolean value will tell the program if the round is over.
    //added keyword volatile because: "To ensure that changes made by one thread are visible to other
    //threads you must always add some synchronization between the threads.
    //The simplest way to do this is to make the shared variable volatile.

    JFrame frame; //create a JFrame.
    Deck deck; //have our deck.
    Component atmosphereComponent; //Two GameComponents: one for the background images, buttons, and the overall atmosphere.
    Component cardComponent; //Other for the cards printing out.

    JButton btnHit; //have two buttons in this game.
    JButton btnStand;
    JButton btnExit;

    public BlackJack(JFrame f) {//with this constructor, initialize the instant fields accordingly.
        // This constructor gets a JFrame as a parameter.
        deck = new Deck();
        deck.shuffleDeck(); //randomize the deck.
        dealerHand = new ArrayList<Card>();
        playerHand = new ArrayList<Card>();
        atmosphereComponent = new Component(dealerHand, playerHand);
        frame = f;
        faceDown = true;
        dealerWon = true;
        roundOver = false;
    }
    public void formGame() {//this method will help to create the background of the game.
        System.out.println("GAME FORMED");
        frame.setTitle("BLACKJACK!"); //make the title of the JFrame 'BLACKJACK!'
        frame.setSize(1130, 665);
        frame.setLocationRelativeTo(null); //this centers the JFrame on the screen.
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setResizable(false); //make it impossible for the computer user to resize the Frame.

        btnHit = new JButton("HIT");
        btnHit.setBounds(390, 550, 100, 50); //set their bounds.
        btnHit.setFont(new Font("Comic Sans MS", Font.BOLD, 16));
        btnStand = new JButton("STAND");
        btnStand.setBounds(520, 550, 100, 50);
        btnStand.setFont(new Font("Comic Sans MS", Font.BOLD, 16));
        btnExit = new JButton("EXIT CASINO");
        btnExit.setBounds(930, 240, 190, 50);
        btnExit.setFont(new Font("Comic Sans MS", Font.BOLD, 16));

        frame.add(btnHit); //add the buttons the JFrame
        frame.add(btnStand);
        frame.add(btnExit);

        btnExit.addActionListener(new ActionListener() { //add an action listener to the exit button.
            public void actionPerformed(ActionEvent e) {
                JOptionPane.showMessageDialog(frame, "You have left " +
                        "the casino with " +  BlackJackGame.currentBalance + ".");
                //print out the message by getting our current balance from the Tester class.
                System.exit(0); //exit the program.
            }
        });

        atmosphereComponent = new Component(dealerHand, playerHand);
        //initialize the GameComponent that will be the overall atmosphere of the game.
        atmosphereComponent.setBounds(0, 0, 1130, 665);
        frame.add(atmosphereComponent);
        frame.setVisible(true);
    }
    public void startGame() {
        //this method starts the game: the cards are drawn and are printed out.
        for(int i = 0; i<2; i++) {
            //add the first two cards on top of the deck to dealer's hand.
            dealerHand.add(deck.getCard(i));
        }
        for(int i = 2; i<4; i++) {
            //add the third and fourth card on top of the deck to the player's hand.
            playerHand.add(deck.getCard(i));
        }
        for (int i = 0; i < 4; i++) { //then remove these cards from the game.
            // This way, literally 'drew' the cards and those four cards are
            // no longer in the deck.
            deck.removeCard(0);
        }
        cardComponent = new Component(dealerHand, playerHand);
        cardComponent.setBounds(0, 0, 1130, 665);
        frame.add(cardComponent);
        frame.setVisible(true);

        checkHand(dealerHand);
        checkHand(playerHand);

        btnHit.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                addCard(playerHand); //will first add a card to player's hand.
                checkHand(playerHand); //then check the player's hand because it could be round over.
                if (getSumOfHand(playerHand)<17 && getSumOfHand(dealerHand)<17){
                    //if the round is not over, and if the total value of dealer's
                    // hand is smaller than 17, add a card to dealer's hand.
                    addCard(dealerHand);
                    checkHand(dealerHand);
                    //as usual, check his hand for any potential round over situation.
                }
            }
        });
        btnStand.addActionListener(new ActionListener() {
            public void actionPerformed(ActionEvent e) {
                while (getSumOfHand(dealerHand)<17) { //if it is appropriate, the dealer draws a card.
                    addCard(dealerHand);
                    checkHand(dealerHand);
                }
                if ((getSumOfHand(dealerHand)<21) && getSumOfHand(playerHand)<21) {
                    //if both hands didn't reach 21, check which hand is better
                    // and print out the result.
                    if(getSumOfHand(playerHand) > getSumOfHand(dealerHand)) {
                        faceDown = false;
                        dealerWon = false;
                        JOptionPane.showMessageDialog(frame, "PLAYER HAS WON BECAUSE OF A BETTER HAND!");
                        rest();
                        roundOver = true;
                    }
                    else {
                        faceDown = false;
                        JOptionPane.showMessageDialog(frame, "DEALER HAS WON BECAUSE OF A BETTER HAND!");
                        rest();
                        roundOver = true;
                    }
                }
            }
        });
    }
    public void checkHand (ArrayList<Card> hand) {
        //this method literally checks the hand for a blackjack or bust.
        if (hand.equals(playerHand)) {
            //checks if the parameter is player's hand.
            if(getSumOfHand(hand) == 21){
                //if it is 21, player has done blackjack and the game
                // is over.
                faceDown = false;
                dealerWon = false; //set it to false because user won.
                JOptionPane.showMessageDialog(frame, "PLAYER HAS DONE BLACKJACK! PLAYER HAS WON!");
                rest();
                roundOver = true;
            }
            else if (getSumOfHand(hand) > 21) {
                //if it is bigger than 21, then the player hand has busted,
                // dealer has won.
                faceDown = false; JOptionPane.showMessageDialog(frame, "PLAYER HAS BUSTED! DEALER HAS WON!");
                rest();
                roundOver = true;
            }
        }
        else { //else condition checks if it is dealer's hand.
            if(getSumOfHand(hand) == 21) {
                //basically look for the same things looked for the player's hand.
                faceDown = false;
                JOptionPane.showMessageDialog(frame, "DEALER HAS DONE BLACKJACK! DEALER HAS WON!");
                rest();
                roundOver = true;
            }
            else if (getSumOfHand(hand) > 21) {
                faceDown = false;
                dealerWon = false;
                JOptionPane.showMessageDialog(frame, "DEALER HAS JUST BUSTED! PLAYER HAS WON!");
                rest();
                roundOver = true;
            }
        }
    }

    public void addCard(ArrayList<Card> hand) {//this method adds a card to the hand.
        hand.add(deck.getCard(0)); //gets a card from the deck to the hand.
        deck.removeCard(0); //removes the card from the deck.
        faceDown = true;
    }

    public boolean hasAceInHand(ArrayList<Card> hand) {//this method checks if the hand has ace.
        for (int i = 0; i < hand.size(); i++){
            //go through the hand that is given as a parameter and check
            // for a card with a value of 11(Ace.)
            if(hand.get(i).getValue() == 11) {
                return true; //return true if there is any.
            }
        }
        return false;
    }

    public int aceCountInHand(ArrayList<Card> hand){//this method finds the total aces found in the hand.
        int aceCount = 0; //initialize an integer which will store the total ace count as 0.
        for (int i = 0; i < hand.size(); i++) { //go through the hand.
            if(hand.get(i).getValue() == 11) { //each time see a card with a value of 11,
                aceCount++;
            }
        }
        return aceCount;
    }
    public int getSumWithHighAce(ArrayList<Card> hand) {//this method gives the total value of the hand where the ace is counted as having a value of 11.
        int handSum = 0; //initialize the integer in which the sum of hand is stored.
        for (int i = 0; i < hand.size(); i++){
            handSum = handSum + hand.get(i).getValue(); //add the values
            // encounter to the integer.
        }
        return handSum;
    }

    public int getSumOfHand (ArrayList<Card> hand) {//this method gives you the sum of the hand, for all cases.
        if(hasAceInHand(hand)) { //if there is an ace,
            if(getSumWithHighAce(hand) <= 21) {
                return getSumWithHighAce(hand); // get the sum with the high ace
                // case (taking aces as 11) if the total sum is smaller than 21 and
                // return this sum.
            }
            else{
                for (int i = 0; i < aceCountInHand(hand); i++) { //if take aces as
                    // 11 and the total value exceeds 21,
                    // then do some calculation to count ace as 1.
                    int sumOfHand = getSumWithHighAce(hand)-(i+1)*10;
                    //for each ace there is in hand, subtract then to get 1 from 11.
                    if(sumOfHand <= 21) {
                        return sumOfHand; //return this sum if it is smaller than 21.
                    }
                }
            }
        }
        else { //if there is no ace in hand, will directly go through
            // the hand and sum up all of the values.
            int sumOfHand = 0;
            for (int i = 0; i < hand.size(); i++) {
                sumOfHand = sumOfHand + hand.get(i).getValue();
            }
            return sumOfHand;
        }
        return 22; //basically set it to the 'bust' case if the method returns nothing up to this point.
    }
    public static void rest() {//this method sleeps the program.
        // It basically serves as a time duration between events.
        try {
            Thread.sleep(500);//this sleeps the program for 1000
            // miliseconds which is equal to 1 second.
        }
        catch (InterruptedException e) {}
    }
}
