#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>
#include <conio.h>
#include <windows.h>

#define NUM_ROUNDS 10
#define TIME_LIMIT 3000
#define AREA_WIDTH 76
#define AREA_HEIGHT 20
#define HALF_WIDTH 38
#define HALF_HEIGHT 10

typedef enum {
    TOP,
    BOTTOM,
    LEFT,
    RIGHT
} Direction;

typedef struct {
    int reaction_times[NUM_ROUNDS];
    int correct_responses;
    int total_attempts;
    double average_reaction_time;
    double accuracy_percentage;
    double final_score;
    char player_name[50];
} GameResult;

void clear_screen() {
    system("cls");
}

long get_time_ms() {
    return (long)(clock() * 1000.0 / CLOCKS_PER_SEC);
}

void display_welcome() {
    clear_screen();

    printf("\n");
    printf("================================================================================\n");
    printf("||                 HUNTINGTON'S DISEASE MOTOR CONTROL GAME                  ||\n");
    printf("||                       Reaction Time Test                                ||\n");
    printf("================================================================================\n");

    printf("\n");
    printf("This game measures your reaction time and accuracy.\n");
    printf("\n");

    printf("HOW TO PLAY:\n");
    printf("  1. An X will appear in one of the four zones.\n");
    printf("  2. Look at the direction of the X.\n");
    printf("  3. Press the corresponding key as quickly as possible.\n");
    printf("\n");

    printf("        W = UP       ^\n");
    printf("        S = DOWN     v\n");
    printf("        A = LEFT     <\n");
    printf("        D = RIGHT    >\n");

    printf("\n");
    printf("  4. You have 3 seconds to respond.\n");
    printf("  5. Complete all 10 rounds.\n");

    printf("\n");
    printf("================================================================================\n");
}

void get_player_name(char *name) {
    printf("\nPlease enter your name: ");
    fgets(name, 50, stdin);

    name[strcspn(name, "\n")] = 0;

    printf("\nWelcome, %s!\n", name);
}

void wait_for_enter() {
    printf("\nPress ENTER to start the game: ");
    getchar();
}

void draw_game_area(Direction target_position) {
    int row, col;

    printf("\n");

    printf("      ");
    for (col = 0; col < AREA_WIDTH; col++)
        printf("=");

    printf("\n");

    for (row = 0; row < AREA_HEIGHT; row++) {
        printf("      |");

        for (col = 0; col < AREA_WIDTH; col++) {

            int is_top = row < HALF_HEIGHT;
            int is_left = col < HALF_WIDTH;
            int print_x = 0;

            if (target_position == TOP &&
                is_top &&
                row == 5 &&
                col == 57) {
                print_x = 1;
            }
            else if (target_position == BOTTOM &&
                     !is_top &&
                     row == 14 &&
                     col == 57) {
                print_x = 1;
            }
            else if (target_position == LEFT &&
                     !is_top &&
                     is_left &&
                     row == 14 &&
                     col == 19) {
                print_x = 1;
            }
            else if (target_position == RIGHT &&
                     !is_top &&
                     !is_left &&
                     row == 14 &&
                     col == 57) {
                print_x = 1;
            }

            if (print_x) {
                printf(" X ");
                col += 2;
            }
            else if (target_position == TOP &&
                     row == 1 &&
                     col == 49) {
                printf(" W = UP ^ ");
                col += 9;
            }
            else if (target_position == BOTTOM &&
                     row == 18 &&
                     col == 49) {
                printf(" S = DOWN v ");
                col += 10;
            }
            else if (target_position == LEFT &&
                     row == 13 &&
                     col == 10) {
                printf(" A = LEFT < ");
                col += 10;
            }
            else if (target_position == RIGHT &&
                     row == 13 &&
                     col == 49) {
                printf(" > RIGHT = D ");
                col += 11;
            }
            else if (col == HALF_WIDTH - 1) {
                printf("|");
            }
            else if (row == HALF_HEIGHT - 1) {
                printf("-");
            }
            else {
                printf(" ");
            }
        }

        printf("|\n");
    }

    printf("      ");
    for (col = 0; col < AREA_WIDTH; col++)
        printf("=");

    printf("\n");
}

int display_target_and_wait(int round, Direction *target_position,
                            int *reaction_time) {
    clear_screen();

    *target_position = (Direction)(rand() % 4);

    printf("\n");
    printf("                         ROUND %d / %d\n",
           round + 1, NUM_ROUNDS);
    printf("                       TIME LIMIT: 3 SECONDS\n");

    printf("\n");
    printf("                 LOOK AT THE X AND PRESS:\n");
    printf("\n");

    printf("                  W = UP ^\n");
    printf("                  S = DOWN v\n");
    printf("                  A = LEFT <\n");
    printf("                  D = RIGHT >\n");

    draw_game_area(*target_position);

    printf("\n");
    printf("              +--------------------------------+\n");
    printf("              |  W = UP    S = DOWN            |\n");
    printf("              |  A = LEFT  D = RIGHT           |\n");
    printf("              +--------------------------------+\n");

    printf("\n");

    fflush(stdout);

    long start_time = get_time_ms();
    int user_response;

    while (1) {
        long current_time = get_time_ms();

        *reaction_time = current_time - start_time;

        if (*reaction_time > TIME_LIMIT) {
            printf("\n        TIME OUT! You didn't respond in time.\n");
            Sleep(1500);
            return 0;
        }

        if (kbhit()) {
            user_response = getch();

            Direction user_direction = (Direction)(-1);

            switch (user_response) {
                case 'w':
                case 'W':
                    user_direction = TOP;
                    break;

                case 's':
                case 'S':
                    user_direction = BOTTOM;
                    break;

                case 'a':
                case 'A':
                    user_direction = LEFT;
                    break;

                case 'd':
                case 'D':
                    user_direction = RIGHT;
                    break;

                default:
                    printf("\n        Invalid key! Use W, A, S, or D only.\n");
                    Sleep(500);
                    continue;
            }

            if (user_direction == *target_position) {
                printf("\n        CORRECT! Reaction time: %d ms\n",
                       *reaction_time);
                Sleep(1200);
                return 1;
            }
            else {
                printf("\n        WRONG! Expected: ");

                switch (*target_position) {
                    case TOP:
                        printf("W (UP)");
                        break;

                    case BOTTOM:
                        printf("S (DOWN)");
                        break;

                    case LEFT:
                        printf("A (LEFT)");
                        break;

                    case RIGHT:
                        printf("D (RIGHT)");
                        break;
                }

                printf(" | You pressed: ");

                switch (user_direction) {
                    case TOP:
                        printf("W (UP)");
                        break;

                    case BOTTOM:
                        printf("S (DOWN)");
                        break;

                    case LEFT:
                        printf("A (LEFT)");
                        break;

                    case RIGHT:
                        printf("D (RIGHT)");
                        break;
                }

                printf("\n");
                printf("        Reaction time: %d ms\n", *reaction_time);

                Sleep(1200);
                return 0;
            }
        }

        Sleep(10);
    }
}

void calculate_score(GameResult *result) {
    int total_reaction = 0;

    for (int i = 0; i < NUM_ROUNDS; i++)
        total_reaction += result->reaction_times[i];

    result->average_reaction_time =
        total_reaction / (double)NUM_ROUNDS;

    result->accuracy_percentage =
        (result->correct_responses / (double)NUM_ROUNDS) * 100;

    double reaction_score = 0;

    if (result->average_reaction_time < 500) {
        reaction_score = 100;
    }
    else if (result->average_reaction_time < 800) {
        reaction_score =
            100 - ((result->average_reaction_time - 500) / 300) * 30;
    }
    else if (result->average_reaction_time < 1300) {
        reaction_score =
            70 - ((result->average_reaction_time - 800) / 500) * 20;
    }
    else {
        reaction_score =
            50 - ((result->average_reaction_time - 1300) / 700) * 10;

        if (reaction_score < 30)
            reaction_score = 30;
    }

    result->final_score =
        (reaction_score * 0.6) +
        (result->accuracy_percentage * 0.4);

    if (result->final_score > 100)
        result->final_score = 100;
}

void display_results(GameResult *result) {
    clear_screen();

    printf("\n");
    printf("================================================================================\n");
    printf("||                         GAME RESULTS SUMMARY                              ||\n");
    printf("================================================================================\n");

    printf("\n");
    printf("Player Name: %s\n", result->player_name);

    printf("\n");
    printf("--------------------------------------------------------------------------------\n");
    printf("PERFORMANCE METRICS:\n");
    printf("--------------------------------------------------------------------------------\n");

    printf("  Total Rounds:              %d / %d\n",
           NUM_ROUNDS, NUM_ROUNDS);

    printf("  Correct Responses:         %d / %d\n",
           result->correct_responses, NUM_ROUNDS);

    printf("  Accuracy:                   %.2f%%\n",
           result->accuracy_percentage);

    printf("  Average Reaction Time:      %.0f ms\n",
           result->average_reaction_time);

    printf("\n");

    printf("  Reaction Score (60%%):      ");

    if (result->average_reaction_time < 500)
        printf("EXCELLENT (< 500 ms)\n");
    else if (result->average_reaction_time < 800)
        printf("GOOD (500-800 ms)\n");
    else if (result->average_reaction_time < 1300)
        printf("FAIR (800-1300 ms)\n");
    else
        printf("NEEDS IMPROVEMENT (> 1300 ms)\n");

    printf("\n");

    printf("================================================================================\n");
    printf("FINAL SCORE:                 %.1f / 100\n",
           result->final_score);
    printf("================================================================================\n");

    printf("\n");
    printf("PERFORMANCE INTERPRETATION:\n");
    printf("--------------------------------------------------------------------------------\n");

    if (result->final_score >= 80) {
        printf("EXCELLENT PERFORMANCE\n");
        printf("  Your reaction time and accuracy were excellent!\n");
        printf("  This indicates good motor control and coordination.\n");
    }
    else if (result->final_score >= 65) {
        printf("GOOD PERFORMANCE\n");
        printf("  Your results show solid reaction time and accuracy.\n");
    }
    else if (result->final_score >= 50) {
        printf("FAIR PERFORMANCE\n");
        printf("  Your results show some variation in reaction time or accuracy.\n");
        printf("  Try again when you're focused and rested.\n");
    }
    else {
        printf("LOWER PERFORMANCE\n");
        printf("  Your score was lower in this test.\n");
        printf("  Please try again when well-rested and focused.\n");
        printf("  If you have concerns about motor control, consult a healthcare professional.\n");
    }

    printf("\n");
    printf("--------------------------------------------------------------------------------\n");
    printf("IMPORTANT NOTES:\n");
    printf("  - This is a SCREENING tool, not a diagnostic tool.\n");
    printf("  - Results can vary based on fatigue, focus, and practice.\n");
    printf("  - Reaction time is measured in milliseconds.\n");
    printf("  - For medical concerns, consult a healthcare professional.\n");
    printf("--------------------------------------------------------------------------------\n");

    printf("\n");
    printf("Press ENTER to exit: ");
    getchar();
}

int main() {
    srand((unsigned int)time(NULL));

    GameResult result;

    result.correct_responses = 0;
    result.total_attempts = 0;

    display_welcome();

    get_player_name(result.player_name);

    wait_for_enter();

    for (int round = 0; round < NUM_ROUNDS; round++) {
        Direction target_position;
        int reaction_time;

        int is_correct = display_target_and_wait(
            round,
            &target_position,
            &reaction_time
        );

        result.reaction_times[round] = reaction_time;
        result.total_attempts++;

        if (is_correct)
            result.correct_responses++;
    }

    calculate_score(&result);
    display_results(&result);

    return 0;
}
