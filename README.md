assignment 4
set A


#include <stdio.h>
#include <stdlib.h>

// Function to find index of the farthest page in the future for FIFO
int findFarthestPage(int pages[], int n, int startIndex, int refString[], int refStringLength) {
    int farthestIndex = -1;
    int farthestPage = -1;
    
    for (int i = startIndex; i < refStringLength; i++) {
        int page = refString[i];
        int found = 0;
        for (int j = 0; j < n; j++) {
            if (pages[j] == page) {
                found = 1;
                break;
            }
        }
        if (!found) {
            for (int j = 0; j < n; j++) {
                int nextPage = -1;
                for (int k = i + 1; k < refStringLength; k++) {
                    if (refString[k] == pages[j]) {
                        nextPage = k;
                        break;
                    }
                }
                if (nextPage == -1) {
                    return j;
                } else if (nextPage > farthestIndex) {
                    farthestIndex = nextPage;
                    farthestPage = j;
                }
            }
        }
    }
    return farthestPage;
}

// Function to implement FIFO page replacement algorithm
int fifoPageReplacement(int refString[], int refStringLength, int n) {
    int pages[n];
    int pageFaults = 0;
    int currentIndex = 0;
    
    for (int i = 0; i < n; i++) {
        pages[i] = -1;
    }
    
    for (int i = 0; i < refStringLength; i++) {
        int page = refString[i];
        int found = 0;
        
        for (int j = 0; j < n; j++) {
            if (pages[j] == page) {
                found = 1;
                break;
            }
        }
        
        if (!found) {
            pageFaults++;
            pages[currentIndex] = page;
            currentIndex = (currentIndex + 1) % n;
        }
    }
    
    return pageFaults;
}

// Function to implement LRU page replacement algorithm
int lruPageReplacement(int refString[], int refStringLength, int n) {
    int pages[n];
    int pageFaults = 0;
    
    for (int i = 0; i < n; i++) {
        pages[i] = -1;
    }
    
    for (int i = 0; i < refStringLength; i++) {
        int page = refString[i];
        int found = 0;
        
        for (int j = 0; j < n; j++) {
            if (pages[j] == page) {
                found = 1;
                break;
            }
        }
        
        if (!found) {
            pageFaults++;
            int farthestPage = findFarthestPage(pages, n, i, refString, refStringLength);
            pages[farthestPage] = page;
        }
    }
    
    return pageFaults;
}

int main() {
    int refString[] = {12, 15, 12, 18, 6, 8, 11, 12, 19, 12, 6, 8, 12, 15, 19, 8};
    int refStringLength = sizeof(refString) / sizeof(refString[0]);
    int n;
    
    printf("Enter the number of memory frames: ");
    scanf("%d", &n);
    
    int fifoFaults = fifoPageReplacement(refString, refStringLength, n);
    int lruFaults = lruPageReplacement(refString, refStringLength, n);
    
    printf("Total Page Faults (FIFO): %d\n", fifoFaults);
    printf("Total Page Faults (LRU): %d\n", lruFaults);
    
    return 0;
}
