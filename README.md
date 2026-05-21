#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>

#define MAX_WORDS 1000
#define MAX_LENGTH 100

// ================= DISPLAY =================
void tampilData(char arr[][MAX_LENGTH], int n) {

    int limit = n;

    if(limit > 20) {
        limit = 20;
    }

    for(int i = 0; i < limit; i++) {
        printf("%s\n", arr[i]);
    }
}

// ================= SHUFFLE =================
void shuffleData(char arr[][MAX_LENGTH], int n) {

    for(int i = 0; i < n; i++) {

        int j = rand() % n;

        char temp[MAX_LENGTH];

        strcpy(temp, arr[i]);
        strcpy(arr[i], arr[j]);
        strcpy(arr[j], temp);
    }
}

// ================= COPY ARRAY =================
void copyArray(char source[][MAX_LENGTH],
               char target[][MAX_LENGTH],
               int n) {

    for(int i = 0; i < n; i++) {
        strcpy(target[i], source[i]);
    }
}

// ================= BUBBLE SORT =================
void bubbleSort(char arr[][MAX_LENGTH], int n) {

    for(int i = 0; i < n - 1; i++) {

        for(int j = 0; j < n - i - 1; j++) {

            if(strcmp(arr[j], arr[j + 1]) > 0) {

                char temp[MAX_LENGTH];

                strcpy(temp, arr[j]);
                strcpy(arr[j], arr[j + 1]);
                strcpy(arr[j + 1], temp);
            }
        }
    }
}

// ================= INSERTION SORT =================
void insertionSort(char arr[][MAX_LENGTH], int n) {

    for(int i = 1; i < n; i++) {

        char key[MAX_LENGTH];
        strcpy(key, arr[i]);

        int j = i - 1;

        while(j >= 0 && strcmp(arr[j], key) > 0) {

            strcpy(arr[j + 1], arr[j]);
            j--;
        }

        strcpy(arr[j + 1], key);
    }
}

// ================= SELECTION SORT =================
void selectionSort(char arr[][MAX_LENGTH], int n) {

    for(int i = 0; i < n - 1; i++) {

        int min = i;

        for(int j = i + 1; j < n; j++) {

            if(strcmp(arr[j], arr[min]) < 0) {
                min = j;
            }
        }

        char temp[MAX_LENGTH];

        strcpy(temp, arr[i]);
        strcpy(arr[i], arr[min]);
        strcpy(arr[min], temp);
    }
}

// ================= MERGE SORT =================
void merge(char arr[][MAX_LENGTH], int l, int m, int r) {

    int n1 = m - l + 1;
    int n2 = r - m;

    char L[n1][MAX_LENGTH];
    char R[n2][MAX_LENGTH];

    for(int i = 0; i < n1; i++) {
        strcpy(L[i], arr[l + i]);
    }

    for(int j = 0; j < n2; j++) {
        strcpy(R[j], arr[m + 1 + j]);
    }

    int i = 0;
    int j = 0;
    int k = l;

    while(i < n1 && j < n2) {

        if(strcmp(L[i], R[j]) <= 0) {

            strcpy(arr[k], L[i]);
            i++;
        }
        else {

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

// ================= QUICK SORT =================
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

    strcpy(temp, arr[i + 1]);
    strcpy(arr[i + 1], arr[high]);
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

// ================= SHELL SORT =================
void shellSort(char arr[][MAX_LENGTH], int n) {

    for(int gap = n / 2; gap > 0; gap /= 2) {

        for(int i = gap; i < n; i++) {

            char temp[MAX_LENGTH];
            strcpy(temp, arr[i]);

            int j;

            for(j = i;
                j >= gap &&
                strcmp(arr[j - gap], temp) > 0;
                j -= gap) {

                strcpy(arr[j], arr[j - gap]);
            }

            strcpy(arr[j], temp);
        }
    }
}

// ================= MAIN =================
int main() {

    FILE *fp;

    char data[MAX_WORDS][MAX_LENGTH];
    char arr[MAX_WORDS][MAX_LENGTH];

    int n = 0;

    srand(time(NULL));

    // ================= READ FILE =================
    fp = fopen("sda.c", "r");

    if(fp == NULL) {

        printf("File sda.c tidak ditemukan!\n");
        return 1;
    }

    while(fgets(data[n], MAX_LENGTH, fp) != NULL) {

        data[n][strcspn(data[n], "\n")] = 0;
        n++;
    }

    fclose(fp);

    int menuUtama, metode;

    do {

        printf("\n===== MENU UTAMA =====\n");
        printf("1. Sorting Dasar\n");
        printf("2. Advance Sorting\n");
        printf("3. Keluar\n");
        printf("Pilih menu: ");
        scanf("%d", &menuUtama);

        switch(menuUtama) {

            // ================= SORTING DASAR =================
            case 1:

                printf("\n===== SORTING DASAR =====\n");
                printf("1. Bubble Sort\n");
                printf("2. Insertion Sort\n");
                printf("3. Selection Sort\n");
                printf("4. Kembali\n");
                printf("Pilih metode: ");
                scanf("%d", &metode);

                copyArray(data, arr, n);
                shuffleData(arr, n);

                printf("\nData sebelum sorting:\n");
                tampilData(arr, n);

                clock_t start, end;
                double waktu;

                start = clock();

                if(metode == 1) {
                    bubbleSort(arr, n);
                }
                else if(metode == 2) {
                    insertionSort(arr, n);
                }
                else if(metode == 3) {
                    selectionSort(arr, n);
                }

                end = clock();

                waktu =
                ((double)(end - start))
                / CLOCKS_PER_SEC;

                printf("\nData sesudah sorting:\n");
                tampilData(arr, n);

                printf("\nWaktu eksekusi: %f detik\n", waktu);

                break;

            // ================= ADVANCE SORTING =================
            case 2:

                printf("\n===== ADVANCE SORTING =====\n");
                printf("1. Merge Sort\n");
                printf("2. Quick Sort\n");
                printf("3. Shell Sort\n");
                printf("4. Kembali\n");
                printf("Pilih metode: ");
                scanf("%d", &metode);

                copyArray(data, arr, n);
                shuffleData(arr, n);

                printf("\nData sebelum sorting:\n");
                tampilData(arr, n);

                start = clock();

                if(metode == 1) {
                    mergeSort(arr, 0, n - 1);
                }
                else if(metode == 2) {
                    quickSort(arr, 0, n - 1);
                }
                else if(metode == 3) {
                    shellSort(arr, n);
                }

                end = clock();

                waktu =
                ((double)(end - start))
                / CLOCKS_PER_SEC;

                printf("\nData sesudah sorting:\n");
                tampilData(arr, n);

                printf("\nWaktu eksekusi: %f detik\n", waktu);

                break;

            case 3:
                printf("\nProgram selesai.\n");
                break;

            default:
                printf("\nPilihan tidak valid!\n");
        }

    } while(menuUtama != 3);

    return 0;
}
