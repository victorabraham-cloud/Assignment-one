#ifndef BOOK_H
#define BOOK_H
#include <iostream>
#include <string>
using namespace std;

class Book {
private:
    int id;
    string title;
    string author;
    bool borrowed;

public:
    Book(int i, string t, string a);

    int getId();
    string getTitle();
    bool isBorrowed();

    void borrow();
    void giveBack();
};

#endif
#include "Book.h"

Book::Book(int i, string t, string a) {
    id = i;
    title = t;
    author = a;
    borrowed = false;
}

int Book::getId() {
    return id;
}

string Book::getTitle() {
    return title;
}

bool Book::isBorrowed() {
    return borrowed;
}

void Book::borrow() {
    borrowed = true;
}

void Book::giveBack() {
    borrowed = false;
}
#ifndef USER_H
#define USER_H

#include <string>
#include <vector>
using namespace std;

class User {
private:
    int id;
    string name;
    vector<int> books;

public:
    User(int i, string n);

    int getId();

    void borrowBook(int bookId);
    void returnBook(int bookId);
};

#endif
#include "User.h"

User::User(int i, string n) {
    id = i;
    name = n;
}

int User::getId() {
    return id;
}

void User::borrowBook(int bookId) {
    books.push_back(bookId);
}

void User::returnBook(int bookId) {
    for (int i = 0; i < books.size(); i++) {
        if (books[i] == bookId) {
            books.erase(books.begin() + i);
            break;
        }
    }
}
#ifndef LIBRARY_H
#define LIBRARY_H

#include <vector>
using namespace std;

#include "Book.h"
#include "User.h"

class Library {
private:
    vector<Book> books;
    vector<User> users;

public:
    void addBook(Book b);
    void addUser(User u);

    bool borrowBook(int userId, int bookId);
    bool returnBook(int userId, int bookId);
};

#endif
#include "Library.h"

void Library::addBook(Book b) {
    books.push_back(b);
}

void Library::addUser(User u) {
    users.push_back(u);
}

bool Library::borrowBook(int userId, int bookId) {
    for (int i = 0; i < users.size(); i++) {
        if (users[i].getId() == userId) {
            for (int j = 0; j < books.size(); j++) {
                if (books[j].getId() == bookId && !books[j].isBorrowed()) {
                    books[j].borrow();
                    users[i].borrowBook(bookId);
                    return true;
                }
            }
        }
    }
    return false;
}

bool Library::returnBook(int userId, int bookId) {
    for (int i = 0; i < users.size(); i++) {
        if (users[i].getId() == userId) {
            for (int j = 0; j < books.size(); j++) {
                if (books[j].getId() == bookId && books[j].isBorrowed()) {
                    books[j].giveBack();
                    users[i].returnBook(bookId);
                    return true;
                }
            }
        }
    }
    return false;
}
#include <iostream>
using namespace std;

#include "Library.h"

int main() {
    Library lib;

    lib.addBook(Book(1, "C++ Basics", "Bjarne"));
    lib.addUser(User(1, "Alice"));

    if (lib.borrowBook(1, 1))
        cout << "Book borrowed" << endl;

    if (lib.returnBook(1, 1))
        cout << "Book returned" << endl;

    return 0;
}
