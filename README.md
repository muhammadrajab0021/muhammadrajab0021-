import java.util.Scanner;
import java.util.Random;

// 1. Parent Class (FOR SHOW INHERITANCE CONCEPT)
class GameEntity {
    int x, y;
    GameEntity(int x, int y) {
        this.x = x;
        this.y = y;
    }
}

// 2. Pacman Class
class Pacman extends GameEntity {
    int score = 0;

    Pacman(int x, int y) { super(x, y); }

    void move(char dir, GameBoard board) {
        int newX = x, newY = y;
        if (dir == 'o') newX--;   //o for UP
        if (dir == 'k') newX++;   //k for DOWN
        if (dir == 'j') newY--;   //j for LEFT
        if (dir == 'l') newY++;   //l for RIGHT

        // IF WALL (#) NOT PRESENT THEN MOVE
        if (board.grid[newX][newY] != '#') {
            this.x = newX;
            this.y = newY;
            // IF THE FOOD(.) PRESENT THEN INCREASE SCORE
            if (board.grid[x][y] == '@') {
                score += 10;
                board.grid[x][y] = ' '; //FOOD END OR DISAPPEAR
            }
        }
    }
}

// 3. Ghost Class
class Ghost extends GameEntity {
    Ghost(int x, int y) { super(x, y); }

    void randomMove(GameBoard board) {
        Random rand = new Random();
        int dir = rand.nextInt(4);
        int newX = x, newY = y;

        if (dir == 0) newX--; // Up
        if (dir == 1) newX++; // Down
        if (dir == 2) newY--; // Left
        if (dir == 3) newY++; // Right

        if (board.grid[newX][newY] != '#') {
            this.x = newX;
            this.y = newY;
        }
    }
}

// 4. Board Class
class GameBoard {
    // Map design: # is wall, AND . is food
    char[][] grid = {
            {'#','#','#','#','#','#','#','#'},
            {'#','@','@','@','@','@','@','#'},
            {'#','@','#','#','@','#','@','#'},
            {'#','@','@','@','@','@','@','#'},
            {'#','#','#','#','#','#','#','#'}
    };

    void draw(Pacman p, Ghost g) {
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[i].length; j++) {
                if (i == p.x && j == p.y) System.out.print("P "); // Pacman
                else if (i == g.x && j == g.y) System.out.print("G "); // Ghost
                else System.out.print(grid[i][j] + " ");
            }
            System.out.println();
        }
    }
}

// 5. Main Game Engine Class
public class Main{
    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);
        GameBoard board = new GameBoard();
        Pacman pacman = new Pacman(1, 1);
        Ghost ghost = new Ghost(3, 6);

        System.out.println("--- WELCOME TO PACMAN GAME (OOP-LAB PROJECT-1) ---");
        System.out.println("--- o,k,j,l and q used up,down,left and right respectively ---");


        while (true) {
            System.out.println("\nScore: " + pacman.score);
            board.draw(pacman, ghost);

            System.out.print("Move (o/k/j/l) or Q to Quit: ");
            char input = sc.next().toLowerCase().charAt(0);

            if (input == 'q') break;

            pacman.move(input, board);
            ghost.randomMove(board);

            // Check if Ghost caught Pacman
            if (pacman.x == ghost.x && pacman.y == ghost.y) {
                board.draw(pacman, ghost);
                System.out.println("GAME OVER! Ghost caught you.");
                break;
            }
        }
        System.out.println("Thanks for playing!");
    }
}
