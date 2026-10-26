# 1번 답안
'''
#include "raylib.h"

#define SCREEN_WIDTH 800
#define SCREEN_HEIGHT 600
#define ROWS 7
#define COLS 10

typedef struct {
    Vector2 position;
    Vector2 speed;
    float radius;
    Color color;
} Ball;

typedef struct {
    Rectangle rect;
    float speed;
    Color color;
} Paddle;

typedef struct {
    Rectangle rect;
    Color color;
    bool active;
} Block;

int main(void) {
    InitWindow(SCREEN_WIDTH, SCREEN_HEIGHT, "Rainbow Breakout");
    SetTargetFPS(60);

    Ball ball = { { SCREEN_WIDTH / 2.0f, SCREEN_HEIGHT / 2.0f }, { 5.0f, -5.0f }, 10.0f, YELLOW };
    Paddle paddle = { { SCREEN_WIDTH / 2.0f - 50, SCREEN_HEIGHT - 40, 100, 15 }, 8.0f, WHITE };

    Color rainbow[ROWS] = { RED, ORANGE, YELLOW, GREEN, BLUE, PURPLE, MAGENTA };
    Block blocks[ROWS][COLS];

    for (int i = 0; i < ROWS; i++) {
        for (int j = 0; j < COLS; j++) {
            blocks[i][j] = (Block){
                { 45.0f + j * 73.0f, 50.0f + i * 25.0f, 68.0f, 20.0f },
                rainbow[i],
                true
            };
        }
    }

    while (!WindowShouldClose()) {
        // Input
        if (IsKeyDown(KEY_LEFT) && paddle.rect.x > 0) paddle.rect.x -= paddle.speed;
        if (IsKeyDown(KEY_RIGHT) && paddle.rect.x < SCREEN_WIDTH - paddle.rect.width) paddle.rect.x += paddle.speed;

        // Ball movement
        ball.position.x += ball.speed.x;
        ball.position.y += ball.speed.y;

        // Wall collisions
        if (ball.position.x - ball.radius <= 0 || ball.position.x + ball.radius >= SCREEN_WIDTH) ball.speed.x *= -1;
        if (ball.position.y - ball.radius <= 0) ball.speed.y *= -1;
        if (ball.position.y >= SCREEN_HEIGHT) {
            ball.position = (Vector2){ SCREEN_WIDTH / 2.0f, SCREEN_HEIGHT / 2.0f };
            ball.speed = (Vector2){ 5.0f, -5.0f };
        }

        // Paddle collision
        if (CheckCollisionCircleRec(ball.position, ball.radius, paddle.rect)) {
            ball.speed.y *= -1;
        }

        // Block collisions
        for (int i = 0; i < ROWS; i++) {
            for (int j = 0; j < COLS; j++) {
                if (blocks[i][j].active) {
                    if (CheckCollisionCircleRec(ball.position, ball.radius, blocks[i][j].rect)) {
                        blocks[i][j].active = false;
                        ball.speed.y *= -1;
                    }
                }
            }
        }

        // Render
        BeginDrawing();
        ClearBackground(DARKGRAY);

        DrawRectangleRec(paddle.rect, paddle.color);
        DrawCircleV(ball.position, ball.radius, ball.color);

        for (int i = 0; i < ROWS; i++) {
            for (int j = 0; j < COLS; j++) {
                if (blocks[i][j].active) {
                    DrawRectangleRec(blocks[i][j].rect, blocks[i][j].color);
                }
            }
        }

        EndDrawing();
    }

    CloseWindow();
    return 0;
}
'''
