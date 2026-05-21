#include <stdio.h>
#include <stdlib.h>
#include <time.h>

#define MAX 100

// ================= DISPLAY =================
void tampilData(int arr[], int n) {
    for(int i = 0; i < n; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");
}

// ================= SHUFFLE =================
void shuffleData(int arr[], int n) {
    for(int i = 0; i < n; i++) {
        int j = rand() % n;

        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
}

// ================= BUBBLE SORT =================
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

// ================= INSERTION SORT =================
void insertionSort(int arr[], int n) {

    int i, key, j;

    for(i = 1; i < n; i++) {

        key = arr[i];
        j = i - 1;

        while(j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }

        arr[j + 1] = key;
    }
}

// ================= SELECTION SORT =================
void selectionSort(int arr[], int n) {

    int i, j, min_idx;

    for(i = 0; i < n-1; i++) {

        min_idx = i;

        for(j = i+1; j < n; j++) {

            if(arr[j] < arr[min_idx]) {
                min_idx = j;
            }
        }

        int temp = arr[min_idx];
        arr[min_idx] = arr[i];
        arr[i] = temp;
    }
}

// ================= MERGE SORT =================
void merge(int arr[], int l, int m, int r) {

    int i, j, k;

    int n1 = m - l + 1;
    int n2 = r - m;

    int L[n1], R[n2];

    for(i = 0; i < n1; i++)
        L[i] = arr[l + i];

    for(j = 0; j < n2; j++)
        R[j] = arr[m + 1 + j];

    i = 0;
    j = 0;
    k = l;

    while(i < n1 && j < n2) {

        if(L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        }
        else {
            arr[k] = R[j];
            j++;
        }

        k++;
    }

    while(i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }

    while(j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}

void mergeSort(int arr[], int l, int r) {

    if(l < r) {

        int m = l + (r - l) / 2;

        mergeSort(arr, l, m);
        mergeSort(arr, m + 1, r);

        merge(arr, l, m, r);
    }
}

// ================= QUICK SORT =================
int partition(int arr[], int low, int high) {

    int pivot = arr[high];
    int i = low - 1;

    for(int j = low; j < high; j++) {

        if(arr[j] < pivot) {

            i++;

            int temp = arr[i];
            arr[i] = arr[j];
            arr[j] = temp;
        }
    }

    int temp = arr[i + 1];
    arr[i + 1] = arr[high];
    arr[high] = temp;

    return i + 1;
}

void quickSort(int arr[], int low, int high) {

    if(low < high) {

        int pi = partition(arr, low, high);

        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

// ================= SHELL SORT =================
void shellSort(int arr[], int n) {

    for(int gap = n/2; gap > 0; gap /= 2) {

        for(int i = gap; i < n; i++) {

            int temp = arr[i];
            int j;

            for(j = i; j >= gap && arr[j-gap] > temp; j -= gap) {
                arr[j] = arr[j-gap];
            }

            arr[j] = temp;
        }
    }
}

// ================= COPY ARRAY =================
void copyArray(int source[], int target[], int n) {

    for(int i = 0; i < n; i++) {
        target[i] = source[i];
    }
}

// ================= MAIN =================
int main() {

    int data[MAX];
    int arr[MAX];
    int n = 20;

    srand(time(NULL));

    // Generate random data
    for(int i = 0; i < n; i++) {
        data[i] = rand() % 1000;
    }

    int pilihanUtama, pilihanMetode;

    do {

        printf("\n===== MENU UTAMA =====\n");
        printf("1. Sorting Dasar\n");
        printf("2. Advance Sorting\n");
        printf("3. Keluar\n");
        printf("Pilih menu: ");
        scanf("%d", &pilihanUtama);

        switch(pilihanUtama) {

            // ================= SORTING DASAR =================
            case 1:

                printf("\n===== SORTING DASAR =====\n");
                printf("1. Bubble Sort\n");
                printf("2. Insertion Sort\n");
                printf("3. Selection Sort\n");
                printf("4. Kembali\n");
                printf("Pilih metode: ");
                scanf("%d", &pilihanMetode);

                copyArray(data, arr, n);
                shuffleData(arr, n);

                printf("\nData sebelum sorting:\n");
                tampilData(arr, n);

                clock_t start, end;
                double waktu;

                start = clock();

                if(pilihanMetode == 1) {
                    bubbleSort(arr, n);
                }
                else if(pilihanMetode == 2) {
                    insertionSort(arr, n);
                }
                else if(pilihanMetode == 3) {
                    selectionSort(arr, n);
                }

                end = clock();

                waktu = ((double)(end - start)) / CLOCKS_PER_SEC;

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
                scanf("%d", &pilihanMetode);

                copyArray(data, arr, n);
                shuffleData(arr, n);

                printf("\nData sebelum sorting:\n");
                tampilData(arr, n);

                start = clock();

                if(pilihanMetode == 1) {
                    mergeSort(arr, 0, n - 1);
                }
                else if(pilihanMetode == 2) {
                    quickSort(arr, 0, n - 1);
                }
                else if(pilihanMetode == 3) {
                    shellSort(arr, n);
                }

                end = clock();

                waktu = ((double)(end - start)) / CLOCKS_PER_SEC;

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

    } while(pilihanUtama != 3);

    return 0;
}
