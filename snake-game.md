# Snake Game

**Difficulty**: Medium  
**Tags**: Queue, Design, Matrix, Game

## Question
Design a Snake game that is played on a device with a screen size of width x height. The snake starts at the top left corner of the screen with a length of 1 unit and moves towards the right. The game is played as follows:

1. The snake can move in four directions: up, right, down, and left.
2. The snake increases in length by 1 unit when it eats food.
3. Food appears at a random position on the screen.
4. The game ends when the snake hits the screen boundary or hits itself.

Implement the `SnakeGame` class:

- `SnakeGame(int width, int height, int[][] food)`: Initializes the game with the screen size and the positions of the food.
- `int move(String direction)`: Moves the snake in the specified direction. Returns the current score (i.e., the number of food eaten) after the move. Return -1 if the game is over.

## Example Input/Output
```
SnakeGame snakeGame = new SnakeGame(3, 2, [[1, 2], [0, 1]]);

// Initial position: snake at (0, 0), food at (1, 2)
snakeGame.move("R"); // Returns 0, snake at [(0, 1)]
snakeGame.move("D"); // Returns 0, snake at [(1, 1)]
snakeGame.move("R"); // Returns 1, snake at [(1, 2)], eats food at (1, 2), new food at (0, 1)
snakeGame.move("U"); // Returns 1, snake at [(0, 2), (1, 2)]
snakeGame.move("L"); // Returns 2, snake at [(0, 1), (0, 2)], eats food at (0, 1)
snakeGame.move("U"); // Returns -1, game over because snake hits the boundary
```

## Solution Algorithm
To implement the Snake game, we need to keep track of the snake's body and the food positions. We can use the following approach:

1. Use a queue (or deque) to represent the snake's body, where each element is a position (row, column).
2. Use a set to quickly check if a position is part of the snake's body.
3. Keep track of the current food index and the food positions.

For each move:
1. Calculate the new head position based on the current head and the direction.
2. Check if the new head position is valid (within bounds and not hitting the snake's body).
3. If the new head position has food, increase the score and don't remove the tail.
4. If the new head position doesn't have food, remove the tail.
5. Update the snake's body and the set of positions.

## Solution Code
```java
import java.util.*;

public class SnakeGame {
    private int width;
    private int height;
    private int[][] food;
    private int foodIndex;
    private int score;
    private Deque<int[]> snake; // Snake body, head is at the front
    private Set<String> snakePositions; // Set of positions for quick lookup
    
    /**
     * Initialize your data structure here.
     * @param width - screen width
     * @param height - screen height
     * @param food - A list of food positions
     */
    public SnakeGame(int width, int height, int[][] food) {
        this.width = width;
        this.height = height;
        this.food = food;
        this.foodIndex = 0;
        this.score = 0;
        
        // Initialize snake at (0, 0)
        this.snake = new LinkedList<>();
        this.snake.offerFirst(new int[]{0, 0});
        this.snakePositions = new HashSet<>();
        this.snakePositions.add("0,0");
    }
    
    /**
     * Moves the snake.
     * @param direction - 'U' = Up, 'L' = Left, 'R' = Right, 'D' = Down
     * @return The game's score after the move. Return -1 if game over.
     */
    public int move(String direction) {
        // Get current head position
        int[] head = snake.peekFirst();
        int row = head[0];
        int col = head[1];
        
        // Calculate new head position
        switch (direction) {
            case "U": row--; break;
            case "D": row++; break;
            case "L": col--; break;
            case "R": col++; break;
        }
        
        // Check if the new head position is out of bounds
        if (row < 0 || row >= height || col < 0 || col >= width) {
            return -1; // Game over
        }
        
        // Check if the new head position hits the snake's body (except the tail)
        String newPos = row + "," + col;
        int[] tail = snake.peekLast();
        snake.pollLast(); // Remove tail temporarily
        snakePositions.remove(tail[0] + "," + tail[1]);
        
        if (snakePositions.contains(newPos)) {
            return -1; // Game over
        }
        
        // Add new head
        snake.offerFirst(new int[]{row, col});
        snakePositions.add(newPos);
        
        // Check if the snake eats food
        if (foodIndex < food.length && row == food[foodIndex][0] && col == food[foodIndex][1]) {
            // Eat food, increase score, and add tail back
            score++;
            foodIndex++;
            snake.offerLast(tail);
            snakePositions.add(tail[0] + "," + tail[1]);
        }
        
        return score;
    }
    
    public static void main(String[] args) {
        int[][] food = {{1, 2}, {0, 1}};
        SnakeGame snakeGame = new SnakeGame(3, 2, food);
        
        System.out.println("Initial position: snake at (0, 0), food at (1, 2)");
        System.out.println("Move right: " + snakeGame.move("R")); // Returns 0
        System.out.println("Move down: " + snakeGame.move("D"));  // Returns 0
        System.out.println("Move right: " + snakeGame.move("R")); // Returns 1, eats food at (1, 2)
        System.out.println("Move up: " + snakeGame.move("U"));    // Returns 1
        System.out.println("Move left: " + snakeGame.move("L"));  // Returns 2, eats food at (0, 1)
        System.out.println("Move up: " + snakeGame.move("U"));    // Returns -1, game over
    }
}
```

```java
// Alternative implementation with visualization
import java.util.*;

public class SnakeGameWithVisualization {
    private int width;
    private int height;
    private int[][] food;
    private int foodIndex;
    private int score;
    private Deque<int[]> snake; // Snake body, head is at the front
    private Set<String> snakePositions; // Set of positions for quick lookup
    
    /**
     * Initialize your data structure here.
     * @param width - screen width
     * @param height - screen height
     * @param food - A list of food positions
     */
    public SnakeGameWithVisualization(int width, int height, int[][] food) {
        this.width = width;
        this.height = height;
        this.food = food;
        this.foodIndex = 0;
        this.score = 0;
        
        // Initialize snake at (0, 0)
        this.snake = new LinkedList<>();
        this.snake.offerFirst(new int[]{0, 0});
        this.snakePositions = new HashSet<>();
        this.snakePositions.add("0,0");
    }
    
    /**
     * Moves the snake.
     * @param direction - 'U' = Up, 'L' = Left, 'R' = Right, 'D' = Down
     * @return The game's score after the move. Return -1 if game over.
     */
    public int move(String direction) {
        // Get current head position
        int[] head = snake.peekFirst();
        int row = head[0];
        int col = head[1];
        
        // Calculate new head position
        switch (direction) {
            case "U": row--; break;
            case "D": row++; break;
            case "L": col--; break;
            case "R": col++; break;
        }
        
        // Check if the new head position is out of bounds
        if (row < 0 || row >= height || col < 0 || col >= width) {
            return -1; // Game over
        }
        
        // Check if the new head position hits the snake's body (except the tail)
        String newPos = row + "," + col;
        int[] tail = snake.peekLast();
        snake.pollLast(); // Remove tail temporarily
        snakePositions.remove(tail[0] + "," + tail[1]);
        
        if (snakePositions.contains(newPos)) {
            return -1; // Game over
        }
        
        // Add new head
        snake.offerFirst(new int[]{row, col});
        snakePositions.add(newPos);
        
        // Check if the snake eats food
        if (foodIndex < food.length && row == food[foodIndex][0] && col == food[foodIndex][1]) {
            // Eat food, increase score, and add tail back
            score++;
            foodIndex++;
            snake.offerLast(tail);
            snakePositions.add(tail[0] + "," + tail[1]);
        }
        
        return score;
    }
    
    /**
     * Visualizes the current state of the game.
     */
    public void visualize() {
        char[][] grid = new char[height][width];
        
        // Fill the grid with empty spaces
        for (int i = 0; i < height; i++) {
            Arrays.fill(grid[i], '.');
        }
        
        // Place food
        if (foodIndex < food.length) {
            int[] currentFood = food[foodIndex];
            grid[currentFood[0]][currentFood[1]] = 'F';
        }
        
        // Place snake body
        Iterator<int[]> it = snake.iterator();
        boolean isHead = true;
        while (it.hasNext()) {
            int[] pos = it.next();
            grid[pos[0]][pos[1]] = isHead ? 'H' : 'S';
            isHead = false;
        }
        
        // Print the grid
        System.out.println("Score: " + score);
        for (int i = 0; i < height; i++) {
            for (int j = 0; j < width; j++) {
                System.out.print(grid[i][j] + " ");
            }
            System.out.println();
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        int[][] food = {{1, 2}, {0, 1}};
        SnakeGameWithVisualization snakeGame = new SnakeGameWithVisualization(3, 2, food);
        
        System.out.println("Initial state:");
        snakeGame.visualize();
        
        System.out.println("Move right:");
        int result = snakeGame.move("R");
        System.out.println("Result: " + result);
        snakeGame.visualize();
        
        System.out.println("Move down:");
        result = snakeGame.move("D");
        System.out.println("Result: " + result);
        snakeGame.visualize();
        
        System.out.println("Move right:");
        result = snakeGame.move("R");
        System.out.println("Result: " + result);
        snakeGame.visualize();
        
        System.out.println("Move up:");
        result = snakeGame.move("U");
        System.out.println("Result: " + result);
        snakeGame.visualize();
        
        System.out.println("Move left:");
        result = snakeGame.move("L");
        System.out.println("Result: " + result);
        snakeGame.visualize();
        
        System.out.println("Move up:");
        result = snakeGame.move("U");
        System.out.println("Result: " + result);
        if (result == -1) {
            System.out.println("Game over!");
        }
    }
}
```

```java
// Interactive Snake Game implementation
import java.util.*;

public class InteractiveSnakeGame {
    private int width;
    private int height;
    private int[][] food;
    private int foodIndex;
    private int score;
    private Deque<int[]> snake; // Snake body, head is at the front
    private Set<String> snakePositions; // Set of positions for quick lookup
    
    /**
     * Initialize your data structure here.
     * @param width - screen width
     * @param height - screen height
     * @param foodCount - Number of food items to generate
     */
    public InteractiveSnakeGame(int width, int height, int foodCount) {
        this.width = width;
        this.height = height;
        this.food = generateRandomFood(width, height, foodCount);
        this.foodIndex = 0;
        this.score = 0;
        
        // Initialize snake at (0, 0)
        this.snake = new LinkedList<>();
        this.snake.offerFirst(new int[]{0, 0});
        this.snakePositions = new HashSet<>();
        this.snakePositions.add("0,0");
    }
    
    /**
     * Generates random food positions.
     */
    private int[][] generateRandomFood(int width, int height, int count) {
        int[][] food = new int[count][2];
        Random random = new Random();
        Set<String> positions = new HashSet<>();
        
        for (int i = 0; i < count; i++) {
            int row, col;
            String pos;
            
            do {
                row = random.nextInt(height);
                col = random.nextInt(width);
                pos = row + "," + col;
            } while (positions.contains(pos) || (row == 0 && col == 0)); // Avoid placing food at snake's initial position
            
            food[i] = new int[]{row, col};
            positions.add(pos);
        }
        
        return food;
    }
    
    /**
     * Moves the snake.
     * @param direction - 'U' = Up, 'L' = Left, 'R' = Right, 'D' = Down
     * @return The game's score after the move. Return -1 if game over.
     */
    public int move(String direction) {
        // Get current head position
        int[] head = snake.peekFirst();
        int row = head[0];
        int col = head[1];
        
        // Calculate new head position
        switch (direction) {
            case "U": row--; break;
            case "D": row++; break;
            case "L": col--; break;
            case "R": col++; break;
        }
        
        // Check if the new head position is out of bounds
        if (row < 0 || row >= height || col < 0 || col >= width) {
            return -1; // Game over
        }
        
        // Check if the new head position hits the snake's body (except the tail)
        String newPos = row + "," + col;
        int[] tail = snake.peekLast();
        snake.pollLast(); // Remove tail temporarily
        snakePositions.remove(tail[0] + "," + tail[1]);
        
        if (snakePositions.contains(newPos)) {
            return -1; // Game over
        }
        
        // Add new head
        snake.offerFirst(new int[]{row, col});
        snakePositions.add(newPos);
        
        // Check if the snake eats food
        if (foodIndex < food.length && row == food[foodIndex][0] && col == food[foodIndex][1]) {
            // Eat food, increase score, and add tail back
            score++;
            foodIndex++;
            snake.offerLast(tail);
            snakePositions.add(tail[0] + "," + tail[1]);
        }
        
        return score;
    }
    
    /**
     * Visualizes the current state of the game.
     */
    public void visualize() {
        char[][] grid = new char[height][width];
        
        // Fill the grid with empty spaces
        for (int i = 0; i < height; i++) {
            Arrays.fill(grid[i], '.');
        }
        
        // Place food
        if (foodIndex < food.length) {
            int[] currentFood = food[foodIndex];
            grid[currentFood[0]][currentFood[1]] = 'F';
        }
        
        // Place snake body
        Iterator<int[]> it = snake.iterator();
        boolean isHead = true;
        while (it.hasNext()) {
            int[] pos = it.next();
            grid[pos[0]][pos[1]] = isHead ? 'H' : 'S';
            isHead = false;
        }
        
        // Print the grid
        System.out.println("Score: " + score);
        System.out.print("  ");
        for (int j = 0; j < width; j++) {
            System.out.print(j + " ");
        }
        System.out.println();
        
        for (int i = 0; i < height; i++) {
            System.out.print(i + " ");
            for (int j = 0; j < width; j++) {
                System.out.print(grid[i][j] + " ");
            }
            System.out.println();
        }
        System.out.println();
    }
    
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        System.out.println("Welcome to Snake Game!");
        System.out.print("Enter the width of the board: ");
        int width = scanner.nextInt();
        
        System.out.print("Enter the height of the board: ");
        int height = scanner.nextInt();
        
        System.out.print("Enter the number of food items: ");
        int foodCount = scanner.nextInt();
        
        InteractiveSnakeGame game = new InteractiveSnakeGame(width, height, foodCount);
        
        System.out.println("\nGame started! Use W (up), A (left), S (down), D (right) to move the snake.");
        System.out.println("H: Snake Head, S: Snake Body, F: Food, .: Empty");
        game.visualize();
        
        int result = 0;
        while (result != -1) {
            System.out.print("Enter your move (W/A/S/D): ");
            String input = scanner.next().toUpperCase();
            
            String direction = "";
            switch (input) {
                case "W": direction = "U"; break;
                case "A": direction = "L"; break;
                case "S": direction = "D"; break;
                case "D": direction = "R"; break;
                default:
                    System.out.println("Invalid input! Use W, A, S, or D.");
                    continue;
            }
            
            result = game.move(direction);
            if (result == -1) {
                System.out.println("Game over! Final score: " + game.score);
            } else {
                System.out.println("Score: " + result);
                game.visualize();
            }
        }
        
        scanner.close();
    }
} 