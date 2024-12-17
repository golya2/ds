# ds
#include<iostream>
using namespace std;

// Node structure for singly linked list
struct Node {
    char data;
    Node* next;
};

// Function to create a linked list
Node* createList(int n) {
    Node *head = NULL, *temp = NULL, *newNode;
    for (int i = 0; i < n; i++) {
        newNode = new Node();
        cout << "Enter student initial: ";
        cin >> newNode->data;
        newNode->next = NULL;
        if (head == NULL) {
            head = newNode;
            temp = head;
        } else {
            temp->next = newNode;
            temp = temp->next;
        }
    }
    return head;
}

// Function to display a linked list
void displayList(Node* head) {
    while (head != NULL) {
        cout << head->data << " ";
        head = head->next;
    }
}

// Function to check if an element exists in a list
bool isPresent(Node* head, char data) {
    while (head != NULL) {
        if (head->data == data) return true;
        head = head->next;
    }
    return false;
}

// Function to compute intersection (A ∩ B)
Node* intersection(Node* A, Node* B) {
    Node *result = NULL, *temp = NULL, *newNode;
    while (A != NULL) {
        if (isPresent(B, A->data)) {
            newNode = new Node();
            newNode->data = A->data;
            newNode->next = NULL;
            if (result == NULL) {
                result = newNode;
                temp = result;
            } else {
                temp->next = newNode;
                temp = temp->next;
            }
        }
        A = A->next;
    }
    return result;
}

// Function to compute symmetric difference (A ∪ B) - (A ∩ B)
Node* symmetricDifference(Node* A, Node* B) {
    Node *result = NULL, *temp = NULL, *newNode;
    while (A != NULL) {
        if (!isPresent(B, A->data)) { // Elements in A but not in B
            newNode = new Node();
            newNode->data = A->data;
            newNode->next = NULL;
            if (result == NULL) {
                result = newNode;
                temp = result;
            } else {
                temp->next = newNode;
                temp = temp->next;
            }
        }
        A = A->next;
    }
    while (B != NULL) {
        if (!isPresent(A, B->data)) { // Elements in B but not in A
            newNode = new Node();
            newNode->data = B->data;
            newNode->next = NULL;
            if (result == NULL) {
                result = newNode;
                temp = result;
            } else {
                temp->next = newNode;
                temp = temp->next;
            }
        }
        B = B->next;
    }
    return result;
}

// Function to compute students who like neither Vanilla nor Butterscotch
Node* complement(Node* U, Node* A, Node* B) {
    Node *result = NULL, *temp = NULL, *newNode;
    while (U != NULL) {
        if (!isPresent(A, U->data) && !isPresent(B, U->data)) {
            newNode = new Node();
            newNode->data = U->data;
            newNode->next = NULL;
            if (result == NULL) {
                result = newNode;
                temp = result;
            } else {
                temp->next = newNode;
                temp = temp->next;
            }
        }
        U = U->next;
    }
    return result;
}

int main() {
    Node *U = NULL, *A = NULL, *B = NULL;
    int totalStudents, vanillaCount, butterscotchCount;

    // Input Universal Set
    cout << "Enter total number of students in SE Comp: ";
    cin >> totalStudents;
    cout << "Enter student initials for Universal Set: \n";
    U = createList(totalStudents);

    // Input Set A
    cout << "\nEnter number of students who like Vanilla Icecream: ";
    cin >> vanillaCount;
    cout << "Enter student initials for Set A: \n";
    A = createList(vanillaCount);

    // Input Set B
    cout << "\nEnter number of students who like Butterscotch Icecream: ";
    cin >> butterscotchCount;
    cout << "Enter student initials for Set B: \n";
    B = createList(butterscotchCount);

    // Display the input sets
    cout << "\nUniversal Set (U): ";
    displayList(U);
    cout << "\nSet A (Vanilla): ";
    displayList(A);
    cout << "\nSet B (Butterscotch): ";
    displayList(B);

    // a) Students who like both Vanilla and Butterscotch (A ∩ B)
    cout << "\n\nStudents who like both Vanilla and Butterscotch: ";
    Node* both = intersection(A, B);
    displayList(both);

    // b) Students who like either Vanilla or Butterscotch but not both
    cout << "\nStudents who like either Vanilla or Butterscotch but not both: ";
    Node* symDiff = symmetricDifference(A, B);
    displayList(symDiff);

    // c) Students who like neither Vanilla nor Butterscotch
    cout << "\nStudents who like neither Vanilla nor Butterscotch: ";
    Node* neither = complement(U, A, B);
    displayList(neither);

    cout << "\n";
    return 0;
}
