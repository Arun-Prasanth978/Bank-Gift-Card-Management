#include<bits/stdc++.h>
using namespace std;
class Account 
{
    string id;
    string password;
    double balance;
    public:
      Account(string id,string password,double balance)
      {
          this->balance=balance;
          this->password=password;
          this->id=id;
      } 
      string getAccId()
      {
          return id;
      }
      string getPassword()
      {
          return password;
      }
      double getBalance()
      {
          return balance;
      }
      void addBalance(double amount)
        {
            balance=balance+amount;
        }
        void removeBalance(double amount)
        {
            balance=balance-amount;
        }
      Account()
      {
          
      }
};
class Customer 
{
    string name;
    string id;
    string password;
    string AccId;
    vector<string>giftcards;
    public:
    Customer(string name,string id,string password,string AccId)
    {
        this->name=name;
        this->id=id;
        this->AccId=AccId;
        this->password=password;
    }
    string getName()
    {
        return name;
    }
    void addGiftcard(string id)
    {
        giftcards.push_back(id);
    }
    string getCusId()
    {
        return id;
    }
    string getCusPass()
    {
        return password;
    }
    string getCusAccount()
    {
        return AccId;
    }
    vector<string> getCusgiftCards()
    {
        return giftcards;
    }
    Customer()
    {
        
    }
};
class GiftCard 
{
    string cardNumber;
    string Password;
    double balance;
    int points;
    vector<string>History;
    public:
    GiftCard(string number,string password,double balance,int points)
    {
        this->balance=balance;
        this->cardNumber=number;
        this->points=points;
        this->Password=password;
    }
    GiftCard()
    {
        
    }
    double getBalance()
    {
        return balance;
    }
    string getId()
    {
        return cardNumber;
    }
    string getPassword()
    {
        return Password;
    }
    void addHistory(string history)
    {
        History.push_back(history);
    }
    void addPoints(int point)
    {
        this->points=this->points+point;
    }
    void removePoints(int point)
    {
        this->points=this->points-point;
    }
    void addBalance(double amount)
    {
        balance=balance+amount;
    }
    void removeBalance(double amount)
    {
        balance=balance-amount;
    }
    int getPoint()
    {
        return points;
    }
    void printHistory()
    {
        cout<<"Transaction History"<<endl;
        for(int i=0;i<History.size();i++)
        {
            cout<<History[i]<<endl;
        }
    }
};
class Main 
{
    static int ai,ci,gi,gpp;
    vector<Account>allaccounts;
    vector<Customer>allcustomers;
    vector<GiftCard>allgiftcards;
    public:
    void addAccount()
    {
        string name,cId,cpass,aId,pass,rpass;
        int amount;
        cout<<"Enter Your Name:";
        cin>>name;
        cout<<"Enter the Customer Password:";
        cin>>cpass;
        cout<<"Enter Account Password:";
        cin>>pass;
        cout<<"Enter deposit Amount:";
        cin>>amount;
        cId="CC"+to_string(ci);
        ci++;
        aId="AA"+to_string(ai);
        ai++;
        allaccounts.push_back(Account(aId,pass,amount));
        rpass=cpass;
        cpass=encrypted(cpass);
        allcustomers.push_back(Customer(name,cId,cpass,aId));
        cout<<endl;
        cout<<"Created Successfully"<<endl;
        cout<<"Your Name:"<<name<<endl;
        cout<<"Your Id:"<<cId<<endl;
        cout<<"Your Password:"<<rpass<<endl;
        cout<<"Your Account Id:"<<aId<<endl;
        cout<<"Your Account Password:"<<pass<<endl;
        cout<<"Your Account Balance:"<<amount<<endl<<endl;
    }
    void createGiftCard(string cid)
    {
        string id,pass;
        id="GG"+to_string(gi);
        gi++;
        pass="GPP"+to_string(gpp);
        gpp++;
        allgiftcards.push_back(GiftCard(id,pass,0,0));
        cout<<endl;
        cout<<"Created Successfully"<<endl;
        cout<<"Your Gift Card Number:"<<id<<endl;
        cout<<"Your Gift Card Password:"<<pass<<endl<<endl;
        for(int i=0;i<allcustomers.size();i++)
        {
            if(cid==allcustomers[i].getCusId())
            {
                allcustomers[i].addGiftcard(id);
                return;
            }
        }
    }
    void TopUp(string cid)
    {
        Account *a;
        GiftCard *g;
        Customer *c;
        vector<string>cards;
        string num,Acc;
        double amount;
        cout<<"Enter Customer Gift Card Number:";
        cin>>num;
        cout<<"Enter Top Up Amount:";
        cin>>amount;
        int i,j,k;
        for(i=0;i<allcustomers.size();i++)
        {
            if(cid==allcustomers[i].getCusId())
            {
                c=&allcustomers[i];
                break;
            }
        }
        Acc=c->getCusAccount();
        for(int j=0;j<allaccounts.size();j++)
        {
            if(Acc==allaccounts[j].getAccId())
            {
                a=&allaccounts[j];
                break;
            }
        }
        if(amount<=a->getBalance())
        {
            a->removeBalance(amount);
        }
        else 
        {
            cout<<"No Money"<<endl;
            return;
        }
        cards=allcustomers[i].getCusgiftCards();
        for(k=0;k<allgiftcards.size();k++)
        {
            if(num==allgiftcards[k].getId())
            {
                g=&allgiftcards[k];
                break;
            }
        }
        g->addBalance(amount);
        string history="Top Up Amount:"+to_string(amount);
        g->addHistory(history);
        cout<<"Top Up Successful"<<endl;
    }
    void TransactionHistory(string cid)
    {
        vector<string>cards;
        string num;
        cout<<"Enter Gift Card Number:";
        cin>>num;
        for(int i=0;i<allgiftcards.size();i++)
        {
            if(num==allgiftcards[i].getId())
            {
               allgiftcards[i].printHistory();
               break;
            }
        }
    }
    void block(string cid)
    {
        string num,Acc;
        cout<<"Enter Your Gift Card Number:";
        cin>>num;
        double amount;
        for(int i=0;i<allcustomers.size();i++)
        {
            if(cid==allcustomers[i].getCusId())
            {
                Acc=allcustomers[i].getCusAccount();
                break;
            }
        }
        for(auto it=allgiftcards.begin();it!=allgiftcards.end();it++)
        {
            if(it->getId()==num)
            {
                amount=it->getBalance();
                allgiftcards.erase(it);
                break;
            }
        }
        for(int i=0;i<allaccounts.size();i++)
        {
            if(Acc==allaccounts[i].getAccId())
            {
                allaccounts[i].addBalance(amount);
                break;
            }
        }
        cout<<"Block Successful"<<endl;
    }
    void Accountlogin()
    {
        string id,pass;
        cout<<"Enter the Customer Id:";
        cin>>id;
        cout<<"Enter the Password:";
        cin>>pass;
        pass=encrypted(pass);
        string cid;
        for(int i=0;i<allcustomers.size();i++)
        {
            cid=allcustomers[i].getCusId();
            string cpass=allcustomers[i].getCusPass();
            if(id==cid && pass==cpass)
            {
                cout<<"Login Successfully"<<endl;
            }
            else 
            {
                cout<<"No Customer Found";
                return;
            }
            break;
        }
        int choice,p=1;
        do 
        {
            cout<<"1.Create Gift Card"<<endl; 
            cout<<"2.Top Up"<<endl; 
            cout<<"3.Transaction History"<<endl; 
            cout<<"4.Block"<<endl; 
            cout<<"5.Log out"<<endl; 
            cin>>choice;
            switch(choice)
            {
                case 1:
                   createGiftCard(cid);
                   break;
                case 2:
                   TopUp(cid);
                   break;
                case 3:
                   TransactionHistory(cid);
                   break;
                case 4:
                   block(cid);
                   break;
                case 5:
                   p=0;
                   cout<<"Log Out Successful"<<endl;
                   break;                   
            }
        }
        while(p);
    }
    string encrypted(string pass)
    {
        string password="";
        for(int i=0;i<pass.size();i++)
        {
          if (isalpha(pass[i])) 
          {
            if (pass[i] == 'z') 
            {
                password=password+'a'; 
            }
            else if (pass[i] == 'Z')
            {
                password=password+'A'; 
            }
            else 
            {
                password=password+char(pass[i]+1);
            }
          } 
          else if (isdigit(pass[i])) 
          {
            if (pass[i] == '9') 
            {
                password=password+'0'; 
            }
            else 
            {
                password=password+char(pass[i]+ 1); 
            }
          }
          else 
          {
            password=password+pass[i];
          }
        }
        return password;
    }
    void purchase()
    {
        string num,pass,Number,password;
        cout<<"Enter your Gift Card Number:";
        cin>>num;
        cout<<"Enter your Password:";
        cin>>pass;
        for(int i=0;i<allgiftcards.size();i++)
        {
            Number=allgiftcards[i].getId();
            password=allgiftcards[i].getPassword(); 
            if(Number==num && pass==password)
            {
                cout<<"Login Successful"<<endl;
            }
            else 
            {
                cout<<"No Gift Card Found"<<endl;
                return;
            }
            break;
        }
        int choice,p=1;
        do 
        {
            cout<<"1.Purchase Amount"<<endl; 
            cout<<"2.Redeem Amount"<<endl; 
            cout<<"3.Exit"<<endl<<endl; 
            cin>>choice;
            switch(choice)
            {
                case 1:
                  purchase(Number);
                  break;
                case 2:
                  redeempoints(Number);
                  break;
                case 3:
                  p=0;
                  break;
            }
        }
        while(p);
    }
    void purchase(string Number)
    {
        GiftCard *g;
        double amount;
        cout<<"Enter Amount:";
        cin>>amount;
        int point;
        for(int i=0;i<allgiftcards.size();i++)
        {
            if(Number==allgiftcards[i].getId())
            {
                if(allgiftcards[i].getBalance()>=amount)
                {
                    g=&allgiftcards[i];
                    g->removeBalance(amount);
                    point=int(amount)/100;
                    g->addPoints(point);
                    cout<<"Purchase Successful"<<endl;
                    string history="Purchase Amount:"+to_string(amount);
                    g->addHistory(history);
                    break;
                }
                else 
                {
                    cout<<"No Required Money"<<endl;
                    return;
                }
            }
        }
    }
    void redeempoints(string GifId)
    {
        GiftCard *g;
        string cusId,AccId;
        double amount;
        cout<<"Enter Customer Id:";
        cin>>cusId;
        for(int i=0;i<allcustomers.size();i++)
        {
            if(cusId==allcustomers[i].getCusId())
            {
                AccId=allcustomers[i].getCusAccount();
                break;
            }
        }
        for(int i=0;i<allgiftcards.size();i++)
        {
            if(allgiftcards[i].getId()==GifId)
            {
                g=&allgiftcards[i];
                amount=double(allgiftcards[i].getPoint());
                allgiftcards[i].removePoints(int(amount));
            }
            break;
        }
        for(int i=0;i<allaccounts.size();i++)
        {
            if(AccId==allaccounts[i].getAccId())
            {
                allaccounts[i].addBalance(amount);
                cout<<"Redeem Successfull"<<endl;
                return;
            }
            
        }
    }
    
};
int Main::gpp=1;
int Main::ai=1;
int Main::ci=1;
int Main::gi=1;
int main()
{
    int choice,l=1;
    Main m;
    do 
    {
        cout<<"1. Add Customer"<<endl;
        cout<<"2. Account login"<<endl;
        cout<<"3. Purchase"<<endl;
        cout<<"4. Exit"<<endl<<endl;
        cin>>choice;
        switch(choice)
        {
            case 1:
              m.addAccount();
              break;
            case 2:
              m.Accountlogin();
              break;
            case 3:
              m.purchase();
              break;
            case 4:
              l=0;
              break;
        }
    }
    while(l);
}
