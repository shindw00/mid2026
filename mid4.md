# 4번 문항
'''
#include <stdio.h>

int main() {
    int N;
    scanf("%d", &N);

    for (int i = 1; i <= N; i++) {
        // 1. 좌측 공백 출력 (중앙 정렬을 위함)
        for (int j = 0; j < N - i; j++) {
            printf(" ");
        }
        // 2. 문자 출력 (홀수 행은 '*', 짝수 행은 '#')
        for (int j = 0; j < 2 * i - 1; j++) {
            if (i % 2 != 0) {
                printf("*");
            } else {
                printf("#");
            }
        }
        printf("\n");
    }

    return 0;
}
'''
