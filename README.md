# Tugas-BesarAlpro
Pemograman ATM sederhana
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_LEN 100
#define PIN "123456"

typedef struct {
    double balance;
} ATM;

void clear() {
    system("cls");
}

void check_balance(ATM *atm) {
    clear();
    printf("Saldo Anda: Rp %.2f\n", atm->balance);
}

void withdraw(ATM *atm, double amount, int denomination) {
    clear();
    if (amount <= atm->balance) {
        atm->balance -= amount;
        printf("Anda telah menarik tunai sebesar Rp %.2f dengan pecahan Rp %d. Saldo Anda sekarang: Rp %.2f\n", amount, denomination, atm->balance);
    } else {
        printf("Saldo tidak mencukupi.\n");
    }
}

void quick_withdraw(ATM *atm) {
    clear();
    int choice;
    double amount = 0;
    int denomination = 0;

    printf("Pilihan Tarik Cepat:\n");
    printf("1. Rp 100000\n");
    printf("2. Rp 300000\n");
    printf("3. Rp 500000\n");
    printf("4. Rp 1000000\n");
    printf("5. Input Nilai Lain\n");
    printf("Pilih opsi: ");
    scanf("%d", &choice);

    switch (choice) {
        case 1: amount = 100000; break;
        case 2: amount = 300000; break;
        case 3: amount = 500000; break;
        case 4: amount = 1000000; break;
        case 5: 
            printf("Masukkan jumlah yang ingin ditarik: Rp ");
            scanf("%lf", &amount);
            break;
        default:
            printf("Pilihan tidak valid.\n");
            return;
    }

    printf("Pilih pecahan uang:\n");
    printf("1. Rp 100000\n");
    printf("2. Rp 50000\n");
    printf("Pilih opsi: ");
    scanf("%d", &choice);

    switch (choice) {
        case 1: denomination = 100000; break;
        case 2: denomination = 50000; break;
        default:
            printf("Pilihan tidak valid.\n");
            return;
    }

    withdraw(atm, amount, denomination);
}

void transfer(ATM *atm) {
    clear();
    char account_no[MAX_LEN];
    double amount;

    printf("Masukkan nomor rekening tujuan (11 digit): ");
    scanf("%s", account_no);

    if (strlen(account_no) != 11) {
        printf("Nomor rekening tidak valid.\n");
        return;
    }

    printf("Masukkan jumlah yang ingin ditransfer: Rp ");
    scanf("%lf", &amount);

    if (amount <= atm->balance) {
        atm->balance -= amount;
        printf("Anda telah mentransfer Rp %.2f ke rekening %s. Saldo Anda sekarang: Rp %.2f\n", amount, account_no, atm->balance);
    } else {
        printf("Saldo tidak mencukupi.\n");
    }
}

void deposit(ATM *atm, double amount) {
    clear();
    atm->balance += amount;
    printf("Anda telah menyetor tunai sebesar Rp %.2f. Saldo Anda sekarang: Rp %.2f\n", amount, atm->balance);
}

void top_up_dana(ATM *atm) {
    clear();
    char dana_no[MAX_LEN];
    double amount;

    printf("Masukkan nomor DANA (12 digit): ");
    scanf("%s", dana_no);

    if (strlen(dana_no) != 12) {
        printf("Nomor DANA tidak valid.\n");
        return;
    }

    printf("\nMasukkan jumlah Top UP DANA: Rp ");
    scanf("%lf", &amount);

    if (amount <= atm->balance) {
        atm->balance -= amount;
        printf("Anda telah Top UP DANA sebesar Rp %.2f ke nomor %s. Saldo Anda sekarang: Rp %.2f\n", amount, dana_no, atm->balance);
    } else {
        printf("Saldo tidak mencukupi.\n");
    }
}

int login() {
    clear();
    char pin[MAX_LEN];

    printf("=====TERIMAKASIH TELAH MENGGUNAKAN LAYANAN KAMI=====\n");
    printf("=========SELAMAT DATANG DI BANK GACOR==========\n");
    printf("============BANK MILIK BAPAK ISKANDAR========\n");
    printf("\nMasukkan PIN Anda: ");
    scanf("%s", pin);

    if (strcmp(pin, PIN) == 0) {
        return 1;
    } else {
        printf("\nPIN salah. Silakan coba lagi.\n");
        return 0;
    }
}

void atm_menu(ATM *atm) {
    int choice;
    double amount;

    while (1) {
        clear();
        printf("\nMenu:\n");
        printf("1. Cek Saldo\n");
        printf("2. Tarik Tunai\n");
        printf("3. Transfer\n");
        printf("4. Setor Tunai\n");
        printf("5. Top UP Dana\n");
        printf("6. Keluar\n");

        printf("\nPilih menu (1-6): ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                check_balance(atm);
                break;
            case 2:
                quick_withdraw(atm);
                break;
            case 3:
                transfer(atm);
                break;
            case 4:
                printf("\nMasukkan jumlah yang ingin disetor: Rp ");
                scanf("%lf", &amount);
                deposit(atm, amount);
                break;
            case 5:
                top_up_dana(atm);
                break;
            case 6:
                clear();
                printf("\n======Terima kasih telah menggunakan layanan ATM BANK GACOR=======\n");
                exit(0);
            default:
                clear();
                printf("===Pilihan tidak valid. Silakan coba lagi===\n");
        }

        printf("Apakah Anda ingin melakukan transaksi lain? \n(1: Ya, 2: Tidak): ");
        scanf("%d", &choice);

        if (choice != 1) {
            clear();
            printf("======Terima kasih telah menggunakan layanan ATM BANK GACOR=====\n");
            break;
        }
    }
}

int main() {
    ATM atm = {10000000};

    clear();

    if (login()) {
        atm_menu(&atm);
    }

    return 0;
}
