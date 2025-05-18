# DOUBLY LINKEDLIST
#include <stdio.h>
#include <stdlib.h>
struct Node {
    int data;
    struct Node* prev;
    struct Node* next;
};
struct Node* head = NULL;
void insertAtFirst(int value) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    if (newNode == NULL) {
        printf("Memory allocation failed.\n");
        return;
    }
    newNode->data = value;
    newNode->prev = NULL;
    newNode->next = head;

    if (head != NULL) {
        head->prev = newNode;
    }
    head = newNode;
}
void insertAtLast(int value) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    if (newNode == NULL) {
        printf("Memory allocation failed.\n");
        return;
    }
    newNode->data = value;
    newNode->next = NULL;

    if (head == NULL) {
        newNode->prev = NULL;
        head = newNode;
        return;
    }

    struct Node* temp = head;
    while (temp->next != NULL) {
        temp = temp->next;
    }

    temp->next = newNode;
    newNode->prev = temp;
}
void insertAtPosition(int value, int position) {
    if (position < 1) {
        printf("Invalid position.\n");
        return;
    }

    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    if (newNode == NULL) {
        printf("Memory allocation failed.\n");
        return;
    }
    newNode->data = value;
    if (position == 1) {
        newNode->prev = NULL;
        newNode->next = head;
        if (head != NULL)
            head->prev = newNode;
        head = newNode;
        return;
    }
    struct Node* temp = head;
    int i;
    for (i = 1; temp != NULL && i < position - 1; i++) {
        temp = temp->next;
    }
    if (temp == NULL) {
        printf("Position out of bounds.\n");
        free(newNode);
        return;
    }
    newNode->next = temp->next;
    newNode->prev = temp;

    if (temp->next != NULL)
        temp->next->prev = newNode;
		temp->next = newNode;
}
void deleteAtFirst() {
    if (head == NULL) {
        printf("List is empty.\n");
        return;
    }
    struct Node* temp = head;
    head = head->next;
    if (head != NULL)
        head->prev = NULL;
    free(temp);
    printf("First node deleted.\n");
}
void deleteAtLast() {
    if (head == NULL) {
        printf("List is empty.\n");
        return;
    }
    if (head->next == NULL) {
        free(head);
        head = NULL;
        printf("Last node deleted.\n");
        return;
    }
    struct Node* temp = head;
    while (temp->next != NULL) {
        temp = temp->next;
    }
    temp->prev->next = NULL;
    free(temp);
    printf("Last node deleted.\n");
}
void deleteAtPosition(int position) {
    if (head == NULL) {
        printf("List is empty.\n");
        return;
    }
    if (position < 1) {
        printf("Invalid position.\n");
        return;
    }
    if (position == 1) {
        struct Node* temp = head;
        head = head->next;
        if (head != NULL)
            head->prev = NULL;
        free(temp);
        printf("Node at position %d deleted.\n", position);
        return;
    }
    struct Node* temp = head;
    int i;
    for (i = 1; temp != NULL && i < position; i++) {
        temp = temp->next;
    }
    if (temp == NULL) {
        printf("Position out of bounds.\n");
        return;
    }
    if (temp->prev != NULL)
        temp->prev->next = temp->next;
    if (temp->next != NULL)
        temp->next->prev = temp->prev;

    free(temp);
    printf("Node at position %d deleted.\n", position);
}
void display() {
    struct Node* temp = head;
    int nodeNumber = 1;

    if (head == NULL) {
        printf("List is empty.\n");
        return;
    }
    printf("\nDoubly Linked List:\n");
    while (temp != NULL) {
        printf("Node %d: Data = %d, Address = %p, Prev = %p, Next = %p\n",
               nodeNumber, temp->data, (void*)temp, (void*)temp->prev, (void*)temp->next);
        temp = temp->next;
        nodeNumber++;
    }
}
int main() {
    int choice, value, position;
    do {
        printf("\n1. Insert at first\n2. Insert at last\n3. Delete from first\n4. Delete from last\n");
        printf("5. Display\n6. Insert at position\n7. Delete at position\n8. Exit\nChoice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                printf("Enter value: ");
                scanf("%d", &value);
                insertAtFirst(value);
                break;
            case 2:
                printf("Enter value: ");
                scanf("%d", &value);
                insertAtLast(value);
                break;
            case 3:
                deleteAtFirst();
                break;
            case 4:
                deleteAtLast();
                break;
            case 5:
                display();
                break;
            case 6:
                printf("Enter position: ");
                scanf("%d", &position);
                printf("Enter value: ");
                scanf("%d", &value);
                insertAtPosition(value, position);
                break;
            case 7:
                printf("Enter position to delete: ");
                scanf("%d", &position);
                deleteAtPosition(position);
                break;
            case 8:
                printf("Exit code.\n");
                break;
            default:
                printf("Invalid choice.\n");
        }
    } while (choice != 8);
    return 0;
}

