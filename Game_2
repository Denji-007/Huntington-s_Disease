#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>
#include <conio.h>
#include <windows.h>

#define MAX_ROUNDS 5
#define DISPLAY_TIME 2500  
#define INITIAL_SEQUENCE_LENGTH 2

typedef enum {
    KEY_A = 0,
    KEY_S = 1,
    KEY_D = 2,
    KEY_F = 3,
    KEY_G = 4,
    KEY_H = 5,
    KEY_J = 6,
    KEY_K = 7
} KeyOption;

#define TOTAL_KEYS 8

typedef struct {
    int round_number;
    int sequence_length;
    int correct;
    KeyOption sequence[MAX_ROUNDS + INITIAL_SEQUENCE_LENGTH];
} RoundStats;

typedef struct {
    char player_name[50];
    int rounds_completed;
    int rounds_passed;
    int max_sequence_length;
    double accuracy_percentage;
    int reaction_times[MAX_ROUNDS];
    RoundStats round_stats[MAX_ROUNDS];
    double final_score;
} GameResult;

void clear_screen() {
    system("cls");
}

long get_time_ms() {
    return (long)(clock() * 1000.0 / CLOCKS_PER_SEC);
}

char direction_to_char(KeyOption key) {
    switch (key) {
        case KEY_A: return 'A';
        case KEY_S: return 'S';
        case KEY_D: return 'D';
        case KEY_F: return 'F';
        case KEY_G: return 'G';
        case KEY_H: return 'H';
        case KEY_J: return 'J';
        case KEY_K: return 'K';
        default: return '?';
    }
}

KeyOption char_to_direction(char c) {
    switch (c) {
        case 'a':
        case 'A': return KEY_A;
        case 's':
        case 'S': return KEY_S;
        case 'd':
        case 'D': return KEY_D;
        case 'f':
        case 'F': return KEY_F;
        case 'g':
        case 'G': return KEY_G;
        case 'h':
        case 'H': return KEY_H;
        case 'j':
        case 'J': return KEY_J;
        case 'k':
        case 'K': return KEY_K;
        default: return (KeyOption)(-1);
    }
}

void display_welcome() {
    clear_screen();
    printf("\n");
    printf("================================================================================\n");
    printf("||              PATTERN MEMORY GAME                                         ||\n");
    printf("||         Test Your Memory and Attention Span                              ||\n");
    printf("================================================================================\n");
    printf("\n");
    printf("Remember the letters and type them in the same order.\n");
    printf("The sequence gets longer in each round.\n");
    printf("\n");
    printf("HOW TO PLAY:\n");
    printf("  1. A sequence of random letters will be displayed (e.g., A S G K)\n");
    printf("  2. The sequence will be hidden after 2-3 seconds\n");
    printf("  3. You must type the sequence exactly as shown\n");
    printf("  4. Each round adds one or more keys to the sequence\n");
    printf("  5. There are 5 rounds in total\n");
    printf("\n");
    printf("AVAILABLE KEYS:\n");
    printf("  A  S  D  F  G  H  J  K\n");
    printf("\n");
    printf("DIFFICULTY:\n");
    printf("  Round 1: 2 keys    Round 2: 3 keys    Round 3: 4 keys\n");
    printf("  Round 4: 5 keys    Round 5: 6 keys\n");
    printf("\n");
    printf("================================================================================\n");
}

void get_player_name(char* name) {
    printf("\nPlease enter your name: ");
    fgets(name, 50, stdin);
    name[strcspn(name, "\n")] = 0;
    printf("\nWelcome, %s!\n\n", name);
}

void wait_for_enter() {
    printf("\nPress ENTER to start the game: ");
    getchar();
}

void display_sequence(KeyOption* sequence, int length, int round_num) {
    clear_screen();
    printf("\n");
    printf("================================================================================\n");
    printf("                    ROUND %d / %d\n", round_num, MAX_ROUNDS);
    printf("                    MEMORIZE THIS SEQUENCE\n");
    printf("================================================================================\n");
    printf("\n");
    printf("                         Sequence:");
    printf("\n\n");

    printf("      ");
    for (int i = 0; i < length; i++) {
        printf("+-----+  ");
    }
    printf("\n");
    
    printf("      ");
    for (int i = 0; i < length; i++) {
        printf("¦  %c  ¦  ", direction_to_char(sequence[i]));
    }
    printf("\n");
    
    printf("      ");
    for (int i = 0; i < length; i++) {
        printf("+-----+  ");
    }
    printf("\n\n");

    printf("      ");
    for (int i = 0; i < length; i++) {
        printf("  %d     ", i + 1);
    }
    printf("\n\n");
    
    printf("    Sequence Length: %d\n", length);
    printf("    Watch carefully... the sequence will disappear soon!\n");
    printf("\n");
    printf("================================================================================\n");
    
    fflush(stdout);
}

void hide_sequence() {
    printf("\n                   Sequence will disappear in...\n");
    printf("                        3... ");
    fflush(stdout);
    Sleep(1000);
    printf("2... ");
    fflush(stdout);
    Sleep(1000);
    printf("1...\n");
    fflush(stdout);
    Sleep(500);
    
    clear_screen();
    printf("\n");
    printf("================================================================================\n");
    printf("                  SEQUENCE HIDDEN - YOUR TURN!\n");
    printf("================================================================================\n");
    printf("\n");
    printf("  Type the sequence you remember (press keys in order)\n\n");
    printf("  Available keys: A  S  D  F  G  H  J  K\n\n");
    printf("  Waiting for input...\n");
    printf("\n");
    printf("================================================================================\n");
}

int get_player_response(KeyOption* sequence, int length, int* reaction_time) {
    long start_time = get_time_ms();
    
    for (int i = 0; i < length; i++) {
        int key_pressed = 0;

        while (1) {
            if (kbhit()) {
                char input = getch();
                KeyOption direction = char_to_direction(input);
                
                if (direction != -1) {
                    key_pressed = 1;
                    printf("%c ", direction_to_char(direction));
                    fflush(stdout);
                    
                    *reaction_time = get_time_ms() - start_time;

                    if (direction != sequence[i]) {
                        printf("\n\n? WRONG! Expected '%c' but got '%c' at position %d\n", 
                               direction_to_char(sequence[i]),
                               direction_to_char(direction),
                               i + 1);
                        Sleep(2000);
                        return 0;
                    }
                    break;
                } else {
                    printf("\n? Invalid key! Use A, S, D, F, G, H, J, K only.\n");
                    printf("Waiting for input...\n");
                }
            }
            Sleep(10);
        }
    }
    
    printf("\n\n? CORRECT! You remembered the entire sequence!\n");
    printf("Reaction time: %d ms\n", *reaction_time);
    Sleep(1500);
    return 1;
}

void generate_sequence(KeyOption* sequence, int length) {
    for (int i = 0; i < length; i++) {
        sequence[i] = (KeyOption)(rand() % TOTAL_KEYS);
    }
}

void run_game_round(int round, GameResult* result) {
    int sequence_length = INITIAL_SEQUENCE_LENGTH + round;
    KeyOption sequence[MAX_ROUNDS + INITIAL_SEQUENCE_LENGTH];

    generate_sequence(sequence, sequence_length);

    result->round_stats[round].round_number = round + 1;
    result->round_stats[round].sequence_length = sequence_length;

    for (int i = 0; i < sequence_length; i++) {
        result->round_stats[round].sequence[i] = sequence[i];
    }

    display_sequence(sequence, sequence_length, round + 1);
    Sleep(DISPLAY_TIME);

    hide_sequence();
    int reaction_time = 0;
    int is_correct = get_player_response(sequence, sequence_length, &reaction_time);
    
    result->reaction_times[round] = reaction_time;
    result->round_stats[round].correct = is_correct;

    result->max_sequence_length = sequence_length;
    
    if (is_correct) {
        result->rounds_passed++;
    }
    
    result->rounds_completed++;
}

void calculate_score(GameResult* result) {
    if (result->rounds_completed == 0) {
        result->accuracy_percentage = 0;
        result->final_score = 0;
        return;
    }

    result->accuracy_percentage = (result->rounds_passed / (double)result->rounds_completed) * 100;

    double rounds_score = (result->rounds_passed / (double)MAX_ROUNDS) * 100;

    double max_possible_length = INITIAL_SEQUENCE_LENGTH + MAX_ROUNDS - 1;

    double sequence_score = (result->max_sequence_length / max_possible_length) * 100;

    result->final_score = (rounds_score * 0.6) + (sequence_score * 0.4);

    if (result->final_score > 100)
        result->final_score = 100;
}

void display_results(GameResult* result) {
    clear_screen();
    printf("\n");
    printf("================================================================================\n");
    printf("||                    GAME RESULTS SUMMARY                                   ||\n");
    printf("================================================================================\n");
    printf("\n");
    printf("Player Name: %s\n", result->player_name);
    printf("\n");
    printf("--------------------------------------------------------------------------------\n");
    printf("PERFORMANCE METRICS:\n");
    printf("--------------------------------------------------------------------------------\n");
    printf("  Rounds Completed:          %d\n", result->rounds_completed);
    printf("  Rounds Passed:             %d\n", result->rounds_passed);
    printf("  Accuracy:                  %.2f%%\n", result->accuracy_percentage);
    printf("  Maximum Sequence Length:   %d keys\n", result->max_sequence_length);
    printf("  Starting Sequence Length:  %d keys\n\n", INITIAL_SEQUENCE_LENGTH);
    
    printf("  Difficulty Progression:\n");
    for (int i = 0; i < result->rounds_completed; i++) {
        printf("    Round %d: %d keys - %s\n", 
               result->round_stats[i].round_number,
               result->round_stats[i].sequence_length,
               result->round_stats[i].correct ? "? PASSED" : "? FAILED");
    }
    printf("\n");
    printf("================================================================================\n");
    printf("FINAL SCORE:                 %.1f / 100\n", result->final_score);
    printf("================================================================================\n");
    printf("\n");

    printf("PERFORMANCE INTERPRETATION:\n");
    printf("--------------------------------------------------------------------------------\n");

    if (result->final_score >= 85) {
        printf("EXCELLENT MEMORY & ATTENTION\n");
        printf("You performed very well in this memory test.\n");
    } else if (result->final_score >= 70) {
        printf("GOOD MEMORY & ATTENTION\n");
        printf("You showed good performance in this memory test.\n");
    } else if (result->final_score >= 50) {
        printf("FAIR MEMORY & ATTENTION\n");
        printf("Your score is in the 50-69 range.\n");
    } else {
        printf("LOWER MEMORY & ATTENTION PERFORMANCE\n");
        printf("Your score is below 50.\n");
    }

    if (result->final_score < 30) {
        printf("\n");
        printf("HEALTH NOTICE:\n");
        printf("Your score is severely low in this memory exercise.\n");
        printf("This result does NOT mean that you have Huntington's disease.\n");
        printf("If you have concerns about your memory or symptoms, please consult a doctor.\n");
        printf("A doctor can check the cause of memory problems properly.\n");
    }

    printf("\n");
    printf("IMPORTANT HEALTH INFORMATION:\n");
    printf("This game is only a memory exercise and cannot diagnose Huntington's disease.\n");
    printf("A low score alone does not mean that you have Huntington's disease.\n");

    printf("\n");
    printf("--------------------------------------------------------------------------------\n");
    printf("MEMORY & ATTENTION INSIGHTS:\n");
    printf("  • Working Memory Capacity: Successfully handled up to %d items\n", result->max_sequence_length);
    printf("  • Success Rate: %d out of %d sequences\n", result->rounds_passed, result->rounds_completed);
    
    if (result->rounds_passed > 0) {
        int total_reaction = 0;
        for (int i = 0; i < result->rounds_passed; i++) {
            total_reaction += result->reaction_times[i];
        }
        int avg_reaction = total_reaction / result->rounds_passed;
        printf("  • Average Reaction Time: %d ms\n", avg_reaction);
    }
    
    printf("--------------------------------------------------------------------------------\n");
    printf("IMPORTANT NOTES:\n");
    printf("  • This is a memory screening tool, not a diagnostic assessment\n");
    printf("  • Results may vary based on fatigue, focus, and practice\n");
    printf("  • Memory performance naturally improves with repeated practice\n");
    printf("  • For concerns about memory changes, consult a healthcare professional\n");
    printf("--------------------------------------------------------------------------------\n");
    printf("\n");
    printf("Press ENTER to exit: ");
    getchar();
}

int main() {
    srand((unsigned int)time(NULL));
    
    GameResult result;
    memset(&result, 0, sizeof(GameResult));
    result.rounds_completed = 0;
    result.rounds_passed = 0;
    result.max_sequence_length = 0;
    
    display_welcome();
    get_player_name(result.player_name);
    wait_for_enter();

    for (int round = 0; round < MAX_ROUNDS; round++) {
        run_game_round(round, &result);

        if (result.round_stats[round].correct == 0) {
            printf("\n??  You made a mistake in this round, but don't worry!\n");
        } else {
            printf("\n? Great job! You passed this round!\n");
        }

        if (round < MAX_ROUNDS - 1) {
            printf("Moving to the next round...\n");
            Sleep(2000);
        } else {
            printf("\n?? All 5 rounds completed! Calculating your score...\n");
            Sleep(2000);
        }
    }

    calculate_score(&result);
    display_results(&result);
    
    return 0;
}
