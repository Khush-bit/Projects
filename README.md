//SUPERMARKET BILLING SYSTEM

#include<iostream>
#include<fstream>
using namespace std;

class shopping
{
    private:
        int pcode;
        float price;
        float discount;
        string pname;

    public:
        void menu();
        void administrator();
        void buyer();
        void add();
        void edit();
        void rem();//remove function...different than the inbuilt one used for fille handling 
        void list();
        void receipt();
};

//refering the class 'shopping'. '::' scope resolution operator
void shopping ::menu()
{
    int choice;
    string email,password; //login

    cout<<"\n\t\t\t____________________________________________________\t\t\t";
    cout<<"\n\t\t\t               SUPERMARKET MAINMENU                  \t\t\t";
    cout<<"\n\t\t\t____________________________________________________\t\t\t";
    cout<<"\n\n";
    cout<<"\n\t\t\t                 1)Administrator";
    cout<<"\n\t\t\t                 2)Buyer";
    cout<<"\n\t\t\t                 3)Exit";
    cout<<"\n\n\t\t\t  Please enter your choice:   ";cin>>choice;

    m:
    switch(choice)
    {
        case 1:
            // system("cls");
            cout<<"\n\t\t\t Please Login using admin email ID";
            cout<<"\n\t\t\t Enter Email: ";cin>>email;
            cout<<"\n\t\t\t Enter Password: ";cin>>password;

            //admin can only be one so admin details is also one
            if(email=="admin_mail_id@email.com" && password=="adminpass")
            {
                cout<<"\n\t\t\t LOGIN SUCCESSFULL!!";
                // system("cls");
                administrator();
            }

            else
            cout<<"SORRY! ADMIN DETAILS DON'T MATCH PLEASE TRY AGAIN!";
            break;

        case 2:
            buyer();
            break;

        case 3:
            cout<<"\n\t\t\tTHANKYOU FOR VISITING! HAVE A GREAT DAY!!";
            exit(0);

        default:
            cout<<"\n\t\t\tPlease enter a valid choice!!";

    }
    goto m;
}

void shopping::administrator()
{
    int choice;

    m:
    cout<<"\n\t\t\t____________________________________________________\t\t\t";
    cout<<"\n\t\t\t               ADMINISTRATOR MENU                  \t\t\t";
    cout<<"\n\t\t\t____________________________________________________\t\t\t";
    cout<<"\n\n";
    cout<<"\n\t\t\t                 1)Add products";
    cout<<"\n\t\t\t                 2)Modify products";
    cout<<"\n\t\t\t                 3)Remove products";
    cout<<"\n\t\t\t                 4)Back to main menu";
    cout<<"\n\n\t\t\t  Please enter your choice:   ";cin>>choice;

    switch(choice)
    {
        case 1:
            add();
            break;
        
        case 2:
            edit();
            break;

        case 3:
            rem();
            break;

        case 4:
            menu();
            break;

        default:
            cout<<"\t\t\t PLEASE ENTER A VALID CHOICE!!";
    }
    goto m;

}

void shopping::buyer()
{
    int choice;

    cout<<"\n\t\t\t____________________________________________________\t\t\t";
    cout<<"\n\t\t\t                       BUYER MENU                  \t\t\t";
    cout<<"\n\t\t\t____________________________________________________\t\t\t";
    cout<<"\n\n";
    cout<<"\n\t\t\t                 1)Buy products";
    cout<<"\n\t\t\t                 2)Back to the main menu";
    cout<<"\n\n\t\t\t  Please enter your choice:   ";cin>>choice;

    m:
    switch(choice)
    {
        case 1:
            receipt();
            break;
        
        case 2:
            menu();
            break;

        default:
            cout<<"\t\t\t PLEASE ENTER A VALID CHOICE!!";
    }
    goto m;

}

void shopping:: add()
{
    m:
    fstream data; //fstream is a class btw
    int c,token=0;
    float p,d;
    string n;

    cout<<"\n\n\t\t Add new product in the list";
    cout<<"\n\t Product code of the new product: ";cin>>pcode;
    cout<<"\n\t Name of the product: ";cin>>pname;
    cout<<"\n\t Price of the product: ";cin>>price;
    cout<<"\n\t Discount on the product (if any): ";cin>>discount;

    data.open("database.txt", ios::in);//to open the file in reading mode
    //"ios::out" will open the file in writing mode

    if(!data)//if this doesn't exist we'll open the file in writing mode to add the item
    {
        data.open("database.txt", ios::app|ios::out);
        data<<" "<<pcode<<" "<<pname<<" "<<price<<" "<<discount<<endl;
        data.close();
    }

    else//if the roduct exists we'll read from the file
    {
        data>>c>>n>>p>>d; // to read the file

        while(!data.eof())//eof is end of file function ...iterating
        {
            if(c==pcode)
            {
                token++;
            }
            data>>c>>n>>p>>d;
        }
        data.close();
    }

    if(token==1)//if there's duplicacy
    {
        goto m;
    }

    else{//if the pcode is unique..we'll write it inside the file
        data.open("database.txt", ios::app|ios::out);
        data<<" "<<pcode<<" "<<pname<<" "<<price<<" "<<discount<<endl;
        data.close();
    }

     cout<<"\n\t\t\t Record updated!!";
     return;

}

void shopping::edit()
{
    fstream data,data1;
    int pkey,token=0,c;
    float p,d;
    string n;

    cout<<"\n\t\t\t Modify the records";
    cout<<"\n\t Enter the product code: ";cin>>pkey;
    
    data.open("database.txt", ios::in);
    
    if(!data)
    cout<<"\n The file doesn't exist!";

    else
    {
        //we'll open another file
        data1.open("database1.txt", ios::app|ios::out);
        //we will edit our data in this file (database1) and then we'll rename it to the original file
        data>>pcode>>pname>>price>>discount; //reading
        while(!data.eof())
        {
            if(pkey==pcode)
            {
                cout<<"\n\t\t Product of new code: ";cin>>c;
                cout<<"\n\t Name of the product: ";cin>>n;
                cout<<"\n\t Price of the product: ";cin>>p;
                cout<<"\n\t Discount on the product (if any): ";cin>>d;

                data1<<" "<<c<<" "<<n<<" "<<p<<" "<<d<<endl;
                cout<<"\n\t\tRECORDS UPDATED!!";
                token++;
            }

            else
            {
                data1<<" "<<pcode<<" "<<pname<<" "<<price<<" "<<" "<<discount<<endl;

            }

            data>>pcode>>pname>>price>>discount;
        }

        data.close();
        data1.close();
        
        //Now we have to rename the database1 to database
        //we'll use remove and rename functions

        remove("database.txt");//it'll remove all the list inside this file
        rename("database1.txt","database.txt"); //it'll rename the first one with the second one

        if(token==0)
        {
            cout<<"\n\n\t Sorry!! Record not found!";
        }
    }

}

void shopping::rem()
{
    fstream data,data1;
    int pkey,token=0;

    cout<<"\n\n\t Delete product";
    cout<<"\n Enter the product code: ";cin>>pkey;

    data.open("database.txt", ios::in);

    if(!data)
    cout<<"\n \t The file does not exist!!";

    else
    {
        data1.open("database1.txt",ios::app|ios::out);
        data>>pcode>>pname>>price>>discount;//reading from the file

        while(!data1.eof())
        {
            if(pkey==pcode)
            {//if the code matches this block will be executed and the loop will move forward
                cout<<"\n\n\t Product deleted successfully!";
                token++;
            }
            else
            {//the iterations when the code will not match those items would be displayed and stored in the database1
            //i.e., all items except the one you want to delete
                data1<<" "<<pcode<<" "<<pname<<" "<<price<<" "<<discount<<endl;
            }
            data>>pcode>>pname>>price>>discount;//to iterate

            data.close();
            data1.close();

            remove("database.txt");
            rename("database1.txt","database.txt");

            if(token==0)
            {
                cout<<"\n\t Record not found!!";
            }
        }

    }
}

void shopping::list()//to show the list to the user
{
    fstream data;
    data.open("database.txt",ios::in);
    cout<<"\n\n_______________________________________________________\n";
    cout<<"ProNo\t\tName\t\tPrice\n";
    cout<<"\n\n_______________________________________________________\n";
    data>>pcode>>pname>>price>>discount;
    while(!data.eof())
    {
        cout<<pcode<<" "<<pname<<" "<<price<<" "<<endl;
        data>>pcode>>pname>>price>>discount;        
    }
    data.close();
}

void shopping:: receipt()
{
    fstream data;
     //since we're storing multiple data of codes, names etc so we'll use array
     int arrc[100],arrq[100],c=0;
     char choice;
     float amount=0,discount=0,total=0;

     cout<<"\n\n\t\t\t RECEIPT";
     data.open("database.txt",ios::in);

     if(!data)
     cout<<"\n \n\t\t EMPTY DATABASE!";

     else{
        data.close();
        list();//to show the user what's available in the supermarket

        cout<<"\n\n_________________________________________________________\n\n\n";
        cout<<"\n                  Please place the order                   \n";
        cout<<"\n\n_________________________________________________________\n\n\n";
        
        m:
        do
        {
            cout<<"\n Enter the product code: ";cin>>arrc[c];
            cout<<"\n Enter the quantity: ";cin>>arrq[c];

            //if the entered product has already been entered in the array you arent supposed to repeat it 
            //basically to avoid repeatations or duplicacy

            for(int i=0;i<c;i++)
            {
                if(arrc[c]==arrc[i])
                cout<<"\n\n\t Dulicate product code. Please try again!";
                goto m;
            }
            c++;//if the product code is unique

            cout<<"\n \n Do you want to buy another product (1/0): ";cin>>choice;


        }while(choice==1);

        cout<<"\n\n\t\t\t____________________RECEIPT__________________________\n";
        cout<<"\n Product No. \t Product Name \t Product quantity \t price \t Total Amount \t Amount after discount\n";

        for(int i=0;i<c;i++)
        {
            data.open("database.txt",ios::in);
            data>>pcode>>pname>>price>>discount;

            while(!data.eof())
            {
                if(pcode==arrc[i])
                {
                    amount=price*arrq[i];
                    discount=amount-(amount*discount/100);
                    total+=discount;

                    cout<<"\n"<<pcode<<"\t\t"<<pname<<"\t\t"<<arrq[i]<<"\t\t"<<price<<"\t"<<amount<<"\t\t"<<discount;

                }
                data>>pcode>>pname>>price>>discount;

            data.close();
            }

        }

        cout<<"\n\n_____________________________________________________________\n";
        cout<<"\nTotal Amount: "<<total;

     }
}

int main()
{
    //creating an object of the shopping class;
    shopping s;
    s.menu();
}
