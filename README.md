#include <iostream>
#include <string>
 
struct Node {
    std::string prn;
    std::string name;
    Node* next = nullptr;
};
 
class Club {
    Node* head = nullptr;
    Node* tail = nullptr;
 
public:
    void addMember(const std::string& prn, const std::string& name) {
        Node* newNode = new Node{prn, name};
        if (!head) {
            head = tail = newNode; // First member
        } else {
            tail->next = newNode; // Add to the end
            tail = newNode;
        }
    }
 
    void deleteMember(const std::string& prn) {
        Node *current = head, *prev = nullptr;
        while (current) {
            if (current->prn == prn) {
                if (prev) prev->next = current->next;
                else head = current->next; // Deleting head
                delete current;
                std::cout << "Member with PRN " << prn << " deleted.\n";
                return;
            }
            prev = current;
            current = current->next;
        }
        std::cout << "Member with PRN " << prn << " not found.\n";
    }
 
    int getTotalMembers() {
        int count = 0;
        for (Node* current = head; current; current = current->next) count++;
        return count;
    }
 
    void displayMembers() {
        if (!head) {
            std::cout << "No members in the club.\n";
            return;
        }
        for (Node* current = head; current; current = current->next)
            std::cout << "PRN: " << current->prn << ", Name: " << current->name << std::endl;
    }
 
    void concatenate(Club& other) {
        if (!head) {
            head = other.head;
            tail = other.tail;
        } else if (other.head) {
            tail->next = other.head;
            tail = other.tail;
        }
    }
};
 
void menu() {
    std::cout << "\nPinnacle Club Membership Management\n";
    std::cout << "1. Add Member\n";
    std::cout << "2. Delete Member\n";
    std::cout << "3. Display Members\n";
    std::cout << "4. Total Members\n";
    std::cout << "5. Concatenate Two Clubs\n";
    std::cout << "6. Exit\n";
}
 
int main() {
    Club division1, division2;
    int choice;
    std::string prn, name;
 
    while (true) {
        menu();
        std::cout << "Enter your choice: ";
        std::cin >> choice;
 
        switch (choice) {
            case 1: // Add Member
                std::cout << "Enter PRN: ";
                std::cin >> prn;
                std::cout << "Enter Name: ";
                std::cin >> name;
                division1.addMember(prn, name);
                break;
 
            case 2: // Delete Member
                std::cout << "Enter PRN to delete: ";
                std::cin >> prn;
                division1.deleteMember(prn);
                break;
 
            case 3: // Display Members
                std::cout << "Members of Division 1:\n";
                division1.displayMembers();
                break;
 
            case 4: // Total Members
                std::cout << "Total Members: " << division1.getTotalMembers() << std::endl;
                break;
 
            case 5: // Concatenate Two Clubs
                std::cout << "Enter PRN and Name for Division 2:\n";
                std::cout << "Enter PRN: ";
                std::cin >> prn;
                std::cout << "Enter Name: ";
                std::cin >> name;
                division2.addMember(prn, name);
                division1.concatenate(division2);
                std::cout << "Division 2 added to Division 1.\n";
                break;
 
            case 6: // Exit
                return 0;
 
            default:
                std::cout << "Invalid choice. Please try again.\n";
        }
    }
}
