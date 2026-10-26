# 2번 문항
'''
#include <stdio.h>
#include <sqlite3.h>

// 데이터 조회 콜백 함수
static int callback(void* NotUsed, int argc, char** argv, char** azColName) {
    for (int i = 0; i < argc; i++) {
        printf("%s = %s\n", azColName[i], argv[i] ? argv[i] : "NULL");
    }
    printf("--------------------\n");
    return 0;
}

int main() {
    sqlite3* db;
    char* err_msg = 0;
    
    // 1. DB 연결 (생성)
    int rc = sqlite3_open("student.db", &db);
    if (rc != SQLITE_OK) {
        fprintf(stderr, "Cannot open database: %s\n", sqlite3_errmsg(db));
        sqlite3_close(db);
        return 1;
    }

    // 2. 테이블 생성 (SQL)
    const char* sql_create = "DROP TABLE IF EXISTS students;"
                              "CREATE TABLE students(id INT, name TEXT, major TEXT);";
    
    rc = sqlite3_exec(db, sql_create, 0, 0, &err_msg);
    if (rc != SQLITE_OK) {
        fprintf(stderr, "SQL error: %s\n", err_msg);
        sqlite3_free(err_msg);
        sqlite3_close(db);
        return 1;
    }

    // 3. 3명 이상의 데이터 삽입 (SQL)
    const char* sql_insert = "INSERT INTO students VALUES(2023001, '김철수', '컴퓨터공학');"
                              "INSERT INTO students VALUES(2023002, '박영희', '전자공학');"
                              "INSERT INTO students VALUES(2023003, '이민수', '데이터사이언스');";

    rc = sqlite3_exec(db, sql_insert, 0, 0, &err_msg);
    if (rc != SQLITE_OK) {
        fprintf(stderr, "SQL error: %s\n", err_msg);
        sqlite3_free(err_msg);
        sqlite3_close(db);
        return 1;
    }

    // 4. 데이터 검색 (SELECT)
    printf("=== Student Database Search Result ===\n");
    const char* sql_select = "SELECT * FROM students WHERE major = '컴퓨터공학';"; // 검색 예시
    
    rc = sqlite3_exec(db, sql_select, callback, 0, &err_msg);
    if (rc != SQLITE_OK) {
        fprintf(stderr, "Failed to select data: %s\n", err_msg);
        sqlite3_free(err_msg);
    }

    // 5. DB 연결 종료
    sqlite3_close(db);
    return 0;
}
'''
