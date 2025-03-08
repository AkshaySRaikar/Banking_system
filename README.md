#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_ACCOUNTS 10
#define FILENAME "bank_accounts.txt"

typedef struct Account
{
    unsigned long long int accountNumber;
    float balance;
    char name[25];
    unsigned long long int adhaar;
    unsigned long long int mobile;
    int age;
} ACCOUNT;

void writeAccounts(ACCOUNT accounts[], int numAccounts)
{
    FILE *file = fopen("assignment4", "w");
    if (file == NULL)
    {
        printf("Error opening file.\n");
        return;
    }

    for (int i = 0; i < numAccounts; i++)
    {
        fprintf(file, "%llu %f %s %llu %llu %d\n", accounts[i].accountNumber, accounts[i].balance, accounts[i].name,
                accounts[i].adhaar, accounts[i].mobile, accounts[i].age);
    }

    fclose(file);
}

void createAccount(ACCOUNT accounts[], int *numAccounts)
{
    if (*numAccounts >= MAX_ACCOUNTS)
    {
        printf("Cannot create more accounts. Limit reached.\n");
        return;
    }

    ACCOUNT newAccount;
    printf("Enter account number: ");
    scanf("%llu", &newAccount.accountNumber);
    printf("Enter name: ");
    scanf("%s", newAccount.name);
    printf("Enter Aadhaar number: ");
    scanf("%llu", &newAccount.adhaar);
    printf("Enter mobile number: ");
    scanf("%llu", &newAccount.mobile);
    printf("Enter age: ");
    scanf("%d", &newAccount.age);
    newAccount.balance = 0;

    accounts[*numAccounts] = newAccount;
    (*numAccounts)++;

    writeAccounts(accounts, *numAccounts);
}

void deposit(ACCOUNT accounts[], int numAccounts, unsigned long long int accountNumber, float amount)
{
    for (int i = 0; i < numAccounts; i++)
    {
        if (accounts[i].accountNumber == accountNumber)
        {
            accounts[i].balance += amount;
            printf("Deposit successful. New balance: %.2f\n", accounts[i].balance);
            writeAccounts(accounts, numAccounts);
            return;
        }
    }
    printf("Account not found.\n");
}

void withdraw(ACCOUNT accounts[], int numAccounts, unsigned long long int accountNumber, float amount)
{
    for (int i = 0; i < numAccounts; i++)
    {
        if (accounts[i].accountNumber == accountNumber)
        {
            if (accounts[i].balance >= amount)
            {
                accounts[i].balance -= amount;
                printf("Withdrawal successful. New balance: %.2f\n", accounts[i].balance);
                writeAccounts(accounts, numAccounts);
            }
            else
            {
                printf("Insufficient balance.\n");
            }
            return;
        }
    }
    printf("Account not found.\n");
}

void display(ACCOUNT accounts[], int numAccounts, unsigned long long int accountNumber)
{
    for (int i = 0; i < numAccounts; i++)
    {
        if (accounts[i].accountNumber == accountNumber)
        {
            printf("Account Number: %llu\n", accounts[i].accountNumber);
            printf("Name: %s\n", accounts[i].name);
            printf("Aadhaar: %llu\n", accounts[i].adhaar);
            printf("Mobile: %llu\n", accounts[i].mobile);
            printf("Age: %d\n", accounts[i].age);
            printf("Balance: %.2f\n", accounts[i].balance);
            return;
        }
    }
    printf("Account not found.\n");
}

int main()
{
    ACCOUNT accounts[MAX_ACCOUNTS];
    int numAccounts = 0;
    int choice;
    unsigned long long int accountNumber;
    float amount;

    printf("Welcome to PESU Banking System\n");

    do
    {
        printf("\nBanking System Menu:\n");
        printf("1. Create Account\n");
        printf("2. Deposit\n");
        printf("3. Withdraw\n");
        printf("4. Display Account\n");
        printf("5. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice)
        {
        case 1:
            createAccount(accounts, &numAccounts);
            break;
        case 2:
            printf("Enter account number: ");
            scanf("%llu", &accountNumber);
            printf("Enter amount to deposit: ");
            scanf("%f", &amount);
            deposit(accounts, numAccounts, accountNumber, amount);
            break;
        case 3:
            printf("Enter account number: ");
            scanf("%llu", &accountNumber);
            printf("Enter amount to withdraw: ");
            scanf("%f", &amount);
            withdraw(accounts, numAccounts, accountNumber, amount);
            break;
        case 4:
            printf("Enter account number: ");
            scanf("%llu", &accountNumber);
            display(accounts, numAccounts, accountNumber);
            break;
        case 5:
            printf("Thank You for using our Banking System\n");
            break;
        default:
            printf("Invalid choice. Please try again.\n");
        }
    } while (choice != 5);

    return 0;
}
