# 3번 문항
'''
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    int com[3], user[3];
    int strikes, balls;
    int attempts = 0;

    // 난수 생성 초기화 (서로 다른 3개의 숫자 설정)
    srand(time(NULL));
    com[0] = rand() % 10;
    do { com[1] = rand() % 10; } while (com[1] == com[0]);
    do { com[2] = rand() % 10; } while (com[2] == com[0] || com[2] == com[1]);

    printf("=== 숫자야구 게임 시작 (0~9 사이 중복 없는 3자리 숫자) ===\n");

    while (1) {
        strikes = 0;
        balls = 0;
        attempts++;

        printf("\n시도 %d - 3자리 숫자 입력 (예: 1 2 3): ", attempts);
        scanf("%d %d %d", &user[0], &user[1], &user[2]);

        // 스트라이크 및 볼 판정
        for (int i = 0; i < 3; i++) {
            for (int j = 0; j < 3; j++) {
                if (user[i] == com[j]) {
                    if (i == j) {
                        strikes++; // 자릿수와 숫자가 모두 같음
                    } else {
                        balls++;   // 숫자는 같지만 자릿수가 다름
                    }
                }
            }
        }

        // 결과 출력
        printf("결과: %d 스트라이크, %d 볼\n", strikes, balls);

        // 3 스트라이크 시 게임 종료
        if (strikes == 3) {
            printf("\n정답입니다! 총 %d번 만에 맞췄습니다.\n", attempts);
            break;
        }
    }

    return 0;
}
'''
