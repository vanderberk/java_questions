# Space Invaders

**Difficulty**: Hard  
**Tags**: Array, Matrix, Game, Design, Object-Oriented Design

## Question
Design and implement a simplified version of the classic arcade game Space Invaders. In this game, the player controls a ship at the bottom of the screen and shoots at aliens that move horizontally and vertically across the screen.

Your implementation should include the following components:

1. A `Player` class that represents the player's ship, which can move left and right and shoot bullets.
2. An `Alien` class that represents the invaders, which move in formation and occasionally shoot bullets.
3. A `Bullet` class that represents projectiles fired by both the player and aliens.
4. A `Game` class that manages the game state, including:
   - Initializing the game with a specified number of aliens and their formation.
   - Updating the game state based on player input and game rules.
   - Detecting collisions between bullets and ships/aliens.
   - Determining when the game is over (player is hit or all aliens are destroyed).

## Example Input/Output
```
SpaceInvaders game = new SpaceInvaders(6, 4, 3); // width, height, number of aliens
// Initialize a 6x4 game with 3 aliens

game.movePlayer("RIGHT"); // Move player to the right
game.playerShoot(); // Player shoots a bullet

game.update(); // Update game state for one time step
// This moves aliens, moves bullets, and checks for collisions

game.getGameState(); // Returns the current state of the game
// This includes positions of the player, aliens, and bullets

game.isGameOver(); // Returns true if the game is over, false otherwise
```

## Solution Algorithm
To implement the Space Invaders game, we need to:

1. **Initialize the game**:
   - Create a player at the bottom center of the screen.
   - Create aliens in a formation at the top of the screen.
   - Set up the game boundaries.

2. **Handle player movement and shooting**:
   - Allow the player to move left and right within the boundaries.
   - Allow the player to shoot bullets that travel upward.

3. **Manage alien movement and shooting**:
   - Move aliens horizontally as a group.
   - When aliens reach the edge of the screen, move them down and reverse direction.
   - Randomly select aliens to shoot bullets that travel downward.

4. **Update game state**:
   - Move all bullets.
   - Check for collisions between bullets and entities.
   - Remove destroyed entities and bullets that are out of bounds.
   - Check if the game is over (player is hit or all aliens are destroyed).

5. **Render the game**:
   - Display the current state of the game, including the positions of the player, aliens, and bullets.

## Solution Code
```java
import java.util.*;

public class SpaceInvaders {
    private int width;
    private int height;
    private Player player;
    private List<Alien> aliens;
    private List<Bullet> bullets;
    private boolean gameOver;
    private boolean playerWon;
    private int alienDirection; // 1 for right, -1 for left
    private int alienMoveTimer;
    private int alienShootTimer;
    private Random random;
    
    /**
     * Initialize the Space Invaders game.
     * @param width The width of the game screen.
     * @param height The height of the game screen.
     * @param alienCount The number of aliens.
     */
    public SpaceInvaders(int width, int height, int alienCount) {
        this.width = width;
        this.height = height;
        this.player = new Player(width / 2, height - 1);
        this.aliens = new ArrayList<>();
        this.bullets = new ArrayList<>();
        this.gameOver = false;
        this.playerWon = false;
        this.alienDirection = 1;
        this.alienMoveTimer = 0;
        this.alienShootTimer = 0;
        this.random = new Random();
        
        // Initialize aliens in a formation
        initializeAliens(alienCount);
    }
    
    /**
     * Initialize aliens in a formation.
     */
    private void initializeAliens(int alienCount) {
        int aliensPerRow = Math.min(alienCount, width - 2);
        int rows = (alienCount + aliensPerRow - 1) / aliensPerRow;
        
        int alienIndex = 0;
        for (int row = 0; row < rows && alienIndex < alienCount; row++) {
            for (int col = 0; col < aliensPerRow && alienIndex < alienCount; col++) {
                int x = col + 1;
                int y = row + 1;
                aliens.add(new Alien(x, y));
                alienIndex++;
            }
        }
    }
    
    /**
     * Move the player in the specified direction.
     * @param direction The direction to move ("LEFT" or "RIGHT").
     */
    public void movePlayer(String direction) {
        if (gameOver) return;
        
        if (direction.equals("LEFT") && player.x > 0) {
            player.x--;
        } else if (direction.equals("RIGHT") && player.x < width - 1) {
            player.x++;
        }
    }
    
    /**
     * Player shoots a bullet.
     */
    public void playerShoot() {
        if (gameOver) return;
        
        bullets.add(new Bullet(player.x, player.y - 1, -1)); // Bullet moves upward
    }
    
    /**
     * Update the game state for one time step.
     */
    public void update() {
        if (gameOver) return;
        
        // Move bullets
        moveBullets();
        
        // Move aliens
        moveAliens();
        
        // Alien shooting
        alienShoot();
        
        // Check collisions
        checkCollisions();
        
        // Check if game is over
        checkGameOver();
    }
    
    /**
     * Move all bullets.
     */
    private void moveBullets() {
        Iterator<Bullet> iterator = bullets.iterator();
        while (iterator.hasNext()) {
            Bullet bullet = iterator.next();
            bullet.y += bullet.direction;
            
            // Remove bullets that are out of bounds
            if (bullet.y < 0 || bullet.y >= height) {
                iterator.remove();
            }
        }
    }
    
    /**
     * Move aliens as a group.
     */
    private void moveAliens() {
        alienMoveTimer++;
        if (alienMoveTimer < 5) return; // Move aliens every 5 time steps
        
        alienMoveTimer = 0;
        
        boolean shouldChangeDirection = false;
        
        // Check if any alien would hit the edge after moving
        for (Alien alien : aliens) {
            if ((alienDirection == 1 && alien.x + alienDirection >= width - 1) ||
                (alienDirection == -1 && alien.x + alienDirection <= 0)) {
                shouldChangeDirection = true;
                break;
            }
        }
        
        if (shouldChangeDirection) {
            // Change direction and move down
            alienDirection *= -1;
            for (Alien alien : aliens) {
                alien.y++;
            }
        } else {
            // Move horizontally
            for (Alien alien : aliens) {
                alien.x += alienDirection;
            }
        }
    }
    
    /**
     * Randomly select aliens to shoot.
     */
    private void alienShoot() {
        alienShootTimer++;
        if (alienShootTimer < 10) return; // Aliens shoot every 10 time steps
        
        alienShootTimer = 0;
        
        if (aliens.isEmpty()) return;
        
        // Randomly select an alien to shoot
        int index = random.nextInt(aliens.size());
        Alien shooter = aliens.get(index);
        
        bullets.add(new Bullet(shooter.x, shooter.y + 1, 1)); // Bullet moves downward
    }
    
    /**
     * Check for collisions between bullets and entities.
     */
    private void checkCollisions() {
        Iterator<Bullet> bulletIterator = bullets.iterator();
        while (bulletIterator.hasNext()) {
            Bullet bullet = bulletIterator.next();
            
            // Check if bullet hits player
            if (bullet.direction > 0 && bullet.x == player.x && bullet.y == player.y) {
                gameOver = true;
                playerWon = false;
                return;
            }
            
            // Check if bullet hits aliens
            if (bullet.direction < 0) {
                Iterator<Alien> alienIterator = aliens.iterator();
                while (alienIterator.hasNext()) {
                    Alien alien = alienIterator.next();
                    if (bullet.x == alien.x && bullet.y == alien.y) {
                        alienIterator.remove();
                        bulletIterator.remove();
                        break;
                    }
                }
            }
        }
    }
    
    /**
     * Check if the game is over.
     */
    private void checkGameOver() {
        // Check if all aliens are destroyed
        if (aliens.isEmpty()) {
            gameOver = true;
            playerWon = true;
            return;
        }
        
        // Check if aliens have reached the bottom
        for (Alien alien : aliens) {
            if (alien.y >= height - 1) {
                gameOver = true;
                playerWon = false;
                return;
            }
        }
    }
    
    /**
     * Get the current state of the game.
     * @return A 2D character array representing the game state.
     */
    public char[][] getGameState() {
        char[][] state = new char[height][width];
        
        // Initialize with empty space
        for (int i = 0; i < height; i++) {
            Arrays.fill(state[i], ' ');
        }
        
        // Place player
        state[player.y][player.x] = 'P';
        
        // Place aliens
        for (Alien alien : aliens) {
            state[alien.y][alien.x] = 'A';
        }
        
        // Place bullets
        for (Bullet bullet : bullets) {
            state[bullet.y][bullet.x] = bullet.direction < 0 ? '^' : 'v';
        }
        
        return state;
    }
    
    /**
     * Check if the game is over.
     * @return true if the game is over, false otherwise.
     */
    public boolean isGameOver() {
        return gameOver;
    }
    
    /**
     * Check if the player won.
     * @return true if the player won, false otherwise.
     */
    public boolean isPlayerWon() {
        return playerWon;
    }
    
    /**
     * Print the current state of the game.
     */
    public void printGameState() {
        char[][] state = getGameState();
        
        // Print top border
        System.out.print("+");
        for (int i = 0; i < width; i++) {
            System.out.print("-");
        }
        System.out.println("+");
        
        // Print game state
        for (int i = 0; i < height; i++) {
            System.out.print("|");
            for (int j = 0; j < width; j++) {
                System.out.print(state[i][j]);
            }
            System.out.println("|");
        }
        
        // Print bottom border
        System.out.print("+");
        for (int i = 0; i < width; i++) {
            System.out.print("-");
        }
        System.out.println("+");
    }
    
    /**
     * Entity classes
     */
    private static class Player {
        int x;
        int y;
        
        Player(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
    
    private static class Alien {
        int x;
        int y;
        
        Alien(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
    
    private static class Bullet {
        int x;
        int y;
        int direction; // -1 for upward (player bullet), 1 for downward (alien bullet)
        
        Bullet(int x, int y, int direction) {
            this.x = x;
            this.y = y;
            this.direction = direction;
        }
    }
    
    public static void main(String[] args) {
        // Create a 10x8 game with 5 aliens
        SpaceInvaders game = new SpaceInvaders(10, 8, 5);
        
        System.out.println("Initial game state:");
        game.printGameState();
        
        // Simulate a few moves
        game.movePlayer("RIGHT");
        game.playerShoot();
        game.update();
        
        System.out.println("\nAfter player moves right and shoots:");
        game.printGameState();
        
        // Simulate a few more updates
        for (int i = 0; i < 5; i++) {
            game.update();
        }
        
        System.out.println("\nAfter 5 more updates:");
        game.printGameState();
        
        // Check if game is over
        if (game.isGameOver()) {
            if (game.isPlayerWon()) {
                System.out.println("Player won!");
            } else {
                System.out.println("Player lost!");
            }
        } else {
            System.out.println("Game in progress...");
        }
    }
}
```

```java
// Interactive Space Invaders implementation
import java.util.*;

public class InteractiveSpaceInvaders {
    private int width;
    private int height;
    private Player player;
    private List<Alien> aliens;
    private List<Bullet> bullets;
    private boolean gameOver;
    private boolean playerWon;
    private int alienDirection; // 1 for right, -1 for left
    private int alienMoveTimer;
    private int alienShootTimer;
    private Random random;
    private int score;
    
    /**
     * Initialize the Space Invaders game.
     * @param width The width of the game screen.
     * @param height The height of the game screen.
     * @param alienCount The number of aliens.
     */
    public InteractiveSpaceInvaders(int width, int height, int alienCount) {
        this.width = width;
        this.height = height;
        this.player = new Player(width / 2, height - 1);
        this.aliens = new ArrayList<>();
        this.bullets = new ArrayList<>();
        this.gameOver = false;
        this.playerWon = false;
        this.alienDirection = 1;
        this.alienMoveTimer = 0;
        this.alienShootTimer = 0;
        this.random = new Random();
        this.score = 0;
        
        // Initialize aliens in a formation
        initializeAliens(alienCount);
    }
    
    /**
     * Initialize aliens in a formation.
     */
    private void initializeAliens(int alienCount) {
        int aliensPerRow = Math.min(alienCount, width - 2);
        int rows = (alienCount + aliensPerRow - 1) / aliensPerRow;
        
        int alienIndex = 0;
        for (int row = 0; row < rows && alienIndex < alienCount; row++) {
            for (int col = 0; col < aliensPerRow && alienIndex < alienCount; col++) {
                int x = col + 1;
                int y = row + 1;
                aliens.add(new Alien(x, y));
                alienIndex++;
            }
        }
    }
    
    /**
     * Move the player in the specified direction.
     * @param direction The direction to move ("LEFT" or "RIGHT").
     */
    public void movePlayer(String direction) {
        if (gameOver) return;
        
        if (direction.equals("LEFT") && player.x > 0) {
            player.x--;
        } else if (direction.equals("RIGHT") && player.x < width - 1) {
            player.x++;
        }
    }
    
    /**
     * Player shoots a bullet.
     * @return true if the player can shoot, false otherwise.
     */
    public boolean playerShoot() {
        if (gameOver) return false;
        
        // Limit the number of player bullets on screen
        int playerBulletCount = 0;
        for (Bullet bullet : bullets) {
            if (bullet.direction < 0) {
                playerBulletCount++;
            }
        }
        
        if (playerBulletCount < 3) {
            bullets.add(new Bullet(player.x, player.y - 1, -1)); // Bullet moves upward
            return true;
        }
        
        return false;
    }
    
    /**
     * Update the game state for one time step.
     */
    public void update() {
        if (gameOver) return;
        
        // Move bullets
        moveBullets();
        
        // Move aliens
        moveAliens();
        
        // Alien shooting
        alienShoot();
        
        // Check collisions
        checkCollisions();
        
        // Check if game is over
        checkGameOver();
    }
    
    /**
     * Move all bullets.
     */
    private void moveBullets() {
        Iterator<Bullet> iterator = bullets.iterator();
        while (iterator.hasNext()) {
            Bullet bullet = iterator.next();
            bullet.y += bullet.direction;
            
            // Remove bullets that are out of bounds
            if (bullet.y < 0 || bullet.y >= height) {
                iterator.remove();
            }
        }
    }
    
    /**
     * Move aliens as a group.
     */
    private void moveAliens() {
        alienMoveTimer++;
        if (alienMoveTimer < 5) return; // Move aliens every 5 time steps
        
        alienMoveTimer = 0;
        
        boolean shouldChangeDirection = false;
        
        // Check if any alien would hit the edge after moving
        for (Alien alien : aliens) {
            if ((alienDirection == 1 && alien.x + alienDirection >= width - 1) ||
                (alienDirection == -1 && alien.x + alienDirection <= 0)) {
                shouldChangeDirection = true;
                break;
            }
        }
        
        if (shouldChangeDirection) {
            // Change direction and move down
            alienDirection *= -1;
            for (Alien alien : aliens) {
                alien.y++;
            }
        } else {
            // Move horizontally
            for (Alien alien : aliens) {
                alien.x += alienDirection;
            }
        }
    }
    
    /**
     * Randomly select aliens to shoot.
     */
    private void alienShoot() {
        alienShootTimer++;
        if (alienShootTimer < 10) return; // Aliens shoot every 10 time steps
        
        alienShootTimer = 0;
        
        if (aliens.isEmpty()) return;
        
        // Randomly select an alien to shoot
        int index = random.nextInt(aliens.size());
        Alien shooter = aliens.get(index);
        
        bullets.add(new Bullet(shooter.x, shooter.y + 1, 1)); // Bullet moves downward
    }
    
    /**
     * Check for collisions between bullets and entities.
     */
    private void checkCollisions() {
        Iterator<Bullet> bulletIterator = bullets.iterator();
        while (bulletIterator.hasNext()) {
            Bullet bullet = bulletIterator.next();
            
            // Check if bullet hits player
            if (bullet.direction > 0 && bullet.x == player.x && bullet.y == player.y) {
                gameOver = true;
                playerWon = false;
                return;
            }
            
            // Check if bullet hits aliens
            if (bullet.direction < 0) {
                Iterator<Alien> alienIterator = aliens.iterator();
                while (alienIterator.hasNext()) {
                    Alien alien = alienIterator.next();
                    if (bullet.x == alien.x && bullet.y == alien.y) {
                        alienIterator.remove();
                        bulletIterator.remove();
                        score += 10;
                        break;
                    }
                }
            }
        }
    }
    
    /**
     * Check if the game is over.
     */
    private void checkGameOver() {
        // Check if all aliens are destroyed
        if (aliens.isEmpty()) {
            gameOver = true;
            playerWon = true;
            return;
        }
        
        // Check if aliens have reached the bottom
        for (Alien alien : aliens) {
            if (alien.y >= height - 1) {
                gameOver = true;
                playerWon = false;
                return;
            }
        }
    }
    
    /**
     * Get the current state of the game.
     * @return A 2D character array representing the game state.
     */
    public char[][] getGameState() {
        char[][] state = new char[height][width];
        
        // Initialize with empty space
        for (int i = 0; i < height; i++) {
            Arrays.fill(state[i], ' ');
        }
        
        // Place player
        state[player.y][player.x] = 'P';
        
        // Place aliens
        for (Alien alien : aliens) {
            state[alien.y][alien.x] = 'A';
        }
        
        // Place bullets
        for (Bullet bullet : bullets) {
            state[bullet.y][bullet.x] = bullet.direction < 0 ? '^' : 'v';
        }
        
        return state;
    }
    
    /**
     * Check if the game is over.
     * @return true if the game is over, false otherwise.
     */
    public boolean isGameOver() {
        return gameOver;
    }
    
    /**
     * Check if the player won.
     * @return true if the player won, false otherwise.
     */
    public boolean isPlayerWon() {
        return playerWon;
    }
    
    /**
     * Get the current score.
     * @return The current score.
     */
    public int getScore() {
        return score;
    }
    
    /**
     * Print the current state of the game.
     */
    public void printGameState() {
        char[][] state = getGameState();
        
        // Print score
        System.out.println("Score: " + score);
        
        // Print top border
        System.out.print("+");
        for (int i = 0; i < width; i++) {
            System.out.print("-");
        }
        System.out.println("+");
        
        // Print game state
        for (int i = 0; i < height; i++) {
            System.out.print("|");
            for (int j = 0; j < width; j++) {
                System.out.print(state[i][j]);
            }
            System.out.println("|");
        }
        
        // Print bottom border
        System.out.print("+");
        for (int i = 0; i < width; i++) {
            System.out.print("-");
        }
        System.out.println("+");
        
        // Print legend
        System.out.println("P: Player, A: Alien, ^: Player Bullet, v: Alien Bullet");
    }
    
    /**
     * Entity classes
     */
    private static class Player {
        int x;
        int y;
        
        Player(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
    
    private static class Alien {
        int x;
        int y;
        
        Alien(int x, int y) {
            this.x = x;
            this.y = y;
        }
    }
    
    private static class Bullet {
        int x;
        int y;
        int direction; // -1 for upward (player bullet), 1 for downward (alien bullet)
        
        Bullet(int x, int y, int direction) {
            this.x = x;
            this.y = y;
            this.direction = direction;
        }
    }
    
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.println("Welcome to Space Invaders!");
        System.out.print("Enter the width of the screen: ");
        int width = scanner.nextInt();
        
        System.out.print("Enter the height of the screen: ");
        int height = scanner.nextInt();
        
        System.out.print("Enter the number of aliens: ");
        int alienCount = scanner.nextInt();
        
        InteractiveSpaceInvaders game = new InteractiveSpaceInvaders(width, height, alienCount);
        
        System.out.println("\nGame started! Controls:");
        System.out.println("- A: Move left");
        System.out.println("- D: Move right");
        System.out.println("- S: Shoot");
        System.out.println("- Q: Quit");
        
        game.printGameState();
        
        while (!game.isGameOver()) {
            System.out.print("Enter command: ");
            String command = scanner.next().toUpperCase();
            
            switch (command) {
                case "A":
                    game.movePlayer("LEFT");
                    break;
                case "D":
                    game.movePlayer("RIGHT");
                    break;
                case "S":
                    game.playerShoot();
                    break;
                case "Q":
                    System.out.println("Game aborted.");
                    scanner.close();
                    return;
                default:
                    System.out.println("Invalid command. Try again.");
                    continue;
            }
            
            game.update();
            game.printGameState();
        }
        
        // Game over
        if (game.isPlayerWon()) {
            System.out.println("Congratulations! You won with a score of " + game.getScore() + "!");
        } else {
            System.out.println("Game over! Your final score is " + game.getScore() + ".");
        }
        
        scanner.close();
    }
} 