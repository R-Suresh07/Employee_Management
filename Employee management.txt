#include <iostream>
using namespace std;
class Employee{
public:
    float salary;
    string name;
    int projects_completed;
    int age;
    int appraisal_score;
    int E_id;
private:
    friend void display_salary(Employee);
public:
    Employee(){
        name="NULL";
        projects_completed=0;
        salary=0;
        age=0;
        appraisal_score=0;
        E_id=0;
    }
    void copy_employee(Employee &E){
        name=E.name;
        projects_completed=E.projects_completed;
        salary=E.salary;
        age=E.age;
        appraisal_score=E.appraisal_score;
        E_id=E.E_id;
    }
    friend ostream & operator << (ostream &out, const Employee &e);
    friend istream & operator >> (istream &in, Employee &e);
    void input(){
        cout<<"Enter the name of the employee"<<endl;
        cin>>name;
        cout<<"Enter the number of projects completed of the employee"<<endl;
        cin>>projects_completed;
        cout<<"Enter the salary of the employee"<<endl;
        cin>>salary;
        cout<<"Enter the age of the employee"<<endl;
        cin>>age;
        cout<<"Enter Appraisal Score of the employee"<<endl;
        cin>>appraisal_score;
        cout<<"Enter Employee ID of the employee"<<endl;
        cin>>E_id;
    }
    void output(){
        cout<<"The name is:"<<name<<endl;
        cout<<"Number of Projects completed is:"<<projects_completed<<endl;
        cout<<"Salary is:"<<salary<<endl;
        cout<<"Age is:"<<age<<endl;
        cout<<"Appraisal Score is:"<<appraisal_score<<endl;
        cout<<"Employee ID is:"<<E_id<<endl;
    }



};
void display_salary(Employee e){
    cout<<"Salary of the employee is:"<<e.salary<<endl;
}
void operator > (Employee e1,Employee e2){
    if(e1.salary>e2.salary){
        cout<<"First object is bigger";
    }
    else
        cout<<"Second object is bigger";
}
ostream & operator << (ostream &out, const Employee &e){
    out<<"The name is:"<<e.name<<endl;
    out<<"Number of Projects completed is:"<<e.projects_completed<<endl;
    out<<"Salary is:"<<e.salary<<endl;
    out<<"Age is:"<<e.age<<endl;
    out<<"Appraisal Score is:"<<e.appraisal_score<<endl;
    out<<"Employee ID is:"<<e.E_id<<endl;
    out<<endl;
    return out;
}

istream & operator >> (istream &in, Employee &e){
    cout<<"Enter the name of the employee"<<endl;
    in>>e.name;
    cout<<"Enter the number of projects completed of the employee"<<endl;
    in>>e.projects_completed;
    cout<<"Enter the salary of the employee"<<endl;
    in>>e.salary;
    cout<<"Enter the age of the employee"<<endl;
    in>>e.age;
    cout<<"Enter Appraisal Score of the employee"<<endl;
    in>>e.appraisal_score;
    cout<<"Enter Employee ID of the employee"<<endl;
    in>>e.E_id;
    return in;
}
class Department{
protected:
    string department_name;
    int D_id;
    int employee_num;
    double budget;
public:
    Department(){
        department_name="NULL";
        employee_num=0;
        budget=0;
        D_id=0;
    }
    Department(string department_name,int employee_num,double budget,int D_id){
        this->department_name=department_name;
        this->employee_num=employee_num;
        this->budget=budget;
        this->D_id=D_id;
    }
    void input_Department(){
        cout<<"Enter Department Name:"<<endl;
        cin>>department_name;
        cout<<"Enter number of employees:"<<endl;
        cin>>employee_num;
        cout<<"Enter Department's budget:"<<endl;
        cin>>budget;
        cout<<"Enter Department ID:"<<endl;
        cin>>D_id;
    }

    void display();
    virtual int average_budget()=0;
};
inline void Department::display() {
    cout<<"Name of the Department is:"<<department_name<<endl;
    cout<<"Number of Employees is:"<<employee_num<<endl;
    cout<<"Department budget is:"<<budget<<endl;
    cout<<"Employee ID is:"<<D_id<<endl;
}
class HRDepartment:public Department{
    Employee *e=new Employee[20];
public:
    void input_details(){
        input_Department();
        for(int i=0;i<employee_num;i++){
            e[i].input();
        }
    }
    void appraise_Employee(){
        cout<<"Enter the ID of the employee to be appraised";
        int temp_id;
        cin>>temp_id;
        for(int i=0;i<employee_num;i++){
            if(e[i].E_id == temp_id){
                try{
                    if(e[i].projects_completed==0)
                        throw 0;
                    if(e[i].appraisal_score>75 && e[i].projects_completed>2)
                        e[i].salary=e[i].salary*1.02;
                    else if(e[i].appraisal_score>85 && e[i].projects_completed>3)
                        e[i].salary=e[i].salary*1.03;
                    else if(e[i].appraisal_score>90 && e[i].projects_completed>4)
                        e[i].salary=e[i].salary*1.05;
                }catch(int a) {
                    cout<<"Employee has done 0 projects and can't be appraised\n";
                }
            }
            else
                cout<<"No such Employee found";

        }
    }
    void best_employee(){
        int max_appraisal_score=-1;
        int max=-1;
        for(int i=0;i<employee_num;i++){
            if(max_appraisal_score<e[i].appraisal_score){
                max_appraisal_score=e[i].appraisal_score;
                max=i;
            }
        }
        cout<<"Best Employee is:"<<endl;
        e[max].Employee::output();
    }
    void display(){
        Department::display();
        for(int i=0;i<employee_num;i++){
            e[i].output();
        }
    }
    int average_budget() override{
        return budget/employee_num;
    }
    ~HRDepartment(){
        delete[] e;
    }
};
int main() {
    cout<<"\t\t************** EMPLOYEE MANAGEMENT ***************\n";
    char choice;
    cout<<"Enter the Number corresponding to the operation to be performed\n";
    cout<<"1)See all functions of the employee class\n";
    cout<<"2)See all functions of the HRDepartment\n";
    cout<<"3)Exit\n";
    cin>>choice;
    switch(choice){
        case '1': {
            Employee e1;
            Employee e2;
            cin >> e1;
            cout << e1;
            cin >> e2;
            cout << e2;
            display_salary(e1);
            display_salary(e2);
            e1 > e2;
        }
            break;
        case '2': {
            HRDepartment h;
            h.input_details();
            cout<<endl;
            h.appraise_Employee();
            cout<<endl;
            h.best_employee();
            cout<<endl;
            h.display();
            cout<<endl<<"The average budget is"<<h.average_budget()<<endl;
        }
            break;
        case '3':
        {
            exit(0);
        }
        default:
        {
            cout<<"Please enter only the above choices\n";
        }
            break;
        }

    return 0;
}
/*
 Concepts used:
 1)class and object
 2)inheritance
 3)friend function
 4)constructor and destructor
 5)operator overloading
 6)pure virtual functions
 7)Exception Handling
 8)function overriding
 9)function overloading
 10)inline functions
 11)Dynamic class objects
 */