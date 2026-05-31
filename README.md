
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define MAX_WORDS 1000
#define MAX_LENGTH 100

// ================= DISPLAY FUNCTIONS =================
void tampilAngka(int arr[], int n) {
    int limit = (n > 20) ? 20 : n;
    printf("\n--- Preview (%d data pertama dari %d) ---\n", limit, n);
    for(int i = 0; i < limit; i++) {
        printf("%4d ", arr[i]);
        if((i+1) % 10 == 0) printf("\n");
    }
    if(n > 20) printf("\n... dan %d data lainnya\n", n - 20);
    printf("\n");
}

void tampilData(char arr[][MAX_LENGTH], int n) {
    int limit = (n > 20) ? 20 : n;
    printf("\n--- Preview (%d data pertama dari %d) ---\n", limit, n);
    for(int i = 0; i < limit; i++) {
        printf("%s\n", arr[i]);
    }
    if(n > 20) printf("... dan %d data lainnya\n", n - 20);
    printf("\n");
}

// ================= BASIC SORTING (INTEGER) =================
void bubbleSort(int arr[], int n) {
    for(int i = 0; i < n-1; i++) {
        for(int j = 0; j < n-i-1; j++) {
            if(arr[j] > arr[j+1]) {
                int temp = arr[j];
                arr[j] = arr[j+1];
                arr[j+1] = temp;
            }
        }
    }
}

void insertionSort(int arr[], int n) {
    for(int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        while(j >= 0 && arr[j] > key) {
            arr[j+1] = arr[j];
            j--;
        }
        arr[j+1] = key;
    }
}

void selectionSort(int arr[], int n) {
    for(int i = 0; i < n-1; i++) {
        int min_idx = i;
        for(int j = i+1; j < n; j++) {
            if(arr[j] < arr[min_idx])
                min_idx = j;
        }
        int temp = arr[min_idx];
        arr[min_idx] = arr[i];
        arr[i] = temp;
    }
}

// ================= ADVANCE SORTING (STRING) =================

// Merge Sort
void merge(char arr[][MAX_LENGTH], int l, int m, int r) {
    int n1 = m - l + 1;
    int n2 = r - m;

    char L[n1][MAX_LENGTH], R[n2][MAX_LENGTH];

    for(int i = 0; i < n1; i++)
        strcpy(L[i], arr[l + i]);
    for(int j = 0; j < n2; j++)
        strcpy(R[j], arr[m + 1 + j]);

    int i = 0, j = 0, k = l;
    while(i < n1 && j < n2) {
        if(strcmp(L[i], R[j]) <= 0) {
            strcpy(arr[k], L[i]);
            i++;
        } else {
            strcpy(arr[k], R[j]);
            j++;
        }
        k++;
    }

    while(i < n1) {
        strcpy(arr[k], L[i]);
        i++;
        k++;
    }

    while(j < n2) {
        strcpy(arr[k], R[j]);
        j++;
        k++;
    }
}

void mergeSort(char arr[][MAX_LENGTH], int l, int r) {
    if(l < r) {
        int m = l + (r - l) / 2;
        mergeSort(arr, l, m);
        mergeSort(arr, m + 1, r);
        merge(arr, l, m, r);
    }
}

// Quick Sort
int partition(char arr[][MAX_LENGTH], int low, int high) {
    char pivot[MAX_LENGTH];
    strcpy(pivot, arr[high]);
    int i = low - 1;

    for(int j = low; j < high; j++) {
        if(strcmp(arr[j], pivot) < 0) {
            i++;
            char temp[MAX_LENGTH];
            strcpy(temp, arr[i]);
            strcpy(arr[i], arr[j]);
            strcpy(arr[j], temp);
        }
    }
    char temp[MAX_LENGTH];
    strcpy(temp, arr[i+1]);
    strcpy(arr[i+1], arr[high]);
    strcpy(arr[high], temp);
    return i + 1;
}

void quickSort(char arr[][MAX_LENGTH], int low, int high) {
    if(low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

// Shell Sort
void shellSort(char arr[][MAX_LENGTH], int n) {
    for(int gap = n/2; gap > 0; gap /= 2) {
        for(int i = gap; i < n; i++) {
            char temp[MAX_LENGTH];
            strcpy(temp, arr[i]);
            int j;
            for(j = i; j >= gap && strcmp(arr[j-gap], temp) > 0; j -= gap) {
                strcpy(arr[j], arr[j-gap]);
            }
            strcpy(arr[j], temp);
        }
    }
}

// ================= COPY FUNCTIONS =================
void copyIntArray(int src[], int dest[], int n) {
    for(int i = 0; i < n; i++)
        dest[i] = src[i];
}

void copyStringArray(char src[][MAX_LENGTH], char dest[][MAX_LENGTH], int n) {
    for(int i = 0; i < n; i++)
        strcpy(dest[i], src[i]);
}

// ================= SUBMENU BASIC SORTING =================
void submenuSortingDasar() {
    int data[MAX_WORDS];
    int dataCopy[MAX_WORDS];
    int n = 1000;

    // Generate 1000 random integers
    srand(time(NULL));
    for(int i = 0; i < n; i++) {
        data[i] = rand() % 10000;
    }

    int pilihan;
    do {
        printf("\n===== SORTING DASAR =====\n");
        printf("1. Bubble Sort\n");
        printf("2. Insertion Sort\n");
        printf("3. Selection Sort\n");
        printf("4. Kembali\n");
        printf("Pilih metode: ");
        scanf("%d", &pilihan);

        if(pilihan >= 1 && pilihan <= 3) {
            copyIntArray(data, dataCopy, n);

            printf("\nData sebelum sorting:");
            tampilAngka(dataCopy, n);

            clock_t start = clock();

            switch(pilihan) {
                case 1:
                    printf("Menggunakan Bubble Sort...\n");
                    bubbleSort(dataCopy, n);
                    break;
                case 2:
                    printf("Menggunakan Insertion Sort...\n");
                    insertionSort(dataCopy, n);
                    break;
                case 3:
                    printf("Menggunakan Selection Sort...\n");
                    selectionSort(dataCopy, n);
                    break;
            }

            clock_t end = clock();
            double waktu = ((double)(end - start)) / CLOCKS_PER_SEC * 1000; // dalam ms

            printf("\nData setelah sorting:");
            tampilAngka(dataCopy, n);
            printf("Waktu eksekusi: %.4f ms\n", waktu);
        }

    } while(pilihan != 4);
}

// ================= SUBMENU ADVANCE SORTING =================
void submenuAdvanceSorting() {
    char data[MAX_WORDS][MAX_LENGTH];
    char dataCopy[MAX_WORDS][MAX_LENGTH];
    int n = 0;

    // Load data dari file
    FILE *fp = fopen("words.txt", "r");
    if(fp == NULL) {
        printf("File words.txt tidak ditemukan!\n");
        printf("Menggunakan data default...\n");
        // Data default jika file tidak ada
        const char *defaultWords[] = {"zebra", "apple", "monkey", "banana", "orange",
                                       "grape", "cherry", "dragon", "eagle", "falcon"};
        n = 10;
        for(int i = 0; i < n; i++)
            strcpy(data[i], defaultWords[i]);
    } else {
        while(n < MAX_WORDS && fgets(data[n], MAX_LENGTH, fp)) {
            data[n][strcspn(data[n], "\n")] = 0;
            if(strlen(data[n]) > 0) n++;
        }
        fclose(fp);
        printf("Berhasil load %d kata dari words.txt\n", n);
    }

    int pilihan;
    do {
        printf("\n===== ADVANCE SORTING =====\n");
        printf("1. Merge Sort\n");
        printf("2. Quick Sort\n");
        printf("3. Shell Sort\n");
        printf("4. Kembali\n");
        printf("Pilih metode: ");
        scanf("%d", &pilihan);

        if(pilihan >= 1 && pilihan <= 3) {
            copyStringArray(data, dataCopy, n);

            printf("\nData sebelum sorting:");
            tampilData(dataCopy, n);

            clock_t start = clock();

            switch(pilihan) {
                case 1:
                    printf("Menggunakan Merge Sort...\n");
                    mergeSort(dataCopy, 0, n-1);
                    break;
                case 2:
                    printf("Menggunakan Quick Sort...\n");
                    quickSort(dataCopy, 0, n-1);
                    break;
                case 3:
                    printf("Menggunakan Shell Sort...\n");
                    shellSort(dataCopy, n);
                    break;
            }

            clock_t end = clock();
            double waktu = ((double)(end - start)) / CLOCKS_PER_SEC * 1000; // dalam ms

            printf("\nData setelah sorting:");
            tampilData(dataCopy, n);
            printf("Waktu eksekusi: %.4f ms\n", waktu);
        }

    } while(pilihan != 4);
}

// ================= MAIN MENU =================
int main() {
    int pilihan;

    do {
        printf("\n===== MENU UTAMA =====\n");
        printf("1. Sorting Dasar\n");
        printf("2. Advance Sorting\n");
        printf("3. Keluar\n");
        printf("Pilih menu: ");
        scanf("%d", &pilihan);

        switch(pilihan) {
            case 1:
                submenuSortingDasar();
                break;
            case 2:
                submenuAdvanceSorting();
                break;
            case 3:
                printf("Terima kasih!\n");
                break;
            default:
                printf("Pilihan tidak valid!\n");
        }
    } while(pilihan != 3);

    return 0;
}
