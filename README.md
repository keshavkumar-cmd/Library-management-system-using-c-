#include<iostream>
#include<fstream>
using namespace std;

class book
{
private:
    int bookid;
    string title;
    string author;
    string publisher;
    int quantity;
    bool isIssued;

public:
    void inputbook();
    void displaybook();

    int getBookId()
    {
        return bookid;
    }

    string getTitle()
    {
        return title;
    }

    string getAuthor()
    {
        return author;
    }

    string getPublisher()
    {
        return publisher;
    }

    int getQuantity()
    {
        return quantity;
    }

    bool getIssued()
    {
        return isIssued;
    }

    // FIX: issue() ab quantity bhi ghatayega
    void issue()
    {
        isIssued = true;
        quantity--;
    }

    // FIX: Return() ab quantity bhi badhayega
    void Return()
    {
        isIssued = false;
        quantity++;
    }

    void loadBook(ifstream &fin)
    {
        fin >> bookid;
        fin.ignore();

        getline(fin, title);
        getline(fin, author);
        getline(fin, publisher);

        fin >> quantity;
        fin >> isIssued;
        fin.ignore();
    }
};
void book::inputbook(){
cout << "enter book id: "; 
cin>>bookid;
cin.ignore();
cout<<"book title: ";
getline(cin,title); 
cout<<"enter author name: ";
getline(cin,author);
cout<<"enter publisher: ";
getline(cin,publisher);
cout<<"enter quantity: ";
cin>>quantity;
isIssued=false;
}
void book::displaybook(){
cout << "bookid: " <<bookid<< endl;  
cout << "title: " <<title<< endl;   
cout << "author: " <<author<< endl; 
cout << "publisher: " <<publisher<< endl;
cout << "quantity: " <<quantity<< endl;
if(isIssued)
cout<<"status:issued"<<endl;
else 
cout<<"status:available"<<endl;
}
class member{
private:
        int memberId;
        string name;
        string course;
        string phone;
public:
       int getMemberId()
{
    return memberId;
}

string getName()
{
    return name;
}

string getCourse()
{
    return course;
}

string getPhone()
{
    return phone;
}

void loadMember(ifstream &fin)
{
    fin >> memberId;
    fin.ignore();

    getline(fin, name);
    getline(fin, course);
    getline(fin, phone);
}
       void inputmember();
       void displaymember();
};
void member::inputmember(){
cout << "Enter member id: ";     
cin>>memberId;
cin.ignore();
cout << "enter name: ";  
getline(cin,name);
cout << "enter course";
getline(cin,course);
cout << "enter phone no: ";
getline(cin,phone);
}
void member::displaymember(){
cout << "member id is: " <<memberId<< endl;
cout << "Name: " <<name<< endl;
cout << "course: " <<course<< endl;
cout << "phone no: " <<phone<< endl;
}
class Library
{
private:
    book books[100];
    member members[100];

    int totalBooks;
    int totalMembers;

public:
    Library();

    void addBook();
    void displayBooks();
    void searchBook();
    void updateBook();
    void deleteBook();

    void addMember();
    void displayMembers();

    void issueBook();
    void returnBook();

    void saveBooks();
    void loadBooks();

    void saveMembers();
    void loadMembers();

    void menu();
};
Library::Library(){
totalBooks=0;
totalMembers=0;
}
void Library::addBook()
{
    books[totalBooks].inputbook();
    totalBooks++;
}
void Library::displayBooks()
{
    if(totalBooks == 0)
    {
        cout << "No books available." << endl;
        return;
    }

    for(int i = 0; i < totalBooks; i++)
    {
       books[i].displaybook();
        cout << "----------displaying books--------------" << endl;
    }
}
void Library::searchBook()
{
    if(totalBooks == 0)
    {
        cout << "No books available." << endl;
        return;
    }

    int id;
    cout << "Enter Book ID to search: ";
    cin >> id;

    bool found = false;

    for(int i = 0; i < totalBooks; i++)
    {
        if(books[i].getBookId() == id)
        {
            cout << "\nBook Found\n";
            books[i].displaybook();
            found = true;
            break;
        }
    }

    if(!found)
    {
        cout << "Book not found." << endl;
    }
}
void Library::updateBook()
{
    if(totalBooks == 0)
    {
        cout << "No books available." << endl;
        return;
    }

    int id;
    cout << "Enter Book ID to update: ";
    cin >> id;

    bool found = false;

    for(int i = 0; i < totalBooks; i++)
    {
        if(books[i].getBookId() == id)
        {
            cout << "\nEnter new details:\n";
            books[i].inputbook();

            cout << "Book updated successfully." << endl;
            found = true;
            break;
        }
    }

    if(!found)
    {
        cout << "Book not found." << endl;
    }
}
void Library::deleteBook()
{
    if(totalBooks == 0)
    {
        cout << "No books available." << endl;
        return;
    }

    int id;
    cout << "Enter Book ID to delete: ";
    cin >> id;

    bool found = false;

    for(int i = 0; i < totalBooks; i++)
    {
        if(books[i].getBookId() == id)
        {
            found = true;

            for(int j = i; j < totalBooks - 1; j++)
            {
                books[j] = books[j + 1];
            }

            totalBooks--;

            cout << "Book deleted successfully." << endl;
            break;
        }
    }

    if(!found)
    {
        cout << "Book not found." << endl;
    }
}
void Library::addMember()
{
    members[totalMembers].inputmember();
    totalMembers++;

    cout << "Member added successfully." << endl;
}
void Library::displayMembers()
{
    if(totalMembers == 0)
    {
        cout << "No members available." << endl;
        return;
    }

    for(int i = 0; i < totalMembers; i++)
    {
        members[i].displaymember();
        cout << "------------------------" << endl;
    }
}
void Library::issueBook()
{
    int id;
    cout << "Enter Book ID: ";
    cin >> id;

    for(int i = 0; i < totalBooks; i++)
    {
        if(books[i].getBookId() == id)
        {
            // FIX: ab stock (quantity) bhi check ho raha hai
            if(books[i].getQuantity() <= 0)
            {
                cout << "Book out of stock, cannot issue." << endl;
            }
            else
            {
                books[i].issue();
                cout << "Book issued successfully." << endl;
                cout << "Remaining quantity: " << books[i].getQuantity() << endl;
            }
            return;
        }
    }

    cout << "Book not found." << endl;
}
void Library::returnBook()
{
    int id;
    cout << "Enter Book ID: ";
    cin >> id;

    for(int i = 0; i < totalBooks; i++)
    {
        if(books[i].getBookId() == id)
        {
            if(!books[i].getIssued())
            {
                cout << "Book is already available." << endl;
            }
            else
            {
                books[i].Return();
                cout << "Book returned successfully." << endl;
                cout << "Updated quantity: " << books[i].getQuantity() << endl;
            }
            return;
        }
    }

    cout << "Book not found." << endl;
}
void Library::saveBooks()
{
    ofstream fout("books.txt");

    fout << totalBooks << endl;

    for(int i = 0; i < totalBooks; i++)
    {
        fout << books[i].getBookId() << endl;
        fout << books[i].getTitle() << endl;
        fout << books[i].getAuthor() << endl;
        fout << books[i].getPublisher() << endl;
        fout << books[i].getQuantity() << endl;
        fout << books[i].getIssued() << endl;
    }

    fout.close();
    cout << "Books saved successfully." << endl;
}
void Library::loadBooks()
{
    ifstream fin("books.txt");

    if(!fin)
    {
        cout << "No saved file found." << endl;
        return;
    }

    fin >> totalBooks;
    fin.ignore();

    for(int i = 0; i < totalBooks; i++)
    {
        books[i].loadBook(fin);
    }

    fin.close();
    cout << "Books loaded successfully." << endl;
}
void Library::saveMembers()
{
    ofstream fout("members.txt");

    fout << totalMembers << endl;

    for(int i = 0; i < totalMembers; i++)
    {
        fout << members[i].getMemberId() << endl;
        fout << members[i].getName() << endl;
        fout << members[i].getCourse() << endl;
        fout << members[i].getPhone() << endl;
    }

    fout.close();
    cout << "Members saved successfully." << endl;
}
void Library::loadMembers()
{
    ifstream fin("members.txt");

    if(!fin)
    {
        cout << "No saved member file found." << endl;
        return;
    }

    fin >> totalMembers;
    fin.ignore();

    for(int i = 0; i < totalMembers; i++)
    {
        members[i].loadMember(fin);
    }

    fin.close();
    cout << "Members loaded successfully." << endl;
}
void Library::menu()
{
    int choice;

    do
    {
        cout << "\n====== Library Management System ======\n";
        cout << "1. Add Book\n";
        cout << "2. Display Books\n";
        cout << "3. Search Book\n";
        cout << "4. Update Book\n";
        cout << "5. Delete Book\n";
        cout << "6. Add Member\n";
        cout << "7. Display Members\n";
        cout << "8. Issue Book\n";
        cout << "9. Return Book\n";
        cout << "10. Save Books\n";
        cout << "11. Load Books\n";
        cout << "12. Save Members\n";
        cout << "13. Load Members\n";
        cout << "0. Exit\n";

        cout << "Enter your choice: ";
        cin >> choice;

        switch(choice)
        {
            case 1: addBook(); break;
            case 2: displayBooks(); break;
            case 3: searchBook(); break;
            case 4: updateBook(); break;
            case 5: deleteBook(); break;
            case 6: addMember(); break;
            case 7: displayMembers(); break;
            case 8: issueBook(); break;
            case 9: returnBook(); break;
            case 10: saveBooks(); break;
            case 11: loadBooks(); break;
            case 12: saveMembers(); break;
            case 13: loadMembers(); break;
            case 0: cout << "Thank You!" << endl; break;
            default: cout << "Invalid Choice!" << endl;
        }

    } while(choice != 0);
}
int main()
{
    Library obj;

    //obj.loadBooks();
    //obj.loadMembers();

    obj.menu();

    obj.saveBooks();
    obj.saveMembers();

    return 0;
}
