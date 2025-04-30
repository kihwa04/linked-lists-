# linked-lists-
linked lists operations
#include <iostream>
using namespace std;
// Node class to represent each element in the list
class Node {
public:
int data;
// Data of the node
Node* next;     // Pointer to the next node
// Constructor
Node(int value) : data(value), next(NULL) {}  // Using NULL instead of nullptr
};
// LinkedList class to manage the nodes
class LinkedList {
private:
Node* head;  // Pointer to the head of the list
public:
// Constructor
LinkedList() : head(NULL) {}  // Using NULL instead of nullptr
// Destructor
~LinkedList();
// Insert a node at a specific position
void insertAtPosition(int value, int position);
// Delete a node by value
bool deleteNode(int value);
    // Display the linked list
    void displayList() const;
};
// Destructor to delete the list and free memory
LinkedList::~LinkedList() {
    Node* current = head;
    Node* nextNode;
    // Delete all nodes
    while (current != NULL) {  // Using NULL instead of nullptr
        nextNode = current->next;
        delete current;
        current = nextNode;
    }
    head = NULL;  // Set head to NULL after deletion
}

// Insert a node at a specific position
void LinkedList::insertAtPosition(int value, int position) {
    Node* newNode = new Node(value);

    // If inserting at the head (position 1)
    if (position == 1) {
        newNode->next = head;
        head = newNode;
        return;
    }
    // Traverse to the position before where the new node should be inserted
    Node* current = head;
    for (int i = 1; i < position - 1 && current != NULL; i++) {
        current = current->next;
    }

    // If the position is valid
    if (current != NULL) {
        newNode->next = current->next;
        current->next = newNode;
    } else {
        cout << "Invalid position!" << endl;
        delete newNode;
    }
}
// Delete a node by its value
bool LinkedList::deleteNode(int value) {
    // If the list is empty
    if (head == NULL) {
        return false;
    }
    // If the node to delete is the head
    if (head->data == value) {
        Node* temp = head;
        head = head->next;
        delete temp;
        return true;
    }

    // Traverse to find the node to delete
    Node* current = head;
    while (current->next != NULL && current->next->data != value) {
        current = current->next;
    }
    // If the node was found, delete it
    if (current->next != NULL) {
        Node* temp = current->next;
        current->next = current->next->next;
        delete temp;
        return true;
    }
    return false;  // Node not found
}
// Display the linked list
void LinkedList::displayList() const {
    Node* current = head;
    if (current == NULL) {
        cout << "The list is empty." << endl;
        return;
    }
    // Traverse and print the list
    while (current != NULL) {
        cout << current->data << " -> ";
        current = current->next;
    }
    cout << "NULL" << endl;
}

// Main function
int main() {
    LinkedList list;
    int choice, value, position;
    do {
        cout << "\nMenu:\n";
        cout << "1. Insert a value at a position\n";
        cout << "2. Delete a value\n";
        cout << "3. Display the list\n";
        cout << "4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;
        switch (choice) {
        case 1:
            cout << "Enter value to insert: ";
            cin >> value;
            cout << "Enter position to insert at (1-based index): ";
            cin >> position;
            list.insertAtPosition(value, position);
            break;

        case 2:
            cout << "Enter value to delete: ";
            cin >> value;
            if (list.deleteNode(value)) {
                cout << "Value " << value << " deleted successfully." << endl;
            } else {
                cout << "Value " << value << " not found in the list." << endl;
            }
            break;
        case 3:
            cout << "Current list: ";
            list.displayList();
            break;
        case 4:
            cout << "Exiting..." << endl;
            break;
        default:
            cout << "Invalid choice! Please try again." << endl;
        }

    } while (choice != 4);

    return 0;
}
