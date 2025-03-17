# Rock Paper Scissors

**Difficulty**: Easy  
**Tags**: Game, Design, Simulation

## Question
Implement a Rock Paper Scissors game where a player can play against the computer. Rock Paper Scissors is a hand game usually played between two people, where each player simultaneously forms one of three shapes with their hand:

- Rock (represented by a closed fist)
- Paper (represented by an open hand)
- Scissors (represented by a fist with the index and middle finger extended)

The rules are:
- Rock beats Scissors
- Scissors beats Paper
- Paper beats Rock
- If both players choose the same shape, it's a tie

Your implementation should include:

1. A method to get the player's choice.
2. A method to generate the computer's choice randomly.
3. A method to determine the winner based on the choices.
4. A method to keep track of the score.

## Example Input/Output
```
Player chooses: Rock
Computer chooses: Scissors
Result: Player wins!

Player chooses: Paper
Computer chooses: Paper
Result: It's a tie!

Player chooses: Scissors
Computer chooses: Rock
Result: Computer wins!

Current score: Player 1 - 1 Computer
```

## Solution Algorithm
To implement the Rock Paper Scissors game, we need to:

1. Define the possible moves (Rock, Paper, Scissors).
2. Get the player's move.
3. Generate a random move for the computer.
4. Compare the moves according to the game rules to determine the winner.
5. Update and display the score.

## Solution Code
```java
import java.util.Random;
import java.util.Scanner;

public class RockPaperScissors {
    private static final String ROCK = "ROCK";
    private static final String PAPER = "PAPER";
    private static final String SCISSORS = "SCISSORS";
    
    private int playerScore;
    private int computerScore;
    private Random random;
    
    public RockPaperScissors() {
        playerScore = 0;
        computerScore = 0;
        random = new Random();
    }
    
    /**
     * Get the computer's move.
     * @return The computer's move (Rock, Paper, or Scissors).
     */
    public String getComputerMove() {
        int randomNum = random.nextInt(3);
        switch (randomNum) {
            case 0:
                return ROCK;
            case 1:
                return PAPER;
            default:
                return SCISSORS;
        }
    }
    
    /**
     * Determine the winner based on the player's and computer's moves.
     * @param playerMove The player's move.
     * @param computerMove The computer's move.
     * @return The result of the game (Player wins, Computer wins, or Tie).
     */
    public String determineWinner(String playerMove, String computerMove) {
        if (playerMove.equals(computerMove)) {
            return "It's a tie!";
        }
        
        if ((playerMove.equals(ROCK) && computerMove.equals(SCISSORS)) ||
            (playerMove.equals(PAPER) && computerMove.equals(ROCK)) ||
            (playerMove.equals(SCISSORS) && computerMove.equals(PAPER))) {
            playerScore++;
            return "Player wins!";
        } else {
            computerScore++;
            return "Computer wins!";
        }
    }
    
    /**
     * Get the current score.
     * @return The current score as a string.
     */
    public String getScore() {
        return "Current score: Player " + playerScore + " - " + computerScore + " Computer";
    }
    
    /**
     * Reset the score.
     */
    public void resetScore() {
        playerScore = 0;
        computerScore = 0;
    }
    
    /**
     * Validate the player's move.
     * @param move The player's move.
     * @return true if the move is valid, false otherwise.
     */
    public boolean isValidMove(String move) {
        return move.equals(ROCK) || move.equals(PAPER) || move.equals(SCISSORS);
    }
    
    public static void main(String[] args) {
        RockPaperScissors game = new RockPaperScissors();
        Scanner scanner = new Scanner(System.in);
        
        System.out.println("Welcome to Rock Paper Scissors!");
        System.out.println("Enter your move (ROCK, PAPER, or SCISSORS) or 'QUIT' to exit:");
        
        while (true) {
            System.out.print("\nYour move: ");
            String playerMove = scanner.nextLine().toUpperCase();
            
            if (playerMove.equals("QUIT")) {
                break;
            }
            
            if (!game.isValidMove(playerMove)) {
                System.out.println("Invalid move. Please enter ROCK, PAPER, or SCISSORS.");
                continue;
            }
            
            String computerMove = game.getComputerMove();
            
            System.out.println("Player chooses: " + playerMove);
            System.out.println("Computer chooses: " + computerMove);
            
            String result = game.determineWinner(playerMove, computerMove);
            System.out.println("Result: " + result);
            
            System.out.println(game.getScore());
        }
        
        System.out.println("\nFinal score: " + game.getScore());
        System.out.println("Thanks for playing!");
        
        scanner.close();
    }
}
```

```java
// Alternative implementation with an enum for moves
import java.util.Random;
import java.util.Scanner;

public class EnhancedRockPaperScissors {
    
    enum Move {
        ROCK, PAPER, SCISSORS;
        
        /**
         * Get a random move.
         */
        public static Move getRandomMove() {
            Random random = new Random();
            return values()[random.nextInt(values().length)];
        }
        
        /**
         * Check if this move beats the other move.
         */
        public boolean beats(Move other) {
            return (this == ROCK && other == SCISSORS) ||
                   (this == PAPER && other == ROCK) ||
                   (this == SCISSORS && other == PAPER);
        }
    }
    
    private int playerScore;
    private int computerScore;
    private int ties;
    
    public EnhancedRockPaperScissors() {
        playerScore = 0;
        computerScore = 0;
        ties = 0;
    }
    
    /**
     * Play a round of Rock Paper Scissors.
     * @param playerMove The player's move.
     * @return The result of the round.
     */
    public String playRound(Move playerMove) {
        Move computerMove = Move.getRandomMove();
        
        String result;
        if (playerMove == computerMove) {
            result = "It's a tie!";
            ties++;
        } else if (playerMove.beats(computerMove)) {
            result = "Player wins!";
            playerScore++;
        } else {
            result = "Computer wins!";
            computerScore++;
        }
        
        return "Player chooses: " + playerMove + 
               "\nComputer chooses: " + computerMove + 
               "\nResult: " + result;
    }
    
    /**
     * Get the current score.
     * @return The current score as a string.
     */
    public String getScore() {
        return "Player: " + playerScore + " | Computer: " + computerScore + " | Ties: " + ties;
    }
    
    /**
     * Get the game statistics.
     * @return The game statistics as a string.
     */
    public String getStatistics() {
        int totalGames = playerScore + computerScore + ties;
        if (totalGames == 0) {
            return "No games played yet.";
        }
        
        double playerWinPercentage = (double) playerScore / totalGames * 100;
        double computerWinPercentage = (double) computerScore / totalGames * 100;
        double tiePercentage = (double) ties / totalGames * 100;
        
        return String.format("Games played: %d\nPlayer win rate: %.1f%%\nComputer win rate: %.1f%%\nTie rate: %.1f%%",
                            totalGames, playerWinPercentage, computerWinPercentage, tiePercentage);
    }
    
    public static void main(String[] args) {
        EnhancedRockPaperScissors game = new EnhancedRockPaperScissors();
        Scanner scanner = new Scanner(System.in);
        
        System.out.println("Welcome to Rock Paper Scissors!");
        System.out.println("Enter your move (ROCK, PAPER, or SCISSORS) or 'QUIT' to exit:");
        
        while (true) {
            System.out.print("\nYour move: ");
            String input = scanner.nextLine().toUpperCase();
            
            if (input.equals("QUIT")) {
                break;
            }
            
            Move playerMove;
            try {
                playerMove = Move.valueOf(input);
            } catch (IllegalArgumentException e) {
                System.out.println("Invalid move. Please enter ROCK, PAPER, or SCISSORS.");
                continue;
            }
            
            String roundResult = game.playRound(playerMove);
            System.out.println(roundResult);
            System.out.println("Score: " + game.getScore());
        }
        
        System.out.println("\nGame over!");
        System.out.println(game.getStatistics());
        System.out.println("Thanks for playing!");
        
        scanner.close();
    }
}
```

```java
// Interactive implementation with a graphical representation
import java.util.Random;
import java.util.Scanner;

public class GraphicalRockPaperScissors {
    
    enum Move {
        ROCK, PAPER, SCISSORS;
        
        /**
         * Get a random move.
         */
        public static Move getRandomMove() {
            Random random = new Random();
            return values()[random.nextInt(values().length)];
        }
        
        /**
         * Get the ASCII art representation of the move.
         */
        public String getAsciiArt() {
            switch (this) {
                case ROCK:
                    return  "    _______\n" +
                            "---'   ____)\n" +
                            "      (_____)\n" +
                            "      (_____)\n" +
                            "      (____)\n" +
                            "---.__(___)\n";
                case PAPER:
                    return  "    _______\n" +
                            "---'   ____)____\n" +
                            "          ______)\n" +
                            "          _______)\n" +
                            "         _______)\n" +
                            "---.__________)\n";
                case SCISSORS:
                    return  "    _______\n" +
                            "---'   ____)____\n" +
                            "          ______)\n" +
                            "       __________)\n" +
                            "      (____)\n" +
                            "---.__(___)\n";
                default:
                    return "";
            }
        }
        
        /**
         * Check if this move beats the other move.
         */
        public boolean beats(Move other) {
            return (this == ROCK && other == SCISSORS) ||
                   (this == PAPER && other == ROCK) ||
                   (this == SCISSORS && other == PAPER);
        }
    }
    
    private int playerScore;
    private int computerScore;
    private int ties;
    private int roundsPlayed;
    
    public GraphicalRockPaperScissors() {
        playerScore = 0;
        computerScore = 0;
        ties = 0;
        roundsPlayed = 0;
    }
    
    /**
     * Play a round of Rock Paper Scissors.
     * @param playerMove The player's move.
     * @return The result of the round.
     */
    public String playRound(Move playerMove) {
        Move computerMove = Move.getRandomMove();
        roundsPlayed++;
        
        System.out.println("\nRound " + roundsPlayed + ":");
        System.out.println("Player chooses: " + playerMove);
        System.out.println(playerMove.getAsciiArt());
        
        System.out.println("Computer chooses: " + computerMove);
        System.out.println(computerMove.getAsciiArt());
        
        String result;
        if (playerMove == computerMove) {
            result = "It's a tie!";
            ties++;
        } else if (playerMove.beats(computerMove)) {
            result = "Player wins!";
            playerScore++;
        } else {
            result = "Computer wins!";
            computerScore++;
        }
        
        return "Result: " + result;
    }
    
    /**
     * Get the current score.
     * @return The current score as a string.
     */
    public String getScore() {
        return "Score: Player " + playerScore + " | Computer " + computerScore + " | Ties " + ties;
    }
    
    /**
     * Get the game statistics.
     * @return The game statistics as a string.
     */
    public String getStatistics() {
        if (roundsPlayed == 0) {
            return "No games played yet.";
        }
        
        double playerWinPercentage = (double) playerScore / roundsPlayed * 100;
        double computerWinPercentage = (double) computerScore / roundsPlayed * 100;
        double tiePercentage = (double) ties / roundsPlayed * 100;
        
        return String.format("Rounds played: %d\nPlayer win rate: %.1f%%\nComputer win rate: %.1f%%\nTie rate: %.1f%%",
                            roundsPlayed, playerWinPercentage, computerWinPercentage, tiePercentage);
    }
    
    public static void main(String[] args) {
        GraphicalRockPaperScissors game = new GraphicalRockPaperScissors();
        Scanner scanner = new Scanner(System.in);
        
        System.out.println("Welcome to Rock Paper Scissors!");
        System.out.println("Enter your move (1 for ROCK, 2 for PAPER, 3 for SCISSORS) or 0 to exit:");
        
        while (true) {
            System.out.println("\n" + game.getScore());
            System.out.print("Your move (1-3, 0 to quit): ");
            
            int choice;
            try {
                choice = scanner.nextInt();
            } catch (Exception e) {
                System.out.println("Invalid input. Please enter a number.");
                scanner.nextLine(); // Clear the input buffer
                continue;
            }
            
            if (choice == 0) {
                break;
            }
            
            Move playerMove;
            switch (choice) {
                case 1:
                    playerMove = Move.ROCK;
                    break;
                case 2:
                    playerMove = Move.PAPER;
                    break;
                case 3:
                    playerMove = Move.SCISSORS;
                    break;
                default:
                    System.out.println("Invalid choice. Please enter 1 for ROCK, 2 for PAPER, or 3 for SCISSORS.");
                    continue;
            }
            
            String roundResult = game.playRound(playerMove);
            System.out.println(roundResult);
        }
        
        System.out.println("\nGame over!");
        System.out.println(game.getStatistics());
        System.out.println("Thanks for playing!");
        
        scanner.close();
    }
} 