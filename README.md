#include <stdio.h>
#include <stdlib.h>
#include <time.h>

void recordHistory(double amount, double result, char from[10], char to[10]) {
    FILE *file = fopen("history.txt", "a"); // append mode
    if (file == NULL) {
        printf("Error opening file!\n");
        return;
    }

    time_t t;
    time(&t);
    fprintf(file, "%s: %.2f %s = %.2f %s\n", ctime(&t), amount, from, result, to);
    fclose(file);
}

void showHistory() {
    FILE *file = fopen("history.txt", "r");
    if (file == NULL) {
        printf("\nNo history found.\n");
        return;
    }

    char line[200];
    printf("\n=== Conversion History ===\n");
    while (fgets(line, sizeof(line), file)) {
        printf("%s", line);
    }
    fclose(file);
}

int main() {
    int choice;
    double amount, converted;
    char from[10], to[10];

    // Example static rates (for simplicity)
    // 1 USD = 110 BDT, 1 EUR = 120 BDT, 1 INR = 1.35 BDT
    const double USD_to_BDT = 110.0;
    const double EUR_to_BDT = 120.0;
    const double INR_to_BDT = 1.35;

    while (1) {
        printf("\n===== Currency Converter =====\n");
        printf("1. USD to BDT\n");
        printf("2. BDT to USD\n");
        printf("3. EUR to BDT\n");
        printf("4. BDT to EUR\n");
        printf("5. INR to BDT\n");
        printf("6. BDT to INR\n");
        printf("7. Show Conversion History\n");
        printf("8. Exit\n");
        printf("Choose an option: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter amount in USD: ");
                scanf("%lf", &amount);
                converted = amount * USD_to_BDT;
                printf("Result: %.2f USD = %.2f BDT\n", amount, converted);
                recordHistory(amount, converted, "USD", "BDT");
                break;
