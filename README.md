#include <stdio.h>
#define MAX_EXPENSES 100

void save_expenses_to_file(float expenses[], int count) {
    FILE *file = fopen("expenses.txt", "w");
    if (file == NULL) {
        printf("Error opening file for writing!\n");
        return;
    }

    for (int i = 0; i < count; i++) {
        fprintf(file, "%.2f\n", expenses[i]);
    }

    fclose(file);
}

void load_expenses_from_file(float expenses[], int *count) {
    FILE *file = fopen("expenses.txt", "r");
    if (file == NULL) {
        printf("No previous expenses found.\n");
        return;
    }

    *count = 0;
    while (fscanf(file, "%f", &expenses[*count]) != EOF && *count < MAX_EXPENSES) {
        (*count)++;
    }

    fclose(file);
}

int main() {
    float expenses[MAX_EXPENSES]; 
    int count = 0; 
    int choice;
    float total = 0.0;

    
    load_expenses_from_file(expenses, &count);

    do {
        
        printf("\n*** Simple Personal Expense Tracker ***\n");
        printf("1. Add Expense\n");
        printf("2. View Expenses\n");
        printf("3. Total Expenses\n");
        printf("4. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1: 
                if (count >= MAX_EXPENSES) {
                    printf("Expense list is full!\n");
                } else {
                    printf("Enter expense amount: ");
                    scanf("%f", &expenses[count]);
                    count++;
                    save_expenses_to_file(expenses, count);  
                    printf("Expense added successfully!\n");
                }
                break;

            case 2: 
                if (count == 0) {
                    printf("No expenses to display.\n");
                } else {
                    printf("\nList of Expenses:\n");
                    for (int i = 0; i < count; i++) {
                        printf("Expense %d: Rs. %.2f\n", i + 1, expenses[i]);
                    }
                }
                break;

            case 3: 
                total = 0.0;
                for (int i = 0; i < count; i++) {
                    total += expenses[i];
                }
                printf("Total Expenses: Rs. %.2f\n", total);
                break;

            case 4: 
                printf("Exiting...\n");
                break;

            default:
                printf("Invalid choice. Please try again.\n");
        }
    } while (choice != 4);

    return 0;
}
