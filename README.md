# Brick-Bricker-Game
import java.util.Scanner;

public class BrickBreakerText {
    private static final int WIDTH = 20;
    private static final int HEIGHT = 10;
    private static final char BALL_CHAR = 'O';
    private static final char PADDLE_CHAR = '=';
    private static final char BRICK_CHAR = '#';
    private static final char EMPTY_CHAR = ' ';

    private static int paddlePosition;
    private static int ballX, ballY;
    private static int ballSpeedX = 1, ballSpeedY = -1;
    private static boolean[][] bricks = new boolean[HEIGHT][WIDTH];

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        initializeBricks();

        while (true) {
            moveBall();
            drawFrame();
            movePaddle(scanner);
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    private static void initializeBricks() {
        for (int i = 0; i < HEIGHT; i++) {
            for (int j = 0; j < WIDTH; j++) {
                bricks[i][j] = true;
            }
        }
    }

    private static void moveBall() {
        ballX += ballSpeedX;
        ballY += ballSpeedY;

        // Ball collision with walls
        if (ballX == 0 || ballX == WIDTH - 1) {
            ballSpeedX = -ballSpeedX;
        }
        if (ballY == 0) {
            ballSpeedY = -ballSpeedY;
        }

        // Ball collision with paddle
        if (ballY == HEIGHT - 2 && ballX >= paddlePosition && ballX < paddlePosition + 5) {
            ballSpeedY = -ballSpeedY;
        }

        // Ball collision with bricks
        if (ballY < HEIGHT - 1) {
            if (bricks[ballY][ballX] && ballY >= 0 && ballX >= 0 && ballX < WIDTH && ballY < HEIGHT) {
                bricks[ballY][ballX] = false;
                ballSpeedY = -ballSpeedY;
            }
        }

        // Game over condition
        if (ballY >= HEIGHT - 1) {
            System.out.println("Game Over!");
            System.exit(0);
        }
    }

    private static void drawFrame() {
        StringBuilder sb = new StringBuilder();

        for (int i = 0; i < HEIGHT; i++) {
            for (int j = 0; j < WIDTH; j++) {
                if (i == 0 || i == HEIGHT - 1 || j == 0 || j == WIDTH - 1) {
                    sb.append('*'); // Draw borders
                } else if (i == HEIGHT - 2 && (j >= paddlePosition && j < paddlePosition + 5)) {
                    sb.append(PADDLE_CHAR); // Draw paddle
                } else if (i == ballY && j == ballX) {
                    sb.append(BALL_CHAR); // Draw ball
                } else if (bricks[i][j]) {
                    sb.append(BRICK_CHAR); // Draw bricks
                } else {
                    sb.append(EMPTY_CHAR); // Draw empty space
                }
            }
            sb.append('\n');
        }

        System.out.print(sb.toString());
    }

    private static void movePaddle(Scanner scanner) {
        if (scanner.hasNext()) {
            char input = scanner.next().charAt(0);
            if (input == 'a' && paddlePosition > 1) {
                paddlePosition--;
            } else if (input == 'd' && paddlePosition < WIDTH - 5) {
                paddlePosition++;
            }
        }
    }
}
