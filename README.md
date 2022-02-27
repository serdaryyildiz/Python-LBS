# Python-LBS
Python Library Management System 

from os import name


students = {}
bookls = {}
booknames = {}
bookISBN = [str(bookls.keys)]
bookname = []
bookauthor = []
bookchecked = {}

with open("books.txt","r") as file:
    for line in file:
        aline = line.rstrip().split(",")
        bookls[aline[0]] = (aline[1] + "," + aline[2] + "," + aline[3])
        booknames[aline[0]] = aline[1]
    file.close()

def books_list():
    file = open("books.txt","r")
    for line in file:
        linesp = line.rstrip().strip().split(",")
        print(linesp[0] + "," + linesp[1] + "," + linesp[2])
    file.close


def checkedbooks():
    eyy = 1
    file = open("books.txt","r")
    for line in file:
        checkedsp = line.strip().split(",")
        if "T" in checkedsp[3]:
            eyy = 2
            bookchecked[checkedsp[0]] = (checkedsp[1] + "," + checkedsp[2])
    if (eyy == 1):
        print("Checked book list are empty!")
    print("Checked books are :")
    for a in bookchecked:
        print(a + "," + bookchecked[a])
    file.close()

def noncheckedbooks():
    asd = 1
    file = open("books.txt","r")
    for line in file:
        checkedsp = line.strip().split(",")
        if "F" in checkedsp[3]:
            asd = 2
            bookchecked.append(checkedsp[0]+ "," + checkedsp[1] + "," + checkedsp[2])
    if (asd == 1):
        print("There is not any available book in library for now.")
    print("Available books are :")
    for a in bookchecked:
        print(a)
    file.close()         

def addbook():
    namebook = input("Please enter the book name:")
    ISBNbook = str(input("Please enter the ISBN of the book:"))
    if len(ISBNbook) == 10:
        if ISBNbook not in bookls.keys():
            authorbook = input("Please enter the author of the book:")
            with open("books.txt","a") as i :
                i.write(ISBNbook + "," + namebook + "," + authorbook + "," + "F" + "\n")
                i.close()
            print(f"Thanks ! You've donated {namebook} to our library ! ^^ ")
        else:
            print("This ISBN number already exists.Please enter valid number.")
    else:
        print("Valid ISBN number.")

def searchbookISBN():
    isbn = input("Please enter the ISBN number: ")
    if isbn in bookls.keys():
            booklsp = bookls[isbn].split(",")
            if booklsp[2] == "F":
                print("Your book is in the library.")
                print(booklsp)
            else:
                print( booklsp[0] + "," + booklsp[1])
                print("Book has already taken.You can display checked books by second method^^")
    else:
        print("Sorry! Your book is not in the library , if you want to donate you can choose the third method ^^")
    
def searchbookname():
    nameofb = input("Please enter the name of book :")
    nameofb = nameofb.lower()
    listOfBooks = open("books.txt","r")
    t = 1
    for aline in listOfBooks:
        alinesp = aline.rstrip().split(",")
        lowercases = alinesp[1].lower()
        if nameofb in lowercases:
            t = 2
            if alinesp[3] == "F":
                print(alinesp[0] + " , " + alinesp[1] + " , "  + alinesp[2] + "\n" + "Book is in library.You can take it")
            else:
                print(alinesp[0] + " , "  + alinesp[1] + " , " + alinesp[2] + "\n" + "Book has already taken.You can display checked out books by second method^^")
    if (t == 1):
        print("Sorry , your book is not in library.If you want to donate you can choose third method^^")
        listOfBooks.close()

def borrowbook():
    t = 1
    k = 1
    studentid = input("Please enter your Student ID: ")
    if len(studentid) == 6:
        listOfBooks = open("books.txt", "r+", encoding="UTF-8")
        listOfStudents = open("students.txt", "r+",encoding="UTF-8")
        studentlines = listOfStudents.readlines()
        for line in studentlines:
            std1 = line.strip().split(",")
            if studentid in std1[0]:
                k = 2
                print(f"Correct ID\nName/Surname: {std1[1]}")
                borrowchoice = input("1 ├ Enter ISBN Number\n2 ├ Display available books\n3 ├ Search a book by name\n4 ├ Back to Main Menu\nPlease enter your choice: ")
                if borrowchoice == "1":
                    isbn = input("Please enter the ISBN Number\nISBN Number :")
                    if len(isbn) != 10:
                        print("Invalid number!")
                        return borrowbook()
                    else:
                        if isbn in bookls.keys():
                            booklsp = bookls[isbn].split(",")
                            if booklsp[2] == "F":
                                print("You've borrowed " + booklsp[0] + " book.Don't forget to bring it back! ^^")
                                fline = bookls[isbn].split(",")
                                fline[-1] = "T"
                                bookls[isbn] = (fline[0] + "," + fline[1] + "," + fline[2])
                                for i in bookls:
                                    listOfBooks.write(i + "," + bookls[i] + "\n")
                                studentlines.insert(studentlines.index(line)+1,(booklsp[0] + "," + booklsp[1] + "\n"))
                                listOfStudents.seek(0)
                                listOfStudents.writelines(studentlines)
                            else:
                                print("This book has already taken.Please choose another one.^^")
                        else:
                            print("Wrong ISBN Number!")
                            return borrowbook()
                elif borrowchoice == "2":
                    return noncheckedbooks()
                elif borrowchoice == "3":
                    nameofb = input("Please enter the name of book :")
                    nameofb = nameofb.lower()
                    for aline in listOfBooks:
                        alinesp = aline.rstrip().split(",")
                        lowercases = alinesp[1].lower()
                        if (nameofb in lowercases) and (alinesp[3] == "F"):
                            t = 2
                            print(alinesp[0] + " , " + alinesp[1] + " , "  + alinesp[2]) 
                    if (t == 1 ):
                        print("Your book is not in library.")
                        choose2 = input("1 | Display available books.\n2 | Back to Main Menu.\nPlease enter your choice: ")
                        if choose2 == "1":
                            return noncheckedbooks()
                        elif choose2 == "2":
                            return mainmenu
                        else:
                            print("Please enter valid number!")
                            return borrowbook()
                else:
                    print("Please enter a valid number!")
                    return mainmenu
        if (k == 1):  
            print("Your Student ID is not correct.")
            choose1 = input("1 | Try again.\n2 | Back to Main Menu.\nPlease enter your choice: ")
            if choose1 == "1":
                return borrowbook()
            elif choose1 == "2":
                return mainmenu
            else:
                print("Please enter valid number!")
    else:
        print("Invalid Number!")
        return borrowbook()
    listOfStudents.close()
    listOfBooks.close()



    


def liststudents():
    with open("students.txt","r",encoding="UTF-8") as file:
        print("Student List :")
        for student in file:
            students = student.strip().rstrip().split(",")
            print(student)
    file.close()

while True:

    mainmenu = input("********************\n1-> List the books\n2-> List checked books\n3-> Add a new book\n4-> Search book\n5-> Borrow book/books\n6-> List students\n7-> Exit\n********************\nPlease enter your choice:")
    if mainmenu == "1":
        books_list()
    elif mainmenu == "2":
        checkedbooks()
    elif mainmenu == "3":
        addbook()

    elif mainmenu == "4":
        sbook = input("\n1 -> Search by ISBN number\n2 -> Search by book name\nPlease choose the searching method:")
        if sbook == "1":
            searchbookISBN()
        elif sbook =="2":
            searchbookname()
        else:
            print("Invalid number!")
    elif mainmenu == "5":
        borrowbook()
    elif mainmenu == "6":
        liststudents()
    elif mainmenu == "7":  
        break
    else:
        print("Please enter valid number!")
