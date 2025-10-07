#include <iostream>
#include <stack>
#include <string>
#include <cctype>
#include <cmath>
using namespace std;

double lastAnswer = 0.0; // store last result

// ----------------- Calculator UI -----------------
void show_calculator() {
    cout << "=========================================\n";
    cout << "|          SCIENTIFIC CALCULATOR        |\n";
    cout << "=========================================\n";
    cout << "|   +   |   -   |   *   |   /   |   %   |\n";
    cout << "| Add   | Sub   | Mul   | Div   | Mod   |\n";
    cout << "-----------------------------------------\n";
    cout << "|   ^   |  sqrt |  log  |  ln   |       |\n";
    cout << "| Power | Root  | log10 |  ln   |       |\n";
    cout << "-----------------------------------------\n";
    cout << "|  sin  |  cos  |  tan  |       |       |\n";
    cout << "| deg   | deg   | deg   |       |       |\n";
    cout << "-----------------------------------------\n";
    cout << "|   (   |   )   |  Ans  | Clear | Exit  |\n";
    cout << "-----------------------------------------\n";
}

// ----------------- Operator precedence -----------------
int precedence(char op) {
    if(op == '+'||op == '-') return 1;
    if(op == '*'||op == '/'||op == '%') return 2;
    if(op == '^') return 3;
    return 0;
}

// ----------------- Apply operation -----------------
double applyOp(double a, double b, char op) {
    switch(op) {
        case '+': return a + b;
        case '-': return a - b;
        case '*': return a * b;
        case '/': 
            if(b == 0) { cout<<"Error: Division by zero!"<<endl; return 0; }
            return a / b;
        case '%': return fmod(a,b);
        case '^': return pow(a,b);
    }
    return 0;
}

// ----------------- Expression evaluator -----------------
double evaluate(string tokens) {
    stack<double> values;
    stack<char> ops;

    for(int i=0; i<tokens.length(); i++) {
        if(tokens[i]==' ') continue;

        // Number parsing (with decimal)
        if(isdigit(tokens[i]) || tokens[i]=='.') {
            double val=0; int dec=-1;
            while(i<tokens.length() && (isdigit(tokens[i]) || tokens[i]=='.')) {
                if(tokens[i]=='.') { dec=0; i++; continue; }
                if(dec==-1) val=val*10+(tokens[i]-'0');
                else val=val+(tokens[i]-'0')/pow(10,++dec);
                i++;
            }
            values.push(val); i--;
        }

        // Function parsing (sin, cos, tan, sqrt, log, ln)
        else if(isalpha(tokens[i])) {
            string func;
            while(i<tokens.length() && isalpha(tokens[i])) { func+=tokens[i]; i++; }

            // Argument in parentheses
            if(tokens[i]=='(') {
                int j=i, br=0; string arg;
                for(; j<tokens.length(); j++) {
                    if(tokens[j]=='(') br++;
                    else if(tokens[j]==')') br--;
                    if(br==0) break;
                    if(j!=i) arg+=tokens[j];
                }
                i=j;

                double val = evaluate(arg);
                if(func=="sin") val=sin(val*M_PI/180.0);
                else if(func=="cos") val=cos(val*M_PI/180.0);
                else if(func=="tan") val=tan(val*M_PI/180.0);
                else if(func=="sqrt") { if(val<0){cout<<"Error: sqrt(-x)"<<endl; val=0;} else val=sqrt(val);}
                else if(func=="log") { if(val<=0){cout<<"Error: log<=0"<<endl; val=0;} else val=log10(val);}
                else if(func=="ln") { if(val<=0){cout<<"Error: ln<=0"<<endl; val=0;} else val=log(val);}
                else { cout<<"Unknown function: "<<func<<endl; val=0; }
                values.push(val);
            }
        }

        // Parentheses
        else if(tokens[i]=='(') ops.push(tokens[i]);
        else if(tokens[i]==')') {
            while(!ops.empty() && ops.top()!='(') {
                double b=values.top(); values.pop();
                double a=values.top(); values.pop();
                char op=ops.top(); ops.pop();
                values.push(applyOp(a,b,op));
            }
            if(!ops.empty()) ops.pop();
        }

        // Operators
        else {
            while(!ops.empty() && precedence(ops.top())>=precedence(tokens[i])) {
                double b=values.top(); values.pop();
                double a=values.top(); values.pop();
                char op=ops.top(); ops.pop();
                values.push(applyOp(a,b,op));
            }
            ops.push(tokens[i]);
        }
    }

    while(!ops.empty()) {
        double b=values.top(); values.pop();
        double a=values.top(); values.pop();
        char op=ops.top(); ops.pop();
        values.push(applyOp(a,b,op));
    }
    lastAnswer = values.top(); // store last result
    return values.top();
}

// ----------------- Display Output -----------------
  void display_output(double answer) {
    cout << "=========================================\n";
    cout << "|           CURRENT OUTPUT               |\n";
    cout << "-----------------------------------------\n";
    cout << "| Result: " << answer << endl;
    cout << "=========================================\n";
}

// ----------------- MAIN -----------------
int main() {
    string expr;
    while(true) {
        system("cls"); // Windows; change to "clear" for Linux/Mac
        show_calculator();
        if(lastAnswer!=0.0) display_output(lastAnswer);

        cout << "Enter expression (or '.' to exit): ";
        getline(cin, expr);
        if(expr=="." || expr=="exit") break;

        // Replace "Ans" with lastAnswer
        size_t pos;
        while((pos = expr.find("Ans")) != string::npos) {
            expr.replace(pos,3,to_string(lastAnswer));
        }

        double result = evaluate(expr);
        cout << "Result = " << result << endl;
        cout << "Press Enter to continue...";
        cin.ignore();
    }
    return 0;
}
