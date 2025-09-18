#include <stdio.h>
#include <stdlib.h>
#include <string.h>

typedef struct Node {
    char url[100];
    struct Node* prev;
    struct Node* next;
} Node;

Node* head = NULL;
Node* tail = NULL;
Node* current = NULL;

Node* createNode(char url[]) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    strcpy(newNode->url, url);
    newNode->prev = newNode->next = NULL;
    return newNode;
}

void visitWebsite(char url[]) {
    Node* newNode = createNode(url);
    if (head == NULL) {
        head = tail = current = newNode;
    } else {
        tail->next = newNode;
        newNode->prev = tail;
        tail = newNode;
        current = newNode;
    }
    printf("Visited: %s\n", url);
}

void displayHistory() {
    Node* temp = head;
    if (!temp) {
        printf("History is empty.\n");
        return;
    }
    printf("Browser History:\n");
    while (temp) {
        printf("%s\n", temp->url);
        temp = temp->next;
    }
}

void deleteWebsite(char url[]) {
    Node* temp = head;
    while (temp) {
        if (strcmp(temp->url, url) == 0) {
            if (temp == head) head = head->next;
            if (temp == tail) tail = tail->prev;
            if (temp->prev) temp->prev->next = temp->next;
            if (temp->next) temp->next->prev = temp->prev;
            free(temp);
            printf("Deleted: %s\n", url);
            return;
        }
        temp = temp->next;
    }
    printf("Website not found: %s\n", url);
}

void navigateForward() {
    if (current && current->next) {
        current = current->next;
        printf("Forward to: %s\n", current->url);
    } else {
        printf("No forward history.\n");
    }
}

void navigateBackward() {
    if (current && current->prev) {
        current = current->prev;
        printf("Backward to: %s\n", current->url);
    } else {
        printf("No backward history.\n");
    }
}

void clearHistory() {
    Node* temp = head;
    while (temp) {
        Node* next = temp->next;
        free(temp);
        temp = next;
    }
    head = tail = current = NULL;
    printf("History cleared.\n");
}

void searchWebsite(char url[]) {
    Node* temp = head;
    int pos = 1;
    while (temp) {
        if (strcmp(temp->url, url) == 0) {
            printf("Website found at position %d: %s\n", pos, url);
            return;
        }
        temp = temp->next;
        pos++;
    }
    printf("Website not found: %s\n", url);
}

void bubbleSortHistory() {
    if (!head) return;
    int swapped;
    Node* ptr;
    Node* last = NULL;
    do {
        swapped = 0;
        ptr = head;
        while (ptr->next != last) {
            if (strcmp(ptr->url, ptr->next->url) > 0) {
                char temp[100];
                strcpy(temp, ptr->url);
                strcpy(ptr->url, ptr->next->url);
                strcpy(ptr->next->url, temp);
                swapped = 1;
            }
            ptr = ptr->next;
        }
        last = ptr;
    } while (swapped);
    printf("History sorted alphabetically.\n");
}

int main() {
    int choice;
    char url[100];

    while (1) {
        printf("\n--- Browser History Manager ---\n");
        printf("1. Visit Website\n");
        printf("2. Delete Website\n");
        printf("3. Display History\n");
        printf("4. Navigate Forward\n");
        printf("5. Navigate Backward\n");
        printf("6. Clear History\n");
        printf("7. Search Website\n");
        printf("8. Sort History (Alphabetical)\n");
        printf("9. Exit\n");
        printf("Enter choice: ");
        scanf("%d", &choice);
        getchar(); // clear input buffer

        switch (choice) {
            case 1:
                printf("Enter website URL: ");
                scanf("%s", url);
                visitWebsite(url);
                break;
            case 2:
                printf("Enter website URL to delete: ");
                scanf("%s", url);
                deleteWebsite(url);
                break;
            case 3:
                displayHistory();
                break;
            case 4:
                navigateForward();
                break;
            case 5:
                navigateBackward();
                break;
            case 6:
                clearHistory();
                break;
            case 7:
                printf("Enter website URL to search: ");
                scanf("%s", url);
                searchWebsite(url);
                break;
            case 8:
                bubbleSortHistory();
                break;
            case 9:
                clearHistory();
                printf("Exiting...\n");
                return 0;
            default:
                printf("Invalid choice.\n");
        }
    }
}
